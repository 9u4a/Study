## 인터페이스(interface)

인터페이스는 일종의 추상클래스이다. 추상클래스보다 추상화 정도가 높아서 추상클래스와 달리 몸통을 갖춘 일반 메소드 또는 멤버변수를 구성원으로 가질 수 없다.

오직 추상메소드와 상수만을 멤버로 가질 수 있다.

클래스와는 달리 다중상속이 가능하다.

**인터페이스의 장점**

- 개발시간을 단축시킬 수 있다.
- 표준화가 가능하다.
    - 기본 틀을 인터페이스로 작성
- 서로 관계없는 클래스들에게 관계를 맺어 줄 수 있다.
    - 인터페이스를 공통적으로 구현하게 하여 관계 형성
- 독립적인 프로그래밍이 가능하다.
    - 클래스의 선언과 구현을 분리시켜 독립적인 프로그램 작성을 가능하게 함.

### 인터페이스의 작성

```java
interface 인터페이스이름 {
	public static final 타입 상수이름 = 값;
	public abstract 메소드이름(매개변수목록);
	default 
}
```

- 인터페이스는 접근 제어자로 public, default를 사용할 수 있다.
- 모든 멤버변수는 public static fianl 이어야 하며, 생략이 가능하다.
- 모든 메소드는 public abstract 이어야 하며, 생략이 가능하다.
    - static메소드와 default 메소드는 예외 ( jdk 1.8이후 )

### 인터페이스의 구현

인터페이스도 추상클래스처럼 그 자체로는 인스턴스를 생성할 수 없다.

implements 키워드를 사용해 인터페이스를 구현할 수 있다.

```java
class 클래스이름 implements 인터페이스이름 {
	//추상메소드 구현
}
```

### 인터페이스를 이용한 다형성

인터페이스 타입의 참조변수로 이를 구현한 클래스의 인스턴스를 참조할 수 있고, 인터페이스 타입으로 형변환이 가능하다. 인터페이스 타입의 매개변수가 갖는 의미는 메소드 호출 시 해당 인터페이스를 구현한 클래스의 인스턴스를 매개변수로 제공해야한다는 것이다.

```java
//인터페이스 = 구현클래스
Fightable f = (Fightable)new Fighter();

//메소드의 리턴타입을 인터페이스로 지정.
Fightable method() {
	...
	Fighter f = new Fighter();
	return f; // 인터페이스를 구현한 클래스의 인스턴스 반환.
}
```

리턴타입을 인터페이스로 지정한 경우 해당 인터페이스를 구현한 클래스의 인스턴스를 반환해야 한다.

### 인터페이스의 이해

인터페이스의 본질적인 측면을 알아보기 위해 염두에 두어야 할 사항

- 클래스를 사용하는 쪽(User)과 클래스를 제공하는 쪽(Provider)이 있다.
- 메소드를 호출하는 쪽(User)에서는 사용하려는 메소드(Provider)의 선언부만 알면 된다.

```java
class A {
	public void methodA (B b) {
		b.methodB();
	}
}

class B {
	public void methodB() {
		...
	}
}

class InterfaceTest {
	public static void main(String args[]) {
		A a = new A();
		a.methodA(new B());
	}
}
```

A클래스와 B클래스는 직접적인 관계에 있다.

→ A클래스가 B의 인스턴스를 생성하고 메소드를 호출함.

직접적인 관계의 두 클래스는 한 쪽이 변경되면 다른 한 쪽도 변경되어야 한다는 단점 존재

인터페이스를 매개체로 해서 관계를 간접적으로 변경이 가능하다.

```java
interface I {
	public abstract void methodB();
}

class B implements I {
	public void methodB() {
		...
	}
}

class A {
	public void methodA (I i) {
		i.methodB();
	}
}

class InterfaceTest {
	public static void main(String args[]) {
		A a = new A();
		a.methodA(new B());
	}
}
```

### default method ( java 8 이후)

추상 메소드의 기본적인 구현을 제공하는 메소드로, 인터페이스의 새로운 메소드를 추가해야 할 경우에 모든 클래스들이 새로운 메소드를 구현해야 했었다. default method를 사용하여 인터페이스를 구현하는 클래스들의 메소드 구현을 강제하지 않는다.

```java
interface DefaultTest {
	default void newMethod() {...}
}
```

---

Reference