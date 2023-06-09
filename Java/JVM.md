**JVM에 대하여 얘기하기 전에 JDK에 대해 알아보자.**

![Untitled](https://github.com/9u4a/Study/assets/81855010/6628981c-4a33-488b-a65e-88bd8b93df5f)



### JDK (Java Development Kit)

자바 개발에서 가장 널리 사용되는 SDK로 컴파일러, JRE, javac 등 개발도구를 포함한다.

**JDK의 bin에 포함된 주요 개발 도구**

- **javac** : 자바소스를 바이트코드로 변환하는 컴파일러
- **java** : JVM을 작동시켜 자바프로그램 실행하는 인터프리터
- **javadoc** : 자바소스로부터 HTML 형식의 API doc 생성
- **jar** : 자바 클래스들을 압축한 자바 아카이브 파일 생성 관리(.jar)
- **jlink** : 응용프로그램에 맞춘 맞춤형 JRE 제공
- **jdb** : 실행 중 오류를 찾는 데 사용하는 디버거
- **javap** : 클래스 파일의 바이트코드를 소스와 함께 보여주는 디어셈블러

### JRE (Java Runtime Environment)

자바 실행 환경으로 JVM과 자바 클래스 라이브러리로 구성되어 자바 애플리케이션의 실행을 지원

(운영 체제에 종속)

### JVM(Java Virtual Machine)

자바 바이트코드를 해석하고 실행하는 가상 기계로 JVM은 자바 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행한다. 플랫폼 독립성 제공

## 자바코드 수행 과정

![Untitled](https://github.com/9u4a/Study/assets/81855010/f5a62e89-7049-4ea8-a609-4e1e2f7d0549)

1. 컴파일러가 소스코드를 바이트코드로 컴파일
2. 클래스 로더가 런타임 데이터 영역에 로딩
3. 실행 엔진이 자바 바이트코드 실행

## 클래스 로더 (Class Loader)

![Untitled](https://github.com/9u4a/Study/assets/81855010/494184b0-50e5-4146-bc18-22126e42db11)


자바는 컴파일타임이 아니라 런타임에 클래스를 처음으로 참조할 때 해당 클래스를 로드하고 링크하는 특징이 있다.  클래스 로더는 클래스 파일을 동적으로 로드하는 컴포넌트다.

### 클래스 로더의 특징

- **계층 구조** : 클래스 로더끼리 부모-자식 관계를 이루어 자식 클래스 로더가 부모 클래스 로더가 로드한 클래스에 접근 가능. → 부모 클래스로더는 자식 클래스 로더에서 클래스 로드 불가(책임 분리)
- **위임 모델** : 계층 구조를 바탕으로 클래스를 로드할 때 먼저 상위 클래스 로더를 확인하고 클래스를 찾지 못하면 그 후에 요청을 받은 클래스 로더가 클래스를 로드한다.
- **언로드 불가** : 클래스 로더는 클래스를 로드할 수는 있지만 언로드할 수 없다. 현재 클래스 로더를 삭제하고 새로운 클래스 로더 생성은 가능.

### 클래스 로더의 계층 구조

- **BootStrap Class Loader**
    - JVM 시작시 실행.  java.lang package 처럼 JVM 실행에 필요한 클래스 로딩.
    - 다른 클래스 로더와 달리 네이티브 코드로 구현됨
- **Platform Class Loader**
    - java.lang.ClassLoader의 인스턴스로 자바에서 기본적으로 제공하는 클래스(Java SE API 등) 로딩시 사용 → 보안 확장 기능 등 로딩
    - 모듈 시스템 도입되면서 Extension Class Loader에서 명칭 변경
- **System Class Loader**
    - java.lang.ClassLoader의 인스턴스로 유저가 작성한 클래스를 로딩할 때 사용됨
    - 모듈 시스템 도입되면서 Application Class Loader에서 명칭 변경

## 클래스 로더의 수행 과정

![Untitled](https://github.com/9u4a/Study/assets/81855010/8908f102-d8d7-4820-bda5-0af21bc99a28)

자바 애플리케이션의 동작은 로딩,링크,초기화 과정을 거쳐 **최종적으로 특정 클래스의 main함수를 실행하는 것이다**. 해당 과정을 실행하면서 다른 클래스들을 로딩,링크,초기화한다.

### **JVM 실행**

JVM이 시작되면 런타임 데이터 영역이 생성되고 메소드,힙 영역이 할당된다. BootStrap Class Loader는 JVM 실행에 필요한 클래스들을 메소드 영역으로 로딩, System Class Loader를 통해 실행한 클래스를 메소드 영역으로 로딩한다.

### 로딩

클래스 또는 인터페이스의 생성은 해당 클래스의 필드,메소드, 런타임 상수 풀 등 클래스의 바이트코드를 찾은 후 JVM의 메소드 영역에 구성하는 것을 의미. 클래스 로더를 통해 로딩을 진행하며 해당 클래스를 로딩했을 때 클래스의 부모 클래스가 존재할 경우 먼저 부모 클래스를 로딩한다.

### 링크

링크 과정은 로딩된 클래스 파일들에 대해 실제 실행 가능한 바이너리 코드로 변환하는 작업을 수행한다. 검증, 준비, 분석의 3단계로 이루어져 있다.

1. **검증 (Verification)** : 로딩된 클래스의 바이트 코드가 JVM의 명세를 따르고 있는지 검증(보안성, 안전성 요구사항 등)
2. **준비 (Preparation)** : 클래스의 정적 변수들을 메모리에 할당하고 초기화한다.(기본값으로 초기화→ Reference Type은 null)
3. **분석 (Resolution)**: 클래스의 런타임 상수 풀 안에 있는 심볼릭 참조를 실제 메모리 상의 고정된 주소 값으로 바꾸는 과정

검증, 준비, 분석 3가지 과정을 거치면서 다른 클래스의 로딩을 추가적으로 요청할 수 있다. 이 때 분석 과정은 검증, 준비 과정과 같은 시간에 일어날 필요가 없다. 보통 심볼릭 참조를 고정된 주소 값으로 변환시키는 분석 과정은 해당 명령이 실행될 때 일어난다.

### 초기화

초기화 단계에서는 클래스의 정적 변수들과 정적 초기화 블록(static)을 실행한다. 개발자가 명시한 값으로 초기화 되거나, 기본 값으로 초기화 된다. 로딩,검증,준비 과정이 모두 끝났을 때 한번만 실행된다.

### 실행

초기화가 완료된 클래스는 JVM이 실행 엔진에 의해 바이트코드가 해석하며 실행한다. 실행 엔진은 바이트코드를 인터프리터 방식이나 JIT 컴파일러를 사용하여 실행한다. 실행단계에서 메모리, 스레드 관리 등의 작업 수행.

### JVM 종료

일부 스레드가 Runtime 클래스의 종료, 중지 메서드나 클래스 시스템의 종료 메서드를 호출하면 JVM 종료 또는 중지 작업이 Security Managerr에 의해 허용된다.
## Runtime Data Area

![image](https://github.com/9u4a/Study/assets/81855010/e47efd2c-f8d1-4a83-bd54-fa8a2dfbf5ba)


Runtime Data Area는 JVM이 프로그램을 수행하기 위해 OS로부터 할당 받는 메모리 영역이다.

Java Stack 영역과 PC Register, Native Method Stack은 각 스레드 별로 생성이 되고,

Method Area 와 Heap은 모든 스레드에게 공유된다.

### Method Area

모든 클래스와 인터페이스의 코드, 상수, 필드, 메소드 등의 정보를 저장하는 영역.

클래스 로더에 의해 로드되며, 프로그램 실행 중에는 변경되지 않는다.

동일한 클래스의 인스턴스가 여러 개 생성되어도 해당 클래스에 대한 정보는 중복 저장되지 않는다.

→ 효율적인 메모리 관리

**Method Area에 저장되는 정보**

- **클래스 정보 :** 클래스의 전체 이름, 접근 제어자, 상위 클래스와 인터페이스, 구현하는 인터페이스 등 클래스에 대한 정보를 저장
- **필드 정보 :** 클래스의 정적 변수와 상수 필드에 대한 정보 (필드의 이름, 타입, 접근 제어자, 초기값 등)를 저장
- **Constant Pool :** 클래스 파일 내에서 사용되는 상수 값들의 **실제 데이터** (문자열, 정수, 부동 소수점, 클래스 및 인터페이스의 참조 등을 포함)를 저장. **Constant Pool을 통해 메소드나 필드의 실제 데이터를 참조하여 중복을 막음**
- **메소드 정보 :** 클래스의 메소드에 대한 정보 ( 메소드의 이름, signature, 접근 제어자, 코드의 주소 등) **실제 메소드 코드는 Java Stack 영역에 저장**

### Heap

[![image](https://github.com/9u4a/Study/assets/81855010/e47efd2c-f8d1-4a83-bd54-fa8a2dfbf5ba)
](https://file.notion.so/f/s/33f04501-78c6-46fd-b728-8d86b340a0d7/Untitled.png?id=771171e9-6f98-46ae-a108-08c67b9fc810&table=block&spaceId=09371ed9-bf30-4aaf-8fdb-69efaba197b9&expirationTimestamp=1686384073197&signature=zLCmRPIfzJYuA3FHe9JEIAKdScdvC0YomtCanfVw_ag&downloadName=Untitled.png)

동적으로 할당되는 객체 인스턴스와 배열을 저장하는 영역(실행 중 크기 조정 가능).

생성된 모든 객체는 Heap에 할당

Garbage Collector (GC라 부르겠다) 에 의해 관리.

**Heap에 저장되는 정보**

- **객체 인스턴스 :**  프로그램에서 생성된 클래스의 객체 인스턴스 (인스턴스 변수, 메소드, 객체의 상태를 나타내는 데이터 등) new 키워드를 이용하여 동적으로 할당 → 더 이상 참조 되지 않을 때 GC가 메모리에서 제거.
- **배열 :** 자바에서 배열은 객체로 간주하여 Heap에 할당.

**Heap의 구조**

- **New/Young Generation**
    - **Eden :** 객체가 처음 생성되는 곳
    - **From (Survivor 영역) :**  Eden 영역이 가득 차 GC가 발생한 후 살아남은 객체가 이동되는 곳.
    - **To (Survivor 영역) :** Eden 영역이 다시 가득 차 GC가 발생한 후 From 영역에서 살아남은 객체가 이동되는 곳
- **Old/Tenured Generation :** Young Generation에서 일정 시간 살아남은 객체들이 이동하는 곳. Young Generation보다 크기가 더 크고 객체들이 더 오래 유지된다.
- **Metaspace :**  Java 8 이전에 존재하던 Permanent Generation의 대체 영역으로 도입. **클래스 메타데이터, 정적 변수 등 JVM 내부에서 관리해야 하는 메타 데이터를 저장한다.** Heap 외부에 할당되는 메모리 영역으로 (Native Area). 자동 크기 조정을 지원해 PermGen 에서 발생할 수 있는 메모리 부족 현상을 완화시킴.

Minor GC, Major GC에 대해서는 Garbage Collector에 대하여 정리할 때 다룰 예정.

### Java Stack

각 스레드마다 할당되는 영역.

메소드를 호출할 때마다 Stack Frame이 생성 (Local Variable, Operand Stack, Runtime Constant Pool에 대한 참조 포함) 메소드 종료 시 Stack Frame 제거

**Java Stack의 특징**

- 스택 기반: 스택(LIFO)의 동작 원리를 따르며 메소드 호출 시 해당 메소드의 Frame이 스택의 맨 위에 쌓이고, 메소드 종료 시 스택에서 제거 → 메소드 호출의 순서를 추적, 재귀 호출 지원

### **PC Register**

스레드가 시작될 때 생성되는 공간으로 스레드마다 독립적으로 존재 현재 실행 중인 스레드가 다음에 실행할 명령어의 주소를 가리키는 역할. → 독립적으로 존재하여 여러 스레드가 실행 가능함. JVM이 명령어를 순차적으로 실행할 수 있게 한다.

### Native Method Stack

자바 언어 이외의 언어로 작성된 네이티브 코드를 실행하는 데 사용.

네이티브 메소드 호출 시 Frame 생성 호출 완료시 스택에서 제거

---

Reference

https://d2.naver.com/helloworld/1230

[https://velog.io/@sgwon1996/JAVA의-동작-원리와-JVM-구조](https://velog.io/@sgwon1996/JAVA%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC%EC%99%80-JVM-%EA%B5%AC%EC%A1%B0)

https://coding-factory.tistory.com/828

https://coding-factory.tistory.com/827

https://kkang-joo.tistory.com/18

https://www.samsungsds.com/kr/insights/1232761_4627.html

https://inspirit941.tistory.com/294

https://junghyungil.tistory.com/94?category=892275

https://www.holaxprogramming.com/2013/07/20/java-jvm-gc/

[https://jaemunbro.medium.com/java-metaspace에-대해-알아보자-ac363816d35e](https://jaemunbro.medium.com/java-metaspace%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-ac363816d35e)

https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html