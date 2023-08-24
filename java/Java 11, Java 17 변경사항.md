# Java 11, Java 17 변경사항
## Java 8 -> Java 11 주요 변경사항
### New Garbage Collector, ZGC 추가
JDK 11부터 공개되었고, “Stop-The-World”로 인한 성능저하를 개선하기 위한 목적을 가지고 Oracle에서 개발

**ZGC 동작 원리 (Wiki, GitHub)**
- Pause Mark Start: Colored pointers 알고리즘을 기반해 ZGC Root에서 가리키는 객체의 상태값을 Mark(저장) 합니다.
- Pause Mark End: 새로 들어온 객체를 대상으로 Mark가 일어나고, ZPage(ZGC에서 다루는 영역)를 찾아 RelocationSet()에 배치합니다.
- Pause Relocate Start: Root 참조 객체에 대한 재배치를 하며, 이후 Load barriers 알고리즘을 통해 모든 객체를 안전하게 업데이트합니다.

**특징**
- 대기 시간이 짧은 Application에 적합한 Garbage Collection입니다. 
- Thread가 실행 중일 때 동시 작업을 수행하기에 모든 작업을 동시에 수행합니다. (병렬 처리)
- 처리 시간이 10ms를 초과하지 않아 짧은 지연시간을 보장합니다.
- 8MB부터 16TB까지의 Heap 크기를 지원합니다.

**적용 방법**
- Java Application 실행 시 다음 옵션 실행 (기본 설정: G1GC)
```
java -XX:+UseZGC -jar Application.java
```

### Collection Factory Method 강화
- Set, List, Map 인터페이스에 Immutable 생성할 수 있는 새로운 메서드가 추가 되었습니다.
- 변경 전
```java
List<String> list = new ArrayList<>();
list.add("a");
list.add("b");
```
- 변경 후
```java
List<String> list = List.of("a","b");
```
- 기존보다 깔끔한 코드스타일을 가져갈 수 있고, Collection 초기화 값을 세팅하여 사용 가능합니다.
- 또한 본 개념대로라면 Immutable 속성을 가지고 있기에 데이터 변경 작업이 어렵지만 데이터의 추가, 수정, 삭제 작업이 필요할 경우, 아래와 같이 진행할 수 있습니다.
```java
List<String> oldList = List.of("a", "b", "c");
List<String> newList = oldList.stream() // 리스트를 스트림으로 변환
                              .filter(item -> !item.equals("a")) // "a"를 제외한 원소 추출
                              .collect(Collectors.toList()); // 추출한 원소로 리스트 생성
```

### Reactive Stream
Non-Blocking Backpressure를 이용한 비동기 스트림 처리의 표준 제공이 목적인 기능으로, 기존 “Observer Pattern”에서 극복이 어려웠던 “C10K” 문제 해결을 위하여 “Subscription”이라는 중계 브로커가 추가된 패턴 방식 입니다.
- C10K Problem: 동시에 만 개의 클라이언트 요청이 올 경우 처리할 수 없는 문제
- Publisher: 데이터 상태 변경 시 생산한 데이터를 전달해주는 데이터 제공자
- Subscriber: Publisher에서 발행한 데이터를 받아 처리하는 데이터 수신자
- Subscription: Publisher에게 전달 받은 데이터를 제어하여 Subscriber에게 전달하는 중계 브로커 또한 이러한 Reactive Stream은 WebFlux와도 연관이 있습니다.

### 로컬 변수 타입 추론 “var”
로컬 변수 선언 시, “타입 추론”을 이용하여 명시적 타입 선언 없이도 변수 선언이 가능하도록 지원하는 신규 Keyword입니다.

기존 Lombok Library에서 지원하던 기능이었으나, JDK 10 버전에 등장하며Java에서 공식 지원하였습니다. 이후 LTS 버전인 JDK 11부터는 람다 타입에서도 사용 가능하도록 지원하고 있습니다.

컴파일 시 변수 타입을 추론하기 때문에 성능에 영향을 주지는 않습니다. 그러나 가독성 높은 코드 작성을 위해 무분별한 “var” Keyword 이용은 지양하는 것이 좋겠습니다.

**제약**

멤버 변수 또는 Method 파라미터로 선언은 불가능하며, 오로지 Method 내 로컬 변수로만 선언 가능합니다.

### 신규 문자열 Method 추가
문자열 내 공백 확인/제거 등 JDK 11 버전부터 String에 여러 편리한 기능을 지원하는 새로운 Method들이 추가 되었습니다.
- isBlank: 문자열이 비어있거나 공백이면 True 반환
- lines: 줄 단위로 나뉘어 있는 문자를 배열로 반환
- strip: 문자열 공백 제거 (기존 “trim()”이 ‘\u0020’ 이하 공백만을 제거 하였다면, “strip()”은 유니코드의 공백들을 전부 제거)
- stripLeading: 문자열 앞의 공백을 제거
- stripTrailing: 문자열 뒤의 공백을 제거
- repeat: 문자열을 파라미터로 주어진 수 만큼 반복

## Java 11 -> Java 17 주요 변경사항
### 텍스트 블록
JDK 15부터 정식 지원하는 새로운 기능입니다.

“”” {String 문자열 } “”” 형식을 이용해 Java 문자열을 보다 가독성있게 작성하도록 도와줍니다.

**예제**
```java
"""
SELECT *
FROM USER
WHERE id = :id
"""
```

### Switch 표현식 기능 향상
1. Switch문 값 직접 반환
2. yield 예약어 이용한 값 리턴 방식 추가.
3. Case문 람다식 지원.

**예제1. 표현식 추가 및 값 반환**
```java
// 기존 표현 방식

String countryName;
City city = City.SEOUL;

switch (city) {
    case SEOUL:
        countryName = "Korea";
        break;
    case NEWYORK:
        countryName = "America";
        break;
    case LONDON:
        countryName = "England";
        break;
    default:
        countryName = "unknown";
}

```
```java
// 변경 표현 방식
City city = City.SEOUL;

String countryName = switch (city) {[Database, JPA 0ab448a7c6d44a04ba92b364ba0b6f70.md](..%2F..%2FDatabase%2C%20JPA%200ab448a7c6d44a04ba92b364ba0b6f70.md)
        case SEOUL -> "Korea";
        case NEWYORK -> "America";
        case LONDON -> "England";
        default -> "unknown";
        };
```
예제2. 반환을 위한 yield 키워드 사용
```java
// 스위치 표현식에서 int 값을 리턴하는 예
int j = switch (day) {
    case MONDAY  -> 0;
    case TUESDAY -> 1;
    default      -> {
        int k = day.toString().length();
        int result = f(k);
        yield result;
    }
};

// 스위치 문장에서 값 리턴 예
int result = switch (s) {
    case "Foo":
        yield 1;
    case "Bar":
        yield 2;
    default:
        System.out.println("Neither Foo nor Bar");
        yield 0;
};
```

### Record Data Class 추가
Record Data Class란, JDK 14 버전부터 공개된 Immutable 객체를 생성하는 새로운 유형의 클래스입니다.
- Record 선언을 하게 되면 기존 toString, equals, hashCode 메소드를 자동으로 구현해주며, 모든 인스턴스 필드를 초기화해주는 생성자가 생성이 됩니다.
- Immutable 객체이기 때문에 모든 값은 생성자를 통해 설정되어야 합니다.
- DTO와 같은 Data Object 용도로 활용 시 보다 편리하고 간결하게 구분할 수 있습니다.
- Record Class는 상속이 불가능합니다. (모든 필드는 “private final ,,”로 선언이 되기에,,)
- Record Class에 validation 및 MapStruct 라이브러리 모두 이용할 수 있다

**예제1. record, class 선언 비교**
```java
// class
public class BookDto {
    private String title;
    private String author;
    private String isbn;
    private String publisher;

    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }
    // ... 수많은 코드 ...
    @Override
    public int hashCode() {
        return Objects.hash(title, author, isbn, publisher);
    }
}

// record 
public record BookDto(String title, String author, String isbn, String publisher) { }
```

예제2. record 컴팩트 생성자
```java
// 일반 생성자
record Rational(int num, int denom) {
    Rational(int num, int demon) {
        // 검증/정규화(Validation/Normalization)
        int gcd = gcd(num, denom);
        num /= gcd;
        denom /= gcd;
        // 초기화
        this.num = num;
        this.denom = denom;
    }
}

// 컴팩트 생성자
record Rational(int num, int denom) { 
	Rational {
    	// 기약분수로 만들기 위해서 분모와 분자를 최대공약수(GCD)로 나눈다.
		int gcd = gcd(num, denom);
		num /= gcd;
		denom /= gcd;
	}
}
```

예제3. Validation, Swagger, MapStruct 적용
- record class도 Validation, Swagger, MapStruct 모두 적용 가능하다
```java
// Validation, Swagger 적용 
public record BookDto(
        @Schema(description = "제목")
        @NotNull
        String title,
        @Schema(description = "작가")
        @NotNull
        String author,
        @Schema(description = "고유번호")
        @NotNull
        String isbn,
        @Schema(description = "출판사")
        @NotNull
        String publisher
) {
}

// MapStruct 적용
@Mapper(componentModel = "spring")
public interface BookMapper {
    @Mapping(source = "isbn", target = "id")
    Book toEntity(BookDto source);
}
```