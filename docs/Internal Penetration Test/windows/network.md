---
title: Windows Network Enumeration
nav_order: 4
parent: Windows Privilege Escalation
grand_parent: Internal Penetration Test
---

# Windows Network Enumeration

윈도우 환경에서 네트워크의 검색은 필수 입니다. 침투 테스팅을 진행을 할때, 무슨 네트워크에 있는지 알아야 다음 동작을 수행할 때에도 중요하게 작용을 합니다. 

- IP 와 네트워크 인터페이스, DNS 나열

```js
ipconfig /all
Get-NetIPConfiguration | ft InterfaceAlias, InterfaceDescription, IPv4Address
Get-DnsClientServerAddress -AddressFamily IPv4 | ft
```

- 라우팅 테이블 나열 

```
route print
Get-NetRoute -AddressFamily IPv4 | ft DestinationPrefix,NextHop, RouteMetric, ifIndex
```

- ARP 테이블 나열 

```
arp -A
Get-NetNeighbor -AddressFamily IPv4 | ft ifIndex, IPAddress, LinkLayerAddress, State
```

- 현재 연결되어있는 컨넥션 활용

```
netstat -ano
```

- 네트워크 쉐어 나열

```
net share
powershell Find-DomainShare -ComputerDomain domain.local
```
