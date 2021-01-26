# 자바 데이터 타입, 변수 그리고 배열

## 목표

자바의 프리미티브 타입, 변수 그리고 배열을 사용하는 방법을 익힙니다.

## 학습할 것

- [프리미티브 타입 종류와 값의 범위 그리고 기본 값](#프리미티브-타입-종류와-값의-범위-그리고-기본-값)
- [프리미티브 타입과 레퍼런스 타입](#프리미티브-타입과-레퍼런스-타입)
- [리터럴](#리터럴)
- [변수 선언 및 초기화하는 방법](#변수-선언-및-초기화하는-방법)
- [변수의 스코프와 라이프타임](#변수의-스코프와-라이프타임)
- [타입 변환, 캐스팅 그리고 타입 프로모션](#타입-변환,-캐스팅-그리고-타입-프로모션)
- [1차 및 2차 배열 선언하기](#1차-및-2차-배열-선언하기)
- [타입 추론, var](#타입-추론,-var)

---

### 프리미티브 타입 종류와 값의 범위 그리고 기본 값

**Primitive Type이란?** java의 기본 타입(원시 타입)들 의미하며 총 8개의 종류가 있다.

어떠한 객체의 참조 값을 가지지 않기 때문에 Null을 저장할 수 없다.

![image](https://user-images.githubusercontent.com/49746644/105824792-2c0a3680-6002-11eb-8de9-ff6ec0b570dc.png)

8bit = 1byte

* boolean : **참, 거짓**을 나타내는 타입
* char : **유니코드 문자**를 나타내는 타입, unsigned 2byte이다.
* byte : 1byte를 차지하는 **정수형** 타입
* short : 2byte를 차지하는 정수형 타입
* int : 4byte를 차지하는 정수형 타입
* long : 8byte를 차지하는 정수형 타입
* float : 4byte를 차지하는 부동소수점 타입
* double : 8byte를 차지하는 부동소수점 타입

> 부동소수점은 정밀도가 있지만 값의 정확한 연산을 보장하지 못하므로 화폐 계산과 같은 연산을 할 때에는 `BigDecimal` 클래스를 사용하는 것이 좋다고 한다.

> 자바에서는 unsigned 타입의 자료형을 지원하지 않지만 자바 8부터 Integer와 Long의 Wrapper class에 unsigned 관련한 static 메소드가 추가되었다.

</br>

### 프리미티브 타입과 레퍼런스 타입

**Reference Type이란?** primitive type를 제외한 모든 타입을 포함한다.

배열, enum(열거), 클래스, 인터페이스 등이 있다.

변수에 값을 저장하는 것이 아닌 객체에 대한 **참조 메모리 주소값을 저장**하여 관리하기 때문에 Null을 사용할 수 있다.

변수에 참조를 저장하기 때문에 여러개의 변수가 같은 참조를 가르키고 있다면 객체의 상태를 변경하였을 때, 다른 변수에서 영향을 받을 수 있다.
⇒ 이러한 영향 때문에 얕은 참조, 깊은 참조의 차이를 알아야한다.

**PrimitiveType vs Reference Type**

* Reference 변수로 생성된 객체에 접근 및 조작하고 값을 가져올 수 있다.
* Null을 사용할 수 있다.
* Generic을 사용할 수 있다.
* Primitive Type은 JVM의 **런타입 스택 영역**에 변수를 저장한다.
* Reference Type은 실제 값을 **힙 영역**에 저장하고 변수는 힙 영역에 존재하는 객체의 주소값을 저장한다.

> Primitive Type을 인스턴스로 다루기 위해 Wrapper Class를 사용할 수 있다.
> ⇒ Boxing, Unboxing을 수행하기 때문에 성능 상의 차이가 있을 수도 있다.

> Boxing : Primitive Type을 Referenece Type으로 변환하는 것
>
> Unboxing : Referenece Type을 Primitive Type으로 변환하는 것

> 얕은 참조 : 변수에 저장된 **주소 값**을 복사하는 것
>
> 깊은 참조 : 완전히 똑같은 새로운 객체를 만들어 **객체**를 복사하는 것

</br>

### 리터럴

**리터럴이란?** 실제로 저장되는 값 그 자체로 메모리에 저장되어 변화하지 않는 고정된 값을 나타낸다.

변수를 초기화할 때, 대입 연산자를 기준으로 `우항에 사용되는 값`을 의미한다고 보면 된다.

**정수 리터럴**

정수 리터럴은 표현에 따라 2진법, 8진법, 10진법, 16진법으로 나타낼 수 있으며 `L` 혹은 `l`을 사용하여 long 리터럴로 생성할 수 있다. (기본은 int 리터럴로 생성된다.)

```java
int a = 10; // 10진법
int b = 010; // 8진법
int c = 0x1a; // 16진법
int d = 0b11010; // 2진법
long e = 1000L; // long 리터럴
```

**부동 소수점 리터럴**

`F` 혹은 `f`를 사용하여 float 리터럴로 `D` 혹은 `d`를 사용하여 double 리터럴로 생성할 수 있다. (기본은 double 리터럴로 생성된다.)

`E` 혹은 `e`를 사용하여 지수 표현식으로도 작성할 수 있다.

```java
float a = 0.01F; // float 리터럴
double b = 0.01; // double 리터럴
double c = 0.01D; // double 리터럴
double d = 1.234e2; // double 리터럴
```

> 자바 7부터 숫자 리터럴의 숫자 사이에 밑줄 문자를 사용할 수 있다.
> ⇒ 문자열에서 문자가 올 것으로 예상되는 위치에 사용 가능

**문자 및 문자열 리터럴**

영어, 한글, 유니코드로 문자 및 문자열 리터럴을 생성할 수 있다.

```java
char a = 'C'; // 문자 리터럴
char b = '한'; // 문자 리터럴
char c = '\u1234'; // 문자 리터럴
String d = "Hello World" // 문자열 리터럴 
```

**논리 리터럴**

논리값을 표현하는 리터럴을 말한다.

```java
boolean a = true;
boolean b = false;
```

</br>

### 변수 선언 및 초기화하는 방법

```java
int a; // 변수 선언 (타입에 맞는 메모리 공간 확보)
a = 100; // 대입 연산자(=)를 통해 변수 초기화 (타입에 맞는 리터럴 저장)

int b = 200; // 변수 선언과 초기화를 한번에 진행할 수 있다.
```

> 변수의 종류
>
> * 인스턴스 변수 : 클래스 선언시 static 없이 선언된 필드, 인스턴스 별로 다른 값을 가진다.
> * 클래스 변수 : 클래스 선언시 **static**과 함께 선언된 필드, 클래스에 의해 생성된 인스턴스들이 공유하는 값으로 클래스 명으로 접근이 가능하다.
> * 로컬 변수 : 메서드 영역에서 선언된 변수, 메서드 영역에서만 사용할 수 있다.
> * 매개 변수 : 메서드의 인자로 전달되는 변수

</br>

### 변수의 스코프와 라이프타임

**스코프 (scope)란?** 변수가 유효한 범위를 의미한다.

**Static Variable (Class Member Variable)**

static 키워드가 붙은 필드 변수로 **클래스 내부에서 전역적으로 접근**이 가능하다.

인스턴스 생성 이전에 미리 로드되어 생성되고 Heap 영역에서 관리된다.

클래스가 참조되는 동안 살아있다가 참조가 사라지면 GC에 의해 종료된다.

```java
public class someClass {
	static int a = 10;
	public static void main(String[] args) {
		System.out.println("result : " + a);
	}
}
```

**Instance Variable**

class scope에서 작성된 변수로 **인스턴스 내부에서 접근**이 가능한다.

인스턴스 생성 시, 메모리 공간을 만들고 값을 초기화한다.

인스턴스 생성 후, 인스턴스가 참조되는 동안 살아있다가 참조가 사라지면 GC에 의해 종료된다.

```java
public class someClass {
    int a = 10;
}

public class otherClass {
    public static void main(String[] args) {
        System.out.println("result : " + a); // 스코프가 다르므로 오류가 난다.
    }
}
```

**Local Variable**

block 내부에서 선언된 지역 변수로 **해당 block 내부에서 접근**이 가능하다.

해당 변수 선언 후, 변수가 선언된 block이 끝나면 사라진다.

```java
public class someClass {
    public void otherMethod() {
    	System.out.prinln"result : " + a); // a 변수가 초기화 되지 않았기 때문에 오류가 난다.
    }
    
    public static void main(String[] args) {
        int a = 10;
        System.out.prinln"result : " + a);
    }
}
```

위 내용을 표로 정리해보면 아래와 같다.

| 변수 종류       | 유효 범위(scope)                          | 라이프타임                                     |
| --------------- | ----------------------------------------- | ---------------------------------------------- |
| Static Variable | 클래스 전체 (class scope)                 | 클래스가 초기화되고 클래스 참조가 끝날 때 까지 |
| Instance Member | 클래스 전체 (class scope)                 | 객체가 생성되고 객체 참조가 끝날 때 까지       |
| Local Variable  | 변수가 선언된 블록({}) 내부 (block scope) | 변수 선언 후, 해당 블록을 벗어날 때 까지       |

</br>

### 타입 변환, 캐스팅 그리고 타입 프로모션

#### 타입 변환 (Type Conversions)

어떤 값이나 변수의 타입을 다른 타입으로 변경하는 것을 말한다.

타입 변환에는 크게 확장, 축소(Casting)으로 나눌 수 있다.

**확장, 자동 형변환 (프로모션, Promotion)**

데이터 타입이 자동으로 변환되는 경우를 말한다.

작은 데이터 타입에서 큰 범위의 데이터 타입에 할당될 때에 동작한다.

자바에서 숫자 타입은 `byte → short → int → long → float → double` 순으로 자동 형변환이 된다.

```java
int a = 10;
long b = a; // 자동 형변환이 수행된다.
```

**축소, 명시적 형변환 (캐스팅, Casting)**

어떠한 타입을 다른 타입으로 **명시적으로 변환**하는 것을 의미한다.

큰 데이터 타입에서 작은 범위의 데이터 타입에 할당할 때 사용한다.
⇒ 원본 데이터가 손실될 수 있기 때문에 조심해서 사용해야한다.

Primitive Type 간의 형변환 말고도 구현, 상속 관계의 Class 간의 형변환도 가능하다.

```java
long a = 10;
int b = (int) a; // (int)을 통해 형변환이 수행된다.

// 아래와 같이 int와 char 형변환도 가능하다.
char ch = 'c';
int num = 10;
ch = (char) num; // 문자는 유니코드 값으로 사용될 수 있기 때문에 형변환이 가능하다.

// 아래와 같이 class 간의 형변환도 가능하다.
// Up Casting : 자식 클래스에서 부모 클래스로 캐스팅 하는 것으로 컴파일러에 의해 수행
// 상위 클래스의 공통 메서드, 상태는 하위 클래스에서 사용 가능
// 하위 클래스의 재정의 메서드, 메서드, 상태는 상위 클래서에서 사용 불가능
Parent child = new Child();
child.name = "name";
child.getDegree(); // Child의 메서드 접근 - 컴파일 에러
// Down Casting : 부모 클래스에서 자식 클래스로 캐스팅 하는 것
Child childTwo = (Parent) childOne;
childTwo.getDegree(); // 사용가능
// 즉, 하위 클래스의 특정 멤버(변수, 메서드)를 사용하려면 다운 캐스팅을 해야한다.
```

#### 타입 프로모션 (Type Promotion)

연산식을 통해 값을 계산할 때, 중간에 피연산자의 범위를 초과할 수 있기 때문에 자동으로 값이 승격되는 것을 말한다.

자바에서는 `byte`, `short`, `char`를 계산시 자동으로 int로 프로모션 된다.

피연산자가 `long`, `float`, `double`인 경우 전체 표현식을 각각 `long`, `float`, `double`로 프로모션 된다.

```java
byte a = 42;
short b = 10;
int c = 100;

int result = a + b + c; // 자동으로 형변환 되어 피연산자의 타입으로 프로모션 된다.
```

> 타입 캐스팅과 타입 프로모션을 구분하기 위해서는 메모리 크기가 아닌 **데이터 표현 범위**를 따져봐야한다.
> ⇒ 타입 변환 시, 변환 할 데이터 타입의 표현 범위가 모두 수용할 수 있으면 타입 프로모션, 수용할 수 없으면 타입 캐스팅이라고 한다.

</br>

### 1차 및 2차 배열 선언하기

**1차원 배열 선언하기**

```java
// 1차원 배열 선언
int[] arr1; // 가장 선호하는 스타일
int []arr2;
int arr3[];

// 1차원 배열 초기화
arr1 = new int[length]; // length만큼의 길이를 가지는 배열 생성
arr2 = {100, 200, 300};
arr3 = new int[] {100, 200, 300};
```

**2차원 배열 선언하기**

```java
// 2차원 배열 선언
int[][] arr1;

// 2차원 배열 초기화
arr1 = new int[length1][length2];
int[][] arr2 = {
    {100, 200, 300},
    {400, 500, 600}
}
```

</br>

### 타입 추론, var

**타입 추론 (Type Inference)이란?** 정적인 언어에서 컴파일러가 **컴파일 시점에서** 변수를 초기화하는 리터럴을 판단하여 해당 타입으로 변환 하는 것을 말한다.

타입 추론을 통해 반복되는 타입 선언을 줄여주고, 코드의 가독성을 높여준다.

타입 추론의 예시는 아래와 같다.

```java
List<String> cs = Collections.<String>emptyList();
List<String> cs = Collections.emptyList(); // 타입 추론에 의해 <String> 생략

Map<String, List<String>> map = new HashMap<String, List<String>>();
Map<String, List<String>> map = new HashMap<>(); // 타입 추론에 의해 String, List<String> 생략

Predicate<String> value = (String x) -> x.length() > 0;
Predicate<String> value = x -> x.length() > 0; // 타입 추론에 의해 String 생략
```

JDK 10부터 **var**라는 새로운 Type를 지원하여 var 변수는 지정된 타입이 없어 타입 추론에 의해 알아서 변수의 타입이 지정된다.
⇒ var로 선언한 변수는 초기화한 데이터의 타입을 사용하기 때문에 변수에 저장된 값을 다른 타입으로 변경하는 것이 불가능하다.

다만, var는 타입 추론을 통해 변수의 타입을 지정하기 때문에 아래와 같은 몇가지 제약조건이 존재한다.

1. 로컬 변수로 사용해야한다.
2. 선언과 동시에 값을 할당해야한다.
3. null로 초기화하면 안된다.
4. 람다 표현식에는 var 타입 변수를 사용할 수 없다.
5. 타입 없이 배열 초기값을 넘겨도 작동하지 않는다.

```java
var str = "Hello world";
var num1 = 10;
var num2 = 10.10D;

System.out.println(str); // Hello world
System.out.println(num1 + num2); // 20.1

var num = 100;
num = "Hello world"; // 컴파일 에러
```

