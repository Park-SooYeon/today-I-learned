# 람다식

## 목표

자바의 람다식에 대해 학습하세요.

## 학습할 것 (필수)

- 람다식 사용법
- 함수형 인터페이스
- Variable Capture
- 메소드, 생성자 레퍼런스

---

### 람다식 사용법

**람다식** 이란? **식별자 없이 실행가능한 함수**로 메소드를 하나의 식으로 표현한 것이다.

람다식을 사용하면 메소드의 이름과 반환값이 없어지므로 **익명 함수(anonymous function)**라고도 한다.

람다식의 장단점은 다음과 같다.

**장점**

* 코드를 간결하게 만들 수 있다.
* 가독성이 향상된다.
* 멀티쓰레드환경에서 용이하다.
* 함수를 만드는 과정 없이 한번에 처리하기에 생산성이 높아진다.

**단점**

* 람다로 인한 익명 함수는 재사용이 불가능하다.
* 디버깅이 많이 까다롭다.
* 람다를 무분별하게 사용하면 코드가 지저분해진다.
* 재귀로 만들 경우 부적합하다.

이러한 람다식의 구조는 다음과 같다.

```java
// 매개변수가 여러개 있을 경우
(매개변수1, 매개변수2) -> 값 // return이 생략된 형태, 값이 return된다.
(매개변수) -> { 바디 } // 매개변수를 받고 바디를 실행하는 함수

// 매개변수가 하나일 경우 () 생략 가능
// 단, 매개변수의 타입이 정의되어 있으면 생략할 수 없다.
매개변수 -> 값
매개변수 -> { 바디 }

// 매개변수가 없을 경우
() -> 값
() -> { 바디 }
```

람다식을 사용한 예는 다음과 같다.

```java
// nx2의 2차원 배열을 [n][0], [n][1] 순으로 오름차순 정렬하는 식
Arrays.sort(array, (o1, o2) -> {
	if(o1[0] == o2[0]) return Integer.compare(o1[1], o2[1]);
	return Integer.compare(o1[0], o2[0]);
});
```

선언된 매개변수의 타입이 추론이 가능한 경우 위 예제와 같이 생략할 수 있다.

> 메소드와 함수의 차이?
> 메소드 : **클래스, 구조체, 열거형에 포함**되어 있는 함수, 클래스 함수라고도 한다
> 함수 : **독립된 기능**을 수행하는 단위
>
> Ref : https://zeddios.tistory.com/233

<br/>

### 함수형 인터페이스

**함수형 인터페이스** 란? **1개의 추상 메소드**를 가지고 있는 인터페이스를 말하며 Single Abstract Method(SAM)라고도 한다.

자바의 모든 메소드는 클래스 내에 포함되어야 하므로 람다식 또한 익명 객체에 포함되어야한다.

이 때, 람다식으로 정의된 익명 객체를 만들 때, 함수형 인터페이스가 사용된다.

```java
public interface CustomFunctional {
	public int sum(int a, int b);
}

// 익명 객체 선언
CustomFunctional func1 = new CustomFunctional() {
    @Override
    public int sum(int a, int b) {
        return a + b;
    }
};
CustomFunctional func2 = (a, b) -> a + b; // 익명 객체를 람다식으로 대체

System.out.println("func1.sum(1, 2) = " + func1.sum(1, 2)); // func1.sum(1, 2) = 3
System.out.println("func2.sum(1, 2) = " + func2.sum(1, 2)); // func2.sum(1, 2) = 3
```

CustomFunctional 인터페이스를 구현한 **익명 객체의 메서드와 람다식의 매개변수 타입과 개수 그리고 반환값이 일치하기 때문에** CustomFunctional 인터페이스를 구현한 익명 객체를 람다식으로 대체할 수 있다.

자바는 Java 8부터 람다식을 지원하면서 함수형 인터페이스 기반의 **java.util.function** 패키지를 지원한다.

[java.util.function](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html) 패키지에서 지원하는 함수형 인터페이스는 다음과 같다. 이 외에도 많은 함수형 인터페이스를 지원한다.

| Interface     | Function Descriptor | Abstract method    |
| ------------- | ------------------- | ------------------ |
| Predicate     | (T) -> boolean      | boolean test(T t); |
| Consumer      | (T) -> void         | void accept(T t);  |
| Function      | (T) -> R            | R apply(T t);      |
| Supplier      | () -> T             | T get();           |
| UnaryOperator | (T) -> T            | T apply(T t);      |

> @FunctionalInterface : 해당 인터페이스가 함수형 인터페이스라고 명시하고, 컴파일러가 SAM 여부를 체크하도록 하는 어노테이션

> 인터페이스의 메소드가 static, default 메소드여도 메소드가 하나라면 함수형 인터페이스이다.

> Ref : https://tourspace.tistory.com/6

<br/>

### Variable Capture

**Variable Scope**

익명 클래스는 새로운 scope를 가지기 때문에 다음과 같이 변수를 로컬변수로 재정의하여 사용할 수 있따.

```java
public interface CustomFunctional {
	public void print();
}

int a = 100;

CustomFunctional func = new CustomFunctional() {
	@Override
	public void print() {
		int a = 10;
		System.out.println(a);
	}
};

func.print(); // 10
```

그러나 람다식에서는 변수의 scope가 **람다를 감싸고 있는 scope를 공유**한다. 이를 `쉐도윙 하지 않는다`고 한다. 따라서, 로컬 변수를 재정의 할 수 없다.

```java
int a = 100;

CustomFunctional func = () -> {
	// int a = 10; // 컴파일 에러 발생
	System.out.println(a);
};

func.print(); // 100
```

**final / effective final**

람다식을 사용할 때, 람다식은 **final 혹은 effective final 변수만 참조할 수 있다**.

```java
// fianl 변수 참조
final int a = 100;

CustomFunctional func1 = () -> {
    System.out.println(a);
};

func1.print(); // 100

// effective final 변수 참조
int b = 200;

CustomFunctional func2 = () -> {
    System.out.println(b);
};

func2.print(); // 200
```

그 외, 가변하는 변수에 대해서는 참조할 수 없다.

```java
int a = 100;
a = 10; // 변수의 값을 변경했으므로 a는 effective final 변수가 아니다.

CustomFunctional func = () -> {
	System.out.println(a); // 컴파일 에러 발생
};
```

> effective final : final이 붙지 않았으나, 변수의 값이 변경되지 않는 것
> => Java 8부터 지원하는 기능으로 익명 클래스 구현체 또는 람다에서 참조할 수 있다.

> 가변하는 변수는 람다식 내부에서 참조할 수 없는 이유?
>
> 1. 람다식에서 사용되는 외부 지역 변수는 복사본이다.
>    *메소드 내 지역변수를 참조하는 람다식을 리턴하는 메소드가 있을 경우, 메소드 block이 끝나면 추후에 람다식이 수행될때 참조할 수 없다.*
> 2. 지역 변수를 관리하는 쓰레드와 람다식이 실행되는 쓰레드가 다를 수 있다.
>    *스택은 각 쓰레드의 고유의 공간이고, 쓰레드끼리 공유되지 않기 때문에 마찬가지로 람다식이 수행될 때 값을 참조할 수 없다.*
>
> 따라서, 람다식이 어떤 쓰레드에서 수행될지 알 수 없기 때문에 최신 값을 보장하기 위해 final, effective final이어야 한다.
>
> ```java
> public interface CustomFunctional {
>   	public void print();
> }
> 
> public class CustomLambda {
>     public static CustomFunctional getFunction() {
>         // int a는 getFunction()이 호출되어 실행하는 동안만 존재하는 지역 변수
>         int a = 100;
>         
>         CustomFunctional func = () -> System.out.println(a);
>         
>         return func;
>     }
>     
> 	public static void main(String[] args) {
> 		CustomFunctional func = getFunction();
>         
>         // getFunction()의 실행이 끝나 int a는 스택에서 사라졌지만 생성한 람다식을 수행할 때 사용할 수 있다. => JS의 클로저와 비슷한 개념
>         // 람다식이 생성될 때, 람다를 감싸고 있는 scope를 복사하여 가지고 있기 때문에 사용 가능하다.
>         func.print(); // 100
> 	}
> }
> ```

<br/>

### 메소드, 생성자 레퍼런스

**메소드 레퍼런스** 란? 람다식을 더 간단하게 표현할 수 있는 방법으로 **전달하는 인수와 사용하려는 메소드의 인수 형태가 같다면** 메소드 레퍼런스를 사용해서 매우 간결하게 표현할 수 있다.

**메소드 레퍼런스의 종류**는 다음과 같다.

1. Construnctor Reference
2. Static Method Reference
3. Instance Method Reference

**Constructor Reference**

> (type)::new

```java
() -> new String
String::new
```

```java
UnaryOperator<String> stringOperator = str -> new String(str);
UnaryOperator<String> refStringOperator = String::new;

String test1 = stringOperator.apply("Constructor Test1");
String test2 = refStringOperator.apply("Constructor Test2");

System.out.println(test1); // Constructor Test1
System.out.println(test2); // Constructor Test2
```

**Static Method Reference**

> (type)::(Static Method)
> (매개변수) -> Class.staticMethod(매개변수)

```java
str -> String.valueOf(str)
String::valueOf
```

```java
Consumer<Integer> consumer = a -> System.out.println(a);
Consumer<Integer> refConsumer = System.out::println;

consumer.accept(100); // 100
refConsumer.accept(200); // 200
```

**Instance Method Reference**

> (Object Reference)::(Instance Method)
> (obj, 매개변수) -> obj.instanceMethod(매개변수)

임의의 객체의 인스턴스 메소드를 참조할 때 사용

```java
(object) -> object.toString()
Object::toString
```

```java
// 매개변수가 필요 없는 경우
UnaryOperator<String> operator = str -> str.toLowerCase();
UnaryOperator<String> refOperator = String::toLowerCase;

String test1 = operator.apply("TEST1");
String test2 = refOperator.apply("TEST2");

System.out.println(test1); // test1
System.out.println(test2); // test2

// 매개변수가 필요한 경우
String[] names = {"A", "B", "D", "C"};
Arrays.sort(names, (str, x) -> str.compareToIgnoreCase(x));
Arrays.sort(names, String::compareToIgnoreCase);
```

> Ref : https://codechacha.com/ko/java8-method-reference/

<br/>

#### 참고 자료

https://www.notion.so/758e363f9fb04872a604999f8af6a1ae

https://sujl95.tistory.com/76

https://giyeon95.github.io/whiteship/whiteship_study_week15/#1-predicate