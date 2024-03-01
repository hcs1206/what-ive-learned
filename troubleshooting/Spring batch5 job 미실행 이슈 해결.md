# 이슈사항

- 스케줄러를 통한 job 실행이 아닌 jenkins를 스케줄러를 이용하여 job을 cli로 실행해왔음
- 하지만 Spring batch 버전 업그레이드 후 잘 실행되던 job이 실행되지 않는 모습을 확인

# 해결

- @EnableBatchProcess 라는 어노테이션은 Springboot2.x.x 까지 BatchAutoConfiguration에 등록된 빈들을 자동으로 등록해주는 역할을 하였지만, Springboot 3.x.x
  부터는 @EnableBatchProcess라는 어노테이션이 없고, DefaultBatchConfiguration이 존재하지 않는 경우만 BatchAutoConfiguration이
  활성화 된다.

```yaml
springdoc:
  swagger-ui:
    configUrl: /domain/v3/api-docs/swagger-config
    url: /domain/v3/api-docs
```

- 위처럼 Swagger 리소스 서버 request 전송 시 domain을 붙혀 찾을 수 있도록 설정 필요

```java
public abstract class ExamAbstractService {
    
    protected abstract Result validRequest(ExamDto dto);

  protected abstract Result saveRequest(ExamDto dto);

  protected abstract Result closeRequest(ExamDto dto);
  
  public Response exam(ExamDto dto) {
      return performExam(
              () -> validRequest(dto),
              () -> saveRequest(dto),
              () -> closeRequest(dto)[WLF_20240221.xml](..%2F..%2FDesktop%2FWLF_20240221.xml)
      );
  }
  
  private Response performExam(Supplier<ExamDto>... exams) {
      ExamDto examDto = new ExamDto();
      for(Supplier<ExamDto> exam : exams) {
          examDto = exam.get();
      }
      
      return new Response(examDto);
  }
}

```