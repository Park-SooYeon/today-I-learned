# 예외 처리

## 목표

자바의 예외 처리에 대해 학습하세요.

## 학습할 것 (필수)

- 자바에서 예외 처리 방법 (try, catch, throw, throws, finally)
- 자바가 제공하는 예외 계층 구조
- Exception과 Error의 차이는?
- RuntimeException과 RE가 아닌 것의 차이는?
- 커스텀한 예외 만드는 방법

---

#### 예외 처리

**예외**란? 

**예외 처리**란? 프로그램 실행도중 문제가 일어났을 때 진행중인 것을 중단하고 다른 처리를 하는 것을 말한다.

### 자바에서 예외 처리 방법 (try, catch, throw, throws, finally)

**try-catch-finally**

Java에서 예외를 처리하는 대표적인 구문으로 아래와 같이 사용할 수 있다.

```java
try {
	// 정상적으로 실행할 코드
    // 하나 이상의 예외를 발생시킬 수 있는 코드
} catch (Exception error1) {
	// try 블록에 감싸진 코드에서 예외가 발생했을 때, 예외를 처리할 코드
    // 예외처리 1
} catch (Excpetion error2) {
    // 예외처리 2
} finally {
	// try 혹은 catch 블록 실행 후, 실행할 코드
    // finally 블록은 생략이 가능하며 메모리 해제, DB 연결 해제 등에 사용된다.
}
```

catch 블록은 여러개 사용할 수 있는데 그 중 **매개변수가 일치하는 단 하나의 catch 블록만 수행**한다. 이 때문에 예외처리를 할 때, **예외에 대한 우선순위를 정하고, 작은 단위의 예외부터 큰 단위의 예외 처리를 해주는 것이 좋다.**

```java
try {
	String str = null;
	boolean result = str.equals(""); // NullPointerException 발생
	System.out.println(result);
} catch (NullPointerException e) {
	System.out.println("NullPointerException 처리");
} catch (Exception e) {
    // Exception 클래스는 모든 Exception 클래스의 부모 클래스이다.
    // 따라서, 모든 에러를 매개변수로 받을 수 있다.
	System.out.println("모든 Exception 처리");
} finally {
	System.out.println("finally 블록 실행");
}

/* 결과
	NullPointerException 처리
	finally 블록 실행
*/
```

> try 블록에서 예외가 발생하면 catch 블록의 매개변수로 선언된 Exception 클래스를 instanceof 연산자를 이용하여 검사하고 검사 결과가 true인 catch 블록을 만날 때까지 검사한다.

> 예외가 발생했을 때 아래 메소드를 통해 예외에 대한 정보를 가져올 수 있다.
>
> * getMessage() : 발생한 예외 클래스의 객체에 저장된 메세지를 얻을 수 있다.
> * printStackTrace() : 예외 발생 당시 호출스택(Call Stack)에 있었던 메소드의 정보와 예외 메세지를 화면에 출력한다.

> **멀티 catch 블록**
>
> Java 7부터 여러 catch 블록을 `|`을 통해 하나의 catch 블록으로 합칠 수 있으며 아래와 같이 사용할 수 있다.
>
> ```java
> try {
> 	...
> } catch (ExceptionA | ExceptionB e) {
> 	e.printStackTrace();
> }
> ```
>
> 묶을 수 있는 예외 클래스의 개수에 제한이 없으며, 만약 묶인 예외 클래스의 관계가 부모-자식의 관계이면 컴파일 에러가 발생한다.

> **try-with-resources문**
>
> Java 7부터 지원하는 try-catch의 변형문으로 자동으로 자원을 반환하고자 할 때 사용한다.
>
> 해당 구문을 사용하면 finally 블록을 사용하여 메모리 및 DB 연결 등을 해제하지 않고 (close() 호출), 아래와 같이 사용할 수 있다.
>
> ```java
> try (FileInputStream fis = new FileInputStream("")){
> 	...
> } catch (IOException e) {
> 	// 예외처리
> }
> ```
>
> try 블록의 매개변수로 생성되는 객체를 **예외 발생 여부에 상관없이 JVM에서 자동으로 자원 해제 작업(close() 호출)을 수행한다.**
>
> Java 9부터는 try 블록의 매개변수로 객체 선언보다는 생성된 객체를 넣어줄 수 있도록 변경되었다.
>
> ```java
> FileInputStream fis = new FileInputStream("");
> try (fis) {
>     ...
> } catch (Exception e) {
>     // 예외처리
> }
> ```

**throw**

특정 상황에서 **강제로 예외를 발생**시킬 때 사용하는 키워드로 사용 방법은 아래와 같다.

```java
throw new RuntimeException(); // 예외를 발생시키고 싶은 부분에 작성
```

예외를 발생시키기 위해서는 Exception 객체를 생성해주어야하기 때문에 new 키워드와 함께 사용한다.

```java
try {
	System.out.println("try 블록");
	throw new RuntimeException("RuntimeExcpetion 발생"); // 강제로 예외 발생
} catch (NullPointerException e) {
	System.out.println("NullPointerException 처리");
    e.getMessage();
} catch (RuntimeException e) {
	System.out.println("RuntimeException 처리");
}

/* 결과
	try 블록
	RuntimeException 처리
	RuntimeExcpetion 발생
*/
```

**throws**

예외를 상위계층(해당 메소드를 호출한 부분)으로 던져서 상위계층에서 예외를 처리해주는 방식으로 아래와 같이 사용할 수 있다.

```java
public void testMethod throws Exception() {
	System.out.println("testMethod 실행");
	throw new RuntimeException("testMethod에서 Exception 발생"); // testMethod를 호출한 곳으로 예외 던짐
}

public void main(String[] args) {
	try {
		testMethod(); // 메소드 내부에서 던져진 예외 발생
	} catch (Exception e) {
		System.out.println(e.getMessage());
	}
}

/* 결과
	testMethod 실행
	testMethod에서 Exception 발생
*/
```

>  main 메소드에서 throws를 할 경우, JVM의 예외처리기로 예외 처리의 책임이 떠넘겨진다.
> 이 때 JVM은 에러가 발생했기 때문에 프로그램을 강제 종료한다.

<br/>

### 자바가 제공하는 예외 계층 구조

자바에서 예외 계층 구조는 아래 그림과 같다.

![image](https://user-images.githubusercontent.com/49746644/106725200-71ef7c00-664c-11eb-8a8d-b85bc26449ba.png)

모든 예외의 최고 조상은 Exception 클래스이며, **RuntimeException 클래스를 상속**받느냐에 따라 CheckedException과 Unchecked Exception으로 나뉜다.

<br/>

### Exception과 Error의 차이는?

Exception과 Error는 자바 실행 시 발생할 수 있는 프로그램 오류를 말한다.

* Error(에러)

  메모리 부족, 스택오버플로우와 같은 **복구할 수 없는 심각한 수준의 오류**를 뜻한다.

  시스템에 비정상적인 상황이 발생한 경우로 개발자가 직접적으로 대처할 방법이 없는 문제들을 의미한다. (`System Level`의 문제)

* Exception(예외)

  개발자의 실수로 발생된 오류로 개발자가 대처할 수 있는 오류를 뜻한다. (`Application Level`의 문제)

<br/>

### RuntimeException과 RE가 아닌 것의 차이는?

Exception의 종류에는 RuntimeException 클래스 상속 여부에 따라 Checked Exception과 Unchecked Exception으로 나눌 수 있다.

* Checked Exception : **사용자의 실수**와 같은 외적인 요인에 의해 발생하는 예외
* Unchecked Exception : **개발자의 실수**로 발생하는 예외

각 Exception의 특징은 아래와 같다.

![image](https://user-images.githubusercontent.com/49746644/106740519-dcf57e80-665d-11eb-87d3-b4e93d4794d5.png)

> Ref : https://www.nextree.co.kr/p3239/

<br/>

### 커스텀한 예외 만드는 방법

자바의 모든 Exception 클래스는 Exception 클래스를 상속받은 자식 클래스이다.

따라서, 커스텀한 예외를 만들기 위해서는 **Exception 클래스를 상속**받은 클래스를 만들어 사용하면된다.

```java
class TestException extends Exception {
	TestException(String msg) {
		super(msg); // getMessage로 불러낼 예외 메세지를 담음
	}
}
```

커스텀 예외 객체를 만들면 throw 키워드로 예외를 생성할 수 있다.

```java
throw new TestException();
```

> 기존은 Exception을 상속받아서 Checked Exception으로 많이 작성하였으나, 요즘은 예외처리를 선택적으로 할 수 있또록 **RuntimeException을 상속**받아 Unchecked Exception을 자성하는 쪽으로 바뀌어가고 있따고 한다.

#### 참고 자료

https://yadon079.github.io/2021/java%20study%20halle/week-09

https://blog.naver.com/swoh1227/222203699618
