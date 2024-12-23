---
title: "[Monitoring]Loki"
date: 2024-12-23
categories: [Monitoring]
tags: [TIL, Monitoring, Grafana, Loki]
---

## Loki 란?

**Loki** 는 Grafana Labs에서 개발한 로그 집계 시스템이다.

**Loki** 는 Prometheus와 유사하게 로그 데이터를 수집 및 쿼리할 수 있도록 설계되었으며, 라벨 기반 메타데이터를 통해 로그를 효율적으로 검색할 수 있다.

주로 Grafana와 연동하여 **로그 시각화** 에 사용된다.
<br /><br />

### # loki-logback-appender

**loki-logback-appender** 는 Logback을 사용하는 Java 애플리케이션에서 로그를 Loki로 직접 전송하기 위한 라이브러리이다. 이를 사용하면 별도의 Promtail 설정 없이도 로그를 Loki로 전송할 수 있다.

<br />

#### loki-logback-appender 설정하기

- `build.gradle`에 **loki-logback-appender** 라이브러리 추가
- `logback.xml`에서 Loki로 전송할 로그 형식 및 HTTP 엔드포인트 설정
- 간단한 Spring Boot 컨트롤러(SampleController)에서 403 에러 로그를 발생시킴

- logback.xml 예시 :

```logback.xml
<appender name="LOKI" class="com.github.loki4j.logback.Loki4jAppender">
    <http>
        <url>http://localhost:3100/loki/api/v1/push</url>
    </http>
    <format>
        <label>
            <pattern>app=my-app,host=${HOSTNAME}</pattern>
        </label>
        <message class="com.github.loki4j.logback.JsonLayout" />
    </format>
</appender>
```

<br /><br />

### # Docker 사용하여 Loki 설치 및 설정하기

- loki-config.yml 파일 생성하여 Loki 동적 설정하기

    ```
    auth_enabled: false

    server:
    http_listen_port: 3100
    grpc_listen_port: 9096

    common:
    instance_addr: 127.0.0.1
    path_prefix: /tmp/loki
    storage:
        filesystem:
        chunks_directory: /tmp/loki/chunks
        rules_directory: /tmp/loki/rules
    replication_factor: 1
    ring:
        kvstore:
        store: inmemory

    query_range:
    results_cache:
        cache:
        embedded_cache:
            enabled: true
            max_size_mb: 100

    schema_config:
    configs:
        - from: 2020-10-24
        store: tsdb
        object_store: filesystem
        schema: v13
        index:
            prefix: index_
            period: 24h

    ruler:
    alertmanager_url: http://localhost:9093

    # By default, Loki will send anonymous, but uniquely-identifiable usage and configuration
    # analytics to Grafana Labs. These statistics are sent to https://stats.grafana.org/
    #
    # Statistics help us better understand how Loki is used, and they show us performance
    # levels for most users. This helps us prioritize features and documentation.
    # For more information on what's sent, look at
    # https://github.com/grafana/loki/blob/main/pkg/analytics/stats.go
    # Refer to the buildReport method to see what goes into a report.
    #
    # If you would like to disable reporting, uncomment the following lines:
    #analytics:
    #  reporting_enabled: false
    ```

- Docker 실행하기
    ```
    docker run --name loki -d -v $(pwd)/loki-config.yml:/mnt/config -p 3100:3100 grafana/loki:3.0.0 -config.file=/mnt/config/loki-config.yml
    ```

<br /><br />

### # Grafana와 연동하기

- Grafana에서 Loki를 데이터 소스로 추가
    - http://host.docker.internal:3100를 Loki의 엔드포인트로 설정

- Explore 페이지에서 Loki 데이터를 쿼리하여 애플리케이션 로그 확인 가능
<br /><br />


참고 : 
- [Grafana Loki란? 개념부터 설치까지](https://wlsdn3004.tistory.com/48)
- [Grafana Loki에 대해 알아보자](https://devocean.sk.com/blog/techBoardDetail.do?ID=163964)
- [스파르타] 6.모니터링 시스템 및 시큐어 코딩 강의 자료
