# 인터페이스

## 목표

자바의 인터페이스에 대해 학습하세요.

## 학습할 것 (필수)

- [인터페이스 정의하는 방법](#인터페이스-정의하는-방법)
- [인터페이스 구현하는 방법](#인터페이스-구현하는-방법)
- [인터페이스 레퍼런스를 통해 구현체를 사용하는 방법](#인터페이스-레퍼런스를-통해-구현체를-사용하는-방법)
- [인터페이스 상속](#인터페이스-상속)
- [인터페이스의 기본 메소드 (Default Method), 자바 8](#인터페이스의-기본-메소드-(Default-Method),-자바-8)
- [인터페이스의 static 메소드, 자바 8](#인터페이스의-static-메소드,-자바-8)
- [인터페이스의 private 메소드, 자바 9](#인터페이스의-private-메소드,-자바-9)

---

### 인터페이스 정의하는 방법

**인터페이스**란? 일종의 추상 클래스로 추상 클래스보다 추상화의 정도가 더 높다.

Java 8 이전까지는 **추상 메소드와 상수**만을 선언할 수 있었다. (일반 메소드, 멤버 변수 선언 불가능)

그러나 Java 8 이후부터는 **default 메소드와 static 메소드**가 추가되었다.

인터페이스를 정의하는 방법은 아래와 같다.

```java
public interface 인터페이스명 {
    // 상수 정의 (가급적이면 사용하지 않는 것을 권장)
    // 인터페이스에서 절대적으로 제공하는 것으로 수정 불가능
	타입 상수이름 = 값;
    
    // 메소드 시그니처 정의
    // 반드시 오버라이딩해서 재구현해야한다. (구현이 강제적)
    타입 메소드명(매개변수);
	
    // default 메소드
    // 인터페이스에서 기본적으로 제공하지만 오버라이딩해서 재구현할 수 있다.(구현이 선택적)
    default 타입 메소드명(매개변수) {
        //구현부
    }
    
    // static 메소드
    // 인터페이스에서 절대적으로 제공하는 것으로 수정 불가능
    static 타입 메소드명(매개변수) {
        // 구현부
    }
}
```

인터페이스 내부 **상수**는 모두 `public static final`이 암묵적으로 정의되어있다.

인터페이스 내부 **모든 메소드**는 `public` 접근 제한자가 암묵적으로 정의되어 있다.

> 인터페이스를 사용하는 이유
>
> 1. 클래스간의 결합도를 낮출 수 있다.
>
>    코드의 종속성을 줄이고 유지보수성을 높이도록 해준다.
>
> 2. 표준화가 가능하다.
>
>    클래스의 기본틀을 제공하여 개발자들에게 정형화된 개발을 강요할 수 있따.
>
> 3. 개발 기간을 단축 시킬 수 있다.

<br/>

### 인터페이스 구현하는 방법

인터페이스도 일종의 추상 클래스이므로 인터페이스로 객체를 생성할 수 없다.

따라서, 클래스를 통해 인터페이스를 구현해야하는데 구현하는 방법은 아래와 같다.

```java
class 클래스명 implements 인터페이스명 {
	// 인터페이스에 정의된 추상 메소드 구현
}
```

인터페이스를 구현하는 클래스를 **구현 클래스**라고 하며 구현 클래스에 의해 생성된 객체를 **인터페이스의 구현 객체**라고 한다.

하나의 구현 클래스는 여러개의 인터페이스를 구현할 수 있다.

```java
class 클래스명 implements 인터페이스명1, 인터페이스명2 {
	// 인터페이스에 정의된 추상 메소드 구현
}
```

구현 클래스가 단 한번만 사용되는 경우라면 구현 클래스를 정의하는 것은 비효율적이다. 따라서, 자바는 `익명 구현 객체`를 만드는 방법을 지원하며 그 방법은 아래와 같다.

```java
인터페이스 변수 = new 인터페이스() {
	// 인터페이스에 정의된 추상 메소드 구현
}
```

<br/>

### 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법

다형성에 의해 자식 클래스의 객체는 부모 클래스의 참조 변수를 참조하는 것이 가능하다.

인터페이스 역시 인터페이스 타입의 참조 변수를 사용할 수 있으며, 인터페이스 타입으로의 형변환도 가능하다.

```java
interface A {
	void print();
}

class B1 implements A {
	@Override
	public void print() {
		System.out.println("A interface 구현 클래스 - B1");
	}
    
    public void bMethod() {
        System.out.println("B1 자식 클래스에만 정의된 메소드");
    }
}

class B2 implements A {
	@Override
	public void print() {
		System.out.println("A interface 구현 클래스 - B2");
	}
    
    public void bMethod() {
        System.out.println("B2 자식 클래스에만 정의된 메소드");
    }
}

// 클래스 타입 레퍼런스
B1 b1 = new B1();
B2 b2 = new B2();

b1.print();
b1.bMethod();
b2.print();
b2.bMethod();
/* 결과
	A interface 구현 클래스 - B1
	B1 자식 클래스에만 정의된 메소드
	A interface 구현 클래스 - B2
	B2 자식 클래스에만 정의된 메소드
*/

// 인터페이스 레퍼런스
// 인터페이스 혹은 부모 클래스에 선언되어 있는 메소드만 사용이 가능하다.
// A Type는 A interface에 의해 구현된 어떤 클래스도 담을 수 있다.
A a1 = new B1();
A a2 = new B2();

a1.print();
a2.print();
a1.bMethod(); // A interface에는 정의되지 않았기 때문에 호출 불가능
((B1) a1).bMethod();
// A Type을 B1 Type으로 변경해줌으로써 구현 클래스에 있는 메소드 사용
/* 결과
	A interface 구현 클래스 - B1
	A interface 구현 클래스 - B2
	B1 자식 클래스에만 정의된 메소드
*/
```

이처럼 하나의 인터페이스로 여러개의 구현 클래스를 구현하고 인터페이스 레퍼런스를 사용하면 인터페이스를 구현한 객체들을 인터페이스 타입으로 받아 사용할 수 있어 변경에 유연해진다는 장점이 있다.

<br/>

### 인터페이스 상속

인터페이스는 인터페이스로부터만 상속을 받을 수 있으며 클래스와 달리 **다중 상속**이 가능하다.

```java
interface A {
	void printA();
}

interface B {
	void printB();
}

// 인터페이스 상속
interface C extends A, B {
	void printC();
}

class D implements C {
    // C 인터페이스는 A, B 인터페이스를 상속받으므로 printA, printB, printC 함수 모두 구현해주어야한다.
	@Override
	public void printA() {
		System.out.println("A 인터페이스의 printA 구현");
	}
    
    @Override
	public void printB() {
		System.out.println("B 인터페이스의 printB 구현");
	}
    
    @Override
	public void printC() {
		System.out.println("C 인터페이스의 printC 구현");
	}
}

D d = new D();
d.printA();
d.printB();
d.printC();
/* 결과
	A 인터페이스의 printA 구현
	B 인터페이스의 printB 구현
	C 인터페이스의 printC 구현
*/
```

> 인터페이스 다중 상속을 할 때, 메소드명과 파라미터 형식은 같지만 리턴 타입이 다른 메소드가 존재하면 둘 중 어떤 것을 상속받느냐에 따라 **규칙이 달라지기 때문에 다중 상속이 불가능하다**
> ⇒ 중복되는 인터페이스의 추상 메소드를 재정의하여 사용할 수 있다.

> Ref : https://www.notion.so/4b0cf3f6ff7549adb2951e27519fc0e6

<br/>

### 인터페이스의 기본 메소드 (Default Method), 자바 8

**인터페이스의 기본 메소드 (Default Method)**란? 추상 메소드의 기본적인 구현을 제공하는 메소드를 의미한다.

기본 메소드가 추가된 이유는 인터페이스가 변화가 되기 때문에 인터페이스 변화에 따른 문제를 해결하기 위해서 추가되었다.

기본 메소드가 추가됨으로 인해 인터페이스에 기본 메소드가 추가되어도 해당 인터페이스를 구현한 클래스를 변경하지 않아도 된다. 따라서, 기본 메소드를 통해 인터페이스를 유연하게 변경하면서, 인터페이스의 장점까지도 챙길 수 있게 되었다.

기본 메소드는 아래와 같이 정의할 수 있다.

```java
interface A {
	void method(); // 추상 메소드
	default void defaultMethod() { // 기본 메소드
        System.out.println("기본 메소드 구현");
    }
}

class B implements A {
    void method() {
        System.out.println("추상 메소드 구현");
    }
}

B b = new B();
b.method();
b.defaultMethod();
/* 결과
	추상 메소드 구현
	기본 메소드 구현
*/
```

기본 메소드는 묵시적으로 public 접근 제한자를 가지고 있으며 추상 메소드와 달리 일반 메소드처럼 몸통이 있어야한다.

<br/>

### 인터페이스의 static 메소드, 자바 8

**인터페이스의 static 메소드**란? 인터페이스의 객체 생성과는 관계없이 인터페이스 타입에서 호출이 가능한 정적 메소드로 묵시적으로 public 접근 제한자를가지고 있다.

```java
interface A {
	static void print() {
		System.out.println("static method");
	}
}

A.print(); // static method
```

<br/>

### 인터페이스의 private 메소드, 자바 9

Java 9 부터 인터페이스에서도 캡슐화를 지원하여 `private 메소드`와 `private static 메소드`를 지원한다.

private 메소드는 인터페이스 내부에서 코드의 재사용성을 향상시키므로 아래 규칙을 지키며 사용해야한다.

* private 메소드는 구현부를 가져야만 한다.
* private 메소드는 오직 인터페이스 내부에서만 사용할 수 있다.
* private static 메소드는 다른 static 또는 static이 아닌 메소드에서 사용할 수 있다.
* static이 아닌 private 메소드는 다른 private static 메소드에서 사용할 수 없다.

```java
interface A {

  void method1();

  default void method2() {
    System.out.println("method2 호출");
    method4(); // 기본 메소드 내부 private 메소드
    method5(); // static이 아닌 메소드 안의 private static 메소드
  }

  static void method3() {
    System.out.println("method3 호출");
    // method4(); // static 메소드 안의 private 메소드
    // method4가 메모리에 올라가기 전에 호출되므로 컴파일 에러
    method5(); // static 메소드 안의 private static 메소드
  }

  private void method4() {
    System.out.println("private method 호출");
  }
    
  private static void method5() {
    System.out.println("private static method 호출");
  }
}

class B implements A {
  public void method1() {
    System.out.println("method1 호출");
  }
}

B b = new B();

b.method1();
b.method2();
A.method3();
// method4와 method5는 private 메소드이므로 구현 클래스인 B에서 접근 불가능
/* 결과
	method1 호출
	method2 호출
	private method 호출
	private static method 호출
	method3 호출
	private static method 호출
*/
```

#### 참고 자료

https://www.notion.so/4b0cf3f6ff7549adb2951e27519fc0e6

https://github.com/ksundong/TIL/blob/master/live-study/interface.md

https://yadon079.github.io/2021/java%20study%20halle/week-08