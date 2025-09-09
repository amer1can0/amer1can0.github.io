---
title: Windows Version and Configuration
nav_order: 2
parent: Windows Privilege Escalation
grand_parent: Internal Penetration Test
---


# Windows Version and Configuration

내부망 모의해킹을 진행할 경우, 해당 호스트의 정보들을 모으고 분석해야될때가 있습니다.

윈도우 기반 호스트의 정보를 얻고자 할때, 다음과 같은 명령어를 고려해볼 수 있습니다.


1. 해당 OS의 이름과 OS의 버전을 알고 싶을때. 
```
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
```

2. 해당 OS의 패치나 업데이트를 알고 싶을때.
```
wmic qfe
```

3. 해당 컴퓨터의 아키텍쳐를 알고 싶을때.
```
wmic os get osarchitecture || echo %PROCESSOR_ARCHITECTURE%
```

4. `env` 환경 변수들을 알고 싶을때
```
set
Get-ChildItem Env: | ft Key,Value
```

5. `Driver`들을 나열 하고 싶을때
```
wmic logicaldisk get caption || fsutil fsinfo drives
wmic logicaldisk get caption,description,providername
Get-PSDrive | where {$_.Provider -like "Microsoft.PowerShell.Core\FileSystem"}|ft Name,Root
```
