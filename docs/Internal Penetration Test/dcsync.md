---
title: DCSync
layout: default
parent: Internal Penetration Test
nav_order: 4.1
description: "Internal Pentest"
---

# DCSync 공격

{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

MITRE ATT&CK: T1003.006
{: .label .label-green }

DCSync는 공격자가 Active Directory (AD)의 도메인 컨트롤러 (DC: Domain Controller)로 위장하여 다른 Domain Controller로 부터 사용자 계정의 비밀번호 해시를 복제 시키는 공격 기법입니다. 

쉽게 말해 Active Directory의 정상적인 복제기능 (Replication)을 악용하는 것이라고 보면 됩니다.

## The Principle of DCSync (DCSync의 원리)

AD 환경에서 여러 개의 DC가 존재하는 경우, 모든 DC의 데이터가 항상 동기화되도록 복제 프로세스가 주기적으로 실행됩니다. DCSync 공격은 해당 복제 과정을 악용하는것을 의미합니다.

1. Acquiring the privileges (권한 획득): 공격자는 먼저 다음과 같은 디렉토리 복제 권한을 가진 계정을 탈취해야 됩니다.

    - Replicating Directory Changes (`DS-Replication-Get-Changes`)
        - AD 에서 Object의 변경 사항을 복제할 수 있는 권한입니다. 이러한 권한은 주로 AD 내에서 데이터를 동기화를 위해 사용되는 권한입니다. 
    - Replicating Directory Changes All (`DS-Replication-Get-Changes-All`)
        - 위 권한 보다 더 높은 권한으로, 암호 해시와 같은 민감한 속성을 포함하여 디렉토리의 모든 변경 내용을 복제할 수 있는 권한입니다. DCSync 공격을 성공적으로 하려면 일반적으로 이 권한이 필요합니다. 

2. Impersonate DC (DC 위장): 탈취한 권한으로 공격자는 자신의 컴퓨터를 AD 도메인의 정상적인 DC인 것 처럼 위장합니다.

3. Replication Request (복제 요청): Impersonated 된 공격자 컴퓨터에서 `Directory Replication Service Remote Protocol (MS_DRSR)` 을 이용해서 다른 DC에게 특정 사용자 계정의 복제 데이터를 요청합니다.

4. Receiving Data (데이터 수신): 요청 받은 정상적은 DC는 공격자의 컴퓨터를 신뢰할 수 있는 DC로 인식하고, 요청받은 사용자 계정의 비밀번호 해시를 포함한 복제 데이터를 전송합니다.

5. Using Hash (해시 사용): 공격자는 해시를 이용하여 `PtH (Pass the Hash)` 나 `Golden Ticket` 같은 다른 공격을 시도할 수 있습니다.

## Attack Methods (공격 방법)

| Linux                                     | Windows               |
|:------------------------------------------|:----------------------|
| `impacket-secretsdump <domain>\<user>@<ip>`|`mimikatz # lsadump::dcsync /domain:<domain> /user:<domain>\user`|
| `impacket-secretsdump -just-dc <user>:"<password>"@<ip>` | `Invoke-Mimikatz -Command '"lsadump::dcsync /domain:<domain> /user:<domain>\<user>"'`|

{: .warning }
> `/user:` 란에 `domain.com`이 아닌 `domain`을 써야될때가 있습니다.


## Detection of the Attack (공격 탐지)

DCSync 공격을 탐지하기 위해서는 네트워크 모니터링, Windows Event를 체크하는것이 필요합니다.

`EventID 4462` 이 Windows 이벤트 로그에서 나오면 `{1131f6ad-9c07-11d1-f79f-00c04fc2dcd2}` 의 GUID를 필터링 하여서 어떠한 유저가 `Replication Directory Changes All`을 사용했는지 찾으면 됩니다.

## References

1. https://medium.com/@urshilaravindran/ad-series-dc-sync-attacks-e76bb54308f5
2. https://velog.io/@penclicker/DCSync%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90