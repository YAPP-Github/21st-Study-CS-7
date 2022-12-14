# SOLID

## 1. SOLID 란 무엇인가요?

```
흔히 객체 지향 설계 원칙이라고도 불리는 SOLID는 프로그래머가 시간이 지나도 유지 보수와 확장이 쉬운 시스템을 만들고자 할 때 이 원칙들을 함께 적용할 수 있습니다. SOLID 원칙들은 소프트웨어 작업에서 프로그래머가 소스 코드가 읽기 쉽고 확장하기 쉽게 될 때까지 소프트웨어 소스 코드를 리팩터링하여 코드 냄새를 제거하기 위해 적용할 수 있는 지침입니다.
```

> 객체 지향 프로그래밍에서 객체는 하나의 책임을 가진 역할로서 협력에 참여하여 소프트웨어의 목적을 달성한다.

---

## 2. 단일 책임 원칙(Single Responsibility principle)이 무엇인가요?

```
하나의 클래스는 하나의 책임만 가져야한다는 원칙입니다. 다시 말하자면, 어떤 변화에 의해서 클래스를 변경해야 하는 이유는 오직 하나뿐이어야 함을 의미합니다.
```

### 꼬리, SRP를 만족하기 위해서 어떻게 리팩토링 해야하나요?

```
SRP를 지키지 않는 대표적인 예시는 산탄총 수술입니다. 이는 어떤 한 변경 사항이 생겼을 때, 여러 모듈(클래스나, 함수)을 수정해야하는 상황입니다. 이때 필드나 메서드를 이동시켜 책임을 기존의 어떤 클래스로 모으거나, 새로운 클래스를 만들어 해결할 수 있습니다.
```

---

## 3. 개방 폐쇄 원칙(Open Closed Principle)은 무엇인가요?

```
소프트웨어의 모든 구성요소는 확장에는 열려있고, 변경에는 닫혀있어야한다는 원칙입니다. 변경을 위한 비용은 가능한 줄이고 확장을 위한 비용은 가능한 극대화해야 한다는 의미로, 요구사항의 변경이나 추가사항이 발생해도, 기존 구성요소는 수정이 일어나지 말아야하며, 기존 구성요소를 쉽게 확장해서 재사용할 수 있어야 한다는 뜻입니다.
```

### 꼬리, OCP를 어떻게 만족할 수 있나요?

```
 추상화를 이용해서, 클라이언트 코드에서는 추상화된 부분에 대해서만 사용하면, 연쇄적으로 수정이 생기는 것을 방지할 수 있습니다.
```

> 기존 코드가 변경에 닫혀 있으며, 새로운 행동을 추가하는 것은 열려있는 것을 OCP(개방-폐쇄 원칙)이라고 합니다. OCP가 돌아갈 수 있는 기본적인 이유는 추상화를 기준으로 분리를 하여 DIP(의존성-역전 원칙)을 만족하기 때문입니다. 이처럼, OCP는 SOLID의 모든 것들이 추구하는 방향입니다. - 조영호 -

---

## 4. 리스코프 치환 법칙(Liskov Substitution Principle)은 무엇인가요?

```
상위 타입의 객체를 하위 타입의 객체로 치환해도 상위 타입을 사용하는 프로그램은 정상적으로 동작해야 한다는 원칙입니다. 업캐스팅 안전 원칙이라고도 하는 이 원칙은 부모쪽으로 업캐스팅하는 것이 안전함을 보장하기 위해 존재합니다.

상위 타입이 ~~기 때문에 기대되는 역할과 행동 규약이 있는데, 이를 벗어나면 안됩니다.
```

### 꼬리, 업캐스팅과 다운캐스팅은 무엇인가요?

```
업캐스팅이란 하위 타입의 객체가 상위 타입으로 형변환 되는 것을 의미하며, 다운 캐스팅은 상위 타입의 객체를 하위 타입으로 변환하는 것을 의미합니다. 자바에서는 다운캐스팅을 할 때 명시적으로 타입을 지정해줘야 합니다.
```

### 꼬리, 리스코프 치환 법칙의 대표적인 사례인 직사각형-정사각형 문제를 알고 계신가요? 알고 계신다면 간단하게 왜 LSP를 위반하는 지 설명 부탁드립니다.

> 출처 : https://steady-coding.tistory.com/383

```
직사각형의 하위 타입으로 정사각형으로 구성하면 LSP가 위배되는 문제입니다. 직사각형은 정사각형이 아니지만, 정사각형은 직사각형입니다. 따라서, 정사각형이 직사각형에게 구현상속을 받는다면 다음과 같은 문제가 발생합니다.

어느 메서드가 직사각형의 가로와 세로를 비교한 다음에, 세로가 가로보다 짧거나 같다면 가로의 길이에 1을 더한 만큼의 길이를 갖게 만드는 기능을 수행하다면, 정사각형이 아닌 직사각형에 대해서는 메서드가 올바르게 작동합니다.

하지만, 정사각형에 대해서는 올바르게 작동하지 않습니다. (기대한 것과 다르다.)  상위 타입에서 따로따로 변경가능하게 명세를 정해놓은 것을 하위 타입에서 지키지 않았기 때문에 LSP 위반입니다.
```

```java
public class Rectangle {
    private int width;
    private int height;

    public void setWidth(final int width) {
        this.width = width;
    }

    public void setHeight(final int height) {
        this.height = height;
    }

    public int getWidth() {
        return width;
    }

    public int getHeight() {
        return height;
    }
}

public class Square extends Rectangle {

    @Override
    public void setWidth(final int width) {
        super.setWidth(width);
        super.setHeight(width);
    }

    @Override
    public void setHeight(final int height) {
        super.setWidth(height);
        super.setHeight(height);
    }
}
```

```java
 public void increaseHeight(final Rectangle rectangle) {
        if (rectangle.getHeight() <= rectangle.getWidth()) {
            rectangle.setHeight(rectangle.getWidth() + 1);
        }
    }
```

### 꼬리, LSP를 만족하도록 어떻게 구현해야 할까요?

```
1. 만약, 두 개체가 같은 일을 한다면, 둘을 하나의 클래스로 표현하고 이들을 구분할 수 있는 필드를 둡니다.

2. 그리고 만약, 똑같은 연산을 제공하지만, 이들을 약간씩 다르게 한다면 공통의 인터페이스를 만들고 두 클래스가 이를 구현해야합니다. (타입 상속)

3. 만약 공통된 연산이 없다면, 완전 별개인 2개의 클래스를 만듭니다.

4. 만약 두 개체가 하는 일에 추가적으로 무언가를 더한다면 구현 상속을 사용합니다.
```

---

## 5. 인터페이스 분리 원칙(Interface Segregation Principle)은 무엇인가요?

```
 어떤 클래스가 다른 클래스에 종속될 때에는 가능한 최소한의 인터페이스만을 사용해야한다는 원칙입니다. 즉, 클라이언트는 사용하지 않는 인터페이스에 강제로 의존해서는 안된다는 의미를 가지고 있습니다.

가령, 어떤 클래스를 이용하는 클라이언트가 여러개이고 이들이 해당 클래스의 특정 부분집합만을 이용한다면, 이들을 따로 인터페이스로 빼내어 클라이언트가 기대하는 메시지만을 전달할 수 있도록 해야합니다.
```

### 꼬리, SRP와 ISP의 차이점은?

```
SRP는 클래스의 단일책임을 강조하며, ISP는 인터페이스의 단일책임을 강조합니다. ISP를 다른말로하면, 하나의 일반적인 인터페이스보다는 여러 개의 구체적인 인터페이스가 낫다고 할 수 있습니다.
```

---

## 6. 의존 역전 원칙(Dependency Inversion Principle)은 무엇인가요?

```
의존 역전 원칙은 아래와 같이 정리할 수 있습니다.

- 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안 된다.

- 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 한다.

- 자신보다 변하기 쉬운 것에 의존하지 마라.

```

### 꼬리, 고수준 모듈과 저수준 모듈이 무엇인가요?

```
고수준 모듈은 어떤 의미 있는 단일 기능을 제공하는 모듈이며
저수준 모듈은 고수준 모듈의 기능을 구현하기 위해 필요한 하위 기능의 실제 구현입니다.
```
