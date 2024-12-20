---
title: "[Monitoring] Prometheus & Grafana"
date: 2024-12-20
categories: [Monitoring]
tags: [TIL, Monitoring, Prometheus, Grafana]
---

## # Prometheus란?

![img](/assets/img/til/prometheus.png)

Prometheus는 **시계열 데이터베이스(TSDB)** 로, 애플리케이션과 시스템의 메트릭 데이터를 수집 및 저장하는 오픈소스 모니터링 도구이다.

- **데이터 수집 방식**: Pull 방식으로 동작하며, 수집 대상(Target)으로부터 데이터를 주기적으로 가져온다.

- **데이터 포맷**: 시계열 데이터(time-series data)로 저장되며, 메트릭(metric)과 레이블(label)로 구성된다.

- **Exporter**: Prometheus는 자체적으로 데이터를 수집하지 않으므로, 데이터를 노출시키는 **Exporter**가 필요하다.
    - 예시 : Node Exporter - 서버의 CPU, 메모리, 디스크 등의 정보를 수집.
    - 예시 : cAdvisor - 컨테이너 메트릭 정보를 수집하여 컨테이너화된 애플리케이션을 모니터링.
    - 예시 : 애플리케이션 자체 Exporter - Spring Boot Actuator 등 직접 Exporter를 구현할 수도 있다.

- **구성 파일** : `prometheus.yml` 파일을 통해 데이터 수집 대상(Target)과 스크래핑 주기 등을 설정한다.
  - 예시 :
    ```
    global:
        scrape_interval: 15s

    scrape_configs:
        - job_name: 'spring-boot'
          metrics_path: '/actuator/prometheus'
          static_configs:
            - targets: ['host.docker.internal:8080']
    ```

<br /><br />

## # Grafana란?

![img](/assets/img/til/grafana.png)

**Grafana** 는 **데이터 시각화 도구** 로, Prometheus 및 다양한 데이터 소스의 메트릭 데이터를 시각적으로 표시하는 대시보드를 생성할 수 있다.

- **데이터 소스** : Prometheus, MySQL, PostgreSQL 등 다양한 데이터 소스를 연결할 수 있다.

- **대시보드** : 데이터를 시각적으로 표현하기 위해 다양한 차트, 그래프, 경고(Alert) 시스템을 지원한다.

- **Alerting** : 특정 조건이 충족되었을 때 이메일, Slack 등으로 알림을 보낼 수 있는 경고 시스템을 제공한다.


<br /><br />

### # Prometheus와 Grafana 연동하기

1. **Docker** 를 사용해서 **Grafana** 컨테이너 실행하기
    ```
    docker run -d --name=grafana -p 3000:3000 grafana/grafana
    ```


2. `localhost:3000` 에 접속하여 로그인을 한다.
   - 아이디 : admin / 비밀번호 : admin


3. 대시보드에서 **DATA SOURCE** 를 클릭한 후, Prometheus를 선택한다.

4. **Name** 을 입력한 후 **Contact** 에 **Prometheus Server URL** 을 입력한다.
   - 예 : host.docker.internal

5. 대시보드 메뉴에 접속하여 **Create Dashboard** 버튼을 클릭한 후, **import dashboard** 를 클릭한다.

6. 임포트 창에서 19004을 입력하고 **Load** 버튼을 클릭한다. 19004는 Grafana에서 제공하는 spring boot3 용 대시보드이다.

7. **prometheus** 를 선택하고 **import** 를 클릭하면 대시보드가 생성된다.

<br /><br />


참고 : 
- [[Infra] 서버 모니터링 구축기 [Docker, Prometheus, Grafana]](https://developer-nyong.tistory.com/49)
- [grafana와 prometheus로 모니터링 시스템 구축하기](https://solo5star.tistory.com/19)
- [Prometheus & Grafana 모니터링 시스템 구축하기](https://rachel0115.tistory.com/entry/Prometheus-Grafana-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)
- [스파르타] 6.모니터링 시스템 및 시큐어 코딩 강의 자료
