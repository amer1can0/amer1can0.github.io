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

### How to find UART (UART를 찾는법)
1. 구글링으로 알아낼수 있습니다.
2. PCB보드에 써져있을수 있습니다 (`RX`,`TX`,`GND`,`VCC`)
3. 위의 방법으로 찾을수 없을경우, 맞는 후보군을 찾아야됩니다.
    - `GND`, `TX`, `RX`, `VCC`의 맞는 위치의 후보군을 찾아야됩니다. 이러한 후보군을 찾는데 Multimeter를 많이 사용합니다. 
    - `Logic Analyzer`로 핀 위치를 알 수 있습니다.
    - `Jtagulator`를 사용하여 위치를 알 수 있습니다.

{: .highlight }
`GND`, `TX`, `RX` 같은 핀들을 찾을때, 주로 4핀이나 3핀이 있는 핀을 찾으면 됩니다.



## References
1. https://www.lenovo.com/us/en/glossary/fcc-id/?orgRef=https%253A%252F%252Fwww.google.com%252F
