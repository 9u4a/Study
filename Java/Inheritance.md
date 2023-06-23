# 상속(Inheritance)

## 상속(Inheritance)

### 상속의 정의

상속이란 기존의 클래스를 재사용하여 새로운 클래스를 작성하는 것이다.

→ **자손 클래스는 조상 클래스의 필드와 메소드를 재사용할 수 있다.**

**→ 코드의 재사용성, 클래스 간 계층구조 분류, 프로그램의 생산성, 유지보수에 크게 기여**

- 생성자와 초기화 블럭은 상속되지 않는다. 멤버만 상속된다.
- 자손 클래스의 멤버 개수는 조상 클래스보다 항상 같거나 많다.
- 다중 상속을 지원하지 않는다.
    - 다중 상속시 서로 다른 클래스로부터 상속받은 멤버들을 구별하기 힘들다.

**상속의 구현**

```java
//조상 클래스
class Parent { }

//자손 클래스
class Child extends Parent { }
```

**extends**  키워드를 사용하여 작성하고 상속해주는 클래스(Parent)를 조상 클래스라 하고 상속 받는 클래스(Child)를 자손 클래스라고 한다.

- 자손 클래스의 인스턴스가 생성 될 때 조상 클래스의 인스턴스가 먼저 생성 된다.

<aside>
💡 접근 제어자가 private, default인 멤버들은 상속되지 않는 것이 아니라 상속은 받지만 자손 클래스로부터의 접근이 제한된다.

</aside>

### 클래스간의 관계 - 포함관계

**포함관계** : 클래스 간의 포함관계를 맺어 주는 것은 한 클래스의 멤버변수로 다른 클래스 타입의 참조변수를 선언하는 것을 뜻함.

→ 상속이외에도 클래스를 재사용하는 또 다른 방법. 

```java
class Circle {
	int x;
	int y;
	int r;
}

class Point {
	int x;
	int y;
}

//포함관계
class Ciricle {
	Point c = new Point();
	int r;	
}
```

### 클래스간의 관계 결정

```java
//포함관계
class Circle {
	Point c = new Point();
	int r;
}

//상속
class Circle extends Point {
	int r;
}
```

다음 예시에서는 포함관계가 더 적절하다.

- 상속관계 :  ~은 ~이다 ( is - a )
    - ex) Circle 은 Point 이다.
- 포함관계 : ~은 ~을 가지고 있다 ( has - a )
    - ex) Circle 은 Point 을 가지고 있다.

### Object 클래스

Object 클래스는 모든 클래스 상속계층도의 최상위에 있는 조상클래스이다.

다른 클래스로부터 상속 받지 않는 모든 클래스들은 자동적으로 Object 클래스로부터 상속받게 된다.

```java
class Tv {
	...
}

//위의 코드를 컴파일 시 컴파일러가 extends Object 추가
class Tv extends Object {
	...
}
```

<aside>
💡 이미 어떤 클래스로부터 상속받도록 작성된 클래스에 대해서는 컴파일러가 extends Object를 추가하지 않음.

</aside>

→ 상위 클래스에서 이미 Object를 상속받아서 그렇다.

### 오버라이딩 (overriding)

조상 클래스로부터 상속받은 메소드의 내용을 변경하는 것.

```java
class Point {
	int x;
	int y;

	String getLocation() {
		return "x :" + x + "y :" + y;
	}
}

class Point3D extends Point {
	int z;
	
	//오버라이딩
	String getLocation() {
		return "x :" + x + "y :" + y + "z :" + z;
	}
}
```

**오버라이딩의 조건**

자손 클래스에서 오버라이딩하는 메소드는 조상 클래스의 메소드와

- 이름이 같아야 한다.
- 매개변수가 같아야 한다.
- 반환타입이 같아야 한다.
    - jdk1.5부터 covariant return type (공변 반환타입)이 추가되어 반환타입을 자손 클래스의 타입으로 변경하는 것은 가능하도록 조건 완화됨.

**오버라이딩을 하려면 선언부가 서로 일치해야 한다.**

(다만 접근 제어자와 예외는 제한된 조건 하에서만 다르게 변경할 수 있다.)

- 접근 제어자는 조상 클래스의 메소드보다 좁은 범위로 변경할 수 없다.
    - 대부분의 경우 같은 범위의 접근 제어자 사용.
- 인스턴스 메소드를 static메소드로 또는 그 반대로 변경할 수 없다.
- 조상 클래스의 메소드보다 많은 수의 예외를 선언할 수 없다.
    - 단순하게 선언된 예외의 개수 문제가 아님
    
    ```java
    class Parent {
    	void pMethod() throws IOException, SQLException {
    		...
    	}
    }
    
    //올바른 경우
    class Child extends Parent {
    	void pMethod() throws IOException {
    	...
    	}
    }
    
    //올바르지 못한 경우
    //Exception은 모든 예외의 최상위 클래스 이므로 더 많은 수를 선언한 것.
    class Child extends Parent {
    	void pMethod() throws Exception {
    	...
    	}
    }
    ```
    

### super

super는 자손 클래스에서 조상 클래스로부터 상속받은 멤버를 참조하는데 사용되는 **참조 변수**이다.

멤버변수와 지역변수의 이름이 같을 때 this를 붙여서 구별했듯이, **상속받은 멤버**와 **자신의 멤버**의 **이름**이 같을 때 super를 붙여서 구별

인스턴스메소드에서만 사용가능

```java
class SuperTest {
	public static void main(String args[]) {
		Child c = new Child();
		c.method();
	}
}

class Parent {
	int x =10;
}

class Child extends Parent {
	int x =20;

	void method() {
		System.out,println("x=" + x); //20
		System.out.println("this.x=" + this.x); //20
		System.out.println("super.x=" + super.x); //10
	}
}

//자손 클래스 x, this.x , 조상 클래스로부터 상속받은 super.x 
```

**super를 사용한 메소드 호출** 

```java
class Point {
	int x;
	int y;
	
	String getLocation() {
		return "x :" + x + "y :" + y;
	}
}

class Point3D extends Point {
	int z;

	//오버라이딩
	String getLocation() {
		return super.getLocation() + "z :" + z;		
	}
}
```

### super() - 조상 클래스의 생성자

조상 클래스의 생성자를 호출하는데 사용.

자손 클래스의 인스턴스가 생성될 때 조상 클래스의 인스턴스도 생성됨.

→ **조상 클래스의 인스턴스 초기화 필요**

Object클래스를 제외한 모든 클래스의 생성자는 첫 줄에 반드시 자신의 다른 생성자 또는 조상의 생성자를 호출해야 한다. 없는 경우 컴파일러가 super()를 생성자 첫 줄에 삽입.

```java
class Point {
	int x = 10;
	int y = 20;
	
	Point(int x, int y){
		//컴파일러가 이 부분에 super(); 삽입.
		this.x = x;
		this.y = y;
	}

class Point3D extends Point {
	int z = 30;
	
	Point3D() {
		this(100,200,300);
	}

	Point3D(int x, int y, int z) {
		super(x, y);
		this.z = z;
	}
}
```

---

Reference