## 05 소프트웨어에서 표현되는 모델

---

#### - 연관관계

#### - Entity( 엔티티 , 참조객체 ) , Value Object ( 값 객체 )

> 어떤 객체가 연속성과 식별성을을 지닌것을 의미하는가? 아니면 다른 상태를 기술하는 속성에 불과한가.
>
> 이 내용은 Entity와 Value Object를 구분하는 가장 기본적인 방법이다.

```java
// Entity
@Entity
class User {
    @Id
    @Generatedvalue(strategy = generationtype.identity)
    private Long 
    
    ...
}

// Value Object 
class Money {
    private BigDecimal value;
    
    ...
}


```

#### - Service ( 서비스 )

> 어떠한 연산을 Entity나 Value Object에게 억지로 맡기는것 보다 Service로 표현하는 편이 나을 때가 있다.
>
> Service는 Client의 요청에 대해 수행되는 무언가를 의미하기도 한다.
>
> 소포트웨어에서 수행해야 하는 것에는 해당하지만 상태를 주고받지는 않는 활동을 모델링 하는 경우가 여기에 해당한다.

#### - Module ( 모듈, 패키지라고도 함 )

> 모든 설계 관련 의사결정은 도메인에 부여된 통찰력을 바탕으로 내려야 한다는 사실을 알게 될 것이다.
>
> 높은 응집도와 낮은 결홉도 라는 개념은 도메인 개념에도 적용할 수 있다 .
>

#### - 모델링 패러다임

<br><br>

### 💻 연관 관계

---

연관 관계는 흔히 개발하면서 들어볼수 있는 1:N, N:1, N:N 과 같은 객체와 객체 or 테이블과 테이블간의 관계를 표현할때 자주 사용하는 용어입니다.

기존에 RDBMS나 JPA ORM 을 사용해보셨다면 모다 알는 1:N, N:1, N:N 과 같은 다양한 연관관계가 있으며, 그중 상당수는 양방향으로 나타난다.

#### 연관관계를 좀더 쉽게 다루는 방법 3가지.

> 1. 탐색 방향을 부여한다( 단방향, 양방향 )
> 2. 한정자(qualifier)를 추가해서 사실상 다중성(multiplicity)을 줄인다.
> 3. 중요하지 않은 연관관계는 제거한다.

가능한 한 관계를 제약하는것이 중요하다. 양방향 연관 관계는 두 객체가 모두 존재해야지만 이해 할 수 있다.

만약 서비스 로직을 개발하는데 요구사항에서 양방향으로 탐색할 필요가 없다면 탐색 방향을 추가하여 상호 의존성을 줄여 설계를 단순하게 할 수 있다.

또한 도메인의 특성이 반영되게끔 연관관계를 일관되게 제약하면 의사전달력이 풍부해지고 구현이 단순해지며, 나머지 양방향 연관관계도 의미를 지니게 된다.

<br>

## 💻 Entity

---

> 어떤 객체를 일차적으로 해당 객체의 식별성으로 정의할 경우 해당 객체를 Entity라고 부른다. 또한 해당 Entity의 식별성은 고유해야 한다.
> 
> Entity는 생명주기 내내 이어지는 연속성과 애플리케이션 사용자에게 중요한 속성과는 독립적인 특징을 가진 것이다.
> 
> 사람, 도시, 자동차, 복권, 은행 등이 Entity가 되기도 한다.

객체 모델링을 할 때 우리는 객체의 속성에 집중하곤 한다.
<br>
Entity의 근본적인 개념은 객체의 생명주기 내내 이어지는 추상적인 연속성이며, 추상적인 연속성은 여러 현태를 거쳐 전달된다.

### Entity의 특징

- Entity에는 모델링과 설계상의 툭수한 고려사항이 포함돼 있다.
- Entity는 자신의 생명주기 동안 형태와 내용이 급격하게 바뀔수도 있지만 연속성은 유지하여야 한다.
  > 해당 Entity의 속성이 변경되었다고 해당 Entity의 식별값이 달라지는것은 아니다.
  > 
  > 아래의 예제를 보겠습니다. 
   ```java
  @Entity
  class User { 
    @Id 
    @Generatedvalue(strategy = generationtype.identity)
    private Long id;
  
    private String address;
  }
  
  // main
  User user = new User(1L, "서울시 관악구 봉천동 ");
  user.changeAddress("서울시 관악구 낙성대");
  
  // 형태와 내용(속성)이 변경되었지만 해당 User 가 다른 Entity는 아니다.
  ```
  
- Entity를 추적하기 위해서는 Entity에 식별성이 정의돼 있어야 한다.
- Entity의 클래스 정의와 책임, 속성,  연관관계는 Entity에 포함된 특정 속성보다는 Entity의 정체성에 초점을 맞춰야 한다.
- Entity가 급격하게 변형되지 않거나 생명주기가 복잡하지 않더라도 의미에 따라 Entity를 분류한다면 모델이 더욱 투명해지고 구현은 견고해 질 것이다.


### Entity 생성시 고려할점 

- 객체가 속성보다는 식별성으로 구분될 경우 모델 내에서 이를 해당 객체의 주된 정의로 삼아라.
- 클래스 정의를 단순하게 하고 생명주기의 연속성과 식별성에 집중하라.
- 객체의 형태나 이력에 관계없이 각 객체를 구별하는 수단을 정의하라
- 객체의 속성으로 객체의 일치 여부를 판단하는 요구사항에 주의하라
- 각 객체에 대해 유일한 결과를 반환하는 연산을 정의하라.


<br>

## 💻 Value Object ( 값 객체 )

---
