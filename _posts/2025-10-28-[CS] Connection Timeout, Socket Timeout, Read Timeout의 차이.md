---
title: "[CS] Connection Timeout, Socket Timeout, Read Timeout의 차이"
date: 2025-10-28
categories: [CS]
tags: [TIL, CS]
---

# # Connection Timeout, Socket Timeout, Read Timeout의 차이


## 1. Connection Timeout

**Connection Timeout**은 클라이언트가 서버에 연결을 시도할 때, 연결이 성립되기까지 기다리는 최대 시간을 의미한다. 만약 **지정된 시간 내에 서버와의 연결이 이루어지지 않으면, 연결 시도가 실패로 간주되고 예외가 발생**한다. 이는 주로 네트워크 지연이나 서버가 응답하지 않는 경우에 발생할 수 있다.

<br />

## 2. Socket Timeout

**Socket Timeout**은 소켓이 데이터를 송수신하는 동안 기다리는 최대 시간을 의미한다. **소켓이 데이터를 보내거나 받을 때, 지정된 시간 내에 작업이 완료되지 않으면 타임아웃이 발생**한다. 이는 네트워크 지연이나 서버의 응답 지연으로 인해 발생할 수 있다.

<br />

## 3. Read Timeout

**Read Timeout**은 소켓이 데이터를 읽는 동안 기다리는 최대 시간을 의미한다. **클라이언트가 서버로부터 데이터를 읽으려고 할 때, 지정된 시간 내에 데이터가 도착하지 않으면 타임아웃이 발생**한다. 이는 서버가 응답을 지연시키거나 네트워크 문제가 있을 때 발생할 수 있다.

<br /><br />

참고 : 
- [Connection Timeout, Socket Timeout, Read Timeout의 차이점은 무엇인가요?](https://www.maeil-mail.kr/question/102)
