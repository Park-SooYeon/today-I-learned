# 상속

## 목표

자바의 상속에 대해 학습하세요.

## 학습할 것 (필수)

- [자바 상속의 특징](#자바-상속의-특징)
- [super 키워드](#super-키워드)
- [메소드 오버라이딩](#메소드-오버라이딩)
- [다이나믹 메소드 디스패치 (Dynamic Method Dispatch)](#다이나믹-메소드-디스패치-(Dynamic-Method-Dispatch))
- [추상 클래스](#추상-클래스)
- [final 키워드](#final-키워드)
- [Object 클래스](#Object-클래스)

---

### 자바 상속의 특징

Java에서 **상속**이란? 자식 클래스가 부모 클래스의 멤버 변수, 메소드를 물려받는 것을 의미한다.

상속을 사용하여 클래스를 확장하게 되면 코드의 중복을 줄여 코드의 재사용성을 높여주고, 코드를 공통적으로 관리할 수 있어 유지보수에 용이하다는는 장점이 있다.

Java에서 상속을 사용하려면 아래와 같이 extends 키워드를 사용하면 된다.

 ```java
class 자식클래스명 extends 부모클래스명 {}
 ```

Java의 상속은 아래와 같은 특징이 있다.

1. 상속은 **단일 상속**만 가능하다.

   즉, 하나의 자식 클래스는 반드시 하나의 부모 클래스만 가질 수 있다.

   ```java
   class 자식클래스명 extends 부모클래스명1, 부모클래스명2 {} // 불가능
   ```

2. 상속 횟수에 제한을 두지 않는다.

3. `Object` 클래스를 제외한 모든 클래스는 반드시 부모 클래스를 가지고 있어야하며 명시적으로 지정해주지 않은 경우, 자동으로 **Object 클래스를 상속**받게된다.

4. 부모 클래스의 멤버 변수와 메소드만 상속되며 생성자는 상속되지 않는다.

<br/>

### super 키워드

super는 자식 클래스에서 부모 클래스를 참조하는데 사용되는 키워드이다.

**super**

부모 클래스의 멤버 변수, 메소드를 호출할 때 사용한다.

자식 클래스에서 메소드 오버라이드를 했을 때, 오버라이드되기 전인 부모 클래스의 메소드를 super 키워드를 통해 호출할 수 있다.

super 키워드를 통해 부모 클래스의 멤버 변수에 접근할 수 있으나 권장되지 않는다.

```java
// 부모 클래스
class Parent {
	String name = "Parent";
	
	public void print() {
		System.out.println("Parent Class");
        System.out.println(name);
	}
}

// 자식 클래스
class Child extends Parent {
    String name = "Child";
    
	@Override // 메소드 오버라이딩 - 관련 설명은 아래에..
    public void print() {
        System.out.println("Child Class");
        System.out.println(name);
    }
    
    public void callParentPrint() {
        super.print(); // 부모 클래스의 print 메소드 실행
    }
    
    public void getParentName() {
        System.out.println(super.name); // 부모 클래스의 name 값
        System.out.println(name); // 자식 클래스의 name 값
    }
}

Child child = new Child();
child.print();
child.callParentPrint();
child.getParentName();
/* 결과
	Child Class
	Child
	Parent Class
	Parent
	Parent
	Child
*/
```

**super()**

위에서 부모 클래스의 생성자는 상속이 되지 않는다고 했다.

하지만, super 키워드로 부모 클래스의 생성자를 호출할 수는 있다.

단, this()와 동일하게 항상 최상단에 위치해야한다.

```java
// 부모 클래스
class Parent {
	public Parent() {
        System.out.println("부모 클래스 생성자 호출");
    }
}

// 자식 클래스
class Child extends Parent {
    public Child() {
        super(); // 추가해주지 않아도 암묵적으로 부모 클래스의 생성자를 호출한다.
        System.out.println("자식 클래스 생성자 호출");
    }
}

Child child = new Child();
/* 결과
	부모 클래스 생성자 호출
	자식 클래스 생성자 호출
*/
```

> super()를 통해 부모 클래스의 생성자를 호출해야하는 이유?
> 자식 클래스가 부모 클래스의 멤버를 사용할 수 있기 때문에 부모 클래스의 멤버들은 모두 초기화 되어 있어야한다.
> ⇒ Object 클래스를 제외한 모든 클래스의 생성자는 첫 줄에 반드시 부모 생성자를 호출해준다.

>  자식 클래스의 생성자가 부모 클래스의 생성자를 실행하면 부모 클래스의 생성자뿐만 아니라 모든 클래스의 부모 클래스인 **Object 클래스의 생성자 Object()까지 호출**한다.
> ⇒ 즉, 체인 형식으로 자식 클래스 생성자 → 부모 클래스1 생성자 → 부모 클래스2 생성자 → Object 클래스 생성자 호출식으로 호출된다고 보면 된다.

<br/>

### 메소드 오버라이딩

**메소드 오버라이딩**이란? 부모 클래스에 존재하는 메소드를 자식 클래스에서 **재정의** 하는 것을 말한다.

메소드 오버라이딩하게되면 자식 클래스의 메소드가 부모 클래스의 메소드를 가리기 때문에 부모 클래스의 메소드를 호출하기 위해서는 super 키워드를 사용해야한다.

메소드 오버라이딩을 하기 위해서는 아래와 같은 조건을 만족해야한다.

* 메소드명은 부모 클래스의 메소드명과 일치해야한다.
* 매개변수는 부모 클래스의 매개변수와 일치해야한다.
* 리턴 타입은 부모 클래스의 리턴 타입과 일치해야한다.

오버라이딩은 단순히 존재하는 메소드를 재정의하는 것이기 때문에 메소드에 대한 정보는 반드시 일치해야했다.

그러나, JDK1 1.5부터 `공변 반환타입(Covariant Return Type)`이 추가되어 오버라이딩 한 메소드의 리턴 타입의 서브 타입을 반환하는 것을 허용하게 된다.

```java
class Parent {
	public Object parentMethod(String str) {
		Object result = str.toLowerCase();
		return result;
	}
}

class Child extends Parent {
	// Integer는 Object의 하위 클래스이므로 호환되어 리턴 타입으로 변경할 수 있다.
    @Override
	public Integer parentMethod(String str) {
        int result = Integer.parseInt(str);
        return result;
	}
}

Child child = new Child();
int result = child.parentMethod("1");
System.out.println(result);
```

또한, 접근제한자는 부모 클래스의 메소드보다 좁은 접근제한자를 사용할 수 없고, 예외처리도 부모 클래스의 메소드보다 많은 수의 예외를 선언할 수 없다.

> 부모 클래스의 static 메소드를 자식 클래스에서 static 메소드로 오버라이드 할 수 있는가?
>
> 가능하다. 다만 이경우에는 메소드를 재정의 한 것이 아니라 각 클래스에 별개의 static 메서드를 정의한 것이다.
>
> ```java
> class Parent {
> 	public static void print() {
> 		System.out.println("Parent Class");
> 	}
> }
> 
> class Child extends Parent {
>     // @Override 오버라이드로 정의하게 되면 에러 남
> 	public static void print() {
>         System.out.println("Child Class");
> 	}
> }
> 
> Parent child1 = new Child();
> Child child2 = new Child();
> child1.print(); // Parent Class;
> child2.print(); // Child Class;
> ```

> @Override : 해당 메소드가 재정의 되는 메소드임을 명시적으로 알려주기 위해 사용하는 어노테이션, 재정의할 메소드가 없을 경우 컴파일 시점에 오류를 발생시켜줌

> Ref : https://github.com/ksundong/TIL/blob/master/live-study/inheritance.md

<br/>

### 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)

**메소드 디스패치**란? 어떤 메소드를 호출할지 결정하여 실행시키는 과정을 의미한다.

메소드 디스패치에는 `정적 메소드 디스패치`, `동적 메소드 디스패치`, `더블 디스패치` 세가지가 존재한다.

**정적 메소드 디스패치 (Static Method Dispatch)**

컴파일 시점에서 컴파일러가 특정 메소드를 호출할 것이라고 **명확하게 알고있는 경우**를 의미한다.

```java
class A {
    public void print() {
        System.out.println("print()");
    }
    
    public void print(String str) {
        System.out.println("print(String str)");
    }
}

A a = new A();
a.print(); // A 클래스의 print() 함수를 호출할 것이라는 것이 명확함
a.print("Hello World"); // A 클래스의 print(String str) 함수를 호출할 것이라는 것이 명확함
```

**동적 메소드 디스패치 (Dynamic Method Dispatch)**

정적 메소드 디스패치와 반대로 컴파일러가 어떤 메소드를 호출하는지 **모르는 경우**를 의미한다.

`런타임 시점`에서 호출할 메소드를 결정한다.

```java
class A {
	public void print() {
		System.out.println("Class A");
	}
}

class B1 extends A {
	@Override
	public void print() {
		System.out.println("Class B1");
	}
}

class B2 extends A {
	@Override
	public void print() {
		System.out.println("Class B2");
	}
}

A a = new B1();
a.print(); // 컴파일러는 런타임 시점에 a 변수에 할당된 객체가 B1인지 B2인지 확인하여야 한다
```

**더블 디스패치(Double Dispatch)**

동적 메소드 디스패치를 두번 하는 것을 의미한다.

```java
-- 출처 : https://dbbymoon.tistory.com/9
interface Post {
    void postOn(SNS sns);
}

class Text implements Post {
    public void postOn(SNS sns) {
        sns.post(this);
    }
}

class Picture implements Post {
    public void postOn(SNS sns) {
        sns.post(this);
    }
}
```

```java
interface SNS {
    void post(Text text);
    void post(Picture picture);
}

class Facebook implements SNS {
    public void post(Text text) {
        // text -> facebook
    }
    public void post(Picture picture) {
        // picture -> facebook
    }
}

class Twitter implements SNS {
    public void post(Text text) {
        // text -> twitter
    }
    public void post(Picture picture) {
        // picture -> twitter
    }
}
```

```java
public static void main(String[] args) {
    List<Post> posts = Arrays.asList(new Text(), new Picture());
    // posts = [Text, Picture]
    List<SNS> sns = Arrays.asList(new Facebook(), new Twitter());
	// sns = [Facebook, Twitter]
    
    posts.forEach(p -> sns.forEach(s -> p.postOn(s)));
    // 1. Post를 구현한 구현체인 Text와 Picture 중, 어느 구현체의 postOn 함수인지 확인해야함
    // 2. postOn에서 sns.post()를 호출하는데 이 때, SNS를 구현한 구현체인 Facdbook과 Twitter 중, 어느 구현체의 어느 매개변수를 받는 post 함수인지 확인해야함
}
```

> **방문자 패턴 (Visitor Pattern)** : 알고리즘을 객체 구조에서 분리시켜 디자인하는 패턴을 의미한다.
> 주로 데이터와 메소드를 구분하기 위해 사용하며 더블 디스패치를 이용한 대표적인 패턴이다.

<br/>

### 추상 클래스

**추상 클래스**란? 클래스를 만들기 위한 일종의 **미완성된 클래스**로 객체를 생성할 수 없는 클래스이다.

 추상 클래스를 사용하기 위해서는 abstract 키워드를 사용하면된다.

```java
abstract class 클래스명 {}
```

추상 클래스는 추상 메소드를 가지고 있으며 추상 클래스를 상속받은 자식 클래스에서 반드시 추상 메소드를 정의해주어야한다.

추상 클래스는 멤버 변수, 생성자, 일반 메소드, 추상 메소드를 가질 수 있으며 아래와 같이 사용할 수 있다.

```java
abstract class AbstractClass {
	// 멤버 변수
    int a = 10;
	
    // 생성자
    public AbstractClass() {
        System.out.println("추상 클래스 생성자");
    }
    
    // 일반 메소드
    public void print() {
        System.out.println("추상 클래스의 print 함수");
    }
    
    // 추상 메소드 : 함수 정의 부분은 작성하지 않고 선언부만 작성하며 abstract 키워드를 사용하여 사용할 수 있다.
    public abstract void abstractMethod();
}

class Child extends AbstractClass {
    @Override // 추상 메소드를 반드시 구현해주어야한다. => 구현하지 않으면 컴파일 에러 남
    public void abstractMethod() {
        System.out.println("자식 클래스에서 추상 메소드 구현");
    }
}

Child child = new Child();
System.out.println(child.a);
child.print();
child.abstractMethod();
/* 결과
	추상 클래스 생성자
	10
	추상 클래스의 print 함수
	자식 클래스에서 추상 메소드 구현
*/
```

**추상 클래스 vs 인터페이스**

* 비슷한 점
  * 둘 다 추상적인 성격을 지니고 있어 객체를 생성하지 못한다.
  * 추상화된 메소드들이 존재하여 자식 클래스에서의 구현을 강제화 할 수 있다.
  * 추상 메소드 상속받은 클래스 및 인터페이스를 구현하기 위한 클래스에서 반드시 추상화된 메소드들을 모두 구현해주어야한다.
* 다른 점
  * 추상 클래스
    * 멤버 변수를 정의할 수 있고, 접근제한자도 자유롭게 사용 가능하다.
    * 추상 클래스도 결국 클래스이기 때문에 단일 상속만 지원한다.
  * 인터페이스
    * 모든 멤버 변수는 `public static final`로 정의되어야한다.
    * 인터페이스는 다중 상속이 지원된다. (클래스가 아니기 때문에..)
* 사용 용도
  * 추상 클래스
    * 추상 클래스는 **상속을 강제화**하여  **기능을 이용하고 확장하는 것**이 목적이다.
      ⇒ 클래스를 상속받기 때문에 부모-자식 관계를 가짐
    * 자식 클래스에서 공통적으로 사용되는 필드 및 메소드는 추상 클래스에서 한번만 정의해주어 사용할 수 있다.
  * 인터페이스
    * **동작을 위한 함수 구현을 강제화**하여 구현 클래스에서 동일한 동작 및 구조를 보장하는 것이 목적이다.
    * 관련 없는 클래스들이 인터페이스를 구현할 것으로 예상되는 경우에 사용할 수 있다.
      ex) Comparable, Cloneable

> 참고로 추상 클래스가 인터페이스를 상속하는 경우, 꼭 모든 메소드를 구현하지 않아도 된다.
> (둘 다 추상적이라는 의미를 지니고 있기 때문에..)

> Ref : https://github.com/ksundong/TIL/blob/master/live-study/inheritance.md

<br/>

### final 키워드

Java의 `final` 키워드는 변경 불가능하다는 의미를 가진다.

final 키워드는 변수, 메소드, 클래스에 사용할 수 있으며 사용 위치에 따라 각각의 의미를 가진다

* final 변수 : **변하지 않는 상수**, 한번만 초기화가 가능한 변수를 의미한다.
* final 메소드 : **변하지 않는 메소드**, 오버라이딩을 통한 재정의가 불가능한 메소드를 의미한다.
* final 클래스 : **변하지 않는 클래스**, 상속을 통해 확장될 수 없는 클래스를 의미한다. (상속 계층에서 마지막 클래스를 의미)

<br/>

### Object 클래스

**java.lang.Object 클래스**는 `모든 클래스의 조상`으로 최상위 클래스이다.

Object 클래스에는 아래와 같은 함수들이 정의되어있다.

![image](https://user-images.githubusercontent.com/49746644/106358941-91d42680-6352-11eb-8c1c-af4f0819412d.png)

모든 클래스는 묵시적으로 java.lang.Object 클래스를 상속받으므로 어떠한 클래스던 위 함수들을 호출하여 사용할 수 있다.

> 이미지 출처 : https://blog.naver.com/swoh1227/222181505425

#### 참고 문서

https://blog.naver.com/swoh1227/222181505425

https://yadon079.github.io/2020/java%20study%20halle/week-06