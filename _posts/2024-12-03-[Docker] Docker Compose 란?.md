---
title: "[Docker] Docker Compose 란?"
date: 2024-12-03
categories: [Docker]
tags: [TIL, Docker]
image:
  path: /assets/img/til/docker.png
---

## 📍 Docker Compose 란?

![img](/assets/img/til/docker_compose.jpg)

도커 컴포즈는 단일 서버에서 여러 개의 컨테이너를 하나의 서비스로 정의해 컨테이너의 묶음으로 관리할 수 있는 작업 환경을 제공하는 관리 도구이다.

<br />

#### # 도커 컴포즈를 사용하는 이유

여러 개의 컨테이너가 하나의 어플리케이션으로 동작할 때 도커 컴포즈를 사용하지 않는다면, 이를 테스트하려면 각 컨테이너를 하나씩 생성해야 한다. 

예를 들면, 웹 어플리케이션을 테스트하려면 웹 서버 컨테이너, 데이터베이스 컨테이너 두 개의 컨테이너를 각각 생성해야 한다.

컨테이너를 가각 실행하기 위해 매번 run 명령어에 옵션을 설정해 CLI로 컨테이너를 생성하기보다는 여러 개의 컨테이너를 하나의 서비스로 정리해 컨테이너 묶음으로 관리할 수 있다면 좀 더 편리할 것아다. 이를 위해 도커 컴포즈는 컨테이너를 이용한 서비스의 개발과 CI를 위해 여러 개의 컨테이너를 하나의 프로젝트로서 다룰 수 있는 작업 환경을 제공한다.

도커 컴포즈는 여러 개의 컨테이너의 옵션과 환경을 정의한 파일을 읽어 컨테이너를 순차적으로 생성하는 방식으로 동작한다. 도커 컴포즈의 설정 파일은 도커 엔진의 run 명령어의 옵션을 그대로 사용할 수 있으며, 각 컨테이너의 의존성, 네트워크, 볼륨 등을 함께 정의할 수 있다. 또한 스웜모드의 서비스와 유사하게 설정 파일에 정의된 서비스의 컨테이너 수를 유동적으로 조절할 수 있으며 컨테이너의 서비스 디스커버리도 자동으로 이뤄진다. 그래서 컨테이너의 수가 많아지고 정의해야 할 옵션이 많아지고 정의해야 할 옵션이 많아진다면 도커 컴포즈를 사용하는 것이 편리하다.
<br /><br />

### # 도커 컴포즈 사용하기

#### 1. docker-compose 설치

docker와는 별도로 공식 문서를 참고하여 해당하는 운영체제에 맞추어 docker-compose 설치한다.

공식 문서 : https://docs.docker.com/compose/install/

<br />

### 2. docker-compose.yml 파일 작성

```
version: "3.9" # docker-compose 설정파일 버전
services:
  service1:
    # 첫번째 서비스 설정
  service2:
    # 두번째 서비스 설정
  # ...

# 네트워크, 볼륨 등 docker resource 설정 
networks:
  # 네트워크 설정
volumes:
  # 볼륨 설정
```

<br />

### 3. docker-compose up
`docker-compose.yml` 파일이 있는 디렉토리의 위치에서 명령어를 입력하여 실행시킨다.

```
// -d(--detach) 옵션을 사용하면 백그라운드로 실행
docker-compose up -d   
```

<br /><br />

참고 : 
- [[Docker] 도커 컴포즈(Docker compose) - 개념 정리 및 사용법](https://seosh817.tistory.com/387)
- [Docker Compose 사용법](https://byungwoo.oopy.io/2758a690-6ec7-42d0-ae41-5ebc75f4e57a)
- [Docker & Docker-compose](https://velog.io/@duckbill/Docker-Docker-compose)

