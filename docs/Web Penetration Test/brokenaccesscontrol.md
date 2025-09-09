---
title: Broken Access Control
layout: default
parent: Web Penetration Test
nav_order: 2.6
description: "Web hacking"
---

# Broken Access Control 

{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Access Control 은 무엇인가?

Access Control이란 "접근제어" 시스템으로 특정 사용자가 웹 사이트 특정 리소스 (페이지, 파일, 데이터) 에 대해 접근하거나 특정 기능을 수행하는 것을 허용하거나 차단하는 보안 기능입니다. 

Access Control은 두가지 핵심 프로세스를 통해 이루어 집니다.

1. Authentication (인증)
2. Authorization (인가)

## Authentication (인증)

Authentication은 사용자가 누구인지 확인하는 과정입니다. 사용자 ID, 비밀번호, 바이오메트릭스, MFA등을 통해 사용자의 신원을 증명 합니다. 

## Authorization (인가)

Authorization은 사용자가 특정 리소스에 대해 어떤 권한 (CRUD: Create, Read, Update Delete) 을 가지고 있는지 결정하는 과정입니다. 

해당 정책에 따라 특정 사용자 또는 그룹에게만 접근을 허용합니다. 

## Vertical Access Control (수직 접근제어)

Vertical Access Control (수직 접근제어)란 민감한 기능들을 특정 권한 혹은 타입의 사용자에게 부여하는것 입니다. 예를들어, 수직 접근제어를 통해 관리자의 역할의 사용자는 다른 일반유저와 다르게, 유저를 초대할수도, 지울수도, 접근을 제한 할 수 있을것 입니다. 

## Horizontal Access Control (수평 접근제어)

Horizontal Access Control (수평 접근제어)란 다른유저의 대한 리소스를 제한하는 것을 의미합니다. 예를들어, 수평 접근 제어를 통해 쇼핑몰 사이트에서 모든 유저들이 장바구니 페이지를 접근할수 있지만, 다른 유저들의 장바구니 페이지를 접근할수는 없을것 입니다. 

## Broken Access Control Vulnerability (접근제어 취약점)

### Unprotected admin functionality 

가장 기본적인 유형의 Broken Access Control 취약점은 관리자권한의 Endpoint나 기능들에 대해 아무런 방어메커니즘을 안해놓은것 입니다.

해당 웹사이트가 아무런 세션토큰이나, 여러 방법으로 Authorization(인가) 체크를 안한다면, 접근이 허용되지 않는 사람 혹은 누구나 관리자 권한의 기능(Functionalities) 나 Endpoint를 알고 있을경우, 관리자 권한을 사용할 수 있게됩니다.

```
# 관리자 기능을 아무런 인증없이 사용할 수 있나
localhost/admin
```

```
# HTTP Request 쿠키나 세션값 없이 관리자 기능을 사용할수 있나
GET /create-user HTTP 1.1
[...]
```

아무리 Admin Functionalities 나 Endpoint의 URI가 어렵다고 하더라도, Web Directory BruteForcing이나, 개발자 주석, 웹 Front-end 소스코드에서 얼마든지 노출이 될 수 있습니다. 


### Parameter-based Access Control Methods

URI의 파라미터로 접근제어를 할 경우, 앞서 말했듯이 세션토큰으로 Authenticate, Authorization을 안할 경우, 굉장히 취약할 수 있습니다.

클라이언트 사이드에서 수정할 수 있는 웹 리소스 항목은 다음과 같습니다.

1. 숨겨진 필드 (Hidden field)
2. 쿠키 (Cookie)
3. 문자열 파라미터 (Preset Query String Paraemter)

예를 들어 다음과 같습니다.

```
localhost/dashboard?admin=true
localhost/page/1?role=1
```

또한 이러한 취약점은 Authentication을 할때도 문제가 될수 있습니다. 이러한 경우 redirection에서 인증이 제대로 안이뤄지는 경우 일수도 있습니다.

이러한 경우, 다음과 같은 추측을 해볼 수 있는데,

1. 클라이언트 측에서만 권한을 확인: 일반 사용자인지 관리자인지 확인 
2. 서버측의 잘못 된 설정: `admin=true`라는 파라미터만 인식하고 권한을 부여하도록 설계

```
localhost/login/home?admin=true
localhost/login/home?role=1
```

{: highlight}
만약 JSON으로 로그인 방법을 할경우, 서버의 응답을 확인해보고 Reflect되는 파라미터를 추가해 볼수도 있을것 입니다. 

### Broken Access Control resulting from platform misconfiguration

접근제어 설정을 웹 플랫폼 레이어해서 했을때 문제가 되는 경우도 있습니다. 오로지 프론트엔드 단에서 처리를 할 경우, non standard HTTP Headers로 다음과 같은 상황에 Bypass가 될수도 있습니다. (`X-Original-URL`, `X-Rewrite-URL`)

```
POST / HTTP/1/1
X-Original-URL: /admin/userinfo
[...]
```

### IDOR (Insecure Direct Object References) 

IDOR 취약점은 Horizontal Broken Access Control이 될수도 있고, Vertical Broken Access Control이 될수도 있습니다.

이러한 취약점은 사용자가 컨트롤이 가능한 파라미터를 수정하여 접근이 허용되지 않는 정보나 기능을 사용하는 것을 이야기 합니다.

```
localhost/myaccount?id=123
localhost/myaccount?id=124
```


## References
 1. https://portswigger.net/web-security/access-control