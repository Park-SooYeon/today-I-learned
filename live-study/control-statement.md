# 제어문

## 목표

자바가 제공하는 제어문을 학습하세요.

## 학습할 것 (필수)

* [선택문](#선택문)
* [반복문](#반복문)

---

### 선택문

선택문(=제어문)은 코드의 실행 흐름(순서)를 제어하는 구문이다.

**if문**

가장 기본적인 제어문으로 if문의 조건이 충족될 때(true)에만 실행된다.

```java
if(조건) {
 System.out.println("조건이 true일 때 실행");
}

// 만약, {} 내부 수행해야할 코드가 한줄일 경우, 중괄호 생략 가능
if(조건) System.out.println("조건이 true일 때 실행");
```

**if-else문**

if절의 조건이 false일 경우, 다음 조건을 확인하여 조건이 충족되는 if문을 실행한다.

```java
if(조건1) {
	System.out.println("조건1이 true일 때 실행");
} else if(조건 2) {
	System.out.println("조건2가 true일 때 실행");
} else {
    System.out.println("조건1, 조건2가 모두 false일 때 실행");
}

// if문과 동일하게 {} 내부 수행해야할 코드가 한줄일 경우, 중괄호 생략 가능
if(조건1) System.out.println("조건1이 true일 때 실행");
else if(조건 2) System.out.println("조건2이 true일 때 실행");
else System.out.println("조건1, 조건2가 모두 false일 때 실행");
```

**switch문**

switch문의 매개변수로 들어오는 값에 따라 들어오는 값과 일치하는 case의 코드를 실행한다.

switch문을 사용하기 위해서는 아래 조건을 만족해야한다.

1.  switch 문의 매개변수는 byte, short, int, long, enum 유형, String 클래스 및 Byte, Short, Int 및 Long과 같은 **일부 래퍼 클래스**를 사용할 수 있다. (단, switch 표현식에서만 wrapper 허용하고, case 에는 wrapper 를 허용하지 않는다.)
2. **switch ()** 대괄호 사이에 변수 또는 표현식을 넣을 수 있다 **.** (float 은 **두** **개의 부동 소수점 숫자를 비교하는 것은 x와 y의 십진수 등가가 합리적인 정밀도로 동일하게 보일 때 정확하지 않을 수** **있어 허용하지 않는다.**)
3. 케이스 값은 리터럴 또는 상수여야 한다. 케이스 값 유형은 표현식 유형이어야한다.
4. 각 케이스는 고유해야한다. 중복 케이스 값을 생성하면 컴파일 타임 오류가 발생한다.

if문을 여러개 사용하면 실행 속도가 느려지기 때문에 if문 조건이 많을 경우 사용하면 좋다.

```java
switch(매개변수) {
	case 0:
		System.out.println("매개변수가 0일 때 실행");
        break;
	case 1:
        System.out.println("매개변수가 1일 때 실행");
        break;
	case 2:
        System.out.println("매개변수가 2일 때 실행");
        break;
    case 3: case 4: // 여러 case에 대해 같은 동작을 수행할 때 사용
        System.out.println("매개변수가 3 또는 4일 때 실행");
        break;
	default:
        System.out.println("매개변수가 어떠한 case와도 일치하지 않을 때 실행");
}
```

break문을 사용하지 않으면 일치하는 case 아래 모든 코드를 실행하기 때문에 모든 case에 break 문을 사용해 주는 것이 좋다.

```java
int a = 1;

// break를 사용하지 않았을 경우
switch(a) {
	case 0:
		System.out.println("a가 0일 때 실행");
	case 1:
        System.out.println("a가 1일 때 실행");
	case 2:
        System.out.println("a가 2일 때 실행");
	default:
        System.out.println("a가 어떠한 case와도 일치하지 않을 때 실행");
}
/* 결과
	a가 1일 때 실행
	a가 2일 때 실행
	a가 어떠한 case와도 일치하지 않을 때 실행
*/

// break를 사용했을 경우
switch(a) {
	case 0:
		System.out.println("a가 0일 때 실행");
        break;
	case 1:
        System.out.println("a가 1일 때 실행");
        break;
	case 2:
        System.out.println("a가 2일 때 실행");
        break;
	default:
        System.out.println("매개변수가 어떠한 case와도 일치하지 않을 때 실행");
}
/* 결과
	a가 1일 때 실행
*/
```

> break문이 없으면 일치하는 case 이후 모든 case를 실행하는 것을 **fall-through**라고 한다.

> 분기문의 종류
> break문 : 현재 실행 중인 블록을 즉시 벗어나기 위해 사용, 주로 loop와 switch문에 사용한다.
> continue문 : 현재 반복을 생략하기 위해 사용, 반복문에서 사용
> return문 : 현재 메서드를 탈출하기 위해 사용, return 뒤에 변수 혹은 값을 적어 메소드 반환 값을 설정해줄 수 있다.

> Ref: https://kils-log-of-develop.tistory.com/349

<br/>

### 반복문

조건이 참이 될 때까지 블록을 반복적으로 실행하는데 사용되는 구문이다.

**for문**

반복 횟수를 고정할 수 있으며 아래와 같이 사용할 수 있다.

```java
for(초기화 ; 조건 ; 증가 / 감소) {
	// 반복할 코드
}
// 초기화 : for문을 실행하기 전에 단 한번만 실행, 변수 선언 및 초기화 가능
// 조건 : for문을 한번 실행할 때마다 조건을 확인하여 true일 경우에만 코드를 실행
// 증가 / 감소 : for문을 한번 실행할 때마다 변수의 값을 업데이트하는데 사용
// 반복할 코드 : for문의 조건이 true일 경우, 실행할 코드

// for문의 초기화, 조건, 증가 / 감소는 선택사항이므로 
```

```java
int sum = 0;

// i를 1로 초기화하고 for문이 한번 실행될 때마다 i의 값이 1씩 증가한다.
for(int i = 1 ; i <= 10 ; i++) {
    System.out.println(i); // i는 1 ~ 10까지 출력된다.
	sum += i;
}

System.out.println(sum); // 55
```

**향상된 for문**

반복문을 사용하여 array를 탐색해야할 때, 사용하면 좋다.

오직 배열의 값을 읽기만 할 수 있고, **수정은 할 수 없다.**

```java
int[] array = {1, 2, 3, 4, 5};

// 일반 for문으로 array 탐색하기
for(int i = 0 ; i < array.length ; i++) {
    System.out.println(array[i]);
}

// 향상된 for문으로 array 탐색하기
for(int value : array) {
    System.out.println(value);
}

/* 결과 : 두 for문의 결과는 동일하다.
	1
	2
	3
	4
	5
*/
```

**while문**

반복 횟수가 고정되지 않은 경우에 사용하면 좋은 반복문으로 아래와 같이 사용할 수 있다.

```java
while(조건) {
	// 반복할 코드
}
// 조건 : 조건이 true일 경우에만 코드를 실행, 조건이 false일 경우 while문을 빠져나옴
// 반복할 코드 : while문의 조건이 true일 경우, 실행할 코드
```

```java
int i = 1;
int sum = 0;

while(i <= 10) {
	sum += i;
    i++;
}

System.out.println(sum); // 55
```

**do-while문**

while문과 동일하게 조건에 따른 반복을 실행하지만 while문의 조건을 평가하기 전에 **무조건 while문을 한 번 실행한다**는 차이점이 있다.

```java
do {
	// 반복할 코드
} while(조건);
```

```java
int i = 1;
int sum = 0;

// while문을 무조건 한번 수행하기 때문에 sum에 i를 더하고 i의 값을 1 증가시킨 후, 조건을 확인한다.
do {
	sum += i;
	i++;
} while(i < 0);

System.out.println(sum); // 1
System.out.println(i); // 2
```

#### 참고 자료

https://kils-log-of-develop.tistory.com/349