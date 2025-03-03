# Primitive vs Reference type

**Primitive type**- 변수에 값 자체를 저장

**Reference type**- 메모리상에 객체가 있는 위치를 저장

종류 - Class, Interface, Array 등

## 차이점

### Primitive type

- 변수에 값 자체를 저장
- 비객체 타입으로,Null을 가질 수 없는 형태
- primitive type은'**스택**' 메모리에 값이 존재
- Reference 타입보다 속도가 빠름

### Reference type

- 메모리상에 객체가 있는 위치를 저장
- 참조타입은 하나의 인스턴스이기 때문에****'**스택**' 메모리에는 참조값만있고,실제 값은**힙** 메모리에 존재
- 값을 필요로 할 때마다 언박싱 과정을 거치기 때문에 **Primitive타입과 비교해서 접근 속도**가 느림
- 예외적으로 엄청 큰 숫자를 복사해야 한다면, 참조값만 넘길 수 있는 참조타입이 좋을 수 도 있음
- 차지하는 메모리에 양도 참조타입이 훨씬 많음

# OOP

컴퓨터 프로그래밍 패러다임 중 하나로, **프로그래밍에서 필요한 데이터를 추상화시켜상태와 행위를 가진 객체를 만들고그 객체들 간의 유기적인 상호작용을 통해 로직을 구성하는 프로그래밍 방법**

### 객체 지향 프로그래밍의 장, 단점

**장점**

- 코드 재사용이 용이
    - 남이 만든 클래스를 가져와서 이용할 수 있고 상속을 통해 확장해서 사용할 수 있다.
- 유지보수가 쉬움
    - 절차 지향 프로그래밍에서는 코드를 수정해야할 때 일일이 찾아 수정해야하는 반면 객체 지향 프로그래밍에서는 수정해야 할 부분이 클래스 내부에 멤버 변수혹은 메서드로 존재하기 때문에 해당 부분만 수정하면 된다.
- 대형 프로젝트에 적합
    - 클래스 단위로 모듈화시켜서 개발할 수 있으므로 대형 프로젝트처럼 여러 명, 여러 회사에서 프로젝트를 개발할 때 업무 분담하기 쉽다.

**단점**

- 처리 속도가 상대적으로 느림
- 객체가 많으면 용량이 커질 수 있음
- 설계시 많은 시간과 노력이 필요

## 추상화

추상화라는것은, 어떤 실체로부터 공통적인 부분이나 관심 있는 특성들만 한곳에 모은것을 의미

객체지향에서의 추상화는 추상화는 객체들의 공통된 특징을 파악해 정의해 놓은 설계 기법

## 캡슐화

비슷한 역할을 하는 속성과 메소드들을 하나의 클래스로 모은것을 캡슐화라고 함

캡슐화를 하면 정보 은닉을 할 수 있다. 캡슐 내부의 로직이나 변수들을 감추고(접근 제한자) 외부에는 필요 기능만을 제공하는것을 의미한다.

## 상속

상속이란 클래스를 재사용하여 코드의 중복을 없애기 위함. 상속이 있기 때문에 코드를 재활용할 수 있고 생산성이 높고 유지보수 좋다는 장점을 가져갈 수 있다

## 다형성

다형성은 형태가 같은데 다른 기능을 하는 것(=같은 모양의 함수가 상황에 따라 다르게 동작 하는것)

또는 하나의 타입에 여러 객체를 대입할 수 있는 성질

### 장점

1) 유지보수가 쉽다

 개발자가 여러 객체를 하나의 타입으로 관리가 가능하기 때문에 코드 관리가 편리해 유지보수가 용이합니다.

2) 재사용성 증가

 다형성을 활용하면 객체를 재사용하기 쉬워지기 때문에 개발자의 코드 재사용성이 높아집니다.

3) 느슨한 결합

 다형성을 활용하면 클래스간 의존성이 줄어들며 확장성이 높고 결합도가 낮아져 안전성이 높아집니다.

- 다형성을 구현하는 방법
    - 오버로딩
    - 오버라이딩
    - 함수형 인터페이스(= 1개의 추상 메소드를 갖는 인터페이스)

# 접근 제어자

private, default, protected, public이 있습니다. private은 해당 클래스 내에서만 접근 가능하고, default는 해당 패키지, protected는 상속한 클래스, public은 전체 영역에서 접근 가능합니다.

접근 제어자를 사용하는 이유는 외부에 보여주고 싶은 정보들을 선택적으로 제공하기 위함이고, 캡슐화와 통하는 면이 있습니다.

# 추상클래스(abstract) vs 인터페이스(interface)

## 추상클래스란?

추상클래스는 일반 클래스와 별 다를 것이 없습니다. 단지, 추상 메서드를 선언하여**상속을 통해서 자손 클래스에서 완성하도록 유도**하는 클래스입니다. 그래서**미완성 설계도**라고도 표현합니다.상속을 위한 클래스이기 때문에 따로 객체를 생성할 수 없습니다.

class 앞에 "abstract" 예약어를 사용하여 상속을 통해서 구현해야한다는 것을 알려주고 선언부만 작성하는 추상메서드를 선언할 수 있습니다.

- 추상클래스란? → 실체클래스의 공통적인 부분(변수,메서드)를 추출해서 선언한 클래스
- 추상클래스는  실체성이 없고 구체적이지 않기 때문에 객체를 생성할 수 없다
- 추상클래스와 실체클래스는 상속관계이다

## 인터페이스란?

추상클래스가 미완성 설계도라면 인터페이스는 구현된 것은 아무것도 없고 밑그림만 그려져 있는**기본 설계도**라고 할 수 있다. 인터페이스도 추상클래스처럼 다른 클래스를 작성하는데 도움을 주는 목적으로 작성하고 클래스와 다르게다중상속(implements)이 가능합니다.

## **추상 클래스와 인터페이스의 공통점**

- 추상 클래스와 인터페이스는 선언만 있고 구현 내용은 없는 클래스이다.
- 인스턴스화(객체를 생성)를 할 수 없다
- 추상 클래스를 extends로 상속받은 자식들과 인터페이스를 implements 하고 구현한 자식들만 객체를 생성할 수 있다

## **추상 클래스와 인터페이스의 차이점**

- 추상 클래스(단일상속) / 인터페이스(다중상속)
- 추상 클래스의 목적은 상속을 받아 기능을 확장시키는 것
- 인터페이스의 목적은 구현하는 모든 클래스에 대해 특정한 메서드가 반드시 존재하도록 강제하는 역할
- 인터페이스는`구현 클래스 is able to 인터페이스`라고 해석할 수 있습니다. 즉, 무엇을 할 수 있는 이라는 표현입니다. 인터페이스는 클래스가 무엇을 할 수 있다라고 하는 기능을 구현하도록 강제하는 특징을 가지고 있습니다.
- 상속은 슈퍼클래스의 기능을 이용하거나 확장하기 위해 사용되고 다중 상속의 모호성 때문에 하나만 상속 받을 수 있다

# Singleton

## 목적

오직 한 개의 클래스 인스턴스만을 갖도록 보장하고, 이에 대한 전역적인 접근점을 제공합니다. 객체 중에는 시스템 혹은 프로세스에서 딱 하나만 존재해야 좋은 것들이 있다. 스레드 풀, 로거, 환경 설정을 위해 존재하는 것들이 예가 될 것이다.

## 장점

### **유일하게 존재하는 인스턴스로의 접근을 통제합니다.**

Singleton 클래스 자체가 인스턴스를 캡슐화하기 때문에, 이 클래스에서 사용자가 언제, 어떻게 이 인스턴스에 접근할 수 있는지 제어할 수 있습니다.

### **이름 공간(name space)을 좁힙니다.**

단일체 패턴은 전역 변수보다 더 좋습니다. 전역 변수를 사용해서 이름 공간을 망치는 일을 없애주기 때문입니다. 즉, 전역 변수를 정의하여 발생하는 디버깅의 어려움 등 문제를 없앱니다.

### **연산 및 표현의 정제를 허용합니다.**

Singleton클래스는 상속될 수 있기 때문에, 이 상속된 서브클래스를 통해서 새로운 인스턴스를 마들 수 잇습니다. 또한 이 패턴을 사용하면, 런타임에 필요한 클래스의 인스턴스를 써서 응용프로그램을 구성할 수도 있습니다.

### **인스턴스의 개수를 변경하기가 자유롭습니다.**

Singleton 클래스의 인스턴스에 접근할 수 있는 허용 범위를 결정하는 연산만 변경하면 됩니다.

### **클래스 연산을 사용하는 것보다 훨씬 유연한 방법입니다.**

단일체 패턴과 동일한 기능을 발휘하는 방법이 클래스 연산을 사용하는 것입니다. (static method). 이러한 방법을 사용할 때에는 클래스의 인스턴스가 하나 이상 존재할 수 있도록 설계를 변경하는 것은 어렵습니다. 또한 static method는 서브클래스들이 이 연산을 오버라이드할 수 없습니다.

## Singleton 주의할 점

싱글 쓰레드에서는 별 문제가 없지만,`멀티 쓰레드`환경에서는 Singleton 인스턴스가 1개만 생성된다는 보장이 안된다.

여러 쓰레드에서`getInstance()`를 호출 할 때를 생각해보자. 쓰레드 A에서는`null`임을 확인하고 인스턴스를 생성했으나, 쓰레드 B에서는 이미 생성이 된 줄 모르고`null`로 판단하여 두 번째 인스턴스를 생성할 수 있다.

## Thread-safe한 Singleton 사용법

### 1. Lazy Initialization (Draconian Synchronization)

단순하게`synchronized`키워드를 사용해서 동기화 시키는 것이다. 하지만`synchronized`는 성능저하가 심하기 때문에 그다지**권장하지 않는 방법이다.**

### 2. DCL(Double Checked Locking)

위 첫 번째 방법에서 성능 개선을 한 것이다. 그냥 한번 더`null`체크하고 단계를 한번 더 거치는건데, 성능차이가 많이 나나? 싶을텐데, 1번 방법에 비해 꽤 빠르다.

이 방법이 의도하고자 한 것은 메서드에`synchronized`를 쓰지 않고, 임계 구역(critical section)에만 동기화를 걸어서 동기화 오버헤드를 줄이는 것이다.

위 코드에 있는`volatile`키워드는 Java 5부터 유효하며,**이 키워드를 붙이지 않으면 제대로 동작하지 않는다.**

`volatile`에 대해서 설명하자면 길지만.. 일단 간략하게 설명하자면 main memory가 있고 각 스레드 마다 working memory가 있다.**main memory <-> working memory**이렇게 두 메모리간 데이터 이동이 있으며, 두 메모리간 동기화가 진행되는 동안 빈틈이 생기게 되기 때문에`volatile`을 쓰는 것이다.

`volatile`키워드 없이 DCL을 구현하면**생성이 되다만 객체를 다른 스레드에서 참조 할 수 있는 문제가**있다. 쓰레드 2개를`T1`,`T2`라고 가정하고, 아래의 시나리오를 생각 해 보자.

1. `T1`이 인스턴스를 생성하고 synchronized 블록을 나옴
2. `T2`가 synchronized 블록에 들어와서 null 체크를 하는 시점에서
3. `T1`에서 만든 인스턴스가 working memory에만 존재하고 main memory에 없을 때 (혹은 main memory에 있으나 working memory에는 없을 때)
4. `T2`는 두 번째 인스턴스를 생성한다. (두 메모리간 완전히 동기화가 되지 않았기 때문)

### 3. Enum

Joshua Bloch가 작성한 Effective Java 2판에 소개된 방법이다. Enum은 상수들만 모아놓은 특별한 클래스이며, 일반 클래스처럼 메소드와 생성자(private)를 가질 수 있다. 런타임이 아닌 컴파일 타임에 모든 값을 알고 있어야 하는 특징이 있다.

Lazy Loading이 아니어서 유연성이 좀 떨어질 수 있지만 강력한 장점들을 가지고 있다.

**구현이 매우 단순**

딱봐도 단순하게 생김

**직렬화/역직렬화를 자동으로 해줌**

`Serializable`를 implements하면 Singleton 패턴이 파괴된다. 역직렬화가 진행 될 때`readObject()`를 호출하면서 새로운 인스턴스를 만들기 때문이다.

`Serializable`를 implements 하면서 Singleton 패턴이 정상적으로 동작하게 할 수도있다. Enum을 쓰면 아래와 같이 번거로운 과정을 거치지 않아도 된다.

1. 모든 필드에`transient`키워드 붙이기
2. `readResolve()`메소드 구현

**Enum 자체가 thread-safe 함**

그렇지만 Enum 내부에 구현한 메서드들도 thread-safe 하지는 않으니 주의해야한다.

**Reflection 공격에 안전**

Reflection의`setAccessible(true)`를 사용하면 모든 private 생성자와 메소드에 접근할 수 있다.`newInstance()`메소드를 통해서 계속해서 새로운 인스턴스들을 만들 수 있는데, 이 같은 문제도 해결 할 수도 있다.

하지만 이 방법 역시 단점이 있는데, Singleton 초기화 과정에서 다른 의존성이 낄 수 있다. 안드로이드 같은 경우는`Context`라는 의존성이 낄 수 있는데, Enum은 컴파일 타임에 초기화가 이루어지기 때문에 매번 메서드를 호출 할 때`Context`를 계속 넘겨줘야 하는 상황이 생길 수 있다.

### 4. LazyHolder

Singleton 클래스 안에 Holder 클래스를 두고, JVM의 Class loader 규칙에 의해 Lazy Loading 하는 것을 보장한다. Java는 동적으로 클래스를 로딩하며 2가지 방식이 있다.

1. 로드타임 동적 로딩(Load-time Dynamic Loading)
2. 런타임 동적 로딩(Run-time Dynamic Loading)

`로드타임 동적 로딩`은 클래스 내부에 다른 클래스 정보가 있다면 모두 로드하는 방식이고,`런타임 동적 로딩`은 실제 클래스 정보를 필요로 할 때 로드하는 방식이다. 여기서 LazyHolder는`런타임 로드`이기 때문에, 실제로 필요로 하기 전 까지 JVM에 올라오지 않는다.

여러 쓰레드에서 LazyHolder를 호출해도 JVM이 알아서 하나만 올려주기 때문에`synchronized`,`volatile`과 같이 동기화를 위한 키워드를 쓰지 않아도 되고 Java 버전을 타지도 않는 장점이 있다.

Java 버전을 타지 않으며 성능도 준수하지만, 다른 방법에서 나오는 단점들을 가지고 있다. 위에서 언급된 Reflection 공격과 역직렬화 할 때 새로운 인스턴스가 생성되는 문제로 인해 번거로운 과정을 거쳐야 하는 점이 있다.

# 동시성 문제

## 개요

자바로 코드를 작성하다 보면 동시성 문제를 한 번쯤 고민하게 된다. 하지만 이는 간단히 해결할 수 있는 문제가 아니다. 동시성 관리가 어려운 이유는 다음과 같다.

1. **코드만 보고 동시성 문제의 발생 가능성을 파악하기 어렵다.**
2. **일반적으로 동시성으로 인해 발생하는 예외는 재현하기 어렵다.**
3. **최악의 경우, 동시성 문제가 발생해도 단순한 일회성 문제로 간주되어 무시될 수 있다.**

대부분의 경우 스레드를 신경 쓰지 않고 코드를 작성하지만, 동시성 문제는 치명적인 결과를 초래할 수 있기 때문에 반드시 이해하고 해결 방법을 숙지해야 한다.

---

## 간단한 동시성 문제 테스트

```java
private static long count = 0;

@Test
void threadNotSafe() throws Exception {
    int maxCnt = 10;

    for (int i = 0; i < maxCnt; i++) {
        new Thread(() -> {
            count++;
            System.out.println(count);
        }).start();
    }

    Thread.sleep(100); // 모든 스레드가 종료될 때까지 대기
    Assertions.assertThat(count).isEqualTo(maxCnt);
}
```

위 코드는 다수의 스레드가 공유 자원 `static long count`를 참조하여 값을 증가시키는 예제이다. 테스트는 대부분 통과하지만, `count++` 연산이 원자성을 보장하지 않기 때문에 아래와 같이 실패할 수 있다.

> **테스트 실패 결과**: `1`이 두 번 출력됨 (0 → 1 증가 연산에서 동시성 문제 발생)

---

## 동시성 문제 해결 방법

### 1. synchronized

`synchronized` 키워드는 특정 블록의 접근을 동기화하여, 하나의 스레드만 해당 블록에 접근할 수 있도록 보장한다.

#### 사용 예제

```java
public class SomeClass {
    // 메서드 전체 동기화
    public synchronized void foo() {
        /* critical section */
    }

    // 특정 블록만 동기화
    public void bar() {
        synchronized (this) {
            /* critical section */
        }
    }
}

// static 메서드 동기화
public class SomeClass {
    public static void syncMethod() {
        synchronized (SomeClass.class) {
            /* critical section */
        }
    }
}
```

#### synchronized 적용 후 테스트

```java
private static long count = 0;

@Test
void threadSafe() throws Exception {
    int maxCnt = 10;

    for (int i = 0; i < maxCnt; i++) {
        new Thread(this::plus).start();
    }

    Thread.sleep(100); // 모든 스레드 종료 대기
    Assertions.assertThat(count).isEqualTo(maxCnt);
}

public synchronized void plus() { // synchronized 사용
    count++;
}
```

`synchronized`를 추가하면 테스트가 100% 통과한다. 하지만, 동기화된 블록에서 다른 스레드가 대기해야 하므로 성능 저하 및 자원 낭비가 발생할 수 있다.

- **공유 자료를 최대한 줄여라.**
- **동기화하는 부분을 최대한 작게 만들어라.**
- **프로세서 수보다 많은 스레드를 실행해 보라.**
- **강제로 실패를 유발하여 테스트해 보라.**

---

### 2. volatile

`volatile` 키워드는 변수를 항상 메인 메모리에서 읽고 쓰도록 보장하여 **캐시 불일치 문제를 해결**한다. 하지만 원자성을 보장하지 않는다.

#### 사용 예제

```java
public volatile long count = 0;
```

#### volatile 적용 후 테스트

```java
private static volatile long count = 0;

@Test
void threadNotSafe() throws Exception {
    int maxCnt = 1000;

    for (int i = 0; i < maxCnt; i++) {
        new Thread(() -> count++).start();
    }

    Thread.sleep(100);
    Assertions.assertThat(count).isEqualTo(maxCnt);
}
```

![](https://user-images.githubusercontent.com/69145799/149614599-42e40249-c115-4bb4-ba6b-14066fd364bc.png)

> **동시성 문제 발생**: `volatile`은 원자성을 보장하지 않음

#### volatile 특징

1. **상호 배제(mutual exclusion)는 제공하지 않지만, 데이터 변경의 가시성을 보장한다.**
2. **원자적 연산에서만 동기화를 보장한다.**

---

### 3. Atomic 클래스

자바는 `synchronized`와 `volatile`을 대신할 수 있는 **Atomic 클래스**를 제공한다.

#### AtomicLong 사용 예제

```java
import java.util.concurrent.atomic.AtomicLong;

public class AtomicExample {
    private static AtomicLong count = new AtomicLong(0);

    @Test
    void threadSafeWithAtomic() throws Exception {
        int maxCnt = 1000;

        for (int i = 0; i < maxCnt; i++) {
            new Thread(() -> count.incrementAndGet()).start();
        }

        Thread.sleep(100);
        Assertions.assertThat(count.get()).isEqualTo(maxCnt);
    }
}
```

#### 내부 동작 원리

`AtomicLong`은 **CAS(Compare-and-Swap)** 알고리즘을 사용하여 원자적 연산을 보장한다.

```java
public final long getAndAddLong(Object o, long offset, long delta) {
    long v;
    do {
        v = getLongVolatile(o, offset);
    } while (!weakCompareAndSetLong(o, offset, v, v + delta));
    return v;
}
```

> **CAS 알고리즘**: 메모리 값과 현재 값을 비교하여 일치하면 변경, 불일치하면 재시도 (락 없이 동기화 가능)

---

## 마무리

`synchronized`는 간단한 해결책이지만, 성능 저하를 초래할 수 있다. `volatile`은 원자성을 보장하지 않는다. 따라서 동시성 문제가 예상되는 경우 **Atomic 클래스를 활용하는 것이 바람직하다.**

자바의 `java.util.concurrent` 패키지를 적극 활용하여, 동시성 문제를 효과적으로 해결하자! 🚀