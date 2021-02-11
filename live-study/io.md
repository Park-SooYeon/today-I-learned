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
| ubt read(byte[] b, int offset, int length) | 입력 스트림으로부터 length개의 바이트만큼 읽고 매개값으로 주어진 바이트 배열 b[offset]부터 length개까지 저장한후, 실제로 읽은 바이트 수를 리턴 |
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

**Byte 스트림**

그림, 멀티미디어, 문자 등 **모든 종류**의 데이터 입출력이 가능하다.

**원시 바이트** 그대로 데이터를 주고받는다.

**Character 스트림**

오로지 **문자 데이터만** 입출력이 가능하다.

**유니코드 문자**로 인코딩하여 데이터를 주고받는다.

<br/>

### 표준 스트림 (System.in, System.out, System.err)

<br/>

### 파일 읽고 쓰기

#### 참고 자료

https://blog.naver.com/swoh1227/222237603565

https://m.blog.naver.com/PostView.nhn?blogId=javainer&logNo=221479451489&proxyReferer=https:%2F%2Fwww.google.com%2F