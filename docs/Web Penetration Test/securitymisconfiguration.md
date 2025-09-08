---
title: Security Misconfiguration
layout: default
parent: Web Penetration Test
nav_order: 2.7
description: "Web hacking"
---


# Bypass 403 Forbidden

{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

"403 Forbidden"은 웹 서버가 요청을 거부했을 때 발생하는 HTTP 상태 코드 중 하나 입니다. 403 오류 메세지는 사용자가 요청한 리소스에 접근할 권한이 없다는 뜻을 의미 합니다. 보통 서버측에서 권한 설정을 잘못 했거나, 요청한 페이지나, 특정 파일이 특정 사용자에게만 허용되었을때 발생할 수 있습니다.

## Reasons of 403 Forbidden

403 Forbidden이 발생하는 일반적인 이유는 다음과 같습니다.

1. Not Enough Privileges (권한 부족): 서버가 해당 리소스에 대한 접근을 제한하거나, 로그인한 사용자가 접근할 수 없는 리소스를 요청했을 때 발생할 수 있습니다.

2. IP 차단: 특정 IP나 지역에서의 접근을 차단했을 때도 발생할수 있습니다.

3. WAF (Web Application Firewall)로 인한 차단: 웹 서버에 대한 다양한 공격들을 감지하면 WAF에서 403 Forbidden 응답을 반환할 수 있습니다. 

4. 봇 탐지 및 차단: 봇이 서버에 불법적으로 접근하거나 스팸, 크롤링, 디도스 등 악의적인 행동을 하게되면 403 Forbidden 오류가 발생할 수 있습니다.

5. CORS (Cross-Origin Resource Sharing): 보안상의 이유로, 웹 브라우저에서 다른 도메인에서 온 리소스에 대한 접근 제한을 할수 있습니다. 

6. 디렉토리 리스팅 차단: 디렉토리 리스팅을 허용하면 공격자가 서버의 파일 구조를 탐색할 수 있습니다. 이것을 방지하기 위해 디렉토리 리스팅 차단하는 설정을 할 수 있습니다.

