# JPA 로그 사용시
```yaml
spring:
  jpa:
    show-sql: true
    properties:
      hibernate:
        format_sql: true
```

# querydsl 사용시
```xml
<logger name="com.querydsl.sql" level="DEBUG" additivity="false">
    <appender-ref ref="CONSOLE"/>
</logger>
```

# JdbcTemplate 사용시
```xml
<logger name="org.springframework.jdbc.core.JdbcTemplate" level="DEBUG" additivity="false">
    <appender-ref ref="CONSOLE"/>
</logger>
```

# p6spy vs log4jdbc
log4jdbc와 p6spy는 둘 다 JDBC 드라이버로써 데이터베이스 쿼리와 관련된 로깅 및 모니터링을 제공하는 라이브러리

- log4jdbc
  - 동작방식: JDBC드라이버 대체 래퍼
  - 설정 및 사용: 간편한 설정과 사용
  - 로깅 기능: 쿼리 실행 로깅
  - 모니터링 기능: 제한적
  - 확장성: 제한적
  - 애플리케이션 연결: 직접적인 연결 없음
  - 동작 범위: 특정 상황에서 동작하지 않을 수 있음

- p6spy
    - 동작방식: JDBC드라이버 래핑 및 가로채기
    - 설정 및 사용: 복잡한 설정
    - 로깅 기능: 쿼리 실행 전후 작업 수행 및 로깅
    - 모니터링 기능: 요청/응답 시간 측정, 결과 집계, 쿼리 분석 등
    - 확장성: 다양한 기능 및 확장성 제공
    - 애플리케이션 연결: 애플리케이션의 코드에 직접 연결
    - 동작 범위: 모든 종류의 쿼리에 대해 동작

## log4jdbc 설정
```yaml
spring:
  datasource:
    driver-class-name: net.sf.log4jdbc.sql.jdbcapi.DriverSpy
    url: jdbc:log4jdbc:oracle:thin@{IP}:{PORT}/{SERVICE_NAME}
```

```xml
<!--SQL 실행 시간-->
<logger name="jdbc.sqltiming" level="OFF"/>
<!--SQL 로그만 남김-->
<logger name="jdbc.sqlonly" level="OFF"/>
<!--호출 정보 로그-->
<logger name="jdbc.audit" level="OFF"/>
<!--Result Set-->
<logger name="jdbc.resultset" level="OFF"/>
<!--Result Set Table Log-->
<logger name="jdbc.resultsettable" level="OFF"/>
<!--Connection 관련 Log-->
<logger name="jdbc.connection" level="OFF"/>
```

## p6spy 설정
```yaml
spring:
  datasource:
    driver-class-name: com.p6spy.engine.spy.P6SpyDriver
    url: jdbc:p6spy:oracle:thin@{IP}:{PORT}/{SERVICE_NAME}
```