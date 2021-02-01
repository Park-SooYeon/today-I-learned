# 패키지

## 목표

자바의 패키지에 대해 학습하세요.

## 학습할 것 (필수)

- [package 키워드](#package-키워드)
- [import 키워드](#import-키워드)
- [클래스패스](#클래스패스)
- [CLASSPATH 환경변수](#CLASSPATH-환경변수)
- [-classpath 옵션](#-classpath-옵션)
- [접근지시자](#접근지시자)

---

### package 키워드

**package**란? 서로 관련이 있는 클래스와 인터페이스를 하나의 묶음으로 구성하는 것을 의미한다. (디렉토리(폴더)와 비슷한 의미)

패키지를 사용함으로써 클래스를 효율적으로 관리할 수 있고, 아래와 같은 편리성을 제공한다.

1. 클래스 이름이 겹치더라도 패키지가 다르면 중복 문제가 발생하지 않는다. (Name Space)
2. 패키지별로 접근제어가 가능해진다.
3. 서로 관련된 클래스를 모아놓기 때문에 필요한 클래스를 찾는게 쉬워진다.
4. 클래스들의 연관성을 쉽게 확인할 수 있다.

패키지는 반드시 **소스파일의 첫번째 라인에 위치해야하며 단 하나만 지정이 가능**하다.

패키지는 아래와 같이 사용할 수 있다.

```java
// 패키지 정의
package 패키지명;
package java.awt.event; // java.awt.event 패키지 정의

// 패키지 가져오기
import 패키지명.클래스명;
import java.awt.event.*; // java.awt.event 패키지의 모든 클래스를 가져오는 것을 의미
```

>  **이름 없는 패키지** : Java의 모든 클래스는 하나의 패키지에 포함되어야하는데, 패키지를 선언하지 않을 경우 자동으로 **default 패키지**에 저장된다.
> ⇒ 이름 없는 패키지의 사용을 지양하는 것이 좋다.

> 패키지 명명 규칙
>
> * 패키지명은 소문자로 작성한다.
> * 자바의 예약어는 사용할 수 없다.
> * 상위 패키지와 하위 패키지는 .을 사용해서 구분한다.
> * 회사에서 사용할 경우, **도메인의 역순**으로 패키지명을 사용한다.
> * java 패키지 : 자바에서 기본적으로 제공하는 패키지
> * javax 패키지 : 자바에서 기본적으로 제공하는 확장 패키지
> * org 패키지 : 비영리 단체(오픈소스)에서 만든 패키지
> * com 패키지 : 기업에서 만든 패키지

<br/>

### import 키워드

**import**

import 키워드는 다른 패키지에 있는 클래스를 가져다 사용하기 위한 키워드이다.

아래와 같이 선언하여 가져온 클래스를 사용할 수 있다.

```java
import 패키지명.클래스명;
import 패키지명.*; // 패키지 내부 모든 멤버를 가져옴 (하위 클래스 미포함)
```

**static import**

static import문을 사용하면 static 필드 혹은 static 메소드를 호출할 때, 클래스 이름을 생략할 수 있다.

특정 클래스의 static 멤버를 자주 사용할 때 편리하다.

```java
import static java.lang.Math.*;  // Math 패키지의 모든 멤버(필드, 메소드) import
import static java.lang.System.out;   // System.out을 out만으로 참조가능.

public class ImportExample {
    public static void main(String[] args) {
        // System.out.println(Math.random());와 동일
        out.println(random());
 
        // System.out.println("Math.PI" + Math.PI);와 동일
        out.println("Math.PI : " + PI);
     }
}
```

> import문을 사용하지 않고 아래와 같이 패키지명과 클래스명으로 클래스를 가져와 사용할 수 있지만 코드 가독성이 안좋아지므로 잘 사용하지 않는다.
>
> ```java
> // java.awt.event 패키지의 ActionEvent 클래스 객체 생성
> java.util.Scanner sc = new java.util.Scanner();
> 
> // import문을 사용했을 경우
> import java.util.Scanner;
> Scanner sc = new Scanner();
> ```

> java.lang 패키지 : 가장 기본이 되는 클래스들을 포함하고 있는 패키지
> ⇒ 모든 소스파일에 묵시적으로 선언되어있어 String, System은 기본적으로 사용할 수 있다.

<br/>

### 클래스패스

**classpath**란? JVM이 프로그램을 실행할 때, 클래스 파일(.class)을 찾는데 **기준이 되는 파일 경로**를 의미한다.

JVM은 프로그램을 실행할 때 classpath를 기준으로 클래스 파일을 찾는다. 만약 해당 classpath가 지정되어 있지 않다면 JVM은 현재 디렉토리를 기준으로 클래스 파일을 찾는다.

**classpath에 사용할 수 있는 값**

* 디렉토리
* zpi 파일
* jar(자바 아카이브) 파일

<br/>

### CLASSPATH 환경변수

**classpath를 지정하는 방법**

1. 시스템 환경 변수를 지정하는 방법

   windows의 경우, window 환경변수에 classpath 변수를 지정함으로써 설정 가능

   `내 PC 우클릭 후 속성 → 고급 시스템 설정 → 환경 변수`에서 classpath를 지정한다.

   ![image](https://user-images.githubusercontent.com/49746644/106438192-83ece580-64b9-11eb-9cb0-672b142a535a.png)

   환경 변수 설정 (;로 경로를 여러개 지정해줄 수 있다. 아래는 Java8, Java11 버전의 classpath 지정)

   ⇒ 실행 프로그램(JVM)에서 사용하는 라이브러리의 위치 지정

   ![image](https://user-images.githubusercontent.com/49746644/106438371-c7475400-64b9-11eb-87ed-f2d7ec58a4fc.png)

2. -classpath 옵션을 사용하여 지정하는 방법

> Ref : https://blog.naver.com/hsm622/220979792342

<br/>

### -classpath 옵션

자바 명령어의 사용법은 아래와 같다.

```
java <option> <classfile> <argument>
```

* option : 옵션
* classfile : 호출될 클래스 파일 이름
* argument : main 함수에 매개변수로 보내질 문자열

java 명령어의 option 중, `-classpath(-cp)`는 시스템 변수에 classpath를 선언하지 않고 컴파일 시기에 참조할 classpath를 선언해주는 옵션이다. jar 파일, zip 파일, 클래스 파일의 디렉토리 위치를 적어주면 된다.

<br/>

### 접근지시자

접근지시자(접근제한자)는 멤버 또는 클래스를 외부에서 접근하지 못하도록 제한하는 역할을 한다.

접근지시자의 종류는 아래와 같다.

| 접근제한자 종류 | 선언 클래스 내부 | 패키지 내부 | 상속관계 클래스 | 다른 패키지/클래스 |
| --------------- | ---------------- | ----------- | --------------- | ------------------ |
| public          | Y                | Y           | Y               | Y                  |
| protected       | Y                | Y           | Y               | N                  |
| (default)       | Y                | Y           | N               | N                  |
| private         | Y                | N           | N               | N                  |

또한, 대상별로 사용 가능한 접근제한자는 아래와 같다.

| 대상      | 사용 가능한 접근제한자                |
| --------- | ------------------------------------- |
| 클래스    | public, (default)                     |
| 메소드    | public, protected, (default), private |
| 멤버 변수 | public, protected, (default), private |
| 지역 변수 | 없음                                  |

#### 참고 자료

https://blog.naver.com/swoh1227/222189270657

https://yadon079.github.io/2020/java%20study%20halle/week-07