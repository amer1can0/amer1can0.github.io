---
title: Android Application Structure
layout: default
parent: Android Penetration Test
nav_order: 7.3
description: "Mobile hacking"
---

# Android Application Structure

{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}


## Android Application Structure 

안드로이드 어플리케이션 구조에는 다음과 같이 구성이 되어있습니다.

1. API
2. Sandbox
3. Packaging
4. Manifest file
5. App Components
6. IPC

## API

안드로이드에서 App이 실행될때 여러 API들을 호출하는것을 볼 수 있습니다. API를 Native Development kit에서 사용할수도 있고 (C, C++을 이용하여 코드를 작성해야됩니다).

이렇게 API는 하드웨어나 OS 컴포넌트들을 NDK의 C, C++을 사용하여 접근할수 있게 해주고, SDK를 이용하여 Java API와도 통신할수 있게 합니다. 

이러한 이유는 해당 API를 사용을 안하게 되면 처음부터 다 만들어야되기 때문에 이런 불편함을 줄일수 있게 됩니다.

## Sandbox

안드로이드에서는 샌드박싱을 필수로 진행을 합니다. 안드로이드 샌드박싱은 각각의 APP마다 VM으로 돌아가며 앱 마다 따로 프포레스, 디렉토리, 유저가 있습니다. 

앱들 사이에서는 따로 `sharaedUserId`라는 것을 이용하여 공유가 될수도 있습니다. 

## Packaging 

안드로이드의 패키징은 여러 패키징이 존재하겠지만 `APK` 와 `AAB`가 있습니다. 

`AAB`는 2021년부터 새로운 패키징으로 진행이 됩니다. 

해당 파일은 마치 `zip` 파일 형식과 비슷하며, 압축을 풀었을때, 여러 폴더와 파일들을 볼 수 있습니다.

패키징 압축을 해제하면 다양한 폴더들을 볼 수 있는데, `Manifest`파일이나, `lib` 폴더 (라이브러리 폴더) 와 같은 다양한 폴더들을 볼 수 있습니다.

### Manifest files

안드로이드의 Manifest 파일은 다음과 같이 구성이 되어있습니다.

1. App components - Main Activity의 이름 등 다양한 컴포넌트 정의
2. Permissions - 해당 어플리케이션이 어떠한 퍼미션이 필요한지 (예를들어 External API가 필요하다 하면 인터넷 Permission이 있어야 될것 입니다.)
3. Flags

### App components

안드로이드의 App components는 다음과 같이 구성이 되어있습니다.

1. Activities - Main Activity로 시작화면을 정할수 있습니다. 다른 엑티비티에서 여러가지의 메소드를 정의를 하고 어떻게 실행할것인지 정의 할수 있습니다.
2. Services - 백그라운드에서 돌아가는 프로세스지만 앱 구성중 하나인 것을ㄴ 말합니다.
3. Broadcast receivers - 다른 앱들에서부터 정보를 가져오는 역할을 담당합니다.
4. Content providers - SQLite 데이터 베이스랑 통신을 하거나 비슷한 역할을 합니다. 