---
title: Android Application Structure IPC
layout: default
parent: Android Penetration Test
nav_order: 7.4
description: "Mobile hacking"
---

# Android Application Structure IPC

{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## IPC (Inter-Process Communication)

IPC란 안전하게 데이터를 샌드박스나 앱들 사이에서 옴기는 것을 의미합니다. IPC는 `Proxy Binder Driver`를 사용하며, 해당 `Binder Driver`를 통하여 다른 프로세스와 통신을 합니다.

이러한 IPC 통신을 위해서 `intent`가 사용됩니다. 