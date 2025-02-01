# Chapter 06: 메시지와 인터페이스

훌륭한 객체지향 코드를 얻기 위해서는 클래스가 아닌 객체를 지향해야 한다.

- **협력 안에서** 객체가 수행하는 `책임`에 초점을 맞춰야 한다.
- `책임`은 **객체가 수신할 수 있는 메시지의 기반**이 된다.

---

# 1. 협력과 메시지

## 클라이언트-서버 모델

협력의 관점에서 객체는 **두 가지 종류의 메시지 집합**으로 구성된다.

- 객체가 `수신`하는 메시지의 집합
- 외부의 객체에게 `전송`하는 메시지의 집합

대부분 **객체가 수신하는 메시지의 집합**에만 초점을 맞추지만, 협력에 적합한 객체를 설계하기 위해서는 **외부에 전송하는 메시지의 집합도** 함께 고려하는 것이 바람직하다.

👉🏻 객체가 더 큰 책임을 수행하기 위해서는 다른 객체와 협력해야 한다.

## 메시지와 메시지 전송

- **메시지**
  - 오퍼레이션 이름, 인자
  - ex) `isSatisfiedBy(screening)`
- **메시지 전송**
  - 오퍼레이션 이름, 인자, 메시지 수신자
  - ex) `condition.isSatisfiedBy(screening)`

## 메시지와 메서드

메시지를 수신했을 때 실제로 실행되는 함수 또는 프로시저를 메서드라고 부른다.

> 코드 상에서 동일한 이름의 변수(`condition`)에게 동일한 메시지를 전송하더라도, **객체의 타입에 따라 실행되는 메서드가 달라질 수 있다.** _(= 컴파일 시점과 실행 시점의 의미가 달라질 수 있음)_

메시지와 메서드의 구분은 `메시지 전송자`와 `메시지 수신자`가 **느슨하게 결합**될 수 있도록 한다.

- 메시지 전송자는 자신이 **어떤 메시지를 전송해야 하는지만** 알면 됨

## 퍼블릭 인터페이스와 오퍼레이션

- **퍼블릭 인터페이스**
  - 객체가 의사소통을 위해 외부에 공개하는 메시지의 집합
- **오퍼레이션**
  - 퍼블릭 인터페이스에 포함된 메시지
  - 흔히 내부 구현 코드는 제외하고, 단순히 **메시지와 관련된 시그니처**를 가리키는 경우가 많음
  - ex) `isSatisfiedBy`

```
- 오퍼레이션은 구현이 아닌 추상화
- 메서드는 오퍼레이션을 구현한 것
```

## 시그니처

> **시그니처**  
> **오퍼레이션(or 메서드)의 이름**과 **파라미터 목록**을 합쳐서 시그니처라고 한다

👉🏻 `다형성`의 축복을 받기 위해서는 **하나의 오퍼레이션에 대해 다양한 메서드를 구현**해야만 한다.

```
[ 용어 정리 ]

- 메시지
    - 객체가 다른 객체와 협력하기 위해 사용하는 의사소통 매커니즘
    - 협력에 참여하는 전송자와 수신자 양쪽 모두를 포함하는 개념

- 오퍼레이션
    - 객체가 다른 객체에게 제공하는 추상적인 서비스
    - 메시지를 수신하는 객체의 인터페이스를 강조
        - 즉, 메시지와 달리 메시지 전송자는 고려하지 않고, 메시지 수신자의 관점만을 다룸

- 메서드
    - 메시지에 응답하기 위해 실행되는 코드 블록
    - 오퍼레이션과 메서드의 구분은 `다형성` 개념과 연결됨

- 퍼블릭 인터페이스
    - 객체가 협력에 참여하기 위해 외부에서 수신할 수 있는 메시지의 묶음
    - 객체를 설계할 때 가장 중요한 것은, 훌륭한 퍼블릭 인터페이스를 설계하는 것

- 시그니처
    - 오퍼레이션이나 메서드의 명세를 나타낸 것으로, 이름과 인자의 목록을 포함
```

중요한 것은 객체가 수신할 수 있는 **메시지**가 객체의 **퍼블릭 인터페이스**와 그 안에 포함될 **오퍼레이션을 결정한다**는 것이다.

- 👉🏻 결국 `메시지`가 **객체의 품질을 결정한다**고 볼 수 있음

---

# 2. 인터페이스와 설계 품질

좋은 인터페이스는 **최소한의 인터페이스**와 **추상적인 인터페이스**라는 조건을 만족해야 한다.

⭐️ **추상적인 인터페이스**는 어떻게 수행하는지가 아닌, `무엇을 하는지`를 표현한다.

여기서 **퍼블릭 인터페이스의 품질**에 영향을 미치는 다음 원칙과 기법에 관해 살펴본다.

- 디미터 법칙
- 묻지 말고 시켜라
- 의도를 드러내는 인터페이스
- 명령-쿼리 분리

## 디미터 법칙

아래 코드는 4장에서 **절차적인 방식**의 영화 예매 시스템 코드 중, 할인 가능 여부를 체크하는 코드이다.

```java
public class ReservationAgency {
    public Reservation reserve(Screening screening, Customer customer, int audienceCount) {
        Movie movie = screening.getMovie();

        boolean discountable = false;
        for (DiscountCondition condition : movie.getDiscountConditions()) {
            if (condition.getType() == DiscountConditionType.PERIOD) {
                discountable = screening.getWhenScreened().getDayOfWeek().equals(condition.getDayOfWeek()) &&
                        condition.getStartTime().compareTo(screening.getWhenScreened().toLocalTime()) <= 0 &&
                        condition.getEndTime().compareTo(screening.getWhenScreened().toLocalTime()) >= 0;
            } else {
                discountable = condition.getSequence() == screening.getSequence();
            }

            if (discountable) {
                break;
            }
        }

        Money fee;
        if (discountable) {
            Money discountAmount = Money.ZERO;
            switch (movie.getMovieType()) {
                case AMOUNT_DISCOUNT:
                    discountAmount = movie.getDiscountAmount();
                    break;
                case PERCENT_DISCOUNT:
                    discountAmount = movie.getFee().times(movie.getDiscountPercent());
                    break;
                case NONE_DISCOUNT:
                    discountAmount = Money.ZERO;
                    break;
            }
            fee = movie.getFee().minus(discountAmount);
        } else {
            fee = movie.getFee();
        }

        return new Reservation(customer, screening, fee, audienceCount);
    }
}
```

이 코드는 `ReservationAgency`와 `Screening` 사이의 결합도가 매우 높기 때문에, `Screening`의 내부 구현을 변경할 때마다 `ReservationAgency`도 함께 변경된다.

- 👉🏻 원인은 `ReservationAgency`가 `Screening` 뿐만 아니라, **`Screening` 내부에 있는** `Movie`와 `DiscountCondition`에도 **직접 접근**하기 때문

> **디미터 법칙**
>
> 이처럼 **협력하는 객체의 내부 구조에 대한 결합**으로 인해 발생하는 설계 문제를 해결하기 위해 제안된 원칙  
> 👉🏻 즉, **객체의 내부 구조**에 강하게 결합되지 않도록 **협력 경로를 제한하라**는 것

디미터 법칙을 따르기 위해서는 클래스가 특정한 조건을 만족하는 대상에게만 메시지를 전송해야 한다.

모든 `클래스 C`와 C에 구현된 모든 `메서드 M`에 대해서, **M이 메시지를 전송할 수 있는 모든 객체**는 다음에 서술된 클래스의 인스턴스여야 한다.

- M의 인자로 전달된 클래스 _(C 자체 `this` 를 포함)_
- C의 인스턴스 변수의 클래스

```
위의 내용이 이해하기 어렵다면, 클래스 내부 메서드가 아래 조건을 만족하는 인스턴스에만 메시지를 전송하도록 프로그래밍해야 한다고 이해하면 된다.

- this 객체
- 메서드의 매개변수
- this의 속성
- this의 속성인 컬렉션의 요소
- 메서드 내에서 생성된 지역 객체
```

4장에서 결합도 문제를 해결하기 위해 수정한 `ReservationAgency` 최종 코드는 다음과 같다.

```java
public class ReservationAgency {
    public Reservation reserve(Screening screening, Customer customer, int audienceCount) {
        Money fee = screening.calculateFee(audienceCount);
        return new Reservation(customer, screening, fee, audienceCount);
    }
}
```

`ReservationAgency`이 `Screening`의 내부 구조에 결합돼 있지 않기 때문에, `Screening`의 내부 구현을 변경할 때 `ReservationAgency`를 함께 변경할 필요가 없다.

디미터 법칙을 따르면

- 불필요한 어떤 것도 다른 객체에게 보여주지 않으며,
- 다른 객체의 구현에 의존하지 않는

코드를 작성할 수 있다.

전형적인 디미터 법칙을 위반하는 코드는 **기차 충돌** 형태를 보인다.

```md
👉🏻 디미터 법칙은 객체의 내부 구조를 묻는 메시지가 아니라, `수신자에게 무언가를 시키는 메시지`가 더 좋은 메시지라고 속삭인다.
```

## 묻지 말고 시켜라

**객체의 외부**에서 **해당 `객체의 상태`를 기반으로 결정을 내리는 것**은 객체의 캡슐화를 위반한다.

**`내부의 상태`를 이용해 어떤 결정을 내리는 로직**이 **객체 외부에 존재**한다면, 해당 객체가 책임져야 하는 어떤 행동이 객체 외부로 누수된 것이다.

- 👉🏻 _상태를 묻는 오퍼레이션_ 을 `행동을 요청하는 오퍼레이션`으로 대체함으로써, 인터페이스를 향상시켜야 한다.

```
객체는 자신이 내부적으로 보유하고 있는 정보나 메시지 전송의 결과로 얻게 되는 정보만 사용해서 의사결정을 내리게 된다.
```

훌륭한 인터페이스를 수확하기 위해서는 _객체가 어떻게 작업을 수행하는지_ 노출해서는 안 된다.

## 의도를 드러내는 인터페이스

메서드 이름에 내부 구현 방법을 드러내서는 안 된다.

- 가령, `isSatisfiedByPeriod`, `isSatisfiedBySequence`는 클라이언트로 하여금 **협력하는 객체의 종류를 알도록 강요**한다.

메서드 이름에는 '어떻게'가 아닌 **`'무엇'`을 하는지** 드러내는 것이 좋다.

- 이는 외부 객체가 메시지를 전송하는 `목적`을 먼저 생각하도록 만든다.

`isSatisfiedByPeriod`, `isSatisfiedBySequence`는 클라이언트 관점에서 **할인 여부를 판단하기 위한** 작업을 수행한다.

따라서 두 메서드 모두 클라이언트의 의도를 담을 수 있도록 `isSatisfiedBy`로 변경하는 것이 적절하다.

이렇게 두 메서드는 **동일한 메시지**를 서로 다른 방법으로 처리하기 때문에, 서로 대체 가능하다. _(with 추상화)_

```
👉🏻 이처럼 무엇을 하느냐에 초점을 맞추면, 클라이언트의 관점에서 동일한 작업을 수행하는 메서드들을 하나의 타입 계층으로 묶을 수 있는 가능성이 커진다.
```

## 함께 모으기

### 디미터 법칙을 위반하는 티켓 판매 도메인

[디미터 법칙을 위반한 Theater의 enter 메서드](https://github.com/objects-book-study/practice-object-book/commit/4f1e9c6798e3f25695b88c1295592e2b823a93f8#diff-73ec35d4fbca4ed96bf7864f06ff0d49bd607186aee3dc2ce82aa2600ade5f1d)

```java
audience.getBag().minusAmount(ticket.getFee());
```

> 여기서 문제는 `Theater`가 `audience`와 `ticketSeller` **내부에 포함된 객체(= `Ticket`)에도 직접 접근한다는 것**이다.

근본적으로 디미터 법칙을 위반하는 설계는 **인터페이스와 구현의 분리 원칙을 위반**한다.

👉🏻 디미터 법칙을 위반한 코드를 수정하는 일반적인 방법은 `Audience`와 `TicketSeller`의 **내부 구조를 묻는 대신** `Audience`와 `TicketSeller`가 <U>**직접 자신의 책임을 수행하도록 시키는 것**</U>이다.

### 묻지 말고 시켜라

- `Theater`가 작업에 필요한 데이터를 묻지 말고, `TicketSeller`에게 시키도록 코드를 변경한다.

  - [`Theater`와 `TicketSeller` 사이 디미터 법칙 위반 해결](https://github.com/objects-book-study/practice-object-book/commit/a595920aa6d7c415d565a186dc648fbdc279a850)

하지만 `TicketSeller` 내부에서 여전히 디미터 법칙이 위배되는 것을 확인할 수 있다.

```java
if (audience.getBag().hasInvitation()) {
    // ...
    audience.getBag().setTicket(ticket);
}
```

- `TicketSeller`가 작업에 필요한 데이터를 묻지 말고, `Audience`에게 시키도록 코드를 변경한다.

  - [`TicketSeller`와 `Audience` 사이 디미터 법칙 위반 해결](https://github.com/objects-book-study/practice-object-book/commit/799b31342a68850cfe63b1d1402c89595ac87d84)

마찬가지로 `Audience`에서는 `Bag`에게 원하는 일을 시키기 전에, `hasInvitation` 메서드를 이용해 초대권을 가지고 있는지 묻는다는 사실을 알 수 있다. _(bag의 상태를 확인하고 내부 상태를 변경하고 있음)_

```java
if (bag.hasInvitation()) {
    bag.setTicket(ticket);
    // ...
}
```

- `Audience`가 작업에 필요한 데이터를 묻지 않고, `Bag`에게 시키도록 코드를 변경한다.
  - [`Audience`와 `Bag` 사이 디미터 법칙 위반 해결](https://github.com/objects-book-study/practice-object-book/commit/1fc057bd0f450d4e8d6c6fed64706b52e3b5fb5a)

```md
`디미터 법칙`과 `묻지 말고 시켜라 스타일`을 따르면, 자연스럽게 자율적인 객체로 구성된 유연한 협력을 얻게 된다.
```

### 인터페이스에 의도를 드러내자

`디미터 법칙`과 `묻지 말고 시켜라 스타일`을 따르는 인터페이스를 얻었다면 _(= 자율적인 객체로 구성된 협력을 얻었다면)_ , 인터페이스가 `클라이언트의 의도`를 올바르게 반영했는지를 확인해야 한다.

앞선 코드에서는 각 인터페이스가 객체의 의도를 드러낼 뿐, 해당 인터페이스를 호출하는 클라이언트의 의도를 담아내지 못하고 있다.

[클라이언트의 의도 드러내기](https://github.com/objects-book-study/practice-object-book/commit/dea7acde2e8cce45eee337abdf117cbf7cd4707d)

오퍼레이션의 이름은 협력이라는 문맥을 반영해야 한다.

- 즉, 객체 자신이 아닌 `클라이언트의 의도`를 표현하는 이름을 가져야 한다.

---

# 3. 원칙의 함정

원칙이 현재 상황에 부적합하다고 판단된다면, 과감하게 원칙을 무시할 줄도 알아야 한다.

## 디미터 법칙은 하나의 도트(.)를 강제하는 규칙이 아니다

```java
IntStream.of(1, 15, 20, 3, 9).filter(x -> x > 10).distinct().count();
```

위 코드가 **기차 충돌**을 초래하기 때문에 디미터 법칙을 위반한다고 생각할 수 있다.

- 👉🏻 디미터 법칙을 잘못 이해한 것
  - 디미터 법칙은 `결합도`와 관련된 것으로, 내부 객체(= 세부 사항)를 과도하게 참조하는 경우 발생한다

위 코드에서 `of`, `filter`, `distinct` 메서드는 모두 `IntStream` 이라는 **동일한 클래스의 인스턴스**를 반환한다.

디미터 법칙은 `객체의 내부 구조`가 **외부로 노출**되어 결합도 측면에서 문제가 발생하는 경우를 말한다.  
하지만 위의 코드에서는 `IntStream`의 내부 구조가 외부로 노출되었다고 볼 수 없다.

## 결합도와 응집도의 충돌

`묻지 말고 시켜라`와 `디미터 법칙`을 준수하는 것이 **항상 긍정적인 결과로만 귀결되는 것은 아니다.**

모든 상황에서 맹목적으로 위임 메서드를 추가하면, 같은 퍼블릭 인터페이스 안에 어울리지 않는 오퍼레이션들이 공존하게 되기 때문이다. _(= 응집도 낮아짐)_

```java
public class PeriodCondition implements DiscountCondition {
    public boolean isSatisfiedBy(Screening screening) {
        return screening.getStartTime().getDayOfWeek().equals(dayOfWeek) &&
        startTime.compareTo(screening.getStartTime().toLocalTime()) <= 0 &&
        endTime.compareTo(screening.getStartTime().toLocalTime()) >= 0;
    }
}
```

위 코드를 얼핏 보면 `Screening` 내부 상태를 가져와서 사용하기 때문에, 캡슐화를 위반한 것으로 보일 수 있다.

따라서 할인 여부를 판단하는 로직을 `Screening`의 `isDiscountable` 메서드로 옮기고, `PeriodCondition`이 이 메서드를 호출하도록 변경한다면, 묻지 말고 시켜라 스타일을 준수하는 퍼블릭 인터페이스를 얻을 수 있다고 생각할지도 모른다.

```java
public class Screening {
    public boolean isDiscountable(DayOfWeek dayOfWeek, LocalTime startTime, LocalTime endTime) {
        return screening.getStartTime().getDayOfWeek().equals(dayOfWeek) &&
        startTime.compareTo(screening.getStartTime().toLocalTime()) <= 0 &&
        endTime.compareTo(screening.getStartTime().toLocalTime()) >= 0;
    }
}

public class PeriodCondition implements DiscountCondition {
    public boolean isSatisfiedBy(Screening screening) {
        return screening.isDiscountable(dayOfWeek, startTime, endTime);
    }
}
```

> 할인 판단 기준이 바뀌면 `DiscountCondition`과 `Screening` 모두 바뀌어야 함 _(= 응집도는 낮아지고 결합도는 높아지는 결과)_

하지만 이렇게 하면 `Screening`이 기간에 따른 할인 조건을 판단하는 책임을 떠안게 된다.

- 이것이 `Screening`이 담당해야 하는 본질적인 책임인가? 👉🏻 ❌

가끔씩은 묻는 것 외에는 다른 방법이 존재하지 않는 경우도 존재한다.  
컬렉션에 포함된 객체들을 처리하는 유일한 방법은 객체에게 물어보는 것이다.

```java
for (Movie each : movies) {
    total += each.getFee();
}
```

> 위의 코드에서는 `Movie`에게 묻지 않고(= 시켜서) `movies` 컬렉션에 포함된 전체 영화의 가격을 계산할 방법이 떠오르지 않는다.

---

# 4. 명령-쿼리 분리 원칙

- **프로시저**
  - 부수효과를 발생시킬 수 있지만, 값을 반환할 수 없다.
  - 명령 _(command)_
- **함수**
  - 값을 반환할 수 있지만, 부수효과를 발생시킬 수 없다.
  - 쿼리 _(query)_

`명령-쿼리 분리 원칙`의 요지는 오퍼레이션은 부수효과를 발생시키는 **명령**이거나, 부수효과를 발생시키지 않는 **쿼리** 중 하나여야 한다는 것이다.

- 이와 같은 **명령-쿼리 분리 원칙**에 따라 작성된 객체의 인터페이스를 `명령-쿼리 인터페이스`라고 한다.

명령은 실행 결과를 제공하지 않기 때문에, **명령 버튼을 누른 직후에는 기계 내부의 상태를 직접 확인할 수는 없다.**

## 반복 일정의 명령과 쿼리 분리하기

일정 관리 어플리케이션을 예시로 설명을 진행한다.

- `이벤트`는 특정 일자에 실제로 발생하는 사건을 의미한다.
- `반복 일정`은 일주일 단위로 돌아오는 특정시간 간격에 발생하는 사건 전체를 포괄적으로 지칭하는 용어다.

[Event 클래스 생성](https://github.com/objects-book-study/practice-object-book/blob/main/minsan/practice_ch06-/src/ch06/Event.java)

"2019년 5월 8일 수요일 10시 30분부터 11시까지 열리는 회의"를 표현하는 Event의 인스턴스는 다음과 같이 생성할 수 있다.

```java
Event meeting = new Event("회의",
    LocalDateTime.of(2019, 5, 8, 10, 30),
    Duration.ofMinutes(30));
```

[RecurringSchedule 클래스 생성](https://github.com/objects-book-study/practice-object-book/commit/be44b3905e19093845b057a43a2a1c1ca39b25f5)

다음은 `isSatisfied` 메서드를 사용해 이벤트가 반복 조건을 만족시키는지 체크하는 코드를 작성한 것이다.

```java
RecurringSchedule schedule = new RecurringSchedule("회의", DayOfWeek.WEDNESDAY, LocalTime.of(10, 30), Duration.ofMinutes(30));
Event meeting = new Event("회의", LocalDateTime.of(2019, 5, 8, 10, 30), Duration.ofMinutes(30));

assert meeting.isSatisfied(schedule) == false;
assert meeting.isSatisfied(schedule) == true; // 다시 호출하면 true를 반환
```

하지만 이 코드에는 버그가 존재한다.

- `isSatisfied` 메서드 내부에 부수 효과가 존재하기 때문

[부수 효과가 포함된 isSatisfied 메소드](https://github.com/objects-book-study/practice-object-book/blob/50c2648a8292a238dd7865fea494cf353accbcb1/minsan/practice_ch06-/src/ch06/Event.java#L21)

> false를 반환하기 전에 `reschedule` 메서드를 호출하여 **`Event` 객체의 상태를 변경**한다.

`isSatisfied` 메서드는 **개념적으로는 `쿼리`** 이지만, 실제로는 부수효과를 포함하고 있어 `명령`과 `쿼리` **두 가지 역할을 수행**하고 있다.

가장 깔끔한 해결 방법은 명령과 쿼리를 명확히 분리하는 것이다.

[isSatisfied의 명령과 쿼리 분리](https://github.com/objects-book-study/practice-object-book/commit/bdf0aea80128c8de25898465e859eadce5f42594)

> 이제 `reschedule` 메서드를 호출할지 여부를 **Event를 사용하는 쪽에서** 결정할 수 있다.

## 명령-쿼리 분리와 참조 투명성

> **참조 투명성**  
> 어떤 표현식 e가 있을 때, e의 값으로 e가 나타나는 모든 위치를 교체하더라도, 결과가 달라지지 않는 특성

어떤 값이 변하지 않는 성질을 `불변성`이라고 부른다.

- 이러한 `불변성`은 **부수효과의 발생을 방지**하고, **참조 투명성을 만족**시킨다.

## 책임에 초점을 맞춰라

디미터 법칙을 준수하고, 묻지 말고 시켜라 스타일을 따르면서도, 의도를 드러내는 인터페이스를 설계하는 쉬운 방법

- `메시지를 먼저 선택`하고, 그 후 `메시지를 처리할 객체를 선택`하는 것

`책임 주도 설계`에서는 메시지가 객체를 선택하기 때문에, **_협력에 적합한 메시지를 결정할 확률이 높아진다._**

> 👉🏻 중요한 것은 협력에 적합한 객체가 아니라 `협력에 적합한 메시지`이다.

## `계약에 의한 설계` 필요성

하지만 `오퍼레이션의 시그니처`는 어떤 조건이 만족돼야만 **오퍼레이션을 호출할 수 있**고, 어떤 경우에 **결과를 반환받을 수 없는지** 표현할 수 없다.

- 👉🏻 협력을 위해 두 객체가 **보장해야 하는 실행 시점의 제약**을 인터페이스에 명시할 수 있는 방법이 없음

이는 `계약에 의한 설계` 개념이 제안된 배경이 된다.  
이는 협력을 위해 **클라이언트와 서버가 준수해야 하는 제약**을 코드 상에 명시적으로 표현하고 강제할 수 있는 방법이다.
