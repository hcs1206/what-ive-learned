# Java Object mapping library 비교
## ObjectMapper
- Spring 기본 제공 라이브러리로 spring-boot-starter-web에 포함되어 있기에 별도로 라이브러리를 추가할 필요는 없다.
- Json to Object 를 수행할 때 주로 사용하는 라이브러리고 RequestBody mapping 시에도 해당 라이브러리를 사용함
- Reflection을 사용해 mapping을 하기에 setter가 없어도 동작할 수 있다
- 좀 더 정확한 동작원리는 Json 필드의 이름을 getter, setter 메서드와 일치시키는데 해당 메서드의 get, set 을 제거하고 첫 번째
문자를 소문자로 치환하여 동작하는 방식. 그렇기에 getter 또는 setter 중 하나만 있어도 동작할 수 있다

## ModelMapper
- ModelMapper는 객체 간의 매핑을 수행하는 데 사용되는 라이브러리로, 필드 이름이 동일하면 자동으로 매핑을 수행한다.
- 기본생성자를 필수적으로 가져야 동작할 수 있다
- JavaBean 구조를 대상으로 하기에 맵핑하려는 클래스에는 Getter 및 Setter 메서드가 있어야 함.
Getter 메서드를 사용하여 참조하거나 각 필드의 값을 반환하며, Setter 메서드를 사용하여 값을 설정합니다.
- 단점: 컴파일러에 의해 최적화 되지 않으며, 최초 작업 시 캐싱되지 않고(이후 작업에는 만들어진 Map을 재사용), 매칭 및 매핑 로직에서 Refliection API를 사용해 객체 필드 정보를 추출하고 Map을 만든 다음 들어온 인자와 매칭시켜주고 Map의 정보를 기준으로 값을 매핑해주는 방식이기에 다른 방식보다 오버헤드가 많아 상대적으로 성능이 좋지 않음


## MapStruct
- 어노테이션 기반으로 복잡한 매핑구조를 설정할 수 있고, Reflection으로 매핑을 수행하는 것이 아닌 컴파일 단계에서 최적화 된
코드를 생성하므로 [성능 최적화](https://www.baeldung.com/java-performance-mapping-frameworks)에 도움을 줄 수 있다
- Setter, Constructor, Builder 등으 target 클래스에 선언되어있으면 최적화한 Java 코드가 생성됨
- 단점: 전혀 다른 형태의 필드 매핑을 시도하면 오히려 Mapper 클래스를 직접 생성하는 것보다 불편할 수 있음
- enum mapping 예시코드
```java
@Mapper(unmappedTargetPolicy = ReportingPolicy.IGNORE, componentModel = "spring", imports = {UserType.cass})
public interface UserMapper{
    @Mapping(target = "userType", expression = "java(UserType.from(source.getUserType()))")
    User toEntity(UserDto source);
}
```