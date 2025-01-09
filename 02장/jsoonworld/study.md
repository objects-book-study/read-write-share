#  객체지향 프로그래밍

# 01. 영화 예매 시스템

## 요구사항 살펴보기

이번 장에서 소개할 예제는 온라인 영화 예매 시스템이다. 사용자는 영화 예매 시스템을 이용해 쉽고 빠르게 보고 싶은 영화를 예매할 수 있다.

- 영화: 영화에 대한 기본 정보를 표현한다.
- 상영: 실제로 관객들이 영화를 관람하는 사건을 표현한다.

두 용어의 차이가 중요한 이유는 사용자가 실제로 예매하는 대상은 영화가 아니라 상영이기 때문이다.
(하나의 영화는 여러 번 상영될 수 있다.)

할인액을 결정하는 두 가지 규칙이 존재하는데, 하나는 할인 조건`discount condition` 이라고 부르고 다른 하나는 할인 정책`discount policy` 이라고 부른다.

- '할인 조건'은 가격의 할인 여부를 결정하며 '순서 조건'과 '기간 조건'의 두 종류로 나눌 수 있다.
	- '순서 조건`sequence condition'` 은 상영 순번을 이용해 할인 여부를 결정하는 규칙이다.
	- '기간 조건 `period condition`' 은 영화 상영 시작 시간을 이용해 할인 여부를 결정한다.

- '할인 정책'은 할인 요금을 결정한다. 할인 정책에는 '금액 할인 정책`amount discount policy` '과 '비율 할인 정책`percent discount policy` '이 있다.
	- '금액 할인 정책'은 예매 요금에서 일정 금액을 할인해주는 방식이며
	- '비율 할인 정책'은 정가에서 일정 비율의 요금을 할인해 주는 방식이다.

- 영화별로 하나의 할인 정책만 할당할 수 있다. 물론 할인 정책을 지정하지 않는 것도 가능하다.
- 이와 달리 할인 조건은 다수의 할인 조건을 함께 지정할 수 있으며, 순서 조건과 기간 조건을 섞는 것도 가능하다.

# 02. 객체지향 프로그래밍을 향해

## 협력, 객체, 클래스

첫째, 어떤 클래스가 필요한지를 고민하기 전에 어떤 객체들이 필요한지 고민하라. 클래스는 공통적인 상태와 행동을 공유하는 객체들을 추상화한 것이다. 따라서 클래스의 윤곽을 잡기 위해서는 어떤 객체들이 어떤 상태와 행동을 가지는지를 먼저 결정해야 한다. 객체를 중심에 두는 접근 방법은 설계를 단순하고 깔끔하게 만든다.

둘째, 객체를 독립적인 존재가 아니라 기능을 구현하기 위해 협력하는 공동체의 일원으로 봐야 한다. 객체는 홀로 존재하는 것이 아니다. 다른 객체에게 도움을 주거나 의존하면서 살아가는 협력적인 존재다. 객체를 협력하는 공동체의 일원으로 바라보는 것은 설계를 유연하고 확장 가능하게 만든다. 객체 지향적으로 생각하고 싶다면 객체를 고립된 존재로 바라보지 말고 협력에 참여하는 협력자로 바라보기 바란다. 객체들의 모양과 윤곽이 잡히면 공통된 특성과 상태를 가진 객체들을 타입으로 분류하고 이 타입을 기반으로 클래스를 구현하라. 훌륭한 협력이 훌륭한 객체를 낳고 훌륭한 클래스를 낳는다.

## 도메인의 구조를 따르는 프로그램 구조

이 시점에서 도메인`domain`이라는 용어를 살펴보는 것이 도움이 될 것이다. 소프트웨어는 사용자가 원하는 어떤 문제를 해결하기 위해 만들어진다. 영화 예매 시스템의 목적은 영화를 좀 더 쉽고 빠르게 예매하려는 사용자의 문제를 해결하는 것이다. 이처럼 문제를 해결하기 위해 사용자가 프로그램을 사용하는 분야를 도메인이라고 부른다.

## 클래스 구현하기

 ```java
 public class Screening {  
    private Movie movie;  
    private int sequence;  
    private LocalDateTime whenScreened;  
  
    public Screening(Movie movie, int sequence, LocalDateTime whenScreened) {  
        this.movie = movie;  
        this.sequence = sequence;  
        this.whenScreened = whenScreened;  
    }  
  
    public LocalDateTime getStartTime() {  
        return whenScreened;  
    }  
  
    public boolean isSequence(int sequence) {  
        return this.sequence == sequence;  
    }  
  
    public Money getMovieFee() {  
        return movie.getFee;  
    }  
}
```

클래스의 내부와 외부를 구분해야 하는 이유는 무엇일까? 그 이유는 경계의 명확성이 객체의 자율성을 보장하기 때문이다. 그리고 더 중요한 이유로 프로그래머에게 구현의 자유를 제공하기 때문이다.

### 자율적인 객체

먼저 두 가지 중요한 사실을 알아야 한다. 첫 번째 사실은 객체가 상태`state` 와 행동`behavior` 을 함께 가지는 복합적인 존재라는 것이다. 두 번째 사실은 객체가 스스로 판단하고 행동하는 자율적인 존재라는 것이다. 두 가지 사실은 서로 깊이 연관돼 있다.

데이터와 기능을 객체 내부로 함께 묶는 것을 캡슐화라고 부른다.
- 외부에서 접근을 통제할 수 있는 접근 제어`access control` 
- 접근 제어를 위해 `public`, `protected`, `private`과 같은 접근 수정자`access modifier`

객체 내부에 대한 접근을 통제하는 이유는 객체를 자율적인 존재로 만들기 위해서다. 객체지향의 핵심은 스스로 상태를 관리하고, 판단하고, 행동하는 자율적인 객체들의 공동체를 구성하는 것이다. 객체가 자율적인 존재로 우뚝 서기 위해서는 외부의 간섭을 최소화해야 한다.

캡슐화와 접근 제어는 객체를 두 부분으로 나눈다.
- 퍼블릭 인터페이스`public interface` 
- 구현`implemnetation` : 외부에서는 접근 불가능하고 오직 내부에서만 접근 가능한 부분

인터페이스와 구현의 분리`separation of interface implementation` 원칙은 훌륭한 객체지향 프로그램을 만들기 위해 따라야 하는 핵심 원칙이다.

### 프로그래머의 자유

클라이언트 프로그래머가 숨겨 놓은 부분에 마음대로 접근할 수 없도록 방지하므로써 클라이언트 프로그래머에 대한 영향을 걱정하지 않고도 내부 구현을 마음대로 변경할 수 있다. 이를 구현 은닉`implementation hiding` 이라고 부른다.

### 협력하는 객체들의 공동체

```java
public class Money {  
    public static final Money ZERO = Money.wons(0);  
  
    private final BigDecimal amount;  
  
    Money(BigDecimal amount) {  
        this.amount = amount;  
    }  
  
    public static Money wons(long amount) {  
        return new Money(BigDecimal.valueOf(amount));  
    }  
  
    public static Money wons(double amount) {  
        return new Money(BigDecimal.valueOf(amount));  
    }  
  
    public Money plus(Money amount) {  
        return new Money(this.amount.add(amount.amount));  
    }  
  
    public Money minus(Money amount) {  
        return new Money(this.amount.subtract(amount.amount));  
    }  
  
    public Money times(double percent) {  
        return new Money(this.amount.multiply(  
                BigDecimal.valueOf(percent)  
        ));  
    }  
  
    public boolean isLessThan(Money other) {  
        return amount.compareTo(other.amount) < 0;  
    }  
  
    public boolean isGreaterThanOrEqual(Money other) {  
        return amount.compareTo(other.amount) >= 0;  
    }  
  
  
}
```


```java
public class Reservation {  
    private Customer customer;  
    private Screening screening;  
    private Money fee;  
    private int audienceCount;  
  
    public Reservation(Customer customer, Screening screening, Money fee, int audienceCount) {  
        this.customer = customer;  
        this.screening = screening;  
        this.fee = fee;  
        this.audienceCount = audienceCount;  
    }  
}
```


### 협력에 관한 짧은 이야기

외부에 공개하는 퍼블릭 인터페이스를 통해 내부 상태에 접근할 수 있도록 허용한다. 객체는 다른 객체의 인터페이스에 공개된 행동을 수행하도록 요청`request` 할 수 있다. 요청을 받은 객체는 자율적인 방법에 따라 요청을 처리한 후 응답`response` 한다.

객체가 다른 객체와 상호작용할 수 있는 유일한 방법은 메시지를 전송`send a message` 하는 것뿐이다. 다른 객체에서 요청이 도착할 때 해당 객체가 메시지를 수신`receive a message` 했다고 이야기한다. 메시지를 수신한 객체는 스스로의 결정에 따라 자율적으로 메시지를 처리할 방법을 결정한다. 이처럼 수신된 메시지를 처리하기 위한 자신만의 방법을 메서드`method` 라고 부른다.

# 03. 할인 요금 구하기

## 할인 요금 계산을 위한 협력 시작하기


```java
public class Movie {  
    private String title;  
    private Duration runningTime;  
    private Money fee;  
    private DiscountPolicy discountPolicy;  
  
    public Movie(String title, Duration runningTime, Money fee, DiscountPolicy discountPolicy) {  
        this.title = title;  
        this.runningTime = runningTime;  
        this.fee = fee;  
        this.discountPolicy = discountPolicy;  
    }  
  
    public Money getFee() {  
        return fee;  
    }  
  
    public Money calculateMovieFee(Screening screening) {  
        return fee.minus(discountPolicy.calculateDiscountAmount(screening));  
    }  
}
```

 이 코드에서는 객체지향에서 중요하다고 여겨지는 두 가지 개념이 숨겨져 있다. 하나는 상속`inheritance` 이고 다른 하나는 다형성이다. 그리고 그 기반에는 추상화`abstraction` 라는 원리가 숨겨져 있다.

## 할인 정책과 할인 조건

할인 정책은 금액 할인 정책과 비율 할인 정책으로 구분된다. 두 가지 할인 정책을 각각 `AmountDiscountPolicy`와 `PercentDiscountPolicy`라는 클래스로 구현할 것이다.
두 클래스는 대부분의 코드가 유사하고 할인 요금을 계산하는 방식만 조금 다르다.

여기서는 부모 클래스인 `DiscountPolicy` 안에 중복 코드를 두고 `AmountDiscountPolicy`와 `PercentDiscountPolicy`가 이 클래스를 상속받게 할 것이다.

```java
public abstract class DiscountPolicy {  
    private List<DiscountCondition> conditions = new ArrayList<>();  
  
    public DiscountPolicy(DiscountCondition ... conditions) {  
        this.conditions = Arrays.asList(conditions);  
    }  
  
    public Money calculateDiscountAmount(Screening screening) {  
        for (DiscountCondition each : conditions) {  
            if (each.isSatisfiedBy(screening)) {  
                return getDiscountAmount(screening);  
            }  
        }  
  
        return Money.ZERO;  
    }  
    abstract protected Money getDiscountAmount(Screening screening);  
}
```

`DiscountPolicy`는 할인 여부와 요금 계산에 필요한 전체적인 흐름은 정의하지만 실제로 요금을 계산 하는 부분은 추상 메서드인 `getDiscountAmount` 메서드에 위임한다. 실제로는 `DiscountPolicy`를 상속 받은 자식 클래스에서 오버라이딩한 메서드가 실행될 것이다. 이처럼 부모 클래스에 기본적인 알고리즘의 흐름을 구현하고 중간에 필요한 처리를 자식 클래스에게 위임하는 디자인 패턴을 `TEMPLATE METHOD PATTERN`이라고 부른다.

```java
public interface DiscountCondition {  
    boolean isSatisfiedBy(Screening screening);  
}
```


```java
public class SequenceCondition implements DiscountCondition {  
    private int sequence;  
  
    public SequenceCondition(int sequence) {  
        this.sequence = sequence;  
    }  
  
    @Override  
    public boolean isSatisfiedBy(Screening screening) {  
        return screening.isSequence(sequence);  
    }  
}
```

```java
public class AmountDiscountPolicy extends DiscountPolicy {  
    private Money discountAmount;  
  
    public AmountDiscountPolicy(Money discountAmount, DiscountCondition... conditions) {  
        super(conditions);  
        this.discountAmount = discountAmount;  
    }  
  
    @Override  
    protected Money getDiscountAmount(Screening screening) {  
        return discountAmount;  
    }  
}
```

```java
public class PercentDiscountPolicy extends DiscountPolicy {  
    protected double percent;  
  
    public PercentDiscountPolicy(double percent, DiscountCondition... conditions) {  
        super(conditions);  
        this.percent = percent;  
    }  
  
    @Override  
    protected Money getDiscountAmount(Screening screening) {  
        return screening.getMovieFee().times(percent);  
    }  
}
```

## 할인 정책 구성하기


# 04. 상속과 다형성

## 컴파일 시간 의존성과 실행 시간 의존성

`Movie` 의 인스턴스는 실행 시에  `AmountDiscountPolicy`나 `PercentDiscountPolicy`의 인스턴스에 의존해야 한다. 하지만 코드 수준에서 Movie 클래스는 이 두 클래스 중 어떤 것에도 의존하지 않는다. 오직 추상 클래스인 `DiscountPolicy`에만 의존하고 있다.

그렇다면 `Movie`의 인스턴스가 코드 작성 시점에는 그 존재조차 알지 못했던 `AmountDiscountPolicy`와 `PercentDiscountPolicy`의 인스턴스와 실행 시점에 협력 가능한 이유는 무엇일까?

```java
Movie avatar = new Movie("아바타", Duration.ofMinute(120), Money.wons(10000),
						new AmountDiscountPolicy(Money.wons(800)));
```

이제 실행 시에 `Movie`의 인스턴스는 `AmountDiscountPolicy` 클래스의 인스턴스에 의존하게 될 것이다.

여기서 이야기하고 싶은 것은 코드의 의존성과 실행 시점의 의존성이 서로 다를 수 있다는 것이다. 다시 말해 클래스 사이의 의존성과 객체 사이의 의존성을 동일하지 않을 수 있다. 그리고 유연하게, 쉡게 재사용할 수 있으며, 확장 가능한 객체지향 설계가 가지는 특징은 코드의 의존성과 실행 시점의 의존성이 다르다는 것이다.

한 가지 간과해서는 안 되는 사실은 코드의 의존성과 실행 시점의 의존성이 다르다면 다를수록 코드를 이해하기 어려워진다는 것이다. 코드를 이해하기 위해서는 코드뿐만 아니라 객체를 생성하고 연결하는 부분을 찾아야 하기 때문이다. 반면 코드의 의존성과 실행 시점의 의존성이 다르면 다를수록 코드는 더 유연해지고 확장 가능해진다. 이와 같은 의존성의 양면성은 설계가 트레이드오프의 산물이라는 사실을 잘 보여준다.

## 차이에 의한 프로그래밍

클래스 하나를 추가하고 싶은데 그 클래스가 기존의 어떤 클래스와 매우 흡사하다고 가정해보자. 그 클래스의 코드를 가져와 약간만 추가하거나 수정해서 새로운 클래스를 만들 수 있다면 좋을 것이다. 더 좋은 방법은 그 클래스의 코드를 전혀 수정하지 않고도 재사용하는 것일 것이다. 이를 가능하게 해주는 방법이 바로 상속이다.

이처럼 부모 클래스와 다른 부분만을 추가해서 새로운 클래스를 쉽고 빠르게 만드는 방법을 차이에 의한 프로그래밍`programming by difference` 이라고 부른다.

## 상속과 인터페이스

자식 클래스가 부모 클래스를 대신하는 것을 업캐스팅`upcasting`이라고 부른다. 업캐스팅이라고 부르는 이유는 일반적으로 클래스 다이어그램을 작성할 때 부모 클래스를 자식 클래스의 위에 위치시키기 때문이다.

## 다형성

다시 한번 강조하지만 메시지와 메서드는 다른 개념이다.
`Movie`는 동일한 메시지를 전송하지만 실제로 어떤 메서드가 실행될 것인지는 메시지를 수신하는 객체의 클래스가 무엇이냐에 따라 달라진다. 이를 다형성이라고 부른다.

- 다형성은 객체지향 프로그램의 컴파일 시간 의존성과 실행 시간 의존성이 다를 수 있다는 사실을 기반으로 한다.
- 다형성이란 동일한 메시지를 수신했을 때 객체의 타입에 따라 다르게 응답할 수 있는 능력을 의미한다.
- 따라서 다형적인 협력에 참여하는 객체들은 모두 같은 메시지를 이해할 수 있어야 한다. 다시 말해 인터페이스가 동일해야 한다는 것이다.

다형성을 구현하는 방법은 매우 다양하지만 메시지에 응답하기 위해 실행될 메서드를 컴파일 시점이 아닌 실행 시점에 결정한다는 공통점이 있다. 다시 말해 메시지와 메서드를 실행 시점에 바인딩한다는 것이다. 이를 지연 바인딩`lazy binding` 또는 동적 바인딩`dynamic binding` 이라고 부른다. 그에 반해 전통적인 함수 호출처럼 컴파일 시점에 실행될 함수나 프로시저를 결정하는 것을 초기 바인딩`early binding` 또는 정적 바인딩`static binding` 이라고 부른다. 객체지향이 컴파일 시점의 의존성과 실행 시점의 의존성을 분리하고, 하나의 메시지를 선택적으로 서로 다른 메서드에 연결할 수 있는 이유가 바로 지연 바인딩이라는 매커니즘을 사용하기 때문이다.

## 인터페이스와 다형성

# 05. 추상화와 유연성

## 추상화의 힘

추상화를 사용할 경우의 두 가지 장점이 있다. 첫 번째 장점은 추상화 계층만 따로 떼어 놓고 살펴보면 요구사항의 정책을 높은 수준에서 서술할 수 있다는 것이다. 두 번째 장점은 추상화를 이용하게 설계가 좀 더 유연해진다는 것이다. 

추상화를 사용하면 세부적인 내용을 무시한 채 상위 정책을 쉽고 간단하게 표현할 수 있다.
추상화를 이용해 상위 정책을 기술한다는 것은 기본적인 애플리케이션의 협력 흐름을 기술하는 것을 의미한다.
재사용 가능한 설계의 기본을 이루는 디자인 패턴`design pattern`이나 프레임워크`framework` 모두 추상화를 이용해 상위 정책을 정의하는 객체지향의 매커니즘을 활용하고 있다.
추상화를 이용해 상위 정책을 표현하면 기존 구조를 수정하지 않고도 새로운 기능을 쉽게 추가하고 확장할 수 있다. 다시 말해 설계를 유연하게 만들 수 있다.

## 유연한 설계

```java
public class NoneDiscountPolicy extends DiscountPolicy{  
    @Override  
    protected Money getDiscountAmount(Screening screening) {  
        return Money.ZERO;  
    }  
}
```

기존 할인 정책의 경우에는 할인할 금액을 계산하는 책임이 `DiscountPolcy`의 자식 클래스에 있었지만 할인 정책이 없는 경우에는 할인 금액이 0원이라는 사실을 결정하는 책임이 `DiscountPolicy`가 아닌 `Movie` 쪽에 있었다. 따라서 `DiscountPolicy` 계층에 0원이라는 할인 요금을 계산할 책임을 부여하자.

중요한 것은 기존의 `Movie`와 `DiscountPolicy`는 수정하지 않고 `NoneDiscountPolicy`라는 새로운 클래스를 추가하는 것만으로 애플리케이션의 기능을 확장했다는 것이다. 이처럼 추상화를 중심으로 코드의 구조를 설계하면 유연하고 확장 가능한 설계를 만들 수 있다.

## 추상 클래스와 인터페이스 트레이드오프

```java
public interface DiscountPolicy {  
    Money calculateDiscountAmount(Screening screening);  
}
```


```java
public abstract class DefaultDiscountPolicy implements DiscountPolicy {  
    private List<DiscountCondition> conditions = new ArrayList<>();  
  
    public DefaultDiscountPolicy(DiscountCondition ... conditions) {  
        this.conditions = Arrays.asList(conditions);  
    }  
  
    public Money calculateDiscountAmount(Screening screening) {  
        for (DiscountCondition each : conditions) {  
            if (each.isSatisfiedBy(screening)) {  
                return getDiscountAmount(screening);  
            }  
        }  
  
        return Money.ZERO;  
    }  
    abstract protected Money getDiscountAmount(Screening screening);  
}
```

```java
public class NoneDiscountPolicy implements DiscountPolicy {  
    @Override  
    public Money calculateDiscountAmount(Screening screening) {  
        return Money.ZERO;  
    }  
}
```


## 코드 재사용

객체지향 설계와 관련된 자료를 조금이라도 본 사람들은 코드 재사용을 위해서는 상속보다는 합성`composition`이 더 좋은 방법이라는 이야기를 많이 들었을 것이다. 합성은 다른 객체의 인스턴스를 자신의 인스턴스 변수로 포함해서 재사용하는 방법을 말한다.

## 상속

상속은 객체지향에서 코드를 재사용하기 위해 널리 사용되는 기법이다. 하지만 두 가지 고나점에서 설계에 안 좋은 영향을 미친다. 하나는 상속이 캡슐화를 위반한다는 것이고, 다른 하나는 설계를 유연하게 하지 못하게 만든다는 것이다.

상속의 가장 큰 문제점은 캡슐화를 위반한다는 것이다. 상속을 이용하기 위해서는 부모 클래스의 내부 구조를 잘 알고 있어야 한다.

두 번째 단점은 설계가 유연하지 못하다는 것이다. 상속은 부모 클래스와 자식 클래스 사이의 관계를 컴파일 시점에 결정한다. 따라서 실행 시점에 객체의 종류를 변경하는 것이 불가능하다.

```java
public class Movie{
	private DiscountPolicy discountPolicy;
	
	public void changeDiscountPolicy(DiscountPolicy discountPolicy) {  
	    this.discountPolicy = discountPolicy;  
	}
}
```

금액 할인 정책이 적용된 영화에 비율 할인 정책이 적용되도록 변경하는 것은 새로운 `DiscountPolicy` 인스턴스를 연결하는 간단한 작업으로 바뀐다.

## 합성

언터페이스에 정의된 메시지를 통해서만 코드를 재사용하는 방법을 합성이라고 부른다.
합성은 상속이 가지는 두 가지 문제점을 모두 해결한다. 인터페이스에 정의된 메시지를 통해서만 재사용이 가능하기 때문에 구현을 효과적으로 캡슐화할 수 있다. 또한 의존하는 인스턴스를 교체하는 것은 비교적 쉽기 때문에 설계를 유연하게 만든다. 상속은 클래스를 통해 강하게 결합되는 데 비해 합성은 메시지를 통해 느슨하게 결합된다. 따라서 코드 재사용을 위해서는 상속보다는 합성을 선호하는 것이 더 좋은 방법이다.

그렇다고 해서 상속을 절대 사용하지 말라는 것은 아니다. 코드를 재사용하는 경우에는 상속보다 합성을 선호하는 것이 옳지만 다형성을 위해 인터페이스를 재사용하는 경우에는 상속과 합성을 함께 조합해서 사용할 수 밖에 없다.

