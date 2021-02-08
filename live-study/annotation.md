# 애노테이션

## 목표

자바의 애노테이션에 대해 학습하세요.

## 학습할 것 (필수)

- [애노테이션 정의하는 방법](#애노테이션-정의하는-방법)
- [@retention](#@retention)
- [@target](#@target)
- [@documented](#@documented)
- [애노테이션 프로세서](#애노테이션 프로세서)

---

### 애노테이션 정의하는 방법

**애노테이션 (annotation)**이란? 주석이라는 뜻으로, 주석처럼 코드에 달아 **클래스에 특별한 의미를 부여하거나 기능을 주입할 수 있다.**

애노테이션은 인터페이스의 일종으로 @를 사용하여 선언하며 런타임에 해석된다.

애노테이션을 사용할 경우 **데이터에 대한 유효성 검사 조건에 애노테이션을 사용하여 유효조건을 보다 쉽게 파악할 수 있고, 코드가 깔끔**해지게 된다. 또한, 애노테이션과 리플랙션을 같이 이용하면 **원하는 클래스를 주입**하는 것도 가능해진다.

애노테이션에는 아래와 같이 두 가지 종류가 있다.

* Built-in Annotation : 자바에서 기본적으로 제공하는 애노테이션

  Java에서는 아래와 같은 종류의 Built-in 애노테이션을 지원하고 있다.

  | 애노테이션           | 설명                                                    |
  | -------------------- | ------------------------------------------------------- |
  | @Override            | 컴파일러에게 해당 메소드가 오버라이딩 된 것을 알려줌    |
  | @Deprecated          | 해당 메소드가 더 이상 사용되지 않음을 표시              |
  | @SupperssWarnings    | 선언한 곳의 컴파일 경고를 무시                          |
  | @SafeVarargs         | 제네릭 같은 가변인자 매개변수를 사용할 때의 경고를 무시 |
  | @FunctionalInterface | 람다 함수 등을 위한 인터페이스 지정                     |
  | @Native              | native 메소드에서 참조되는 상수 앞에 붙임               |

* Meta Annotation : 커스텀 애노테이션을 만들 수 있게 제공되는 애노테이션

커스텀 애노테이션을 정의하는 방법은 인터페이스를 정의하는 방법과 동일하되 `@` 기호를 붙여주어야한다.

```java
@interface 애노테이션명 {
	타입 요소이름(); // 애노테이션의 요소를 선언
}
```

애노테이션은 암묵적으로 java.lang.annotation.Annotation을 확장하기 때문에 extends 절을 가질 수 없다. (상속 허용 X)

> 애노테이션 요소란??
>
> 애노테이션 내에 선언된 메소드를 애노테이션의 요소라고 한다.
>
> 요소의 개수에 따라 아래와 같이 3가지 종류로 분류할 수 있다.
>
> * Maker 애노테이션 : 요소가 한개도 없으며 단순 표식으로 사용되는 어노태이션
>
>   컴파일러에게 어떤 의미를 전달하는데 사용한다.
>
>   ```java
>   public @interface CustomAnnotation {};
>   ```
>
> * Single-value 애노테이션 : 요소로 단일 변수만 갖는 애노테이션
>
>   단일 변수 밖에 없기 때문에 값만을 명시하여 데이터를 전달할 수 있다.
>
>   ```java
>   public @interface CustomAnnotation {
>   	String value();
>   }
>   
>   @CustomAnnotation("test") // 요소가 하나이므로 요소의 이름을 생략할 수 있다.
>   public class CustomClass { ... }
>   ```
>
> * Full 애노테이션 : 둘 이상의 요소를 갖는 애노테이션
>
>   데이터를 배열 안에 key-vaelu 형태로 전달하며 이 요소에는 아래와 같은 일정한 규칙이 존재한다.
>
>   1. 요소의 타입은 기본형, String, enum, 애노테이션, Class만 허용
>   2. 요소의 ()안에 매개변수를 선언할 수 없다.
>   3. 예외를 선언할 수 없다.
>   4. 요소를 타입 매개변수로 정의할 수 없다.
>   5. 애노테이션의 각 요소는 기본값을 가질 수 있다.
>
>   ```java
>   public @interface CustomAnnotation {
>   	String value();
>   	int count() default 1;
>   }
>   
>   @CustomAnnotation(value="java", count = 2)
>   public class CustomClass { ... }
>   ```

> Ref : https://asfirstalways.tistory.com/309

<br/>

### @retention

**애노테이션이 유지(retention)되는 기간을 지정하는데 사용**되는 메타 애노테이션이다.

해당 애노테이션은 `java.lang.annotation.RetentionPolicy`에 정의된 세가지 유지정책(retention policy)을 사용할 수 있다.

1. SOURCE : 소스 파일에만 존재하며, 클래스 파일에는 존재하지 않는다. (컴파일 전까지만 유효)

   `@Override`, `@SuppressWarnings`처럼 컴파일러가 사용하는 애노테이션의 유지 정책

2. CLASS : 클래스 파일에는 존재하지만 런타임 시에 사용이 불가능하다. (컴파일러가 클래스를 참조할 때까지 유효, 유지정책의 기본값)

   런타임 시에 사용이 불가능하여 잘 사용되지 않는다.

3. RUNTIME : 클래스 파일에 존재하며 런타임 시에도 사용 가능하다. (컴파일 이후에도 JVM에 의해 계속 참조 가능)

   런타임 시에 리플렉션을 통해 클래스 파일에 저장된 애노테이션 정보를 읽어 처리할 수 있다.

아래와 같이 지정하여 사용할 수 있으며, 반드시 한번만 사용 가능하다.

```java
@Retention(RetentionPolicy.RUNTIME)
public @interface CustomAnnotation {
	...
}
```

> **리플렉션 (reflection)**이란?
> 구체적인 클래스 타입을 알지 못해도 해당 클래스의 객체 생성 메소드, 타입, 변수들에 접근할 수 있도록 도와주는 Java API를 말한다.

<br/>

### @target

**애노테이션이 적용될 위치를 지정**하는 메타 애노테이션이다.

`java.lang.annotation.ElementType`에 정의된 사용 종류에는 아래와 같은 종류가 있으며 여러개를 사용하고 싶을 경우 {}를 사용한다.

| 타입            | 의미                                 |
| --------------- | ------------------------------------ |
| PACKAGE         | 패키지 선언                          |
| TYPE            | 타입 선언 (클래스, 인터페이스, enum) |
| ANNOTATION_TYPE | 애노테이션 타입 선언                 |
| CONSTRUCTOR     | 생성자 선언                          |
| FIELD           | 멤버변수 선언 (멤버 변수, enum 상수) |
| LOCAL_VARIABLE  | 지역변수 선언                        |
| METHOD          | 메소드 선언                          |
| PARAMETER       | 전달인자 선언                        |
| TYPE_PARAMETER  | 전달인자 타입 선언                   |
| TYPE_USE        | 타입 선언                            |

```java
// 애노테이션 적용 위치가 한개일 경우
@Target(ElementType.ANNOTATION_TYPE)
public @interface CustomAnnotation1 {
	...
}

// 애노테이션 적용 위치가 한개 이상일 경우
@Target({ElementType.ANNOTATION_TYPE, ElementType.PACKAGE})
public @interface CustomAnnotation2 {
	...
}
```

> TYPE vs TYPE_USE
>
> * TYPE : **타입 선언** 시, 애노테이션 사용 가능
>
> * TYPE_USE : 해당 타입의 **변수를 선언**할 때, 애노테이션 사용 가능

<br/>

### @documented

**애노테이션에 대한 정보가 javadoc으로 작성한 문서에 포함되도록 할 때 사용**하는 애노테이션이다.

built-in-annotation 중 `@Override`와 `@SuppressWarnings`를 제외하고는 모두 이 메타 애노테이션이 붙어있다.

```java
@Documented
public @interface CustomAnnotation {
	...
}
```

<br/>

### 애노테이션 프로세서

**애노테이션 프로세서**란? 자바 컴파일러 플러그인의 일종으로 애노테이션에 대한 코드베이스를 검사, 수정, 생성하는 역할을 가지는 플러그인을 말한다.

애노테이션 프로세스는 **AbstractProcessor 클래스를 상속**받아 작성된다.

애노테이션 프로세서의 동작 구조는 아래와 같다.

1. 애노테이션 클래스를 생성한다.
2. 애노테이션 파서 클래스를 생성한다.
3. 애노테이션을 사용한다.
4. 컴파일하면 애노테이션 파서가 애노테이션을 처리한다.
5. 자동 생성된 클래스가 빌드 폴더에 추가된다.

#### 참고 자료

https://blog.naver.com/swoh1227/222229853664

https://yadon079.github.io/2021/java%20study%20halle/week-12