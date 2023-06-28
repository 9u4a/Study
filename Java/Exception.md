# 예외 처리(Exception Handling)

## 예외 처리

자바에서는 실행 시 발생할 수 있는 프로그램 오류를 에러와 예외로 구분한다.

**에러(error)** : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류

**예외(exception)** : 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

예외처리란 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것이며,

예외처리의 목적은 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것이다.

발생한 예외를 처리하지 못하면, 프로그램은 비정상적으로 종료되며, 처리되지 못한 예외는 JVM의 예외처리기가 받아서 예외의 원인을 출력한다.

### Exception 클래스 계층구조

![image](https://github.com/9u4a/Study/assets/81855010/cc4356e8-8261-4d07-b622-f9a70268e197)


자바에서 모든 예외의 조상 클래스가 되는 Exception 클래스는 크게 다음과 같이 구분할 수 있다.

1. **Checked Exception** :  Runtime Exception을 제외한 클래스
2. **Unchecked Exception** :  Runtime Exception 하위 클래스들

**Unchecked Exception**은 주로 치명적인 예외 상황을 발생시키지 않는 예외들로 구성.

→ 프로그램을 작성하면서 예외가 발생하지 않도록 주의해야 한다.

**Checked Exception**은 치명적인 예외 상황을 발생시킨다.

→ 반드시 예외를 처리해야만 한다.

**예외 클래스의 인스턴스에 접근**

**printStackTrace()** : 예외발생 당시의 호출 스택에 있었던 메소드의 정보와 예외 메시지를 출력한다.

**getMessage()** : 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

### 예외처리 try / catch

예외가 발생할 메소드 내에서 직접 처리하고자 할 때 사용됨.

```java
try {
	//예외가 발생할 가능성이 있는 문장
} catch (Exception1 e1) {
	//예외처리를 위한 문장
} catch (Exception2 | Exception3 e2) {
	//멀티 catch블록 (jdk1.7부터 가능)
}finally{
	//예외의 발생여부에 관계없이 항상 수행되는 문장
}
```

예외가 발생하면, 발생한 예외에 해당하는 클래스의 인스턴스가 만들어 진다.

catch 블록 내에 선언된 참조변수의 종류와 생성된 예외클래스의 인스턴스에 instanceof 연산을 이용해 검사하며 검사결과가 true를 반환할 때까지 검사를 진행한다.

### 예외 발생시키기

연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만들어 키워드 throw를 사용해서 예외를 발생시킨다.

```java
Exception e = new Exception("exception");
throw e;
```

### 메소드에 예외 선언하기

메소드의 선언부에 키워드 throws를 사용해서 메소드 내에서 발생할 수 있는 예외를 적어주면 된다.

```java
void method() throws Exception1, Exception2 {
	...
}
```

### try-with-resources

jdk1.7부터 추가되었으며, 주로 I/O과 관련된 클래스를 사용할 때 유용하다.

→resource를 반환하기 위하여 close()필요

```java
try( FileInputStream fis = new FileInputStream("~~~");){
	...
} catch (IOException ie) {
	...
}
```

try문의 ( ) 안에 객체를 생성하는 코드가 있다면, 이 객체는 따로 close()를 호출하지 않아도 try블록을 벗어나는 순간 자동으로 close()를 호출한다.

→ AutoCloseable 인터페이스를 구현한 클래스인 경우에 해당.

### 예외 되던지기

한 메소드 내에서 발생할 수 있는 예외가 여럿인 경우, 메소드 내에서 자체적으로 처리하고, 나머지는 선언부에 지정하여 호출한 메소드에서 나눠서 처리하도록 할 수 있다.

→예외를 처리한 후 인위적으로 다시 발생시키는 방법을 통해 가능하게 한다.

```java
class ExceptionTest {
	public static void main(String[] args) {
		try {
			mehtod1();
		} catch (Exception e) {
			System.out.println("main");
		}
	}

	static void method1() throws Exception {
		try {
			throw new Exception();
		} catch (Exception e) {
			System.out.println("method1");
			throw e;
		}
	}
}
```

### 연결된 예외

한 예외가 다른 예외를 발생시킬 수도 있다. 어떠한 예외가 다른 예외를 발생시켰다면, 어떠한 예외를 원인 예외라고 부른다.

→ 여러가지 예외를 하나의 큰 분류의 예외로 묶어서 다루기 위해서 사용.

Throwable 클래스에 정의된 initCause()를 사용하여 원인예외를 등록할 수 있다.

```java
try {
	startInstall();
	copyFiles();
} catch (SpaceException e) {
	InstallException ie = new InstallException("install exception");
	ie.initCause(e);
	throw ie;
} 
...
```

CheckedException을 UncheckedException으로 감싸서 예외 처리를 강제하지 않을 수 있다.

```java
RuntimeException(Throwable cause) // 원인 예외를 등록하는 생성자

throw new RuntimeException(new MemoryException());
```

---

Reference

http://www.tcpschool.com/java/java_exception_class