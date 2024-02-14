# 이슈사항
- 개발서버 배포 시 Swagger ui에서 Failed to load remote configuration 문구가 발생
- 개발서버는 inner gateway에서 reverse proxy를 구성하고 활용하고 있어 swagger resource를 제대로 가져오지 못하고 있었음

# 해결
- Gateway 설정을 변경하여도 되지만 권한 문제로 application.yml을 수정하여 해결
- 작성한 yml 설정

```yaml
springdoc:
  swagger-ui:
    configUrl: /domain/v3/api-docs/swagger-config
    url: /domain/v3/api-docs
```

- 위처럼 Swagger 리소스 서버 request 전송 시 domain을 붙혀 찾을 수 있도록 설정 필요