---
title: API Penetration Testing
layout: default
parent: Web Penetration Test
nav_order: 2.6
description: "Web hacking"
---

# API Penetration Testing

{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## API Testing (API 테스팅)

API란 Application Programming Interface를 의미합니다. 해당 기능은 소프트웨어 시스템과 어플리케이션이 데이터를 공유하기위해 만들어 졌습니다. 

모든 동적 웹사이트들은 API들로 이루어져있습니다. 

## OWASP Top 10 API Security Risks - 2023 (2023년도 API 보안위협 탑 10)

1. **Broken Object Level Authorization (객체 수준 권한 오류)**
    - API가 객체 식별자(ID) 기반으로 데이터를 처리할때, 해당 객체에 접근할 권한이 있는지 제대로 검사하지 않으면 다른 사용자의 데이터에 접근하거나 조작할수 있는 취약점입니다. (IDOR)
2. **Broken Authentication (인증 오류)**
    - 인증 메커니즘이 잘못 되었을 경우, 공격자가 인증 토큰을 탈취하거나 인증을 우회해 다른 사용자로 가장할 수 있습니다.
3. **Broken Object Property Level Authorization (객체 속성 수준 권한 오류)**
    - 과도한 데이터 노출 (Excessive Data Exposure)이나 Massive Assignment가 관련된 취약점 입니다. 객체 전체가 아닌 특정 속성 하나에 대한 접근 권한까지 제대로 검증을 안해서 민감한 정보가 노출되거나 변경될수 있습니다.
4. **Unrestricted Resource Consumption (자원 무제한 소비)**
    - 요청량이나 데이터 크키에 제한이 없으면, DoS를 만들수 있습니다. 공격자가 API를 통해 시스템 자원이나 외부 리소스를 무제한 호출할 수 있는 경우를 이야기 합니다.
5. **Broken Function Level Authorization (기능 수준 권한 오류)**
    - 사용자와 관리자 등 기능 간 권한 경계가 명확하지 않거나 접근 제어가 제대로 적용되지 않는 경우, 공격자가 관리자 기능에 접근하거나 다른 사용자의 리소스를 조작하는 것을 말합니다.
6. **Unrestricted Access to Sensitive Business Flows (민감한 비지니스 정보에 대한 접근)**
    - 댓글 게시 같은 민감한 비지니스 프로세스가 자동화된 방식으로 남용될때 충분히 보호하지 않으면 비지니스에 큰 피해가 발생할수 있습니다. (가짜 계정 생성)
7. **Server Side Request Forgery (SSRF)**
    - 사용자가 제공한 URI를 검증없이 서버가 원격 리소스를 요청할 경우, 공격자가 방화벽이나 VPN 뒤 내부 시스템에 접근이 가능합니다.
8. **Security Misconfiguration (보안 설정 오류)**
    - API 또는 관련 시스템 환경이 잘못 설정되어 있을 경우 (예: 허용되지 않는 메소드, 잘못된 CORS 설정 등), 예상치 못한 보안 취약점이 발생할 수 있습니다. 
9. **Improper Inventory Management (부적절한 엔드포인트 관리)**
    - API 엔드포인트 수가 많아지면 문서화*관리되지 않는 엔드포인트 (구 버전, 디버그용 등)가 노출되어 잠재적 공격 위험이 커집니다.
10. **Unsafe Consumption of APIs (API의 불안전한 소비)**
    - 서드파티 API에서 받은 데이터를 일반 사용자 입력보다 더 신뢰해서 보안 검증을 소흘이 할 경우, 보안 허점이 발생할 수 있습니다. 

## Discovering API documentation (API 문서 찾기)

요즘 많은 웹사이트들이 API 디버깅이나 테스팅을 위해 API 문서화를 해놓는 경우가 많습니다. 

해당 문서화된 웹사이트를 발견할수 있는지 먼저 찾아볼수 있을것 입니다.

- `/api`
- `/swagger/index.html`
- `/openapi.json`

## Identifying API endpoints

API 문서화된 페이지를 찾으면 좋겠지만 때로는 그럴수 없는 경우도 많습니다. 그럴땐 여러가지 방법을 사용할 수 있습니다.

웹사이트를 돌아다니면서 API의 HTTP 요청을 찾았을 경우, 해당 URI를 보고 Brute Forcing을 해볼수 있을것입니다.

- Response Status Code로 해당 엔드포인트가 존재하는 엔드포인트인지 구별할 수 있습니다.
- 다른 방법, 타이밍이나, 리다이렉트나 여러가지 방법으로도 구별할 수 있습니다.

## Mass Assignment Vulnerability

API는 보통 RESTful 방식을 많이 사용합니다. RESTful API란 (Representational State Tranfer) 원칙에 맞게 구현한것 입니다. 주로 HTTP 기반에서 자원을 URI로 표현하고, HTTP Method (GET, POST, PUT, DELETE 등)을 활용합니다.

{: .important}
> 왜 RESTful을 많이 쓸까요?
> 단순성과 표준성: HTTP Protocol위에서 동작하고, URI로 자원을 표현합니다.
> 확장성과 효율성: 클라이언트-서버가 느슨하게 결합되어 서비스 확장이 용이합니다. (다양한 환경에서도 사용 가능합니다.)
> 무상태성 (Stateless): 각 요청이 독립적 입니다. (서버 확장 및 로드 밸런싱에 유리합니다)
> 도구/라이브러리 생태계 풍부: 대부분 언어와 프레임워크가 RESTful API호출을 기본 지원합니다.

### Identify hidden paramters

Mass Assignment을 이용하여 숨겨진 패러미터를 발견할수 있습니다. 예를 들어서 

`PATCH /api/users/`로 아래 유저의 정보를 수정한다 해봅시다.

```json
{
    "username": "hi",
    "email": "admin@admin"
}
```

해당 유저 정보를 수정하고난 다음 또 다른 API 정보를 다음과 같이 수집하였다고 생각해 봅시다. `GET /api/users/123`

```json
{
    "id": 123,
    "name": "HELLO",
    "email": "hello@hello",
    "isAdmin": "false"
}
```
해당 정보를 이용하여 숨겨진 패러미터들을 이용하고 공격해 볼수 있습니다.

```json
{
    "username": "hi",
    "email": "admin@admin",
    "isAdmin": "true"
}
```

## Server-Side Parameter Pollution

### Overriding existing parameters

예를들어 다음과 같은 유저를 불러오는 Request가 있다고 생각해봅시다.

```
GET /userSearch?name=peter&back=/home
```

만약 잘못 설정되어있는 웹서버면 다음과 같이 같은 파라미터를 오버라이딩 할 수 있을것 입니다.

```
GET /useSearch?name=peter&26name=carlos&back=/home
```

해당 취약점이 존재할 경우, 다양한 웹 엔진에서는 다르게 처리를 합니다.
- PHP 경우, 마지막 파라미터만 파싱 합니다. `carlos`
- ASP.NET 경우, 둘다 파싱합니다. `peter`, `carlos`
- Node.js 경우, 첫번쨰 파라미터만 파싱합니다. `peter`

### RESTful API server-side parameter pollution 

RESTful API같은 경우, 전부 URL 엔드포인트로 들어가게 됩니다.

```
GET /api/private/users/peter
```

다음과 같이 parameter pollution을 시도해 볼수 있을것 입니다.

```
GET /api/private/users/peter/../admin
```

그러면 다음과 같은 요청이 될것입니다 `/api/private/users/admin`