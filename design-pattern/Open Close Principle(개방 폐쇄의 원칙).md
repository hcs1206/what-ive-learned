# Open/Closed Principle (개방-폐쇄 원칙)

## 개요

소프트웨어 개발에서는 요구사항이 지속적으로 변경된다. 이런 변화에 유연하게 대응하지 못하면 유지보수 비용이 증가하고 코드의 안정성이 떨어진다. 이러한 문제를 해결하는 원칙 중 하나가 바로 개방-폐쇄 원칙(Open/Closed Principle, OCP)이다.

OCP는 **"확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다"**는 원칙이다. 즉, 기존 코드를 수정하지 않고도 새로운 기능을 추가할 수 있도록 설계해야 한다. 본 문서에서는 OCP의 개념과 중요성, 위반 사례 및 해결 방법을 살펴본다.

---

## 개방-폐쇄 원칙이란?

OCP는 객체지향 프로그래밍(OOP)에서 중요한 원칙으로, **기존 코드의 변경 없이 기능을 확장할 수 있도록 설계해야 한다**는 의미를 가진다.

- "확장에는 열려 있어야 한다" → 새로운 기능이 추가될 수 있어야 한다.
- "변경에는 닫혀 있어야 한다" → 기존 코드를 수정하지 않고도 기능을 확장할 수 있어야 한다.

이 원칙을 잘 지키면 코드의 유지보수성과 확장성이 크게 향상된다.

---

## OCP 위반 사례

### 예제: 결제 시스템의 `PaymentProcessor` 클래스

```java
public class PaymentProcessor {
    public void processPayment(String paymentType) {
        if (paymentType.equals("credit")) {
            System.out.println("Processing credit card payment");
        } else if (paymentType.equals("paypal")) {
            System.out.println("Processing PayPal payment");
        } else {
            throw new IllegalArgumentException("Unsupported payment type");
        }
    }
}
```

### 문제점
1. 새로운 결제 방식(예: `Bitcoin`)이 추가될 때마다 `processPayment` 메서드를 수정해야 한다.
2. 기존 코드에 영향을 주기 때문에 **변경할 때 오류가 발생할 가능성이 높다**.
3. OCP 원칙을 위반하여 **유지보수가 어렵고 확장성이 낮다**.

---

## OCP 적용 방법

### 1. 다형성을 활용한 책임 분리
OCP를 적용하려면 기존 코드를 수정하지 않고 새로운 기능을 추가할 수 있어야 한다. 이를 위해 **인터페이스와 다형성(Polymorphism)** 을 활용할 수 있다.

#### **책임 분리 후의 클래스 설계**

```java
// 결제 처리 인터페이스
public interface PaymentMethod {
    void processPayment();
}

// 신용카드 결제
public class CreditCardPayment implements PaymentMethod {
    public void processPayment() {
        System.out.println("Processing credit card payment");
    }
}

// PayPal 결제
public class PayPalPayment implements PaymentMethod {
    public void processPayment() {
        System.out.println("Processing PayPal payment");
    }
}

// 새로운 결제 방식 추가(Bitcoin)
public class BitcoinPayment implements PaymentMethod {
    public void processPayment() {
        System.out.println("Processing Bitcoin payment");
    }
}

// 결제 처리기
public class PaymentProcessor {
    public void process(PaymentMethod paymentMethod) {
        paymentMethod.processPayment();
    }
}
```

### 개선된 설계의 장점
✅ 새로운 결제 방식이 추가될 때 `PaymentProcessor`를 변경할 필요가 없다.
✅ 기존 코드의 변경 없이 확장이 가능하므로 **OCP를 준수**한다.
✅ 코드의 **유지보수성이 향상**되고 **유연성이 증가**한다.

---

## OCP 위반의 문제점

OCP를 위반하면 다음과 같은 문제가 발생한다:

1. **코드 수정이 빈번하게 발생** → 새로운 기능 추가 시 기존 클래스를 변경해야 하므로 유지보수 비용이 증가한다.
2. **오류 발생 가능성 증가** → 기존 코드 수정 시 의도치 않은 버그가 발생할 확률이 높다.
3. **확장성 저하** → 코드가 특정 기능에 종속되어 유연한 변경이 어렵다.

---

## 결론

OCP(Open/Closed Principle)는 유지보수성과 확장성을 고려한 객체지향 설계의 중요한 원칙이다. 기존 코드를 변경하지 않고 새로운 기능을 추가할 수 있도록 설계하는 것이 핵심이다.

### OCP 적용의 주요 이점:
✔ **코드 안정성 증가** → 기존 코드 수정 없이 새로운 기능 추가 가능
✔ **유지보수성 향상** → 코드 변경으로 인한 버그 발생 가능성 감소
✔ **확장성 증가** → 다형성을 활용하여 손쉽게 기능 추가 가능

결국 OCP를 지키는 것은 **소프트웨어 품질을 높이고, 변경에 유연하게 대응할 수 있는 설계를 구축하는 핵심 요소**이다.

