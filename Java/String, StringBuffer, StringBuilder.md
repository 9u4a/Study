# String, StringBuffer, StringBuilder

**String은 불변하다 (immutable)**

→ 값을 수정하려면 새로운 객체를 생성하고 값을 재할당해야 한다.

```java
String text1 = "text";
text1 += "텍스트";
// text1 => "text텍스트"
```

기존 문자열에서 추가되었다고 생각할 수 있지만, 실제로는 새로운 String 객체를 만든 후 문자열을 저장해서 참조하도록 하는 과정이 일어난다.

```java
String str1 = "text";
String str2 = "text";
String str3 = new String("text");
String str4 = new String("text");
```

이 경우에는 str1과 str2는 동일한 값을 가지는 객체이므로 str2는 str1에서 만들어진 text를 재사용한다. → str1과 str2는 같은 객체이다 

그러나 str3, str4의 경우에는 str1에서 만들어진 text를 재사용하지 않고 new 연산자를 통하여 새로운 String 객체를 생성한다.

<aside>
💡 리터럴로 생성된 String 값은 String Constant Pool에서 관리가 되기 때문에 재사용 할 수 있지만,

new 연산자를 통하여 생성된 String 값은 각각 Heap 영역을 차지하기 때문에 재사용하여 생성 할 수 없다.

</aside>

### String이 불변한 이유

- **캐싱** : String Constant Pool이 존재하기에 문자열들을 상수화하여 다른 변수,객체들과 공유하게 되어 메모리 절약
    - 한 String객체를 1000번 사용해야 할 경우 1번의 생성으로 재사용을 할 수 있다.
- **보안** : String이 불변하지 않드면 특정 공격으로 값이 변경될 가능성이 있다.
    - String으로 다루어지는 모든 값들이 가변적이라면 소켓 통신에서 host,port 처럼 String값으로 다루어지는 정보들이 변경될 수 있다.
- **동기화 :** String 객체가 불변하다면 멀티스레드 환경에서 동시에 특정 String 객체를 참조하여도 안전하다.
    - 여러 애플리케이션에서 특정 String 객체를 참조하고 있을 때 그 값은 불변하므로 안전하다고 할 수 있다.

### String과 StringBuffer, StringBuilder

String이 불변이라서 생기는 장점도 있지만 단점들도 존재한다.

- 장점 :
    - 연산이 적게 사용되고, 문자열 값의 수정 없이 읽기가 많은 경우는 String클래스의 사용이 더 적합하다.
- 단점:
    - 기존 문자열을 변경하거나 수정하게 되는 경우 그 전에 사용하던 객체는 GC의 대상이 된다.
        - 성능상의 문제가 생길 수 있다.
    - String클래스가 덧셈 연산을 수행할 때마다 두 문자열을 모두 읽어 들이고 새로운 메모리에 복사한다.
        - StringBuilder,StringBuffer는 append() 사용

이러한 String의 단점을 보완할 수 있는 StringBuilder,StringBuffer가 존재한다.

StringBuilder 와 StringBuffer는 동일한 기능들을 제공하지만 한가지 차이점이 있다.

동기화 처리 문제에 대해 StringBuilder는 동기화를 지원하지 않고, StringBuffer는 동기화를 지원한다.

### StringBuffer

내부 구조

```java
public final class StringBuffer
    extends AbstractStringBuilder
    implements java.io.Serializable, Comparable<StringBuffer>, CharSequence
{
```

> A thread-safe, mutable sequence of characters. A string buffer is like a String, but can be modified. At any point in time it contains some particular sequence of characters, but the length and content of the sequence can be changed through certain method calls.
> 

**공식문서에 따르면 스레드로부터 안전하고 문자열과 비슷하지만 수정이 가능하다고 한다.** 

StringBuffer 클래스는 버퍼라고 하는 공간을 가진다.

버퍼 크기의 기본값은 16개의 문자를 저장할 수 있는 크기이며, 생성자를 통해 크기를 별도로 설정할 수 있다.

인스턴스 생성 시 설정한 크기보다 언제나 16개의 문자를 더 저장할 수 있도록 여유 있는 크기로 생성됩니다.

```java
//capacity()메소드를 이용하여 StringBuffer 인스턴스의 현재 버퍼 크기를 반환할 수 있다.
StringBuffer str1 = new StringBuffer();
StringBuffer str2 = new StringBuffer("text");
System.out.println(str1.capacity()); // 16
System.out.println(str2.capacity()); // 20
```

문자열 버퍼에는 기본적으로 용량이 제한되어 있지만 내부 버퍼가 오버플로우 되면 자동으로 더 커진다고 한다. 

- **동기화를 지원한다.**
    - 동기화 : 여러 스레드가 한 자원을 사용하려고 할 때 다른 스레드의 접근을 막는 것.
        - 데이터의 무결성 보장 ↔ 멀티스레드 작업에 안전한 환경.

### StringBuilder

내부 구조

```java
public final class StringBuilder
    extends AbstractStringBuilder
    implements java.io.Serializable, Comparable<StringBuilder>, CharSequence
{
```

**StringBuffer와 호환되는 API를 제공하지만 동기화를 보장하지 않는다. 문자열 버퍼가 단일 스레드에서 사용되는 경우 StringBuffer보다 우선적으로 StringBuilder를 사용하는 것을 권장한다.** 

**문자열 버퍼에는 기본적으로 용량이 제한되어 있지만 내부 버퍼가 오버플로우 되면 자동으로 더 커진다고 한다.** 

### 메소드

StringBuffer와 StringBuilder에서는 메소드를 사용하여 문자열을 변경한다.

**append(str) :** 전달된 값을 문자열로 변환한 후,해당 문자열의 마지막에 추가한다.

```java
StringBuffer str = new StringBuffer("text");
str.append("append"); // textappend
```

String클래스의 concat() 메소드와 같은 값을 반환하지만, 처리 속도가 훨씬 빠르다.

**insert(index, str)** : 지정된 지점에 문자열을 추가한다.

```java
StringBuffer str = new StringBuffer("text");
str.insert(3, "insert"); // texinsertt
```

**delete(start index, end index)** : 인덱스에 해당하는 부분 문자열을 문자열에서 제거한다.

(start index는 포함, end index는 포함되지 않는다)

```java
StringBuffer str = new StringBuffer("textdelete");
str.delete(4,9); // texte
```

---

Reference

https://junghyungil.tistory.com/70?category=892275

https://wonit.tistory.com/588

https://cjh5414.github.io/why-StringBuffer-and-StringBuilder-are-better-than-String/

http://www.tcpschool.com/java/java_api_stringBuffer

https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html

https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html