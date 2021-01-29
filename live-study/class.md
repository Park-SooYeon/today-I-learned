# 클래스

## 목표

자바의 Class에 대해 학습하세요.

## 학습할 것

- [클래스 정의하는 방법](#클래스-정의하는-방법)
- [객체 만드는 방법 (new 키워드 이해하기)](#객체-만드는-방법-(new-키워드-이해하기))
- [메소드 정의하는 방법](#메소드-정의하는-방법)
- [생성자 정의하는 방법](#생성자-정의하는-방법)
- [this 키워드 이해하기](#this-키워드-이해하기)

---

### 클래스 정의하는 방법

Java에서 클래스를 정의하기 위해서는 아래와 같이 class 키워드를 사용해야한다.

```java
class ClassName {
	// 클래스 내부
    // 멤버 변수
    int a = 0;
    
    // 생성자
    // 생성자는 반환값이 없고, 클래스 이름과 동일한 이름을 가진다.
    // 생성자를 정의하지 않으면 자동으로 매개변수가 빈 생성자가 생성된다.
    // 모든 클래스는 반드시 하나의 생성자를 가지고 있어야한다.
    // 생성자는 매개변수를 다르게하여 여러개 선언할 수 있다.(생성자 오버로딩)
    public ClassName() {}
    
    // 메소드
    // 메소드는 반환값이 있고, 클래스 이름과 동일한 이름을 가지면 안된다.
    public void methodName() {}
}
```

클래스 내부에는 멤버 변수, 생성자, 메소드를 선언할 수 있는데 각각은 다음과 같다.

* 멤버 변수 : 인스턴스 필드일 경우에는 **각 인스턴스의 속성**, 클래스 필드일 경우에는 **클래스의 속성**을 나타낸다.
* 생성자 : 객체 생성 시, 객체 초기화를 위한 코드 선언부를 의미한다.
* 메소드 : 클래스에서 일어나는 **행위**를 나타낸다.

클래스는 멤버 변수와 메소드를 통해 크게 **속성과 행위**에 대한 정보를 담고 있다고 할 수 있다.

또한, 클래스는 다른 클래스를 상속받거나 인터페이스 구현체가 될 수 있다.

```java
// 클래스 상속
// 부모 클래스 : OtherClass, 자식 클래스 : ClassName
// extends를 통해 클래스 상속을 할 때, 단 하나의 클래스만 상속할 수 있다.
class ClassName extends OtherClass {} (O)
class ClassName extends OtherClass1, OtherClss2 {} (X)

// 인터페이스 구현
// 인터페이스는 클래스 상속과는 다르게 여러개의 인터페이스를 구현할 수 있다.
class ClassName implements OtherInterface {} (O)
class ClassName implements OtherInterface1, OtherInterface2 {} (O)

// 클래스 상속과 인터페이스 구현을 동시에 할 수도 있다.
class ClassName extends OtherClass implements OtherInterface {} (O)
```

> 멤버 변수 : **클래스 내부**에 선언된 변수를 의미

> Ref : https://javacan.tistory.com/entry/37

<br/>

### 객체 만드는 방법 (new 키워드 이해하기)

클래스는 단독으로 사용될 수 없기 때문에 new 키워드와 클래스의 생성자를 통해 객체를 생성하여 사용할 수 있다.

new 키워드를 사용하면 생성된 객체를 **동적 메모리 할당 영역에 생성**하여 GC에 의해 객체를 관리하게 된다.

아래 코드는 new 키워드와 생성자를 사용하여 객체를 생성할 수 있다.

```java
class ClassName {
    public ClassName() {} // 매개변수가 없으므로 생략 가능
}

ClassName object = new ClassName(); // ClassName 객체를 생성하여 object 변수에 할당
```

위 객체 생성 구문은 세 가지 순서로 진행된다.

1. 객체를 저장할 변수 선언 : 컴파일러에게 사용할 객체 타입을 알려준다.
2. 인스턴스화 : new 키워드로 컴파일러에게 해당 타입의 인스턴스를 생성할 것임을 알려준다.
3. 초기화 : 생성자 호출이 이루어지면서, 객체를 초기화한다.

> new 연산자는 새로운 객체를 위한 메모리를 할당하고, 그 메모리에 대한 참조를 반환한다.
> ⇒ 즉, **객체 주소**에 대한 정보를 반환한다.

<br/>

### 메소드 정의하는 방법

메소드는 아래와 같이 정의할 수 있다.

```java
class ClassName {
	public void methodName() {
		// 메소드 내부 실행 블록
	}
}
```

메소드는 크게 접근제한자, 리턴 타입, 메소드 명, 매개변수로 구성되어있다.

1. 접근제한자

   해당 메소드에 접근을 제한하기 위해 사용하는 키워드이다.

   접근 제한자의 종류는 아래와 같은 종류가 있다.

   | 접근제한자 종류 | 선언 클래스 내부 | 패키지 내부 | 상속관계 클래스 | 다른 패키지/클래스 |
   | --------------- | ---------------- | ----------- | --------------- | ------------------ |
   | public          | Y                | Y           | Y               | Y                  |
   | protected       | Y                | Y           | Y               | N                  |
   | (default)       | Y                | Y           | N               | N                  |
   | private         | Y                | N           | N               | N                  |

   OOP(객체 지향 프로그래밍)에서 `캡슐화` 및 `은닉화`를 위해 사용하는 키워드로 대게 **속성은 private로 메소드는 public으로 선언**한다.

2. 리턴 타입

   메소드의 리턴 타입을 지정해주는 키워드이다.

   기본 자료형뿐만 아니라 문자열, 객체 등 참조 타입도 사용할 수 있다.

3. 메소드 명

   메소드의 이름을 지정하는 부분이다.

   주로 카멜케이스로 동사를 사용하여 작성하는 것이 일반적이다.

4. 매개변수

   해당 메소드를 호출할 때, 전달받을 값들을 지정하는 부분이다.

   Java는 메소드 오버로딩을 지원하기 때문에 **매개변수의 데이터 타입 및 개수**에 따라 동일한 메소드명이여도 선언하여 사용할 수 있다.

   단, 매개변수가 동일하고 반환 값만 다른 경우에는 컴파일 에러가 난다.

   ```java
   // 메소드 오버로딩 예
   public void methodName(String s) {
   	System.out.println("매개변수가 String s");
   }
   public void methodName(int a) {
   	System.out.println("매개변수가 int a");
   }
   public void methodName(String s, int a) {
   	System.out.println("매개변수가 String s, int a");
   }
   
   String s = "Hello world";
   int a = 10;
   methodName(s); // 매개변수가 String s
   methodName(a); // 매개변수가 int a
   methodName(s, a); // 매개변수가 String s, int a
   ```

   ```java
   public void methodName(String s) {}
   public int methodName(String s) {} // 컴파일 에러
   ```

> 메소드 (=클래스 함수) : 독립적으로 존재할 수 없다, **클래스 내부**에 존재
> 함수 : 독립적으로 존재할 수 있다, **단독**으로 존재

> 캡슐화 : 관련이 깊은 속성과 기능을 하나의 클래스를 묶는다는 것을 의미
> 은닉화 : 속성을 외부에 노출시키지 않는 것을 의미

> Ref : https://dnight.tistory.com/entry/%ED%95%A8%EC%88%98Funtion%EC%99%80-%EB%A9%94%EC%86%8C%EB%93%9CMethod%EC%9D%98-%EC%B0%A8%EC%9D%B4

<br/>

### 생성자 정의하는 방법

생성자는 아래와 같은 특징을 가지고 있다.

* 생성자는 반환값이 없고, 클래스 이름과 동일한 이름을 가진다.
* 생성자를 정의하지 않으면 자동으로 매개변수가 빈 생성자가 생성된다. (기본 생성자)
* 모든 클래스는 반드시 하나의 생성자를 가지고 있어야한다.
* 생성자는 매개변수를 다르게하여 여러개 선언할 수 있다. (생성자 오버로딩)

따라서, 생성자는 아래와 같이 정의하여 사용할 수 있다.

```java
class ClassName {
    // 기본 생성자
	public ClassName() {
        System.out.println("매개변수가 없는 생성자");
    }
    
    public ClassName(String s) {
        System.out.println("String s 매개변수가 있는 생성자");
    }
    
    public ClassName(String s, int a) {
        System.out.println("String s, int a 매개변수가 있는 생성자");
    }
}

ClassName object1 = new ClassName(); // 매개변수가 없는 생성자
ClassName object2 = new ClassName("Hello world"); // String s 매개변수가 있는 생성자
ClassName object3 = new ClassName("Heelo world", 10); // String s, int a 매개변수가 있는 생성자
```

<br/>

### this 키워드 이해하기

this는 this 키워드를 호출한 생성자나 메서드의 현재 객체를 참조한다.

즉, 가장 가까운 클래스 혹은 객체를 참조한다고 생각하면 된다.

`this`를 사용하면 현재 객체의 모든 멤버를 참조할 수 있고, `this()`를 사용하여 생성자를 호출할 수 있다.

**this**

해당 변수가 객체의 멤버 변수임을 명시적으로 알려주기 위해 사용한다.

```java
class ClassName {
	private int a = 10;
    private int b = 20;
    
    public ClassName(int a, int b) {
        // a = a;로도 작성할 수 있지만 a가 매개변수의 a인지 멤버 변수의 a인지 알기 힘들다.
        this.a = a;
        b = b;
        System.out.println(this.a + ", " + a);
        System.out.println(this.b + ", " + b);
    }
}

ClassName object = new ClassName(1, 2);
/* 결과
	1, 1
	2, 2
*/
```

**this()**

생성자 내에서만 쓰일 수 있으며, 동일한 클래스의 **다른 생성자를 호출**하기 위해 사용한다.

this()는 항상 최상단에 존재해야한다.

```java
class ClassName {
	public ClassName() {
		System.out.println("ClassName() 기본 생성자");
	}
    
    public ClassName(int a) {
        this();
        System.out.println("ClassName(int a) 생성자");
    }
}

ClassName object = new ClassName(1);
/* 결과
	ClassName() 기본 생성자
	ClassName(int a) 생성자
*/
```

### 참고 문서

https://blog.naver.com/hsm622/222175480708

https://github.com/ksundong/TIL/blob/master/live-study/class.md