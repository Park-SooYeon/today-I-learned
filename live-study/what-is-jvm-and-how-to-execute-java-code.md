# JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가.

## 목표

자바 소스 파일(.java)을 JVM으로 실행하는 과정 이해하기.

## 학습할 것

- [JVM이란 무엇인가](#JVM이란-무엇인가)
- [컴파일 하는 방법](#컴파일-하는-방법)
- [실행하는 방법](#실행하는-방법)
- [바이트코드란 무엇인가](#바이트코드란-무엇인가)
- [JIT 컴파일러란 무엇이며 어떻게 동작하는지](JIT-컴파일러란-무엇이며-어떻게-동작하는가)
- [JVM 구성 요소](#JVM-구성-요소)
- [JDK와 JRE의 차이](#JDK와-JRE의-차이)

---

### JVM이란 무엇인가

**JVM (Java Virtual Machine)** 은 자바를 실행하기 위한 가상 머신으로 하는 역할은 크게 아래 두가지가 있다.

역할

* Java 코드와 OS 사이의 중개자 역할을 하여 `OS에 독립적으로 실행`할 수 있다.
  ⇒ OS에 구애받지 않고 Java 코드 재사용이 가능하다.
* 메모리 관리를 위해 `Garbage Collection(GC)`를 수행한다.

자바 애플리케이션은 모두 JVM을 통해 실행되기 때문에 자바 애플리케이션을 실행하려면 반드시 JVM이 필요하며 JVM은 JDK에 포함되어 있다.

또한, JVM은 Java Bytecode를 인터프리터 방식으로 읽어서 기계어로 번역해주기 때문에 다른 언어에 비해 실행 속도가 느리다는 단점이 있다.
⇒ JIT Compiler HotSpot JVM 등의 도입으로 Native 언어와 유사한 수준의 실행속도를 가지게 되어 성능상 별 큰 차이가 없다.

**가상머신이란?** 프로그램을 실행하기 위해 필요한 물리적 머신과 유사한 머신을 소프트웨어로 구현한 것

> Java하면 WORA(Write Once Run Anywhere)를 얘기할 수 있는데 이것 또한 JVM 때문에 가능한 이야기라고 한다.

> CPU와 OS에 따라 컴퓨터가 인식할 수 있는 기계어가 다르다.
> ⇒ JVM이 CPU, OS에 맞는 기계어로 번역해준다.

</br>

### 컴파일 하는 방법

**컴파일 한다는 것의 의미?** `.java` 파일을 `.class` 파일(Java Bytecode)로 만든다는 것을 의미한다.

JDK 내부 bin 폴더 안에 javac라는 java compiler가 포함되어 있으며 아래와 같은 명령으로 java 파일을 컴파일 할 수 있다.

```
javac <filename>.java
```

만약, 작성한 코드에 문제가 있을 경우, 컴파일 에러가 나며 .class 파일이 생성되지 않는다.

> 환경변수 설정을 통해 터미널에서 현재 위치에 상관없이 javac 명령을 사용할 수 있다.
> ⇒ 환경변수란? 운영체제가 실행할 수 있는 실행파일들이 위치한 디렉토리를 지정해두어 이를 운영체제가 참조해서 사용할 수 있도록 한 것

> class 파일에는 해당 Bytecode가 어떤 버전에 호환되도록 작성되었는지 기록되어 있다.

</br>

### 실행하는 방법

컴파일 된 Java 코드를 아래 명령어로 수행할 수 있다. (.class 파일이 존재하지 않으면 실행되지 않는다.)

```
java <package>.<classname>
```

수행하는 클래스의 .class 파일이 존재하지 않을 경우, 아래와 같은 오류가 발생한다.

![image](https://user-images.githubusercontent.com/49746644/105714991-63c0a200-5f60-11eb-9eb1-18c5fd8def35.png)

`-cp` 옵션으로 classpath를 지정해줄 수 있으며 classname 뒤에 문자열 형태의 매개변수를 전달해 줄 수 있다.

**javac vs java**

* javac : 해당 파일 컴파일 수행
* java : 패키지에 존재하는 클래스를 실행

</br>

### 바이트코드란 무엇인가

**Bytecode란?** 가상 머신이 이해할 수 있는 언어(코드)이다.

Java에서는 JVM이 Java 파일을 컴파일한 결과로 나오는 코드로 JVM이 읽어서 기계어로 번역할 수 있는 코드이다.

아래 명령어로 Bytecode를 읽을 수 있다.

```
javap -c <classname> || javap -v <classname>
```

> 바이트코드는 명령어들이 1byte를 차지한다.

</br>

### JIT 컴파일러란 무엇이며 어떻게 동작하는지

**JIT (Just In Time) 컴파일러**는 런타임에 인터프리터 방식으로 동작하는 java의 한계를 보완한 기술로 프로그램을 실제 실행하는 시점에 기계어로 번역하여 실행한다.

즉, 인터프리터 방식으로 실행하다가 적절한 시점에서 바이트코드 전체를  컴파일하여 네이티브 코드로 변경하고, 이후에는 더이상 인터프리팅 하지 않고 네이티브 코드로 직접 실행하는 방식이다.

네이티브 코드를 캐시에 보관하기 때문에 한 번 컴파일된 코드를 다시 번역하지 않고 캐싱된 내용을 실행하여 컴파일을 빠르게 수행한다.
⇒ 즉, 자주 쓸만한 코드들을 기계어로 변환 시켜놓고 저장해서 사용하는 방식이다.

JIT 컴파일 과정은 별도의 스레드에서 실행되고, JVM 스레드는 JIT 컴파일 스레드의 영향을 받지 않기 때문에 애플리케이션의 실행에 영향을 주지 않는다. 컴파일이 진행중일 때에는 인터프리터 방식으로 동작하지만, 컴파일이 완료되면 컴파일 된 버전을 사용하게 되는데 이를 on-stack replacement(OSR) 이라고 한다.

</br>

### JVM 구성 요소

JVM 구성 요소에는 크게 `Class Loader`, `Execution Engine`, `Garbage Collector`, `Runtime Data Area`가 있다.

![JVMimage](https://user-images.githubusercontent.com/49746644/105714955-59060d00-5f60-11eb-9064-7765ad093745.png)

- Class Loader

  컴파일된 `.class 파일(바이트 코드)`들을 엮어 Runtime Data Area 형태로 `메모리에 적재`하는 역할

  ※ 자바는 런타임 시에 `동적`으로 클래스를 로드하기 때문에 클래스를 처음으로 참조할 때, 해당 클래스를 로드하고 링크한다. → 클래스 로더가 수행하며 필요한 부분만 로딩해서 사용한다.

- Execution Engine

  메모리에 적재된 클래스들을 `기계어로 변경`해 명령어 단위로 실행하는 역할

  - 인터프리터 방식

    명령어를 하나하나 실행하는 방식 → 한줄씩 실행해서 느리다.

  - JIT(Just - In - Time) 방식

    인터프리터 방식으로 실행하다가 적절한 시점에서 바이트코드 전체를  컴파일하여 네이티브 코드로 변경하고, 이후에는 더이상 인터프리팅 하지 않고 네이티브 코드로 직접 실행하는 방식
    → 네이티브 코드는 캐시에 보관하기 때문에 한 번 컴파일된 코드는 빠르게 수행함

    → 즉, 자주 쓸만한 코드들을 기계어로 변환 시켜놓고 저장해서 사용하는 방식

- Garbage Collector

  Heap 메모리 영역에 생성된 객체들중에 Reachability를 잃은 객체를 탐색 후 제거하는 역할

- Runtime Data Area

  ![image](https://user-images.githubusercontent.com/49746644/105714970-5d322a80-5f60-11eb-9db1-ac801255cfbf.png)

  프로그램을 수행하기 위해 OS에서 할당받은 메모리 공간 (JVM의 메모리 공간)

  - Method Area (=Class Area = Static area)

    클래스 정보를 처음 메모리 공간에 올릴 때 `초기화되는 대상을 저장하기 위한 메모리 공간` → `클래스 데이터`를 위한 공간

    올라가는 정보의 종류

    - Field Information : 멤버변수의 이름, 데이터 타입, 접근 제어자에 대한 정보
    - Method Information : 메소드의 이름, 리턴타입, 매개변수, 접근제어자에 대한 정보
    - Type Information : class인지 interface인지의 여부 저정 / Type의 속성, 전체 이름, super class의 전체이름(interface 이거나 object인 경우 제외)

    ⇒ 클래스 멤버 변수, 메소드 정보, Type(Class or interface)정보, Constant Pool, static, final 변수 등이 생성

    `Garbage Collection의 대상`이 되는 영역

    ※ 사실상 컴파일 된 바이트 코드의 대부분이 메소드 바이트코드이기 때문에 거의 모든 바이트 코드가 올라간다고 봐도 된다.

    ※ Runtime Constant Pool : 상수 자료형을 저장하여 참조하고 중복을 막는 역할 수행 → 모든 Symbolic Refernce를 포함

  - Heap Area

    동적으로 생성된 오브젝트와 배열이 저장되는 곳 → `객체`를 위한 공간

    new 연산자로 생성된 객체와 배열을 저장

    `Garbage Collection의 대상`이 되는 영역

    객체는 소멸 방법과 소멸 시점이 지역 변수와 다르기 떄문에 별도의 영역에 할당 → 더이상 객체의 존재 이유가 없을 때 소멸시킨다.

  - Stack Area

    지역 변수, 파라미터 등이 생성되는 영역 → 프로그램 실행과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역

    동적으로 객체를 생성하면 실제 객체는 Heap에 할당되고 `해당 레퍼런스`만 Stack에 저장

    Stack은 `스레드별로 독자적`으로 가진다.

  - PC Register

    현재 쓰레드가 실행되는 부분의 주소와 명령을 저장 (CPU의 PC Register와 다르다)

    Thread가 시작될 때 생성되며 생성될 때마다 생성되는 공간으로 스레드마다 하나씩 존재함

  - Native Method Stack

    실행할 수 있는 기계어로 작성된 프로그램을 실행 시키는 영역

    자바외 언어로 작성된 네이티브 코드(기계어)를 위한 메모리 영역

</br>

### JDK와 JRE의 차이

**JDK vs JRE**

* JDK (Java Development Kit) : 자바 개발 도구 + JRE
* JRE (Java Runtime Environment) : 자바 실행 환경

자바 언어로 프로그램을 개발하기 위해서는 JDK를 설치해야 한다.

반대로, 단지 자바 언어로 작성된 프로그램을 실행하기 위해서는 JRE를 설치해야 한다.



---

### ETC

**Java 실행방식**

1. 프로그램이 실행되면 JVM은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당 받음
   → JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리함
2. 자바 컴파일러가(javac)가 자바 소스코드(.java)를 읽어들여 자바 바이트코드(.class)로 변환
3. Class Loader를 통해 class파일들을 JVM으로 로딩
4. 로딩된 class파일들은 Execution engine을 통해 해석됨
5. 해석된 바이트코드는 Runtime Data Areas에 배치되어 실질적인 수행이 이루어짐

⇒ 이러한 실행과정 속에서 JVM은 필요에 따라 Thread Synchronization과 GC같은 관리작업을 수행
