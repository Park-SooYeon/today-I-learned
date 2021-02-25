# 제네릭

## 목표

자바의 제네릭에 대해 학습하세요.

## 학습할 것 (필수)

- 제네릭 사용법
- 제네릭 주요 개념 (바운디드 타입, 와일드 카드)
- 제네릭 메소드 만들기
- Erasure

---

### 제네릭 사용법

**제네릭(Genecric)** 이란? 다양한 타입의 객체들을 다루는 메소드나 컬렉션 클래스에 **컴파일 시의 타입체크를 해주는 기능**으로 JDK 1.5에 처음 도입되었다.

제네릭은 객체의 타입을 컴파일 시에 체크하기 때문에 객체의 타입 안정성을 높이고 형변환의 번거로움을 줄여준다.

제네릭을 사용하게 되는 가장 대표적인 예시는 List와 같은 Collection 라이브러리를 사용할 때이다. List를 사용할 때, 아래와 같이 <>에 타입을 작성함으로써 List에 담을 객체의 타입을 지정해준다.

```java
List<Integer> list = new ArrayList<>(); // 제네릭을 사용한 경우
list.add(100);
int temp = list.get(0);
```

List를 사용할 때, 제네릭을 사용하지 않고 아래와 같이 사용할 수 있는데 이럴 경우에는 List에 모든 타입의 객체를 담을 수 있기 때문에 List에 담긴 객체의 타입이 모호해지며 데이터를 가져올 때, 반드시 타입 변환을 해주어야한다.

```java
List list = new ArrayList(); // 제네릭을 사용하지 않은 경우
list.add(100);
list.add("200");
int temp1 = (int) list.get(0); // 타입변환을 해주지 않으면 컴파일 에러가 남
int temp2 = (int) list.get(1); // String을 Integer로 변환해야하는데java.lang.ClassCastException 에러 발생
```

그렇다면 이러한 제네릭은 어떻게 정의하여 사용할 수 있을까?

제네릭을 정의해서 사용하는 방법을 다음과 같다.

```java
public class 클래스명<T> { ... }
public interface 인터페이스명<T> { ... }
```

여기서 T는 `타입 변수(type variable`)이라고 하며 자주 사용하는 타입 변수는 다음과 같다.

| 타입 변수 | 설명    |
| --------- | ------- |
| <T>       | Type    |
| <E>       | Element |
| <K>       | Key     |
| <N>       | Number  |
| <V>       | Value   |
| <R>       | Result  |

제네릭을 사용하여 간단하게 10개의 객체를 담을 수 있는 ArrayList 클래스를 만들어보자.

```java
public class CustomArrayList<T> {
	private int size = 0;
	private Object[] data = new Object[10];
    // new 연산자는 컴파일 시점에 타입이 무엇인지 명확히 알아야하기 때문에 new T[10]과 같은 배열을 생성할 수 없다.
	
	public void add(T value) {
		data[size++] = value;
	}
	
	public T get(int index) {
		return (T) data[index];
	}
}
```

이렇게 만든 제네릭 클래스는 다음과 같이 사용할 수 있다.

```java
CustomArrayList<Integer> list = new CustomArrayList<>();
list.add(10);
list.add(20);
list.add("30"); // 컴파일 오류가 발생한다
int a = list.get(0);
int b = list.get(1);
System.out.println(a); // 10
System.out.println(b); // 20
```

> 제네릭을 사용할 수 없는 경우
>
> 1. 제네릭 타입의 배열을 생성할 경우
>    ⇒ new T[10]은 컴파일 시점에 배열의 타입을 알 수 없기 때문에 사용할 수 없다.
>
> 2. static 변수에 사용할 경우
>
>    ⇒ static 변수는 인스턴스에 종속되지 않으므로 인스턴스별로 타입이 바뀔 수 없기 때문에 사용할 수 없다.
>    	단, **static 메소드에는 제네릭을 사용할 수 있다**.

> Ref : https://coding-factory.tistory.com/573

### 제네릭 주요 개념 (바운디드 타입, 와일드 카드)

**바운디드 타입**

바운디드 타입은 제네릭으로 사용될 **타입 파라미터의 범위를 제한하는 방법**이다.

제네릭 타입에 `extends`를 사용하여 타입 파라미터의 범위를 제한할 수 있다. (인터페이스나 클래스 모두 extends를 사용한다.)

```java
public class CustomArrayList<T extends Number> {
	private int size = 0;
	private Object[] data = new Object[10];
	
	public void add(T value) {
		data[size++] = value;
	}
	
	public T get(int index) {
		return (T) data[index];
	}
}
```

```java
CustomArrayList<Integer> list1 = new CustomArrayList<();
CustomArrayList<String> list2 = new CustomArrayList<>(); // 타입 변수를 Number로 제한했기 때문에 컴파일 에러남
```

> 하나의 타입 변수를 클래스와 인터페이스 두개로 제한하고 싶으면 `&` 기호를 사용할 수 있다.
>
> ```java
> public class CustomClass<T extends OtherClass & CustomInterface> {
> 	...
> }
> ```

**와일드 카드**

제네릭으로 타입을 선언하면 제네릭 메소드는 해당 타입만 매개변수로 받게 된다. 이러한 제한을 해결하기 위해 와일드 카드를 사용하여 부모/자식 클래스도 받을 수 있게 한다.

와일드 카드 타입에는 총 세가지의 형태가 있으며 `물음표(?)` 키워드로 표현된다.

<?> : 제한 없음, 모든 클래스나 인터페이스 타입이 올 수 있다.

<? extends 상위타입> : 와일드 카드의 범위를 특정 객체의 **자식 클래스** 만 올 수 있다.

<? super 하위타입> : 와일드 카드의 범위를 특정 객체의 **부모 클래스** 만 올 수 있다.

와일드 카드를 사용한 예제는 다음과 같다.

```java
// https://coding-factory.tistory.com/573 예제 참조
public class Calcu {
    public void printList(List<?> list) {
       for (Object obj : list) {
    	   System.out.println(obj + " ");  
       }
    }

    public int sum(List<? extends Number> list) {
      int sum = 0;
      for (Number i : list) {
    	  sum += i.doubleValue();  
      }
      return sum;
    }

   public List<? super Integer> addList(List<? super Integer> list) {
      for (int i = 1; i < 5; i++) {
    	 list.add(i); 
      }
      return list;
    }
}
```

### 제네릭 메소드 만들기

**제네릭 메소드** 란? 제네릭 타입이 선언된 메소드이다.

제네릭 메소드는 리턴타입에 상관없이 제네릭 메소드라는 것을 컴파일에게 알려주어야한다. 따라서 리턴타입을 정의하기 전에 **제네릭 타입에 대한 정의를 반드시 명시해야한다**.

또한, 제네릭 메소드는 제네릭 클래스가 아닌 일반 클래스 내부에서도 정의할 수 있다. 즉, **클래스에 지정된 타입 변수와 제네릭 메소드에 정의된 타입 변수는 관계가 없다**는 말로 제네릭 메소드는 자신만의 타입 변수를 가진다는 것이다. (제네릭 메소드의 타입 변수는 메소드 내부에서만 사용된다.)

제네릭 메소드는 다음과 같이 선언하여 사용할 수 있다.

```java
public static <T> 반환타입 메소드명(매개변수)
```

<T>말고도 extends를 사용하여 타입 변수에 제약을 줄 수 있다.

제네릭 메소드를 정의한 예시는 다음과 같다.

```java
public static <T> void printArray(List<? extends Number> list) {
    StringBuilder sb = new StringBuilder();
    for(int i = 0 ; i < list.size() ; i++) {
    	sb.append(list.get(i) + " ");
    }
    System.out.println(sb);
}
```

만약, 아래와 같이 와일드 카드를 사용한 매개변수가 많을 경우 제네릭 메소드를 정의할 때의 타입 변수를 바꾸어 사용함으로써 코드를 간결하게 사용할 수 있다. 그러나 이렇게 사용했을 경우, 매개변수의 타입이 동일해야한다.

```java
public static <T> void printAllArray1(List<? extends Number> list1, List<? extends Number> list2) {
    StringBuilder sb = new StringBuilder();

    for(Number number : list1) {
    	sb.append(number + " ");
    }
    sb.append("\n");
    for(Number number : list2) {
    	sb.append(number + " ");
    }

    System.out.println(sb);
}
```

```java
public static <T extends Number> void printAllArray2(List<T> list1, List<T> list2) {
    StringBuilder sb = new StringBuilder();

    for(Number number : list1) {
        sb.append(number + " ");
    }
    sb.append("\n");
    for(Number number : list2) {
        sb.append(number + " ");
    }

    System.out.println(sb);
}
```

둘 다 매개변수로 Number의 자식 클래스를 저장하는 List를 받는다.

그러나 첫번째의 경우에는 list1과 list2의 타입 변수의 타입이 다르더라도 받아서 처리할 수 있으나 두번째는 lists1과 list2의 타입 변수의 타입이 다르면 컴파일 에러가 난다.

```java
List<Integer> list1 = new ArrayList<>();
List<Integer> list2 = new ArrayList<>();
List<Double> list3 = new ArrayList<>();

list1.add(0); list1.add(1); list1.add(2);
list2.add(3); list2.add(4); list2.add(5);
list3.add(0.5); list3.add(1.5); list3.add(2.5);

printAllArray1(list1, list2);
printAllArray1(list2, list3);

printAllArray2(list1, list2);
printAllArray2(list2, list3); // 컴파일 에러남
// <T extends Number>에 의해 타입 변수의 타입이 하나로 고정되기 때문에 컴파일 에러가 난다.
```

> Ref :  https://wedul.site/283

### Erasure

컴파일러는 제네릭 타입을 이용해 소스파일을 검사하고 런타임에는 해당 타입의 정보를 알 수 없다는 개념이다. 즉, 컴파일된 파일(*.class)에는 제네릭 타입에 대한 정보가 없는 것이다.

이렇게 컴파일 후에 제네릭 타입을 제거하는 이유는 이전 버전 자바와의 **하위 호환성** 을 유지하기 위해서이다.

> [Erasure의 함정](https://medium.com/asuraiv/java-type-erasure%EC%9D%98-%ED%95%A8%EC%A0%95-ba9205e120a3)

#### 참고 자료

https://www.notion.so/4735e9a564e64bceb26a1e5d1c261a3d

https://yaboong.github.io/java/2019/01/19/java-generics-1/

https://hoi-hoon.github.io/study/live-study-14/#%EC%A0%9C%EB%84%A4%EB%A6%AD