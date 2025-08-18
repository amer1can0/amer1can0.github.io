---
title: File Upload Vulnerability
layout: default
parent: Web Penetration Test
nav_order: 2.5
description: "Web hacking"
---

# File Upload Vulnerability (파일 업로드 취약점)

{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## What is file upload vulnerability (파일 업로드 취약점이란)

파일 업로드 취약점이란, 웹 서버에서 사용자가 파일을 업로드할때, 파일의 이름, 타입, 내용, 사이즈 등 다양한 정보들을 점검을 안하고 서버에 저장을 할때, 업로드 취약점이 발생 합니다.

웹서버에서 보통 위험한 파일 타입, 속성들을 막지만 이러한 설정을 똑바로 하지않으면 타입 같은경우, 여러 파일타입을 같이 넣어서 웹서버에 혼동을 줄수도 있고, 속성 같은경우, 웹 프록시로 수정하여 우회 할수도 있을것 입니다. 

## What is the impact of file upload vulnerablity (파일 업로드 취약점의 임팩트)

서버가 마비되거나, Remote Command Execution (RCE), 파일 업로드를 트로이젠으로 쓴다거나, 등 같은 다양한 크리티컬한 취약점을 발생 시킬수 있습니다. 

## How web servers handle request for static files (웹 서버에서 Static File의 대한 핸들링)

1. Non Executable File (실행이 불가능한 파일): 웹 서버에서 파일 컨텐츠를 HTTP Resposne로 그냥 보낼수 있습니다. 이러한 경우, 웹 서버에서 해당 파일을 읽게되는 경우 Stored XSS 취약점이 발생 할 수 있습니다.


{: .important-title }
> Proof of Concept
> {: .label .label-blue }
>```
><html>
><script>alert("NO FILTER?")
></html>
>```

2. Executable File (실행이 가능한 파일): 웹 서버에서 해당 파일 컨텐츠를 실행이 가능하게 시켰을 경우, 서버는 스크립트를 실행하기 전에 HTTP 요청의 헤더와 파라미터를 기반으로 변수를 할당합니다. 그 결과로 생성된 출력물은 HTTP 응답으로 클라이언트에게 전송 될수 있습니다.

{: .important-title }
> Proof of Concept
> {: .label .label-blue }
>```
><?php echo system($_GET['cmd']); ?>
>```

위의 POC경우, `?cmd=` 파라미터로 해당 코드를 실행 할수 있을것 입니다.

3. Executable File (실행이 가능한 파일): 웹 서버에서 파일을 실행하도록 설정이 안되어 있다면, 일반적으로 Error로 응답합니다. 하지만 경우에 따라서 파일이 Plain Text로 클라이언트에게 그대로 전송되게 할수 있습니다.


## Bypassing file type validation (파일 타입 우회)

보통 파일 업로드를 진행을 할때, `Content-Type` 헤더가 들어갑니다. 이러한 헤더는 웹 서버에게 "MIME" 타입을 알려주는 역할을 하게 됩니다. 그러면 웹서버가 `Content-Type` 헤더를 보고 예측되는 "MIME" 타입이 맞게 되면 파일 업로드를 허용합니다.

허나 문제가 발생하는 경우는 웹서버가 파일의 실제 내용을 점검을 안하고 오로지 "MIME" 타입과 `Content-Type` 매칭을 보고 판단을 할경우 `Content-Type`을 바꿔서 해당 필터링을 우회 할 수 있습니다. 

{: .important-title }
> Proof of Concept
> {: .label .label-blue }
>```
>Content-Type: image/jpeg 
>```