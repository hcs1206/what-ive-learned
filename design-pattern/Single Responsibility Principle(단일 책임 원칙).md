# Single Responsibility Principle (단일 책임 원칙)

## 개요

오래전 질레트(Gillette)는 면도기와 면도날 사업에서 벗어나 샴푸 등 다양한 일용품 사업으로 다각화를 시도했다. 하지만 실적은 기대에 미치지 못했고, 결국 다각화한 사업을 정리하고 다시 면도용품 사업에 집중하면서 세계 최고의 면도용품 회사라는 명성을 되찾았다.

이처럼 기업들이 위험을 분산하고 실적을 향상시키기 위해 다각화를 추진하지만, 경우에 따라 다각화는 오히려 실적 악화로 이어질 수 있다. 월스트리트의 전설적인 투자자인 피터 린치(Peter Lynch)는 이에 대해 "다각화는 대부분 다악화(diworsification)로 끝난다"고 충고했다.

소프트웨어 세계에서도 비슷한 원칙이 적용된다. \*\*하나의 클래스는 하나의 책임만 가져야 한다는 단일 책임 원칙(Single Responsibility Principle, SRP)\*\*이 바로 그것이다. 이번 글에서는 객체가 가지는 책임이란 무엇이며, 객체가 단일 책임만 가지는 것이 왜 중요한지를 살펴보고, SRP를 위반했을 때 발생하는 문제점과 이를 해결하는 방법에 대해 논의한다.

---

## 단일 책임 원칙이란?

SRP의 핵심 개념은 \*\*"하나의 클래스는 하나의 책임만 가져야 한다"\*\*는 것이다. 여기서 책임이란 \*\*"변경을 위한 이유"\*\*를 의미한다. 즉, 하나의 클래스에 두 개 이상의 변경 이유가 있다면, 해당 클래스는 SRP를 위반하고 있는 것이다.

### 예제: 국제 거래 은행의 `잔고` 클래스

다음은 국제 거래 은행에서 사용하는 `잔고(Balance)` 클래스의 예제이다.

```java
public class Balance {
    private double amount;

    public double getAmount() {
        return amount;
    }
    
    public double calculateExchangeRate(double rate) {
        return amount * rate;
    }
}
```

이 클래스는 두 가지 책임을 가진다:

1. 금액(amount) 관리
2. 환율 계산(calculateExchangeRate)

이 클래스는 서로 다른 두 개의 애플리케이션에서 사용될 수 있다:

- **이율 관리 애플리케이션**: `getAmount()`를 사용
- **환율 조정 애플리케이션**: `calculateExchangeRate()`를 사용

문제는 **잔고 클래스가 어느 애플리케이션에 배포되느냐에 따라 특정 기능이 불필요해진다는 점**이다. 또한 `calculateExchangeRate()`의 로직이 변경될 경우, `getAmount()`를 사용하는 이율 관리 애플리케이션에도 영향을 미치게 된다. 이는 강한 결합도를 유발하며, 유지보수성을 저하시킨다.

이러한 문제를 해결하기 위해, `잔고` 클래스의 책임을 분리하는 것이 필요하다.

---

## SRP 적용 방법

### 1. 책임 분리

잔고 클래스의 두 가지 책임을 분리하여 각각의 역할에 맞는 클래스를 만든다.

#### **책임 분리 후의 클래스 설계**

```java
public class Balance {
    private double amount;

    public double getAmount() {
        return amount;
    }
}

public class ExchangeRateCalculator {
    public double calculate(double amount, double rate) {
        return amount * rate;
    }
}
```

이제 `Balance` 클래스는 금액 관리만 담당하고, 환율 계산은 `ExchangeRateCalculator`가 처리한다. 이로써 변경의 영향을 최소화할 수 있다.

### 2. 데이터 접근과 비즈니스 로직 분리

데이터베이스(DB)와 관련된 로직을 비즈니스 로직과 분리하는 것이 SRP를 적용하는 또 다른 방법이다. 다음은 전형적인 SRP 위반 사례이다.

```java
public class Person {
    private String name;
    private int age;
    
    public void saveToDatabase() {
        // DB 저장 로직
    }
}
```

위의 `Person` 클래스는 **데이터 저장**과 **비즈니스 로직** 두 가지 책임을 동시에 수행하고 있다. 이는 SRP를 위반하는 대표적인 사례이다.

이를 해결하기 위해 **DAO(Data Access Object) 패턴**을 적용할 수 있다.

#### **DAO 패턴을 활용한 SRP 적용**

```java
public class Person {
    private String name;
    private int age;

    public String getName() {
        return name;
    }
}

public class PersonDAO {
    public void save(Person person) {
        // DB 저장 로직
    }
}
```

이제 `Person` 클래스는 비즈니스 로직만 담당하며, `PersonDAO`가 데이터베이스 관련 책임을 맡는다. 이를 통해 유지보수성이 향상되고, 변경의 영향을 최소화할 수 있다.

---

## SRP 위반의 문제점

SRP를 위반하면 다음과 같은 문제가 발생한다:

1. **불필요한 코드 의존성 증가**
    - 예를 들어, 특정 기능이 변경될 때 관련이 없는 코드까지 영향을 받게 된다.
2. **유지보수 비용 증가**
    - 변경이 필요할 때 많은 부분을 수정해야 하므로 비용과 시간이 증가한다.
3. **재사용성 감소**
    - 다른 기능에서 특정 클래스를 사용하려 해도 불필요한 기능이 함께 따라와 재사용이 어렵다.

---

## 결론

단일 책임 원칙(SRP)은 유지보수성과 확장성을 고려한 객체지향 설계의 중요한 원칙이다. 하나의 클래스는 오직 하나의 책임만 가져야 하며, 이를 통해 변경에 유연하게 대응할 수 있다.

**SRP를 적용하면 다음과 같은 이점이 있다:**

- 코드의 **명확성** 증가
- **유지보수성** 향상
- **확장성** 개선

즉, SRP를 지키는 것은 "변경의 영향을 최소화"하고 "더 나은 아키텍처"를 구축하는 핵심 요소라 할 수 있다.

