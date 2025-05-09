### 객체 협력 및 의존성에 대한 통찰

전반적으로 인터페이스의 개념을 다이어그램과 코드를 통해 이해해 나가는 과정이 매우 인상적이었고, 
효과적인 학습 방법이었다고 느꼈다. 특히, **객체들 간의 협력을 위해 클라이언트 프로그래머가 내부 구현에 마음대로 접근하지 못하도록 설계하는 것이 중요**하다는 점을 다시 한번 상기하게 되었다. 
이를 통해 내부 구현을 자유롭게 변경할 수 있는 설계의 유연성과 안정성을 확보할 수 있다는 점을 깨달았다.

---

### 컴파일 시간 의존성과 실행 시간 의존성

이번 학습을 통해 **컴파일 시간 의존성과 실행 시간 의존성**의 차이를 다시 한번 학습할 수 있었다. 
자바를 주 언어로 사용하다 보니 실행 시간 의존성에 익숙해 기본적인 사항으로 여기고 있었지만, 이를 의도적으로 활용하려는 노력이 중요하다는 점을 알게 되었다. 

특히, 코드와 실행 시점 의존성이 다를 때의 설계 트레이드오프가 흥미로웠다. 

1. **의존성의 차이**:  
   - 코드 의존성과 실행 시점 의존성은 항상 동일하지 않을 수 있다. 클래스 간의 의존성과 객체 간의 의존성 역시 다를 수 있다. 
   - 이러한 차이를 통해 더 유연하고 재사용 가능하며 확장 가능한 객체지향 설계를 할 수 있다.

2. **트레이드오프**:  
   - 코드 의존성과 실행 시점 의존성이 다를수록 코드를 이해하기 어려워지는 단점이 존재한다. 
   - 그러나 동시에, 코드가 더욱 유연해지고 확장 가능해지는 장점이 있다. 
   - 이러한 의존성의 양면성은 설계가 항상 **트레이드오프**의 결과라는 점을 보여준다.

---

### 상속보다 합성을 선호해야 하는 이유

코드 재사용 관점에서 **상속보다는 합성을 선호해야 한다는 점**을 다시 한번 확인하게 되었다. 

#### 합성의 장점
1. **캡슐화 강화**:  
   - 인터페이스에 정의된 메시지를 통해서만 재사용이 가능하므로 구현 세부사항을 효과적으로 캡슐화할 수 있다.
2. **유연성 증가**:  
   - 의존하는 인스턴스를 교체하기 쉬워 설계가 더 유연해진다.
3. **느슨한 결합**:  
   - 상속은 클래스 간 강한 결합을 초래하는 반면, 합성은 메시지를 통해 느슨하게 결합된다.

#### 상속의 적절한 사용
합성을 선호하더라도 상속을 완전히 배제할 필요는 없다. 
**다형성을 위해 인터페이스를 재사용하는 경우에는 상속과 합성을 조합하여 사용하는 것이 필요**하다. 
하지만, 순수한 코드 재사용을 목적으로 할 때는 상속보다 합성을 선택하는 것이 더 바람직하다는 점을 강조된다.

---

### 마무리
이번 학습을 통해 객체 협력, 의존성, 상속과 합성의 개념을 보다 깊이 이해할 수 있었고, 
설계의 트레이드오프를 고려해야 한다는 점을 다시 한번 느꼈다. 
이러한 원칙들을 실제 코드 설계에 적용함으로써 더 유연하고 재사용 가능한 구조를 만들어 나가고자 한다.
