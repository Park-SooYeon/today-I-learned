# I/O

## 목표

자바의 Input과 Ontput에 대해 학습하세요.

## 학습할 것 (필수)

- 스트림 (Stream) / 버퍼 (Buffer) / 채널 (Channel) 기반의 I/O
- InputStream과 OutputStream
- Byte와 Character 스트림
- 표준 스트림 (System.in, System.out, System.err)
- 파일 읽고 쓰기

---

### 스트림 (Stream) / 버퍼 (Buffer) / 채널 (Channel) 기반의 I/O

**스트림 (Stream)**이란? **데이터가 이동하는 통로, 흐름**을 말한다.

자바에서는 스트림을 통해 입출력을 수행하게 되며, 스트림은 아래와 같은 특징이 있다.

* **FIFO (First In First Out) 구조**를 가진다.
* **단방향**이다.
* Java에서 스트림은 읽기, 쓰기가 동시에 되지 않는다.
* 실제 데이터가 모두 전송되기 전까지는 지연상태를 유지한다.
  처리 속도가 떨어질 수 있다는 단점을 가진다. (이를 보완하기 위해 버퍼와 채널을 사용하는 NIO(New IO) 사용)

**버퍼 (Buffer)**란? 데이터를 전송하는 저, 고속 장치간의 속도차이를 극복하기 위해 사용하는 **임시 저장 공간**을 의미한다.

![image](https://user-images.githubusercontent.com/49746644/108061384-92332800-709b-11eb-9fe2-df49d42bf7e1.png)

I/O에서 버퍼는 보조스트림이라고도 하며 데이터를 묶어서 전송하는 역할을 한다.

> 보조스트림이란?
> 입출력 대상이 되는 파일이나 네트워크를 직접 읽거나 쓸 수 없는 스트림을 의미한다.
> 스트림의 기능을 향상시키거나 새로운 기능을 추가하는데 사용된다.

**채널 (Channel)**이란? 스트림의 향상된 버전으로 양방향 접근이 가능하다.

또한, 스트림과 다르게 채널은 비동기적으로 닫고 중단할 수 있다.

**NIO (New IO)**란? IO의 단점(속도가 느림)을 개선하기 위해 Java 4부터 추가된 패키지이다.

스트림과 달리 **채널 기반의 입출력 방식**을 사용하며 단방향이 아닌 양방향으로 입출력이 가능하다.

또한, 기본적으로 입출력시 **버퍼를 사용**하며 **넌블로킹을 지원**한다.

**NIO 핵심요소**

| 명칭     | 설명                                                         |
| -------- | ------------------------------------------------------------ |
| Buffer   | 버퍼를 나타내며, 입출력 데이터를 임시로 저장할 때 사용       |
| Charset  | 캐릭터셋을 나타내며, 바이트 데이터와 문자 데이터를 인코딩/디코딩 할 때 사용 |
| Channel  | 채널을 나타내며, 데이터가 통과하는 스트림을 의미             |
| Selector | 하나의 스레드에서 다중의 채널로부터 들어오는 입력 데이터를 처리할 수 있도록 해주는 멀티플렉서<br />NIO 논블로킹 입출력의 핵심 요소 |

**IO vs NIO**

| 구분                   | IO                        | NIO                                         |
| ---------------------- | ------------------------- | ------------------------------------------- |
| 입출력 방식            | 스트림 방식               | 채널 방식                                   |
| 버퍼 방식              | 넌버퍼 (non-buffer)       | 버퍼 (buffer)                               |
| 비동기 방식            | 지원 안함                 | 지원                                        |
| 블로킹 / 넌블로킹 방식 | 블로킹 방식만 지원 (동기) | 블로킹 / 넌블로킹 모두 지원 (동기 / 비동기) |

> Ref : https://blog.naver.com/swoh1227/222244309304

<br/>

### InputStream과 OutputStream

자바의 기본적인 데이터 입출력은 Java.io 패키지에서 제공한다. Java.io는 아래와 같은 클래스들을 제공한다.

| Java.io 패키지의 주요 클래스                                 | 설명                                                      |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| File                                                         | 파일 시스템의 파일 정보를 얻기 위한 클래스                |
| Console                                                      | 콘솔로부터 문자를 입출력하기 위한 클래스                  |
| InputStream / OutputStream                                   | **바이트 단위** 입출력을 위한 최상위 입출력 스트림 클래스 |
| FileInputStream / FileOuputStream<br />DataInputStream / DataOutputStream<br />ObjectInputStream / ObjectOutputStream<br />PrintStream<br />BufferedInputStream / BufferedOutputStream | 바이트 단위 입출력을 위한 하위 스트림 클래스              |
| Reader / Writer                                              | **문자 단위** 입출력을 위한 최상위 입출력 스트림 클래스   |
| FileReader / FileWriter<br />InputStreamReader / OutputStreamWriter<br />PrintWriter<br />BufferedReader / BufferedWriter | 문자 단위 입출력을 위한 하위 스트림 클래스                |

스트림에서 데이터를 **입력**할 경우에는 InputStream으로 결정되고, 데이터를 **출력**할 경우에는 OutputStream으로 결정된다.

**InputStream**

`Byte Input`을 수행하는데 필요한 메소드를 정의하는 추상 클래스를 의미한다.

모든 바이트 기반 입력 스트림은 이 클래스를 상속받아서 만들어지며 기본적으로 가져야 할 메소드들은 아래와 같다.

| 메소드                                     | 설명                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| int read()                                 | 입력 스트림으로부터 1바이트를 읽고 읽은 바이트를 리턴<br />(반환값은 0~255의 아스키코드 값, 읽을 수 없을 때는 -1을 반환) |
| int read(byte[] b)                         | 입력 스트림으로부터 읽은 바이트들을 매개값으로 주어진 바이트 배열 b에 저장하고 실제로 읽은 바이트 수를 리턴 |
| int read(byte[] b, int offset, int length) | 입력 스트림으로부터 length개의 바이트만큼 읽고 매개값으로 주어진 바이트 배열 b[offset]부터 length개까지 저장한후, 실제로 읽은 바이트 수를 리턴 |
| void close()                               | 자원을 반납하고 입력 스트림을 닫는다.                        |
| int available()                            | 현재 읽을 수 있는 바이트 수를 리턴                           |

**OutputStream**

`Byte Output`을 수행하는 메소드를 정의한 추상 클래스를 의미한다.

모든 바이트 기반 출력 스트림은 이 클래스를 상속받아서 만들어지며 기본적으로 가져야 할 메소드들은 아래와 같다.

| 메소드                                       | 설명                                    |
| -------------------------------------------- | --------------------------------------- |
| void write(int b)                            | 정수 b의 값을 바이트로 변환하여 출력    |
| void write(byte[] b)                         | b의 내용을 출력                         |
| void write(byte[] b, int offset, int length) | b[offset]부터 length만큼 출력           |
| void flush()                                 | 버퍼에 남아있는 모든 출력 스트림을 출력 |
| void close()                                 | 자원을 반납하고 출력 스트림을 닫는다.   |

> Ref : https://coding-factory.tistory.com/281

<br/>

### Byte와 Character 스트림

**Byte 스트림** : 가장 기본이 되는 스트림으로 입출력의 단위가 1Byte이다.

그림, 멀티미디어, 문자 등 **모든 종류**의 데이터 입출력이 가능하다.

**원시 바이트** 그대로 데이터를 주고받는다.

**Character 스트림** : 입출력의 단위가 2Byte이다.

오로지 **문자 데이터만** 입출력이 가능하다.

**유니코드 문자**로 인코딩하여 데이터를 주고받는다.

<br/>

### 표준 스트림 (System.in, System.out, System.err)

**표준 스트림**이란? 컴퓨터 프로그램에서 표준적으로 입력으로 받고 출력으로 보내는 데이터와 매체를 총치하는 용어로 Java의 경우 System 클래스를 사용한다.

| 표준 스트림 | 스트림 종류           | 설명                   |
| :---------- | --------------------- | ---------------------- |
| System.in   | 표준 입력 스트림      | 키보드로 데이터를 입력 |
| System.out  | 표준 출력 스트림      | 모니터로 데이터를 출력 |
| System.err  | 표준 에러 출력 스트림 | 모니터로 에러를 출력   |

System 클래스는 java.lang.* 패키지에 속하며 입출력 뿐만아니라 **자바 시스템 전반에 걸친 제어와 보안을 담당**한다.

> System.out과 System.err는 거의 동일한 스트림이다.
> 그러나 에러의 경우, 정확하고 빠르게 출력되어야하기 때문에 System.err는 버퍼링을 지원하지 않으며 콘솔창에만 출력된다는 차이점이 있다.

> Ref : https://m.blog.naver.com/PostView.nhn?blogId=force44&logNo=130096406237&proxyReferer=https:%2F%2Fwww.google.com%2F

<br/>

### 파일 읽고 쓰기

```java
public class FileIOTest {
  public static void main(String[] arges) {
    try {
      FileInputStream file = new FileInputStream("./test.txt");
      // 두번째 인자는 덮어쓰기(false), 이어쓰기 (true) 여부
      FileOutputStream fileOut = new FileOutputStream("./output.txt", false);
      int fileSize = file.available();

      byte[] contents = new byte[fileSize];

      int readSize = file.read(contents);

	  // fileSize와 readSize는 동일한 크기가 나옴
      System.out.println(fileSize);
      System.out.println(readSize);

      fileOut.write(contents);

      file.close();
      fileOut.close();
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}
```

#### 참고 자료

https://blog.naver.com/swoh1227/222237603565

https://m.blog.naver.com/PostView.nhn?blogId=javainer&logNo=221479451489&proxyReferer=https:%2F%2Fwww.google.com%2F

