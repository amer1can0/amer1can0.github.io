---
title: CSRF (Cross-site Request Forgery)
layout: default
parent: Web Penetration Test
nav_order: 2.4
description: "Web hacking"
---

# CSRF (Cross-site Request Forgery)

{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

CSRF 공격은 공격자가 해당 유저에게 의도하지 않는 행동 (action)을 취하게 만듭니다. 공격자는 의도하지 않은 행동을 취하게 하려고 특정 웹사이트에 접속하도록 유도를 합니다. 해당 피해자가 그 웹사이트에 접속을 하면, 그 웹사이트에서 해당 유저의 권한으로 의도하지 않는 행동을 하게 만듭니다.


## CSRF Requirement (CSRF 조건)

CSRF 공격을 성공적으로 실행시키려면 다음과 같은 조건이 필요합니다.

1. **A relevant action**: 공격자가 해당 공격을 실행시킬려는 의도와 유도가 필요합니다. 
2. **Cookie-based session handling**: 해당 웹 어플리케이션이 쿠키기반의 세션 핸들링을 하여아합니다. (쿠키 탈취후 해당유저의 권한으로 인증이 가능하기 때문입니다.)
3. **No unpredictable request parameters**: 예측할수없는 패러미터가 요청에 들어가지 않아야합니다. HTTP 요청에 예측되지않는 패러미터값이 들어갈 경우, 공격자는 이것을 예측하기 힘들게 되어 공격을 할수 없게됩니다.

## CSRF Methods (CSRF 방법)

### NO Same Origin Policy (SOP가 존재 안할경우)

SOP (Same Origin Policy)가 존재 안할경우, 공격자 도메인에서 해당 도메인의 HTTP Path를 안다면 아래와 같은 스크립트를 추가 할수 있습니다. (`/change-email`의 Path를 알경우)

```
<form method="POST" action="http://localhost/myaccount/change-email"> <input type="hidden" name="email" value="attacker@email"> </form> <script> document.forms[0].submit();</script>
```
### Existence of Same Origin Policy (SOP가 존재할 경우)

SOP가 존재할경우, HTML태그나, 다양한 XSS, 다양한 공격과 함께 공격 시도를 해볼 수 있습니다. (아래의 예시는 `HTTP Interchange` 취약점이 가능할때 예시입니다.)

```
<img src="http://localhost/myaccount/change-email?email=attacker@attacker">
```

## Mitigation
1. CSRF Token을 적용시킵니다.
2. SameSite Cookies를 적용시킵니다.
3. Referer-based validation 레퍼럴 헤더로 필터링을 시킵니다.


## References
1. https://medium.com/infosecmatrix/mastering-csrf-a-comprehensive-guide-to-finding-cross-site-request-forgery-vulnerabilities-2024-8d1d13d83547