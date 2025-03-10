1장을 읽으면서 객체지향이 무엇인지에 대해서 조금 더 명확한 개념을 잡을 수 있었다.  
특히, **절차지향 코드**와 **객체지향 코드**를 비교하면서 `데이터`와 `프로세스` 관점으로 둘의 차이점을 쉽게 이해할 수 있었다.

그리고 프리코스 과정에서 고민했던 내용들이 그대로 담겨있어서, 복습하는 느낌도 들었다. _(그만큼 프리코스에 열심히 참여한 것 같다는 생각도 들고 조금 뿌듯했다!)_

## 1. 객체지향 설계는 의존성을 적절하게 관리하는 것이 핵심이다.

서로 다른 객체 간에 소통을 한다는 점에서, 객체지향에서는 의존성을 완전히 없앨 수 없다.

하지만, 1장의 예제처럼 기능을 처리하는 프로세스를 하나의 클래스(`Theater`)가 전부 담당한 채 나머지 객체는 그저 데이터로서 존재하는 경우에는 너무 많은 의존성을 가지게 된다.

- 즉, 좋은 객체지향 설계를 위한 고민 과정을 거치지 않는다면 의존성 문제를 방치하게 된다. _(기능 수정이 어려워진다.)_

```
👉🏻 따라서, 좋은 객체지향 설계의 핵심은 의존성을 적절히 관리하여 객체 사이의 결합도를 낮추는 것이다.
```

## 2. 캡슐화: 의존성을 줄이는 방법

> **캡슐화**  
> 객체 내부의 세부적인 사항을 감추는 것을 의미한다.

캡슐화가 객체지향의 핵심 요소 중 하나로 꼽힌다는 것은 익히 알고 있었지만, 구체적으로 **캡슐화가 왜 좋은 설계에 필요한 요소라는 것**인지 잘 납득되지 않았다.
하지만 이번 오브젝트 1장 예제를 통해서 캡슐화가 왜 의존성을 줄이는 방법이 될 수 있는지 명확하게 이해할 수 있었다.

- 객체 외부에서 `객체 내부의 세부 사항들`(= **데이터**)을 너무 많이 알고 있으면, 해당 `객체가 처리하는 기능` 그 자체(= **인터페이스**)뿐만 아니라, 그 `기능을 처리하는 데 필요한 데이터`(= **구현**)에도 의존할 수 밖에 없다.

- 하지만 캡슐화를 통해 **데이터를 감추고**, 기능을 처리하는 과정에 필요한 **데이터 접근 권한은 자신에게만 부여**하면 의존성을 낮출 수 있다.

```
👉🏻 즉, 캡슐화는 객체의 기능을 사용함에 있어서 인터페이스에만 의존하게 하도록 설계하는 것에 도움이 된다.
```

## 3. 객체가 어떤 데이터를 가지냐보다는, 객체에 어떤 책임을 할당할 것이냐에 초점을 맞춰야 한다.

객체를 설계하다보면 객체 내부에만 몰두해서 데이터에만 집중하는 경우가 많다.  
하지만 좋은 설계를 따르기 위해서는 어떤 데이터를 가질 것인가가 아닌, `객체가 어떤 책임을 갖고 어떤 기능을 처리할 것이냐`에 집중해야 한다.

```
👉🏻 객체지향 프로그램은 "서로 다른 객체가 각자 자기만의 역할을 가진 채", 서로 "메시지를 주고받으며" 문제를 해결하는 것이기 때문이다.

👉🏻 따라서 객체지향 프로그램에서는 원활한 의사소통이 중요하다.
이를 위해서는 "어떤 메시지를 통해서, 외부에 원하는 결과를 전달할 수 있을지" 고민하는 것이 더 중요하다.
```
