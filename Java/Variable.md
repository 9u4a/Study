## 변수(Variable)

단 하나의 값을 저장할 수 있는 메모리 공간

### 변수의 선언과 초기화

변수를 사용하기에 앞서 선언과 초기화를 해야한다.

클래스, 인스턴스 변수는 초기화를 생략할 수 있지만 지역변수는 초기화를 반드시 해야 함.

```java
//변수 선언
int a;
int b;
int c = 10;
```

### 변수의 명명규칙

- 대소문자 구분, 길이 제한 x
- 예약어 x
- 숫자로 시작 x
- 특수문자는 _  $만 사용가능

## 변수의 타입

자료형 : 값의 종류에 따라 저장될 공간의 크기와 저장형식을 정의한 것.

### 기본형과 참조형

**기본형 (Primitive type) :** 실제 값을 저장함 (Stack에 저장)

- OS에 상관없는 자료형의 길이
- 비객체 타입으로 NULL 값을 가질 수 없음.
- NULL값을 넣고 싶다면 Wrapper Class를 활용해서 사용해야 함.

![image](https://github.com/9u4a/Study/assets/81855010/ebae254a-1535-4425-b8a5-35eb52be1033)

**참조형 (Reference type) :** 객체의 주소를 저장함 (Heap에 저장)

- 기본형 8개를 제외한 나머지 타입 (Class, Interface, Enum, Array)
- 빈 객체를 의미하는 NULL이 존재
- 객체나 배열을 NULL값으로 받으면 NPE 발생

참조형 변수를 선언할 때는 변수의 타입으로 클래스의 이름을 사용한다.

```java
//클래스이름 변수이름;
Date today = new Date(); 
//Date객체를 생성해서 주소를 today에 저장
```

참조변수는 NULL 또는 객체의 주소를 값으로 가진다.

## 상수와 리터럴 (constant, literal)

**상수** : 상수는 변수와 달리 값을 저장하면 다른 값으로 변경할 수 없다. 

변수의 타입 앞에 final 키워드를 붙여서 사용, 상수는 반드시 선언과 동시에 초기화해야 함.

→ jdk1.6부터 사용하기 전에만 초기화하면 되도록 변경

```java
//선언, 초기화
final int MAX_SPEED = 10; 
```

**리터럴 :** 값 그자체를 뜻 함. 프로그래밍에서는 상수를 저장하면 변경할 수 없는 저장공간으로 정의해서 이와 구분하기 위해 리터럴이라고 부름.

```java
//변수 = 리터럴;
int year = 2000;
```

- long 타입은 접미사 L,l 사용
- int, byte, short 타입은 접미사x
- float 타입은 접미사 F,f 사용
- double 타입은 접미사 D,d 사용 (생략 가능)
- 16진수는 접두사 0x 사용
- 8진수는 접두사 0 사용
- 2진수는 접두사 0b 사용
- 리터럴에 소수점이나 10의 제곱을 나타내는 E,e 혹은 접미사 f,d 를 포함하면 실수형 리터럴로 취급

```java
flaot pi = 3.14; // float타입 변수에 double타입 리터럴 저장불가 접미사 f 필요
double pi = 3.14; // double타입 변수에 double타입 리터럴 저장
```

### 타입의 불일치

리터럴의 타입은 저장될 변수의 타입과 일치하는 것이 보통이지만, 타입이 달라도 저장범위가 넓은 타입에 좁은 타입의 값을 저장하는 것은 허용.

```java
int i = 'A';
long l = 123;
dobule d = 3.14f;
```

### 문자 리터럴과 문자열 리터럴

문자 리터럴 :‘A’와 같이 작은 따옴표로 문자 하나를 감싼 것

문자열 리터럴 : “STR” 같이 두글자 이상을 큰 따옴표로 감싼 것

String 객체를 new 키워드 없이 리터럴로 생성시 String Constant Pool에 저장된다

```java
String str = ""; // 빈 문자열 저장 가능
char ch = ''; // 문자 필요
char ch = ' '; //공백 문자로 초기화 가능
String name1 = new Stirng("JAVA") // String 객체 생성 Heap에 저장
String name2 = "JAVA" // String 상수 값으로 관리 String Constant Pool에 저장
```

**printf() 지시자**

- **%b :** 불리언(boolean) 형식으로 출력
- **%d :** 10진수(decimal) 형식으로 출력
- **%o :** 8진수(octal) 형식으로 출력
- **%x, %X :** 16진수(hexa-decimal) 형식으로 출력
- **%f :** 부동 소수점(floating-point) 형식으로 출력
- **%e, %E :** 지수(exponent) 형식으로 출력
- **%c :** 문자(character) 형식으로 출력
- **%s :** 문자열(String) 형식으로 출력 ****

## 정수형 - byte,short,int,long

### 정수형의 표현형식

어떤 진법의 리터럴을 변수에 저장해도 실제로는 2진수로 바뀌어 저장된다.

모든 정수형은 부호있는 정수 이므로 첫 번째 비트를 부호 비트로 사용한다.

(양수는 0, 음수는 1)

![image](https://github.com/9u4a/Study/assets/81855010/14ca68c6-12b4-4f5b-aac0-f921a35098a2)


### 정수형의 오버플로우

오버플로우 : 타입이 표현할 수 있는 값의 범위를 넘어서는 것

```java
최대값 + 1 = 최소값
최소값 - 1 = 최대값
```

## 실수형 - float, double

실수형은 값을  부동 소수점수의 형태로 저장한다. 

부동 소수점수는 부호(S), 지수(E) , 가수(M)의 세 부분으로 이루어져 있다.

![image](https://github.com/9u4a/Study/assets/81855010/69f5d3c2-4e0d-4e70-a99e-64d8e78085ad)

![image](https://github.com/9u4a/Study/assets/81855010/b9145fbd-7a7d-47dc-9c87-9a8ed24c9a3b)

- 부호 : S는 부호비트를 의미하며 1bit이다. 0이면 양수 1이면 음수.
- 지수 : E는 지수를 저장하는 부분으로 부호있는 정수를 뜻한다.
- 가수 : M은 실제 값인 가수를 저장하는 부분으로 정밀도를 뜻한다.
    - float의 경우 2진수 23자리를 저장할 수 있음
    - double의 경우 2진수 52자리를 저장할 수 있음

 

## 형변환 (Casting)

형변환이란 변수 또는 상수의 타입을 다른 타입으로 변환하는 것이다.

형변환의 방법은 형변환하고자 하는 변수나 리터럴의 앞에 변환하고자 하는 타입을 붙여주면 된다.

```java
//(타입)피연산자
double d = 85.4;
int score = (int)d;
```

기본형에서 boolean을 제외한 나머지 타입들은 서로 형변환이 가능하지만 기본형과 참조형간의 형변환은 불가능하다.

큰 타입에서 작은 타입으로의 변환하는 경우는 크기의 차이만큼 값이 손실된다.

반대로 작은 타입에서 큰 타입으로의 변환하는 경우는 형변환 후 나머지 빈 공간은 0,1로 채워진다.

→ 보통 0으로 채우지만, 변환하려는 값이 음수인 경우에는 빈 공간을 1로 채움

float 타입의 범위를 넘는 값을 float 타입으로 형변환 하는 경우 ± 무한대 또는 ± 0을 얻는다.

### 자동 형변환

경우에 따라 편의상의 이유로 형변환을 생략할 수 있는데 컴파일러가 생략된 형변환을 자동적으로 추가한다.

**기존의 값을 최대한 보존할 수 있는 타입으로 자동 형변환이 이루어진다.**

→ 표현 범위가 더 넓은 쪽으로 형변환

![image](https://github.com/9u4a/Study/assets/81855010/d4880636-fa71-47ce-87c5-6c6261d31b69)



---

Reference

https://jiwondev.tistory.com/114

https://jeeneee.dev/java-live-study/week2-datatype-variable-array/

[https://coder-in-war.tistory.com/entry/Java-13-Java의-자료형-Primitive-type-Reference-type](https://coder-in-war.tistory.com/entry/Java-13-Java%EC%9D%98-%EC%9E%90%EB%A3%8C%ED%98%95-Primitive-type-Reference-type)