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

## Linux Kernel

리눅스 커널은 다음과 같은Basic system services를 담당합니다:

1. Memory Management (메모리 관리)
2. Process Management (프로세스 관리)
3. Network stack (네트워크 스택 관리)
4. Device drivers (디바이스 드라이버)

`Exploit Database` 에서 여러 리눅스 커널 취약점들을 찾아볼수 있습니다.

안드로이드 `adbshell` 에서 `uname -a`를 할경우, 커널버전을 보여줍니다.

## Hardware Abstraction Layer 

하드웨어 앱스트랙션 레이어는 Native C/C++ Libraries 와 하드웨어의 컨넥션 포인트라고 보시면 됩니다. 해당 레이어 에서는 다양한 하드웨어 컴포넌트 드라이버들이 존재하는데, 오디오, 블루투스, 와이파이, 카메라, 센서등 여러 드라이버가 존재합니다.

쉽게 표현하자면 Software Hooks (하드웨어와 안드로이드 플렛폼을 연결시켜주는 소프트웨어) 라고 보면 편합니다.

하드웨어 레이어는 각각 Containment가 되어있기때문에, 해당 레이어들끼리의 통신은 불가능 합니다.

해당 하드웨어 레이어도 여러 취약점이 존재합니다. (HAL Driver Exploit)

## Android Libraries 

안드로이드에서 사용되는 라이브러리는 Native Libaries 라고 불리웁니다. Native Libraries는 다양한 다른 토픽을 핸들하는데, 예를들어 SSL 통신, 오디오 매니저, SQLite, Webkit (웹 브라우징) 등 다양한 기능들을 담당합니다.

이러한 라이브러리는 C와 C++로 이루어져있습니다. 이러한 라이브러리에서도 여러 취약점이 발생할 수 있습니다. SQLite의 SQL Injection 취약점이라던지 여러 라이브러리 취약점들이 발생할 수 있습니다. 

## Android Runtime

안드로이드 런타임 이란, 안드로이드 앱이 실제로 실행되는 환경을 이야기 합니다. 쉽게 말해 안드로이드 앱을 구동하기 위해 OS가 사용하는 실행 엔진 입니다.

1. 앱 실행 과정
    - 안드로이드 앱은 Java 혹은 코틀린 같은 언어로 작성 됩니다.
    - 앱이 빌드가 되면 바이트 코드 (DEX, Dalvik Executable) 형태로 변환이 됩니다. 
    - 이 DEX 파일을 실제 기기에서 실행하는 역할을 하는게 바로 안드로이드 런타임 (ART) 입니다.

2. Dalvik -> ART 변화
    - 예전 (안드로이드 4.x 이하)에는 Dalvik VM이 런타임 역할을 했습니다. 다만 안드로이드 5.0 롤리팝 부터는 ART (Android Runtime)으로 바뀌었습니다. 
    - 두가지의 변화의 차이점은 다음과 같습니다.
        - Dalvik은 앱 실행 시마다 JIT (Just In Time) 컴파일을 진행합니다. 이러한것은 실행 속도는 느리지만 설치는 빠르다는 장점이 있습니다. 
        - ART는 앱 설치시 AOT(Ahead Of Time) 컴파일을 진행합니다. 실행 속도는 빠르지만 설치가 조금 느리다는 단점이 있습니다.
    - 최신 안드로이드 ART에서는 JIT + AOT 혼합 방식을 사용해서 성능과 효율을 동시에 잡고 있습니다.

3. 역할은 다음과 같습니다.
    - 앱 실행 속도 관리
    - 메모리 관리 (가비지 컬렉션)
    - 앱이 하드웨어/OS 와 잘 동작하도록 중간 계층 역할

이러한 안드로이드 런타임도 여러 취약점에 취약할수 있습니다. 

## Android Framework (Java API Framework)

안드로이드 프레임 워크 혹은 자바 API 프레임 워크로는 다양한 기능들을 사요하는데, 주로 `Content Providers`와 `View System` 기능이 존재합니다.

이러한 레이어 에서는 Activity나 유저의 인터페이스를 만든다거나, 유저의 입력값을 처리한다, 푸쉬 노티피케이션을 관리한다거나 여러 디바이스의 기능들을 접근할 수 있는 API들이 있습니다.

자바 API에 관련해서는 많은 취약점은 아직 많이는 없지만 그래도 여러 취약점이 발생할수 있는 곳중 한곳입니다.

## Applications

안드로이드 아키텍쳐중 최상단에 위치한 어플리케이션 입니다. 어플리케이션은 주로 자바나 코틀린으로 인해 개발이 되고, APK 파일로 패키지가 되며, 해당 APK파일은 어플리케이션 코드, 리소스, Manifest 파일 같은 다양한것을 가지고 있습니다.


## References
1. https://www.mobilehackinglab.com