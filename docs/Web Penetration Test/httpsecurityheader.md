---
title: HTTP Security Headers
layout: default
parent: Web Penetration Test
nav_order: 2.2
description: "Web hacking"
---

# HTTP Security Header

HTTP 보안 헤더는 Web Application의 보안을 강화하는데 매우 중요하게 쓰입니다. 

1. **HSTS (HTTP Strict Transport Security)**
- 브라우저가 HTTP 대신 HTTPS를 통해서만 웹사이트에 접속하도록 강제하는 헤더입니다. 
- `max-age`: 브라우저가 HTTPS를 사용하도록 기억하는 시간을 나타냅니다. `31536000`초 1년을 권장합니다.
- `includeSubDomains` 모든 하위 도메인에도 HSTS를 적용해야됩니다.

2. **CSP (Content Security Policy)**
- CSP는 XSS 공격을 비롯한 다양한 Injection 공격을 막기 위한 보안 정책입니다. 

3. **X-Content-Type-Options**
- 브라우저가 MIME 스니핑을 통해 컨턴츠 타입을 추측하는것을 막습니다. 
- 파일업로드 취약점으로 연계될수 있습니다.
    - 보통 서버는 `JavaScript` 파일의 MIME 타입을 `Application/Javascript`로 설정하여 응답합니다. MIME 스니핑은 브라우저가 서버에서 제공하는 MIME 타입을 신뢰하지 않고, 리소스의 내용을 직접 분석하여 MIME 타입을 추측합니다. 예를들어서 `JavaScript` 파일을 `text/html`로 만들 수 있습니다. 

4. **X-Frame-Options**
- 클릭재팅 (Clickjacking) 공격을 방지하기 위해 웹페이지가 `<frame>`, `<iframe>`, `<object>`안에 포함되는 것을 막습니다.  
    - `DENY`: 어떤 페이지에도 iframe으로 포함되는 것을 차단합니다
    - `SAMEORIGIN`: 같은 도메인 내의 페이지에서만 iframe으로 포함되도록 허용합니다.

5. **Referrer-Policy**
- 다른 사용자가 다른사이트로 이동할 때 Referer 정보가 얼마나 상세하게 전달될지 제어합니다. 
    - `strict-origin-when-cross-origin`: 같은 출처로 요청할 때는 전체 URL을 보내고, 다른 출처로 요청할때는 Origin 만 보냅니다. 