# 배열(Array)

## 배열

배열이란 같은 타입의 여러 변수를 하나의 묶음으로 다루는 것이다.

### 배열의 선언과 생성

```java
//배열의 선언

//타입[] 변수이름
int[] score;

//타입 변수이름[]
int score[];
```

```java
//배열의 생성
score = new int[5];
int[] score = new int[5];
```

연산자 new에 의해서 메모리에 score배열을 저장할 공간이 마련된 후 기본값으로 초기화 된다. 참조변수에 배열의 주소가 저장된다.

### 배열의 길이

자바에서는 JVM이 모든 배열의 길이를 별도로 관리하며, 배열이름.length를 통해 배열의 길이를 얻을 수 있다. 배열의 길이는 한번 선언되고 나면 길이를 변경할 수 없다.

```java
int[] arr = new int[5];
int tmp = arr.length;
```

### 배열의 초기화

배열은 생성과 동시에 자동적으로 타입에 해당하는 기본값으로 초기화되므로 사용하기 전에 따로 초기화를 해주지 않아도 된다. 원하는 값을 지정하려면 각 요소마다 값을 지정해 줘야한다.

```java
//생성과 초기화
int[] score = new int[]{50, 60, 70, 80, 90};
int[] score = {50, 60, 70, 80, 90};
```

### System.arraycopy()를 이용한 배열의 복사

for문 대신 System클래스를 사용하면 간단하고 빠르게 배열을 복사할 수 있다.

```java
for(int i=0; i < num.length; i++) { newNum[i] = num[i]; }

-> for문을 arraycopy() 이용으로 변경

System.arraycopy(num, 0, newNum, 0, num.length);
//num 배열 0번 인덱스에서 num.length만큼 newNum 0번 인덱스부터 데이터를 채움
```

## String배열

### String배열의 선언과 생성

```java
String[] name = new String[3];
```

**기본형 배열이 아닌 참조형 배열의 경우 실제 객체가 아닌 객체의 주소가 저장됨**

메모리에 저장된 주소인 4byte의 정수값 또는 NULL이 저장된다.

### String클래스의 주요 메소드

```java
char charAt(int index) //문자열에서 해당 위치에 있는 문자를 반환한다.

int length() //문자열의 길이를 반환한다.

String substring(int from, int to) // 문자열에서 from ~ to -1에 있는 문자열을 반환한다.

boolean equlas(Object obj) // 문자열의 내용이 obj와 같은지 확인 같으면 true, 다르면 false 반환한다.

char[] toCharArray()) // 문자열을 문자배열(char[])로 변환해서 반환한다.
```

## 다차원 배열

### 2차원 배열의 선언과 인덱스

```java
//타입[][] 변수이름;
int[][] score;

//타입 변수이름[][];
int score[][];

//타입[] 변수이름[];
int[] score[];
```

### 2차원 배열의 초기화

```java
int[][] arr = new int[][] { {1,2,3}, {4,5,6} };
int[][] arr = { {1,2,3}, {4,5,6} };
```

2차원 배열은 배열의 배열로 구성되어 있다.

주소값을 가지고 접근

![image](https://github.com/9u4a/Study/assets/81855010/8a27680a-65c2-4835-83f1-4c3d56efcf7c)

---

Reference

https://blog.hexabrain.net/112