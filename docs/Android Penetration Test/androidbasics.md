---
title: Android Basics 
layout: default
parent: Android Penetration Test
nav_order: 7.1
description: "Mobile hacking"
---

# Android Basics 

{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Android Architecture 

안드로이드는 리눅스 기반의 오픈소스 소프트웨어 입니다. 안드로이드의 메인 Component는 다음과 같이 구성되어 있습니다.

1. Linux Kernel - 리눅스 커널로부터 시스템이 시작됩니다.
2. Hardware Abstraction Layer - 드라이버로 하드웨어를 연결합니다.
3. Libraries - C, C++ 로 만들어진 Native Library가 존재합니다. 
4. Android Runtime - 특정된 VM기반의 런타임 머신이 돌아갑니다. (자바)
5. Android Framework - 자바기반의 안드로이드 프레임워크 API들이 존재합니다.
6. Applications - Built-in 어플리케이션들이 존재합니다 (카메라나, 알람등 다양한 기본 어플리케이션을 사용합니다.)

### Linux Kernel

리눅스 커널은 다음과 같은Basic system services를 담당합니다:

1. Memory Management
2. Process Management
3. Network stack
4. Device drivers

`Exploit Database` 에서 여러 리눅스 커널 취약점들을 찾아볼수 있습니다.

안드로이드 `adbshell` 에서 `uname -a`를 할경우, 커널버전을 보여줍니다.

### Hardware Abstraction Layer 

하드웨어 앱스트랙션 레이어는 Native C/C++ Libraries 와 하드웨어의 컨넥션 포인트라고 보시면 됩니다. 해당 레이어 에서는 다양한 하드웨어 컴포넌트 드라이버들이 존재하는데, 오디오, 블루투스, 와이파이, 카메라, 센서등 여러 드라이버가 존재합니다.

쉽게 표현하자면 Software Hooks (하드웨어와 안드로이드 플렛폼을 연결시켜주는 소프트웨어) 라고 보면 편합니다.

하드웨어 레이어는 각각 Containment 가 되어있기때문에, 해당 레이어들끼리의 통신은 불가능 합니다.

해당 하드웨어 레이어도 여러 취약점이 존재합니다. (HAL Driver Exploit)