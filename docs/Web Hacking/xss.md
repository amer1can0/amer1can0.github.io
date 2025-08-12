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
- 공격자가 악성스크립트가 포함된 URL을 Victim에게 보내는 형식으로 이루어집니다.

{: .important-title }
> Proof of Concept
> ```js 
> https://donotexistwebsitedontclickit.com?url=<script>alert("XSS")</script> 
>```

- Victim이 해당 URL을 클릭하면, 서버에서 해당 악성 스크립트를 Victim의 브라우저로 **"반사"** 해서 보냅니다.
- 브라우저는 해당 스크립트를 신뢰하는 웹사이트에서 온것으로 간주하고 실행합니다.