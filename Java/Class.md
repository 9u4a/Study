# 클래스(Class)

## 객체지향언어

객체지향이론의 기본 개념 : 실제 세계는 사물로 이루어져 있으며, 발생하는 모든 사건들은 사물간의 상호작용이다.

**객체지향언어의 특징**

- 코드의 재사용성이 높다.
    - 새로운 코드를 작성할 때 기존의 코드를 이용하여 쉽게 작성할 수 있다.
- 코드의 관리가 용이하다.
    - 코드간의 관계를 이용해서 적은 노력으로 쉽게 코드를 변경할 수 있다.
- 신뢰성이 높은 프로그래밍을 가능하게 한다.
    - 제어자와 메소드를 이용해서 데이터를 보호하고 코드의 중복을 제거하여 오동작을 방지할 수 있다.

## 클래스와 객체

클래스란 객체를 정의해놓은 것(객체의 설계도)으로 객체를 생성하는데 사용되며

객체는 클래스에 정의된 대로 생성된다.

클래스로부터 객체를 만드는 과정을 클래스의 **인스턴스화**라고 하며, 만들어진 객체를 그 클래스의 **인스턴스**라고 한다.

클래스는 **속성**(멤버 변수)과 **기능**(메소드)으로 이루어져 있다. 

### 클래스 정의

```java
//접근지정자 class 클래스이름 { }
public class TestClass {
	...
}
```

### 생성자 (constructor)

생성자는 인스턴스가 생성될 때 호출되는 인스턴스 초기화 메소드이다.

- 생성자의 이름은 클래스의 이름과 같아야 한다.
- 생성자는 리턴 값이 없다.

**생성자 정의 방법**

```java
클래스이름(타입 변수명, 타입 변수명, ...){
	//인스턴스 생성 시 수행될 코드,
	//주로 인스턴스 변수의 초기화 코드를 적는다.
}

class Card {
	Card(){ //매개변수가 없는 생성자.
		...
	}

	Card(String k, int num){ //매개변수가 있는 생성자
		...
	}
}

Card c = new Card();
```

연산자 new가 인스턴스를 생성하는 것이지 생성자가 인스턴스를 생성하는 것이 아니다.

생성자는 단순히 인스턴스 변수들의 초기화에 사용되는 메소드이다.

### 인스턴스의 생성과 사용

```java
class Tv{
 Tv(){
		...
	}
}

//클래스명 변수명;
//변수명 = new 클래스명();

Tv t;
t = new Tv();
```

인스턴스는 참조변수를 통해서만 다룰 수 있으며, 참조변수의 타입은 인스턴스의 타입과 일치해야 한다.

**인스턴스 생성 코드의 수행과정**

1. 연산자 new에 의해서 메모리(heap)에 Tv 클래스의 인스턴스가 생성된다.
2. 생성자 Tv()가 호출되어 수행된다.
3. 연산자 new의 결과로, 생성된 Tv 인스턴스의 주소가 반환되어 참조변수 t에 저장된다.

### 객체 배열의 생성

```java
Tv[] tvArr = new Tv[3];
tvArr[0] = new Tv();
tvArr[0] = new Tv();
tvArr[0] = new Tv();

Tv[] tvArr = { new Tv(), new Tv(), new Tv() };
```

객체 배열 안에 객체가 저장되는 것은 아니고 객체의 주소가 저장된다. → 참조 변수 배열

### 기본 생성자 (default constructor)

모든 클래스에는 반드시 하나 이상의 생성자가 정의되어 있어야 한다.

컴파일 할 때, 소스파일의 클래스에 **생성자가 하나도 정의되지 않은 경우** 컴파일러는 자동적으로 기본 생성자를 추가하여 컴파일한다.

→ 매개변수도 없고 아무런 내용도 없는 생성자 추가.

특별히 인스턴스 초기화 작업이 요구되어지지 않는다면 생성자를 정의하지 않아도 된다.

```java
클래스이름() {}
Tv() {}
```

### 매개변수가 있는 생성자

생성자도 메소드처럼 매개변수를 선언하여 호출 시 값을 넘겨받아 인스턴스의 초기화 작업에 사용할 수 있다. 

```java
class Car {
	String color;
	String gearType;
	int door;
	
	Car() {}
	
	Car(String c, String g, int d) {
		color = c;
		gearType = g;
		door = d;
	}
}

//매개변수가 없는 생성자 사용해서 변수 초기화
Car c = new Car();
c.color = "white";
c.gearType = "auto";
c.door = 4;

//매개변수가 있는 생성자 사용해서 변수 초기화
Car c = new Car("white","auto",4);
```

### this(), this

같은 클래스의 멤버들 간에 서로 호출할 수 있는 것처럼 생성자 간에도 서로 호출이 가능하다.

단, 다음의 두 조건을 만족시켜야 한다.

1. 생성자의 이름으로 클래스이름 대신 this()를 사용한다.
2. 한 생성자에서 다른 생성자를 호출할 때는 반드시 첫 줄에서만 가능하다. 

- **this** : 인스턴스 자신을 가리키는 참조변수, 인스턴스의 주소가 저장되어 있다. 모든 인스턴스 메소드에 지역변수로 숨겨진 채로 존재한다.
    - this 를 사용하여 클래스 내부의 인스턴스 변수와 생성자의 매개변수로 선언된 변수의 이름을 구별.
    - 클래스 메소드(static) 에서는 인스턴스 멤버들을 사용할 수 없는 것처럼, this도 사용할 수 없다.
- **this(), this(매개변수)** :  생성자, 같은 클래스의 다른 생성자를 호출할 때 사용한다.

```java
class Car {
	String color;
	String gearType;
	int door;
	
	Car() {
		this("white","auto",4); //Car(String color, String gearType, int door) 호출
	}
	
	Car(String color) {
		this(color,"auto",4);
	}
	Car(String color, String gearType, int door) {
		this.color = color;
		this.gearType = gearType;
		this.door = door;
	}
}

class CarTest{
	public static void main(String[] args){
		Car c1 = new Car(); // color = white, gearType = auto, door = 4
		Car c2 = new Car("blue");
	}
}
```

## 변수의 초기화

**멤버 변수의 초기화 방법**

- 명시적 초기화
- 생성자
- 초기화 블럭
    - 인스턴스 초기화 블럭 : 인스턴스 변수를 초기화 하는데 사용
    - 클래스 초기화 블럭 : 클래스 변수를 초기화 하는데 사용

### 명시적 초기화

변수를 선언과 동시에 초기화 하는 것.

가장 기본적이면서 간단한 초기화 방법이므로 가장 우선적으로 고려되어야 한다.

```java
class Car {
	int door = 4; //기본형 변수의 초기화
	Engine e = new Engine(); //참조형 변수의 초기화
}
```

### 초기화 블럭 (initialization block)

초기화 블럭 내에서는 메소드 내에서와 같이 조건,반복,예외처리 등 자유롭게 사용 가능.

- 클래스 초기화 블럭 : 클래스 변수의 복잡한 초기화에 사용된다.
    - 클래스가 메모리에 처음 로딩될 때 한번만 수행
- 인스턴스 초기화 블럭 : 인스턴스 변수의 복잡한 초기화에 사용된다.
    - 인스턴스를 생성할 때 마다 수행 (생성자 보다 먼저 수행됨)

```java
class InitBlock {
	static {...} //클래스 초기화 블럭
	
	{...} //인스턴스 초기화 블럭

}
```

ex) 클래스의 모든 생성자에 공통으로 수행되어야 하는 문장들을 초기화 블럭을 사용

→ 재사용성을 높이고 중복을 제거

```java
Car() {
	count++;
	serialNo = count;
	color = "White";
	gearType = "Auto";
}

Car(String color, String gearType) {
	count++;
	serialNo = count;
	this.color = color;
	this.gearType = gearType;
}

//초기화 블럭 사용

{
	count++;
	serialNo = count;
}

Car() {
	color = "White";
	gearType = "Auto";
}

Car(String color, String gearType) {
	this.color = color;
	this.gearType = gearType;
}
```

### 멤버변수의 초기화 시기와 순서

- 클래스 변수의 초기화 시점 : 클래스가 처음 로딩될 때 단 한번 초기화 된다.
    - 초기화 순서 : 기본값 → 명시적 초기화 → 클래스 초기화 블럭
- 인스턴스 변수의 초기화 시점 : 인스턴스가 생성될 때마다 각 인스턴스 별로 초기화가 이루어진다.
    - 초기화 순서 : 기본값 → 명시적 초기화 → 인스턴스 초기화 블럭 → 생성자

프로그램 실행 도중 클래스에 대한 정보가 요구될 때 클래스는 메모리에 로딩된다.

→클래스 멤버를 사용했을 때, 인스턴스를 생성할 때 등.

하지만 이미 해당클래스가 메모리에 로딩되어 있다면, 다시 로딩하지 않는다.

**초기화 과정**

```java
class InitTest {
	static int cv = 1; //명시적 초기화
	int iv = 1; //명시적 초기화

	static { cv = 2; } //클래스 초기화 블럭

	{ iv = 2; } //인스턴스 초기화 블럭

	InitTest() { //생성자
		iv = 3;
	}
}
```

1, 2, 3 = 클래스 초기화

4, 5, 6, 7 = 인스턴스 초기화

| 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| --- | --- | --- | --- | --- | --- | --- |
| 기본값 | 명시적 초기화 | 클래스 초기화 블럭 | 기본값 | 명시적 초기화 | 인스턴스 초기화 블럭 | 생성자 |
| cv = 0 | cv = 1 | cv = 2 | cv = 2, iv = 0 | cv = 2, iv = 1 | cv = 2, iv=2 | cv = 2, iv=3 |
1. cv가 메모리(method area)에 생성, cv에 int형 기본값 0 저장
2. 명시적 초기화에 의해서 cv에 1 저장
3. 클래스 초기화 블럭 수행으로 cv에 2 저장
4. InitTest클래스의 인스턴스가 생성되면서 iv가 메모리(heap)에 존재하게 된다. iv에 int형 기본값 0 저장
5. 명시적 초기화에 의해서 iv에 1 저장
6. 인스턴스 초기화 블럭 수행으로 iv에 2 저장
7. 생성자가 수행되어 iv에 3 저장

### 내부 클래스 (inner class)

클래스 내에 선언된 클래스로 클래스에 다른 클래스를 선언하는 이유는 두 클래스가 서로 긴밀한 관계에 있기 때문이다.

**내부 클래스의 장점**

- 내부 클래스에서 외부 클래스의 멤버들을 쉽게 접근할 수 있다.
- 코드의 복잡성을 줄일 수 있다.

### **내부 클래스의 종류**

내부 클래스의 종류는 변수의 선언위치에 따른 종류와 같다.

| 내부 클래스 | 특징 |
| --- | --- |
| 인스턴스 클래스 | 외부 클래스의 멤버변수 선언위치에 선언. 인스턴스 멤버처럼 다루어진다. |
| 스태틱 클래스 | 외부 클래스의 멤버변수 선언위치에 선언. static 멤버처럼 다루어진다. |
| 지역 클래스 | 외부 클래스의 메소드나 초기화블록 안에 선언. 선언된 영역 내부에서만 사용가능 |
| 익명 클래스 | 클래스의 선언과 객체의 생성을 동시에 하는 일회용 클래스 |

### 내부 클래스의 선언

선언위치에 따라 변수의 선언과 같은 scope와 접근성을 갖는다.

```java
class Outer {
	class InstanceInner {}
	static class StaticInner {}
	
	void outerMethod() {
		class LocalInner {}
	}
}
```

### 익명 클래스(annoymouus class)

클래스의 선언과 객체의 생성을 동시에 하기 때문에 단 한번만 사용될 수 있고 오직 하나의 객체만을 생성할 수 있는 일회용 클래스이다.

```java
new 조상클래스이름() {
	//멤버 선언
}

new 구현인터페이스이름() {
	//멤버 선언
}
```

이름이 없기 때문에 생성자도 가질 수 없으며, 오로지 단 하나의 클래스를 상속받거나 단 하나의 인터페이스만을 구현할 수 있다.

```java
class InnerTest {
	public static void main(String[] args) {
		Button b = new Button("Start");

		b.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				System.out.println("ActionEvnet");
			}
		}

	}
}
```

---

Reference