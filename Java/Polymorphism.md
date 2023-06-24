## 다형성(Polymorphism)

객체지향개념에서 다형성이란 **여러 가지 형태를 가질 수 있는 능력**을 의미한다.

→ 조상 클래스 타입의 **참조변수**로 자손 클래스의 **인스턴스**를 참조할 수 있도록 함.

예시)

```java
class Tv {
	boolean power;
	int channel;
	
	void power() { ... }
}

class CaptionTv extends Tv {
	String text;
	
	void caption() { ... }
}
```

Tv와 CaptionTv가 상속관계에 있을 때 조상 클래스 타입의 참조변수로 자손 클래스의 인스턴스를 참조하도록 하는 것이 가능하다.

```java
CaptionTv c = new CaptionTv();

//조상 클래스 타입 = 자손 클래스 인스턴스
Tv t = new CaptionTv();
```

t의 경우 실제 인스턴스가 CaptionTv타입이지만 인스턴스의 모든 멤버를 사용할 수 없다.

→ CaptionTv인스턴스에서 Tv클래스의 멤버들만 사용가능. ( text, caption() 사용 불가)

반대로 자손타입의 참조변수로 조상타입의 인스턴스 참조는 불가능하다.

```java
//컴파일 에러
CaptionTv c = new Tv();
```

실제 인스턴스인 Tv의 멤버 개수보다 참조변수 c가 사용할 수 있는 멤버 개수가 더 많기 때문.

→ 참조변수가 사용할 수 있는 멤버의 개수는 인스턴스의 멤버 개수보다 같거나 적어야 한다.

### 참조변수의 형변환

서로 상속관계에 있는 클래스사이에서 참조변수의 형변환이 가능하다.

- Up-casting (자손타입 → 조상타입) : 형변환 생략가능
- Down-casting (조상타입 → 자손타입) : 형변환 생략불가

```java
CaptionTv c = new CaptionTv();
CaptionTv c2 = null;
Tv t = null;

t =  c; // 업캐스팅
c2 = (CaptionTv)t; //다운캐스팅
```

참조변수의 형변환은 참조하고 있는 인스턴스에서 사용할 수 있는 멤버의 범위를 조절하는 것 뿐이다. 인스턴스에 영향 x

### instanceof연산자

참조변수가 참조하고 있는 인스턴스의 실제 타입을 알아보기 위해 사용된다.

연산의 결과로 true, false 반환

```java
void doWork(Car c) {
	if (c instanceof FireEngine) {
		FireEngine fe = (FireEngine)c;
		...
	} else if (c instanceof Ambulance) {
		Ambulance a = (Ambulance)c;
		...
	}
}
```

**어떤 타입에 대한 instanceof 연산의 결과가 true라면 검사한 타입으로의 형변환이 가능하다는 것을 뜻한다.**

### 참조변수와 인스턴스의 연결

조상클래스와 자손 클래스에 같은 이름의 인스턴스 변수를 중복으로 정의했을 때,

조상타입의 참조변수로 자손 인스턴스를 참조하는 경우와 자손타입의 참조변수로 자손 인스턴스를 참조하는 경우 다른 결과를 가진다.

```java
class Parent {
	int x =100;

	void method() {
		System.out.println("Parent");
	}
}

class Child extends Parent {
	int x = 200;

	void method() {
		System.out.println("Child");
	}
}

...

Parent p = new Child();
Child c = new Child();

System.out.println("p.x = " + p.x); //100
p.method(); //Child
System.out.println("c.x = " + c.x); // 200
c.mehtod(); //Child
```

메소드의 경우 참조변수의 타입에 관계없이 실제 인스턴스의 타입의 메소드가 호출되지만,

인스턴스 변수의 경우 참조변수의 타입에 따라 달라진다.

→ 외부 클래스에서 참조변수를 통해 직접적으로 인스턴스 변수에 접근할 수 있게 하지 말자.

### 매개변수의 다형성

```java
class Product{
	int price;
	int bonusPoint;
}

class Tv extends Product {}
class Computer extends Product {}
class Audio extends Product {}

class Buyer {
	int money = 1000;
	int bonusPoint = 00;

	void buy(Tv t) {
		money = money - t.price;
		bonusPoint = bonusPoint + t.bonusPoint;
	}
	
	void buy(Computer c) {
		money = money - c.price;
		bonusPoint = bonusPoint + c.bonusPoint;
	} 
	....

	//매개변수의 다형성을 이용
	void buy(Product p) {
		money = money - p.price;
		bonusPoint = bonusPoint + p.bonusPoint;
	}
}

```

제품의 종류가 늘어날 때마다  buy메소드를 추가할 필요 없이 모든 제품이 Product 클래스를 상속 받는 점을 이용해 다음과 같이 사용할 수 있다.

### 추상 클래스 (abstract class)

하나 이상의 추상 메소드를 포함하는 클래스를 추상 클래스라고 한다.

추상클래스로 인스턴스 생성 불가

상속을 통해 자손 클래스를 만들고. 추상 클래스의 모든 추상 메소드를 오버라이딩해야 자손 클래스의 인스턴스를 생성 가능.

```java
abstract class 클래스이름 {
	...
}
```

### **추상 메소드 (abstract method)**

선언부만 작성하고 구현부는 작성하지 않은 것을 추상 메소드라고 한다.

추상 메소드를 사용하는 이유는 상속받는 자손 클래스에서 추상 메소드를 구현하도록 하기 위함이다. 

```java
abstract 반환타입 메소드이름();
```

### 추상 클래스의 작성

여러 클래스에 공통적으로 사용될 수 있는 클래스를 바로 작성하기도 하고, 기존의 클래스의 공통적인 부분을 뽑아서 추상 클래스로 만들어 상속하도록 하는 경우도 있다.

**추상화 :** 클래스간의 공통점을 찾아내서 공통의 조상을 만드는 작업

**구체화 :** 상속을 통해 클래스를 구현, 확장하는 작업

기존의 클래스에서 공통된 부분을 뽑아내어 추상 클래스로 만들기

```java
class Marine {
	int x, y;
	void move(int x, int y){...}
	void stop(){...}
	void stimPack(){...}
}

class Tank {
	int x, y;
	void move(int x, int y){...}
	void stop(){...}
	void changeMode(){...}
}

class Dropship {
	int x, y;
	void move(int x, int y){...}
	void stop(){...}
	void load(){...}
	void unload(){...}
}

//추상클래스를 사용

abstract class Unit {
	int x, y;
	abstract void move(int x, int y);
	void stop() {...}
}

class Marine extends Unit {
	void move(int x, int y) {...}
	void stimPack() {...}
}

class Tank extends Unit {
	void move(int x, int y) {...}
	void changeMode() {...}
}

class Dropship extends Unit {
	void move(int x, int y) {...}
	void load(){...}
	void unload(){...}
}
```

---

Reference