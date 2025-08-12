---
title: XSS (Cross-Site Scripting)
layout: default
parent: Web Hacking
nav_order: 2.3
description: "Web hacking"
---

# Cross Site Scripting (XSS)

XSS는 웹사이트에 악성 스크립트 `<script>` 태그나 HTML의 `on` 태그 속성을 이용해서 클라이언트 측 브라우저에서 실행되도록 하는 공격입니다.

XSS의 공격은 크게 3가지 유형으로 나뉩니다.

## Reflected XSS (반사형) 
- 악성스크립트가 포함된 URL을 Victim에게 보내는 형식으로 이루어집니다.

{: .important-title }
> Proof of Concept
> {: .label .label-blue }
> ``` 
> https://donotexistwebsitedontclickit.com?url=<script>alert("XSS")</script> 
>```

- Victim이 해당 URL을 클릭하면, 서버에서 해당 악성 스크립트를 Victim의 브라우저로 **"반사 (Reflect)"** 해서 보냅니다.
- 브라우저는 해당 스크립트를 신뢰하는 웹사이트에서 온것으로 간주하고 실행합니다.

## Stored XSS (저장형)
- 악성 스크립트를 게시판, 댓글, 포럼 등 웹 서버의 데이터베이스에 저장하는것을 의미합니다.
- 다른 사용자가 해당 페이지에 접속하면, 서버에 저장된 스크립트가 해당 사용자 브라우저에서 실행이 됩니다.
- Reflected XSS와 다르게 특정 URL을 클릭할 필요 없이 불특정 다수에게 피해를 줄 수 있습니다.

## DOM-based XSS
- 서버를 거치지 않고 사용자 브라우저의 Document Object Model (DOM) 환경에서 공격이 이루어 집니다.
- 주로 자바스크립트를 이용해 HTML 페이지를 조작하는 방식으로 작동합니다

{: .important-title }
> Proof of Concept
> {: .label .label-blue }
> ``` 
> https://donotexistwebsitedontclickit.com/#javascript:alert("XSS") 
>```

- URL의 매개변수 값 등을 조작해 스크립트를 실행시킵니다.

{: .warning }
> 많은분들이 "Reflected XSS" 와 혼동을 하시는 분들이 많습니다. 두 종류의 차이점은 악성 스크립트가 주입되는 시점에서 찾을수 있습니다.
> 1. DOM-based XSS 공격은 네트워크에서 서버에게 별도 요청없이 URL 주소 해시에 심은 악성 스크립트가 실행이 됩니다.
> 2. Reflected XSS 공격은 악성 스크립트가 일단 서버에 전달이 되어야합니다. 

