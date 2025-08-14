---
title: Information Gathering
layout: default
parent: IoT Penetration Test
nav_order: 6.1
description: "IoT Pentest"
---

# Information Gathering

{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}


IoT 모의해킹도 다른 모의해킹과 마찬가지로 정보수집은 필수적인 단계입니다.

## Who makes the device and ODM? (기기 제조사 찾기)
 
IoT 기기를 모의해킹을 진행을 할경우, 다음과 같은 내용들을 정리 및 검색해보면 좋습니다.

1. Identify Model Name (모델 이름 찾기)
    - 구글을 이용해서 모델 이름을 검색해서 해당 메뉴얼 PDF가 있는지, 해당 자료를 제공하는 사이트가 존재하는지 검색을 해볼 수 있습니다.
    - User Manual 혹은 System Manual 을 구글에 검색해 볼수 있습니다. (보통은 User Manual보다는 System Manual이 더 자세한 내용을 가지고 있을 확률이 큽니다.)
        - 무슨 하드웨어를 쓰는지 나열을 해볼 수 있습니다.
        - FCC ID를 찾아 메뉴얼을 찾아볼 수 있습니다.

{: .highlight }
**FCC ID란 무엇일까요?**
FCC is Federal Communications Commission Identification (FCC ID). FCC ID is a unique code assigned to electronic devices by the FCC in the United States. This code is used to identify and certify that the device meets the necessary regulatory standards for wireless communication.
FCC란  Federal Communications Comissions Identification (FCC ID)의 약자로 미국에서 유니크한 코드를 해당 기기마다 고유번호를 매깁니다. 이러한 코드는 해당 기기가 무선 통신 규격에 맞게 개발되었는지 고유번호로 해당 기기들을 식별 및 인증합니다.

2. SIN Number 로 검색해보기가 있습니다.
    - `S/N` 넘버는 대부분 기기아래 스티커 설명란에서 많이 찾을수 있습니다.
    - `S/N` 넘버를 읽는 방법은 대부분 다음과 같습니다.
        - `S/N: GMK170210005623` 다음과 같은 `S/N`넘버가 주어졌을때, 
            - `17`: 년도를 의미합니다.
            - `02`: 월을 의미합니다.
            - `10`: 일을 의미합니다.
            - `005623`: 시리얼 넘버를 의미합니다.

## Open the Device (기기 열어보기)
- 정보수집이 끝나면 해당 기기의 구조를 보기위해 기기를 분해합니다.
- 안보이는 부분이 있다면 분필을 써서 세겨져있는 하드웨어 정보를 얻을수도 있습니다.

## Locate the UART (UART 포트 찾기)

UART 통신은 Universal Asynchronous Receiver Transmitter의 약자이며, PC와 IoT기기 간의 송수신 역할을 담당합니다. 해당 UART통신을 이용해서 사용자의 컴퓨터와 IoT기기를 연결할 경우, 터미널로 접속이 가능합니다. 

### What is UART (UART란 무엇인가)

대부분의 임베디드나 IoT 기기들은 `Serial Console`을 다양한 목적으로 가지고 있습니다. 해당 콘솔은 제조과정에서 쓰이거나, 해당 기기를 디버깅 할때, 등 다양한 목적과 용도로 쓰이고 있습니다.

우리가 UART로 통신을 할수 있다면 `Root Shell`을 접근할수도 있습니다. 
- 해당 쉘을 접근할 경우, 손쉽게 해당 기기를 탐색해볼수 있습니다.
- 펌웨어나, 파일 시스템 내용들을 받아볼 수 있습니다.
- 암호화된 메모리 접근 권한이나 부트로더를 공략할 수 있습니다.

하지만 대부분의 스마트 기기 제조사들은 이러한 무분별한 접근이나 공격을 막기 위해서 `Secret Key`나 `Password`로 접근을 제한하는 경우가 대부분 입니다. 혹은 `Read Only Console` 같은 읽기만 가능한 콘솔을 만들어서 애초에 접근을 못하게 하는 부분도 있습니다.

이러한 제한적인 부분에도 모의해킹을 진행할때, `Root Shell`의 접근권한을 얻으면 좋은것은 다양한 정보를 `Read Only Console`이나 비밀번호 접근제한이 걸려있는 상황에서도 얻을수 있기 때문입니다.

- Boot Log에서 얻을 수 있는 소프트웨어 및 하드웨어 정보들
- Crash Log에서 얻을 수 있는 소프트웨어 및 하드웨어 정보들 

### How to find UART (UART를 찾는법)
1. 구글링으로 알아낼수 있습니다.
2. PCB보드에 써져있을수 있습니다 (`RX`,`TX`,`GND`,`VCC`)
3. 위의 방법으로 찾을수 없을경우, 맞는 후보군을 찾아야됩니다.
    - `GND`, `TX`, `RX`, `VCC`의 맞는 위치의 후보군을 찾아야됩니다. 이러한 후보군을 찾는데 Multimeter를 많이 사용합니다. 
    - `Logic Analyzer`로 핀 위치를 알 수 있습니다.
    - `Jtagulator`를 사용하여 위치를 알 수 있습니다.

{: .highlight }
`GND`, `TX`, `RX` 같은 핀들을 찾을때, 주로 4핀이나 3핀이 있는 핀을 찾으면 됩니다.
`VCC`같은 경우 보통 무시해도 상관은 없습니다. 

| Device1   | Connection    | PC        |
|:----------|:--------------|:----------|
|`TX`       | `->`          |   `RX`    |
|`RX`       | `<-`          |   `TX`    |
|`GND`      | `<->`         |   `GND`   |

PCB에도 여러가지의 연결 방법이 있습니다.

1. PCB보드에 구멍이 있는경우
    - 납땜을 하거나 (Soldering) 혹은 Articulated Arms를 사용해서 연결을 할 수 있습니다.
2. PCB보드에 골드핑거 가 있는경우
    - Edge Connector가 필요합니다.

### Using Multimeter to find UART PIN (Multimeter로 UART PIN 찾는 방법)

1. `GND`핀을 찾기 위해서는 멀티미터기를 `Continuity Mode`로 만든 다음 2개의 포인터를 하나는 철판쪽, 하나는 PIN 자리들을 번갈아가며 "삡" 울리는 소리를 들으면 됩니다.
2. `VCC`핀을 찾기 위해서는 기기의 전원을 키고 PIN자리들을 번갈아가면서 `3.3` voltage가 뜨는곳을 찾으면 됩니다.
3. `RX` 핀을 찾기 위해서는 기기의 전원을 켜고 `0` voltage가 뜨는곳을 찾으면 됩니다.
4. `TX` 핀을 찾기 위해서는 여러 변화하는 숫자들을 나타내는 핀을 찾으면 됩니다.


### Possible ways to gain access to Root Shell (UART통신으로 Root Shell 접근방법)

- Brute Forcing 공격으로 해당 쉘의 비밀번호를 무차별 대입해볼 수 있습니다. (다만 무차별 대입 공격으로 인해 접근제한이 걸리지 않게 조심해야 될것입니다.)
- Firmware를 덤프해서 시리얼 콘솔의 제한적 쉘을 얻을수 있습니다. (다만 펌웨어를 덤핑할때는 복사본이 있거나 기존 데이터가 손상되지 않도록 조심해야 될것입니다.)
- 해당 기기와 UART 통신을 할 경우 `mimicom` 혹은 `screen`으로 연결을 하면 됩니다.


## References
1. https://www.lenovo.com/us/en/glossary/fcc-id/?orgRef=https%253A%252F%252Fwww.google.com%252F
