---
title: "[CS] TCP 3-way handshake 란?"
date: 2025-10-09
categories: [CS]
tags: [TIL, CS]
---

# # TCP 3-way handshake 란?

TCP 3-way handshake는 TCP(Transmission Control Protocol)에서 연결을 설정하기 위해 사용하는 과정이다. 이 과정은 클라이언트와 서버 간의 신뢰성 있는 통신을 보장하기 위해 세 단계로 이루어져 있다.

1. **SYN (Synchronize)**: 클라이언트가 서버에 연결 요청을 보내기 위해 SYN 패킷을 전송한다. 이 패킷에는 초기 시퀀스 번호(Sequence Number)가 포함되어 있다.

2. **SYN-ACK (Synchronize-Acknowledge)**: 서버는 클라이언트의 SYN 패킷을 수신하고, 이에 대한 응답으로 SYN-ACK 패킷을 보낸다. 이 패킷에는 서버의 초기 시퀀스 번호와 클라이언트의 시퀀스 번호에 대한 확인 응답(Acknowledgment Number)이 포함되어 있다.

3. **ACK (Acknowledge)**: 클라이언트는 서버의 SYN-ACK 패킷을 수신하고, 이에 대한 응답으로 ACK 패킷을 보낸다. 이 패킷에는 서버의 시퀀스 번호에 대한 확인 응답이 포함되어 있다.

이 세 단계가 완료되면 클라이언트와 서버 간에 TCP 연결이 설정되고, 이후 데이터 전송이 가능해진다. 3-way handshake는 네트워크 통신에서 신뢰성을 확보하고, 데이터의 순서와 무결성을 보장하는 데 중요한 역할을 한다.

<br /><br />

참고 : 
- [TCP 3-way handshake에 대해서 설명해주세요.](https://www.maeil-mail.kr/question/76)