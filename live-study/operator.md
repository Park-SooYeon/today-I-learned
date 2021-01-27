# 연산자

## 목표

자바가 제공하는 다양한 연산자를 학습하세요.

## 학습할 것

- [산술 연산자](#산술-연산자)
- [비트 연산자](#비트-연산자)
- [관계 연산자](#관계-연산자)
- [논리 연산자](#논리-연산자)
- [instanceof](#instanceof)
- [assignment(=) operator](#assignment(=)-operator)
- [화살표(->) 연산자](#화살표(->)-연산자)
- [3항 연산자](#3항-연산자)
- [연산자 우선 순위](#연산자-우선-순위)
- [(optional) Java 13. switch 연산자]((optional)-Java-13.-switch-연산자)

---

#### 용어 정리

* 연산자 : 연산에 사용되는 기호
* 피연산자 : 연산에 대상이 되는 데이터

### 산술 연산자

수학적 계산(사칙 연산)에 사용되는 연산자를 의미한다.

산술 연산자를 통한 연산 시, 자료형 크기에 따른 오버플로우 문제를 고려해주어야 한다.

정수 나누기의 경우, 0으로 나누게 되면 ArithmeticException 오류가 발생한다.

실수 나누기의 경우, 0.0으로 나누게 되면 `INF` 및 `NaN`이 발생할 수 있다.

실수는 연산 과정에서 항상 오차가 발생할 수 있기 때문에 오차 허용 범위를 반드시 지정해주어야한다.

| 연산자 | 설명                                                         |
| ------ | ------------------------------------------------------------ |
| +      | 더하기 (문자열 추가에도 사용할 수 있다.)                     |
| -      | 빼기                                                         |
| *      | 곱하기                                                       |
| /      | 나누기 (몫)                                                  |
| %      | 나누기 (나머지)                                              |
| INF    | 엄청 큰 숫자를 의미<br />오버플로를 일으키는 부동 연산 또는 0으로 나누는 부동 소수점 연산에 의해 생성 |
| NaN    | 숫자가 아니거나 의미없는 작업으로 인해 발생하는 값<br />0을 0으로 나누거나 0을 나머지로 계산하여 생성 |

```java
// 정수 연산
int a = 10;
int b = 2;
int c = 0;

int sum1 = a + b; // 12
int diff1 = a - b; // 8
int divide1 = a / b; // 5
int remainder1 = a % b; // 0
int exception1 = a / c; // 컴파일 에러
int exception1 = a % c; // 컴파일 에러

// 실수 연산
double d = 10;
double e = 2;
double f = 0;

double sum2 = d + e; // 12.0
double diff2 = d - e; // 8.0
double divide2 = d / e ; // 5.0
double remainder2 = d % e ; // 0.0

double infinity = d / f; // INF
double nan1 = d % f // NaN
double nan2 = f / f; // NaN
```

#### 단항 연산자

| 연산자  | 설명                                                         |
| ------- | ------------------------------------------------------------ |
| + / -   | 양수, 음수를 나타냄                                          |
| ++ / -- | ++ 혹은 -- 가 피연산자의 앞 : **식 읽기 전**, 1 증가 및 감소<br />++ 혹은 -- 가 피연산자의 뒤 : **식 읽은 후**, 1 증가 및 감소 |

```java
int n = 1;
int m = 10;

// 해당 명령 실행 전, n에 1을 더함
int sum1 = ++n + m; // 12, (n + 1) + m 와 동일
// 해당 명령 실행 후, n에 1을 더함
int sum2 = n++ + m; // 12, n + m 과 동일
System.out.println(n); // 3
```

> 산술 연산을 실행할 때, 타입 프로모션이 실행되는데 이때 타입 변환은 피연산자 중, 가장 큰 타입으로 결과 유형을 가진다.
> ex) int + long = long / float + double = double / int + double = double

<br/>

### 비트 연산자

비트(bit) 단위로 논리 연산에 사용되는 연산자를 말한다.

각각의 비트를 대상으로 연산을 진행하며 피연산자는 **반드시 정수**여야 한다.

비트 연산은 일반 산술 연산보다 속도가 빠르기 때문에 성능 개선을 위해서 필요한 부분에 사용한다.

| 비트 연산자 | 설명                                                       |
| ----------- | ---------------------------------------------------------- |
| &           | 비트단위로 AND 연산 수행                                   |
| \|          | 비트 단위로 OR 연산 수행                                   |
| ^           | 비트 단위로 XOR 연산 수행                                  |
| ~           | 피연산자의 모든 비트를 반전시키는 NOT 연산 수행 (1의 보수) |

각 연산에 따른 진리표 결과 값은 아래와 같다

| 입력 1 | 입력 2 | AND 결과 | OR 결과 | XOR 결과 | NOT (입력 1 기준) 결과 |
| :----: | :----: | :------: | :-----: | :------: | :--------------------: |
|   0    |   0    |    0     |    0    |    0     |           1            |
|   0    |   1    |    0     |    1    |    1     |           1            |
|   1    |   0    |    0     |    1    |    1     |           0            |
|   1    |   1    |    1     |    1    |    0     |           0            |

| 비트 시프트 연산자 | 설명                                                         |
| ------------------ | ------------------------------------------------------------ |
| <<                 | 피연산자의 비트 열을 왼쪽으로 이동<br />이동에 따른 빈 공간은 0으로 채움 |
| >>                 | 피연산자의 비트 열을 오른쪽으로 이동<br />이동에 따른 빈 공간은 음수는 1, 양수는 0으로 채움 |
| >>>                | 피연산자의 비트 열을 오른쪽으로 이동<br />이동에 따른 빈 공간은 0으로 채움 |

```java
int a = 3; // 0000 0011
int b = 1; // 0000 0001
int c = -1; // 1111 1111

// 비트 연산
int and = a & b; // 0000 0011 & 0000 0001 => 0000 0001
int or = a | b; // 0000 0011 | 0000 0001 => 0000 0011
int xor = a ^ b; // 0000 0011 ^ 0000 0001 => 0000 0010
int not = ~a; // ~0000 0011 => 1111 1100

// 비트 시프트 연산
int leftShift = a << 1; // 0000 0110
int rightShift = c >> 1; // 1111 1111
int threeRightShift = c >>> 1; // 0111 1111
```

<br/>

### 관계 연산자

피연산자를 비교할 때 사용되는 연산자를 말한다.

관계 연산자의 결과 유형은 모두 boolean이다.

| 관계 연산자 | 설명                                                         |
| ----------- | ------------------------------------------------------------ |
| ==          | 피연산자가 동일하면 true                                     |
| !=          | 피연산자가 다르면 true                                       |
| >           | 왼쪽 피연산자의 값이 크면 true                               |
| >=          | 왼쪽 피연산자의 값이 오른쪽 피연산자의 값보다 크거나 같으면 true |
| <           | 오른쪽 피연산자의 값이 크면 true                             |
| <=          | 오른쪽 피연산자의 값이 왼쪽 피연산자의 값보다 크거나 같으면 true |

```java
int a = 10;
int b = 5;

System.out.println(a == b); // false
System.out.println(a >= b); // true

// 문자열 비교
String str = "Test";
System.out.println(str == new String("Test")); // false
```

> String에 대한 비교를 하려면 관계 연산자보다는 `equals()`를 사용하는 것이 좋다.
> ⇒ String은 참조형이기 때문에 **주소값을 비교**해서 문자열이 같아도 false가 나오는 경우가 있다.

<br/>

### 논리 연산자

논리식들의 결과를 얻기 위해 사용되는 연산자이다.

| 논리 연산자 | 설명                                         |
| ----------- | -------------------------------------------- |
| &&          | 논리식이 모두 참이면 true (논리 AND 연산)    |
| \|\|        | 논리식이 하나라도 참이면 true (논리 OR 연산) |
| !           | 논리식의 결과를 반환 (논리 NOT 연산)         |

```java
int a = 5;
int b = 10;

System.out.println(a > b && a == b + 5); // true && true => true
System.out.println(a < b && a == b + 5); // false && true => false
System.out.println(a > b || a == b - 5); // true && false => true
System.out.println(a < b || a == b - 5); // false && false => false
System.out.println(!(a < b)); // false => true
System.out.println(!(a > b)); // true => false
```

> 논리 연산은 **Short-Circuit Evaluation (SCE)**의 특성을 가지고 있다.
> ⇒ && 연산일 때, 왼쪽이 false면 이후 논리식을 확인하지 않는다.
> ⇒ || 연산일 때, 왼쪽이 true면 이후 논리식을 확인하지 않는다.

<br/>

### instanceof

객체가 특정 클래스 혹은 인터페이스의 유형인지를 확인하는 명령어이다.

주로 생성된 객체가 **클래스(부모 클래스 포함) 혹은 인터페이스의 인스턴스인지를 확인**할 때 많이 사용된다.

사용 방법은 아래와 같다.

```
(객체) instanceof (클래스 혹은 인터페이스)
```

```java
interface ParentInterface {}
class Parent {}

class Child1 extends Parent implements ParentInterface {}
class Child2 extends Parent {}
class Child3 implements ParentInterface {}

public class Main {
  public static void main(String[] arges) {
    Parent parent = new Parent();
    Child1 child1 = new Child1();
    Child2 child2 = new Child2();
    Child3 child3 = new Child3();

    System.out.println(parent instanceof Child1); // false
      
    System.out.println(child1 instanceof Parent); // true
    System.out.println(child1 instanceof ParentInterface); // true
    System.out.println(child1 instanceof Child1); // true

    System.out.println(child2 instanceof Parent); // true
    System.out.println(child2 instanceof ParentInterface); // false
    System.out.println(child2 instanceof Child2); // true
	
    // instanceof는 객체를 클래스 혹은 인터페이스로 casting 가능 유무를 확인한다.
    // 따라서 객체와 클래스가 전혀 상호 상속관계가 없으면 캐스팅이 불가능해서 컴파일 에러가 난다.
    System.out.println(child3 instanceof Parent); // 컴파일 에러
    System.out.println(child3 instanceof ParentInterface); // true
    System.out.println(child3 instanceof Child3); // true
  }
}
```

> null은 어떤 것의 인스턴스가 아니다. 따라서 instanceof의 결과는 항상 **false** 이다.

> Ref : https://sowon-dev.github.io/2020/08/15/200815javai/

<br/>

### assignment(=) operator

**대입 연산자**는 연산자 오른쪽의 값을 연산자 왼쪽 변수에 대입할 때 사용하는 연산자이다.

다른 연산자와 묶어서 사용이 가능하다.

![image](https://user-images.githubusercontent.com/49746644/105977431-b753fc80-60d4-11eb-9dc1-caaaa667d266.png)

> Java에서 참조 타입은 주소값을 대입값으로 할당한다.

> a = b = c = 4; 로 한꺼번에 여러개의 변수에 값을 대입할 수 있다.

<br/>

### 화살표(->) 연산자

Java 8버전부터 추가된 람다 표현식을 사용하기 위해 사용하는 연산자이다.

화살표 연산자는 람다식으로 함수형 프로그래밍을 할 때, 사용된다.

아래와 같이 사용할 수 있다.

```
(파라미터) -> {함수 내부}
```

```java
// 일반 함수식 사용
public void func(int a) {
	System.out.println(a);
}

// 람다식 사용
// 람다식은 익명으로 생성되어 사용된다.
(a) -> {
	System.out.println(a);
}
```

> 람다식 사용 예제 : https://coding-factory.tistory.com/265

<br/>

### 3항 연산자

if-else문과 비슷하며 조건식에 따라 값을 다르게 세팅할 수 있다.

아래와 같이 사용할 수 있다.

```java
조건 ? 참일 때 값 : 거짓일 때 값;
```

```java
int a = 10;
int b = 5;

String c = a > b ? "true" : "false"; // "true"
```

<br/>

### 연산자 우선 순위

연산자 우선 순위에 따라 프로그램이 의도한대로 동작하지 않을 수 있으므로 우선 수행하고 싶은 연산은 `()`로 묶어주는 것이 좋다.

![image](https://user-images.githubusercontent.com/49746644/105977637-f2563000-60d4-11eb-8607-1b109b5524b3.png)

<br/>

### (optional) Java 13. switch 연산자

기존의 switch 구문의 사용방법이 변화되었다.

Java 12 버전에서 콜론(:) 대신 **화살표(->) 연산자**를 사용할 수 있다.

Java 13 버전에서 break 대신 **yield**를 사용한다.

```java
// 기존의 switch 문
int num = 0;

switch (num) {
    case 0:
        System.out.println("case 0");
        break;
    case 1:
    case 2:
        System.out.println("case 1 or case 2");
        break;
    default:
        throw new Exception("not match");
}

// 변경된 switch 문
int num = 0;

switch (num) {
    case 0 -> {
        System.out.println("case 0");
        break;
    }
    case 1, 2 -> {
        System.out.println("case 1");
        break;
    }
    default -> {
        throw new Exception("not match");
    }
}
```

> 콜론을 사용했을 경우, yield가 빠지면 **런타임 에러**가 발생
> 화살표 연산자를 사용했을 경우, yield가 빠지면 **컴파일 에러**가 발생한다.

> Ref : https://docs.oracle.com/en/java/javase/13/language/switch-expressions.html

### 참고 자료

https://github.com/seovalue/java-study/blob/main/note/3.%20%EC%97%B0%EC%82%B0%EC%9E%90.md

https://kils-log-of-develop.tistory.com/336

