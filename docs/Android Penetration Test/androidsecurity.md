---
title: Android Security
layout: default
parent: Android Penetration Test
nav_order: 7.2
description: "Mobile hacking"
---

# Android Security 

{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Android Security (안드로이드 보안)

안드로이드 보안은 다음과 같이 분류될 수 있습니다. 

1. System Security (시스템 보안)
2. Network Security (네트워크 보안)
3. Software Isolation (소프트웨어 분리)
4. Anti-Exploitation (안티 익스플로잇테이션)

## System Security (시스템 보안)

안드로이드의 시스템 보안으로는 디바이스 암호화가 있습니다.

- File based for Android 7+ (파일 베이스 암호화): 파일 기반의 암호화는 파일 단위로 암호화를 시킬수 있으며, 다이렉트 부트 (Direct Boot: 암호화된 기기를 바로 락스크린으로 부팅 시킬수 있습니다)를 지원합니다. 
- Full disk for Android 5+ (디스크 전체 암호화): 안드로이드 10+ 이상에서는 지원하지 않습니다. 5부터 9까지만 지원이 되며, 오로지 하나의 키로 유저데이터 전체를 암호화 시키는것을 말합니다. 

### Trusted Execution Environment (TEE) 

TEE란 보안이 적용된 구역에서 디바이스 메인 프로세서가 안전하게 실행될수있게 민감한 정보나 데이터를 보호하는 역할을 합니다. 

Secure Boot를 지원하며 Interception of the Network traffic을 보호하는 역할을 맡습니다. 

- Hardware: 암호화키나 생체정보등을 보호합니다.
- Software: Trusted Applications과 API를 보호합니다.

Verified Boot를 지원하며 해당 기능은 모든 소스가 안전하고 믿을수있는 소스로부터 올수있게 하는 역할을 합니다. 

{: .highlight }
만약 Jail breaking을 할경우, Verified boot를 disable 해야됩니다.

## Network Security

안드로이드에서는 TLS와 DNS over TLS가 안드로이드 버전 9 이상부터 기본으로 적용이 됩니다.

또한 각각의 앱에 따라 분리된 Network Security Configuration을 가질수 있습니다. 

Certificate Pinning의 기능도 제공이 됩니다. Certificate Pinning이란 오로지 신뢰된 Certificate만 사용될수 있도록 하는 기능을 이야기 합니다. 

## Software isolation

안드로이드는 앞서 말했듯 리눅스를 사용하는데, 해당 리눅스는 SELinux (Security Enhanced Linux) 라는 것을 포함하고 있습니다. 해당 시스템은 Access Control 을 관리하고 샌드박스에서 격리되게 어플리케이션이 돌아가도록 합니다.

또한 Manifest파일에서 각각의 어플리케이션마다 Permissions들을 설정해 줍니다. 

## Anti-Exploitation

안드로이드는 여러 커널 공격과 버퍼 오버플로우에 대한 여러 보안 Protection들이 존재합니다.

1. ASLR (Address Space Layout Randomization) 안드로이드 4.1 + 
2. KASLR (Kernel Address Space Layout Randomization) 안드로이드 8 +
3. DEP (Data Execution Prevention)
4. SECCOMP filter, syscalls을 보호하기 위한 장치


## References
1. https://www.mobilehackinglab.com