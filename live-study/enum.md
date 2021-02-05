# Enum

## 목표

자바의 열거형에 대해 학습하세요.

## 학습할 것 (필수)

- enum 정의하는 방법
- enum이 제공하는 메소드 (values()와 valueOf())
- java.lang.Enum
- EnumSet

---

### enum 정의하는 방법

**Enum**이란? Java 5부터 추가된 **열거형 상수**이다. 기본적으로 static과 final이 내장되어 있어 한번 정의된 값을 바꿀 수 없다.

서로 관련된 상수를 묶어서 사용하기에 좋고, 값뿐만 아니라 타입까지 관리(type-safe)하기 때문에 논리적인 오류를 줄일 수 있다는 장점이 있다.

또한, 모든 enum 상수들은 내부적으로 new를 통해 인스턴스화 되기 때문에 **객체타입의 enum**이다.

enum 키워드를 사용하여 아래와 같이 정의하여 사용할 수 있다.

```java
public enum Day {
	SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}

// enum 키워드를 사용하면 내부적으로는 아래와 같은 코드가 정의되어 동작된다고 한다.
class Day {
    static final int SUNDAY = 0;
    static final int MONDAY = 1;
    static final int TUESDAY = 2;
    static final int WEDNESDAY = 3;
    static final int THURSDAY = 4;
    static final int FRIDAY = 5;
    static final int SATURDAY = 6;
}
```

만약 열거형 상수에 임의의 값을 부여하고 싶다면 아래와 같이 사용하면 된다.

단, 반드시 **생성자와 멤버변수**를 명시해주어야한다.

```java
public enum Day {
	SUNDAY(6), MONDAY(5), TUESDAY(4), WEDNESDAY(3), THURSDAY(2), FRIDAY(1), SATURDAY(0);
    
    private int value;
    
    private Day(int value) {
        this.value = value;
    }
    
    public int getValue() {
        return value;
    }
}

Day day1 = Day.MONDAY;
System.out.println(day1.getValue()); // 5
```

<br/>

### enum이 제공하는 메소드 (values()와 valueOf())

| 메소드                                    | 설명                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| Class<E> getDeclaringClass()              | 열거형의 Class 객체를 리턴                                   |
| String name()                             | 열거형 상수의 이름을 문자열로 리턴                           |
| int ordinal()                             | 열거형 상수가 정의된 순서를 리턴 (0부터 시작)                |
| T valueOf(Class<T> enumType, String name) | 지정된 열거형에서 name과 일치하는 열거형 상수를 리턴         |
| T valueOf(String name)                    | name과 일치하는 열거형 상수 리턴 (값이 없으면 Exception 발생) |
| T[] values                                | 열거형 상수들을 **배열 형태**로 리턴                         |
| int compareTo(E o)                        | ordinal의 차이를 리턴                                        |

```java
public enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}

Day day1 = Day.MONDAY;
Day day2 = Day.SUNDAY;

System.out.println(day1.getDeclaringClass()); // class Test$Day (class 클래스명$enum명)
System.out.println(day1.name()); // MONDAY
System.out.println(day1.ordinal()); // 1
System.out.println(day1.valueOf("MONDAY")); // MONDAY
System.out.println(day1.values()[0]); // SUNDAY
System.out.println(day1.compareTo(day2)); // 1
```

> Ref : https://hyeonstorage.tistory.com/174

<br/>

### java.lang.Enum

모든 enum 타입은 java.lang.Enum 클래스를 상속받는다. Enum 클래스는 추상 클래스이기 때문에 Enum 클래스의 객체를 만들 수 없다.

java.lang.Enum도 클래스의 한 종류이므롤 최고 조상인 Object 클래스를 상속받는다. 이 때, Object 클래스에서 정의된 메소드를 재정의하며 메소드 중 일부는 final로 선언되어 다시 재정의 할 수 없게 만든다.

위에 정의된 메소드들 역시 java.lang.Enum 클래스에 정의된 함수이다.

<br/>

### EnumSet

**EnumSet**이란? **Set 인터페이스를 기반**으로 열거형 타입의 집합을 표현해주는데 사용된다.

지정해놓은 요소들을 보다 빠르게 배열처럼 다룰 수 있는 기능을 제공한다.

```java
enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}

EnumSet<Day> enumSet = EnumSet.allOf(Day.class); // 지정한 Type의 모든 원소를 포함하는 EnumSet 생성
System.out.println(enumSet); // [SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY]

EnumSet<Day> partEnumSet = EnumSet.of(Day.SUNDAY, Day.THURSDAY); // 지정한 원소를 포함하는 EnumSet 생성
System.out.println(partEnumSet); // [SUNDAY, THURSDAY]
```

EnumSet은 다음과 같은 특징을 가지고 있다. 

1. EnumSet은 Set 인터페이스를 완벽하게 구현하고있다.
2. 타입이 안전하다.
3. EnumSet 내부는 비트 벡터를 사용하고, enum 값 개수가 64개 이하인 경우에는 EnumSet은 long 값 하나만 사용한다. (껍데기는 Set이지만 내부는 비트필드이다.)
4. 불변객체는 생성할 수 없다.

> 비트필드란?
> 비트별 OR을 사용해 여러 상수를 하나의 집합으로 모을 수 있으며, 이렇게 만들어진 집합을 말한다.
>
> ```java
> public class Text {
>     public static final int STYLE_BOLD =          1 << 0; //1
>     public static final int STYLE_ITALIC =        1 << 1; //2
>     public static final int STYLE_UNDERLINE =     1 << 2; //4
>     public static final int STYLE_STRIKETHROUGH = 1 << 3; //8
>     
>     // 매개 변수 styles는 0개 이상의 STYLE_ 상수를 비트별 OR 한 값이다.
>     public void applyStyle(int styles) {...}
> }
> 
> Text text = new Text();
> 
> text.applyStyle(STYLE_BOLD | STYLE_ITALIC); // STYLE_BOLD | STYLE_ITALIC 의 조합을 3으로 표현.
> text.applyStyle(STYLE_BOLD | STYLE_UNDERLINE); // STYLE_BOLD | STYLE_UNDERLINE 의 조합을 5로 표현.
> text.applyStyle(STYLE_ITALIC | STYLE_UNDERLINE); // STYLE_ITALIC | STYLE_UNDERLINE 의 조합을 6으로 표현.
> text.applyStyle(STYLE_BOLD | STYLE_ITALIC | STYLE_UNDERLINE); // STYLE_BOLD  | STYLE_ITALIC | STYLE_UNDERLINE 의 조합을 7로 표현.
> ```

> Ref : https://jaehun2841.github.io/2019/02/04/effective-java-item36/

#### 참고 자료

https://blog.naver.com/swoh1227/222219695440

https://yadon079.github.io/2021/java%20study%20halle/week-11