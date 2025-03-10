이번 챕터에서는 데이터 중심 설계를 했을 때에 문제점에 대해서 많이 배울 수 있었다.

특히 기억에 남았던 주의 사항은 다음과 같다.

### 캡슐화를 지켜라

속성의 가시성을 private으로 설정했다고 해도 접근자와 수정자를 통해 속성을 외부로 제공하고 있다면 캡슐화를 위반하는 것이다!

내부 구현의 변경이 외부로 퍼져나가는 파급 효과(ripple effect)는 캡슐화가 부족하다는 명백한 증거다.

> 캡슐화의 진정한 의미
>
> 캡슐화가 단순히 객체 내부의 데이터를 외부로부터 감추는 것 이상의 의미를 가진다는 것을 보여준다.
> 사실 캡슐화는 변경될 수 있는 어떤 것이라도 감추는 것을 의미한다.
> 내부 속성을 외부로부터 감추는 것은 '데이터 캡슐화'라고 불리는 캡슐화의 한 종류일 뿐이다.
>
> 그것이 속성의 타입이건, 할인 정책의 종류건 상관 없이 내부 구현의 변경으로 인해 외부의 객체가 영향을 받는다면 캡슐화를 위반한 것이다.
> 설계에서 변하는 것이 무엇인지 고려하고 변하는 개념을 캡슐화해야 한다.


---

단순히 접근자 수정자를 노출시키는 것, 또는 접근 제한자를 열어두는 것 등으로 캡슐화를 지키지 않는 경우 외에도

메서드 명 또는 메서드에서 받는 매개변수 등으로 해당 객체가 어떤 상태를 가지고 있는 지 들어내는 것 또한 캡슐화를 해친다는 내용이 가장 인상깊었고 주의해야 할 점이라고 생각한다.

---

데이터 중심 설계가 변경에 취약한 이유를 잘 숙지해두도록 하자!

- 데이터 중심의 설계는 본질적으로 너무 이른 시기에 데이터에 관해 결정하도록 강요한다!
- 데이터 중심의 설계에서는 협력이라는 문맥을 고려하지 않고 객체를 고립시킨 채 오퍼레이션을 결정한다!

---