---
title: Windows User Enumeration
nav_order: 3
parent: Windows Privilege Escalation
grand_parent: Internal Penetration Test
---

# Windows User Enumeration 

내부망 호스트 모의해킹을 진행 할때, 해당 호스트의 유저 정보와 권한 정보를 알아내는것은 굉장히 중요합니다. 
다음과 같은 명령어로 해당 테스트 호스트의 유저의 정보나 권한들을 알아볼 수 있습니다.

1. 현재 유저의 유저 이름 검색

```
echo %USERNAME% || whoami
$env:username
whoami
```

2. 현재 유저의 권한 검색
```
whoami /priv
whoami /group
```

3. 해당 호스트에 존재하는 유저들 검색
```
net user
whoami /all
Get-LocalUser | ft Name,Enabled,LastLogon
Get-ChildItem C:\Users -Force | select Name
```

4. 해당 유저의 정보 검색
```
net user <username>
```

5. Local 환경의 그룹을 리스트 하고 싶을 경우
```
net localgroup
Get-LocalGroup | ft Name
```

6. Local 그룹의 디테일 정보 검색
```
net localgroup <localgroup>
Get-LocalGroupMember <localgroup> | ft Name, PrincipalSource
```