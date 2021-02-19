


## week13. I/O Streams

### 학습할 것
- Stream / Buffer / Channel 기반의 I/O
- InputStream과 OutputStream
- Byte와 Character 스트림
- 표준 스트림(System.in, System.out, System.err)
- 파일 읽고 쓰기

***

### Stream / Buffer / Channel 기반의 I/O

#### Stream
- I/O Stream은 input source와 output destination으로 표현 된다.
- Stream은 디스크 파일, 장치, 기타 프로그램 및 메모리 어레이를 포함하여 다양한 종류의 Source 및 Destination이 될 수 있다.
- 단순히 byte 전달, primitive data, localized characters 및 objects를 포함한 다양한 종류의 데이터를 보내고 받을 수 있도록 지원한다.
- 일부 스트림은 단순히 데이터를 전달하며, 다른 스트림은 데이터를 유용한 방식으로 조작하고 변환하기도 한다.
- 내부적으로 어떻게 작동하든, 모든 스트림은 사용하는 프로그램에 동일한 간단한 모델을 제공한다.
- 스트림은 데이터의 시퀀스이다. 프로그램은 Input Stream을 사용하여 소스에서 한 번에 한 항목씩 데이티를 읽고, Output Stream을 사용하여 한 번에 한 항목씩 대상에 데이터를 쓴다.
- Input Stream과 Output Stream을 사용하는 ㄴ곳은 데이터를 저장, 생성 또는 사용하는 모든 대상일 수 있다.
- 디스크 파일을 포함해 다른 프로그램, 주변 장치들, 네트워크 소켓 또는 배열에서 사용할 수 있다.

#### Buffered Streams
- 각 읽기 또는 쓰기 요청은 기본 OS에서 직접 처리된다. 이러한 I/O 요청은 디스크 엑세스, 네트워크 작업 또는 상대적으로 비용이 많이 드는 다른 작업을 트리거하는 경우가 많기 때문에 프로그램의 효율성이 훨씬 떨어질 수 있다. 이러한 오버헤드를 줄이기 위해 Java플랫폼은 버퍼링된 I/O 스트림을 구현한다.
- 버퍼링된 입력은 버퍼로 알려진 메모리 영역에서 데이터를 읽는다. 네이티브 입력 API는 버퍼가 비어 있을 때만 호출된다. 마찬가지로 버퍼링된 출력 스트림은 버퍼에 데이터를 쓰고, 네이티브 출력 API는 버퍼가 가득 찰 때만 호출된다.
- unbuffered stream을 buffered stream 클래스 생성자에 전달해서 변환시킬 수 있다
  ```java
  /*
   * unbuffered stream을 wrapping하는데 사용되는 클래스 4가지
   */
  BufferedInputStream
  BufferedOutputStream
  BufferedReader
  BufferedWriter
  ```
  ```java
  inputStream = new BufferedReader(new FileReader("input.txt"));
  outputStream = new BufferedWriter(new FileWriter("output.txt"));
  ```
- 버퍼가 채워질 때까지 기다리지 않고 critical points 때 버퍼를 출력하는 것이 좋을때가 있는데, 이를 flushing이라고 한다.

#### Channel
- 채널은 하드웨어 장치, 파일, 네트워크 소켓 또는 프로그램 구성요소와 같은 하나 이상의 고유한 I/O 작업을 수행할 수 있는 엔티티에 대한 열린 연결을 나타낸다.
- 채널은 열려 있거나 닫혀 있을 수 있으며, 생성 시 열려 있으며 닫히면 닫힌 상태로 유지된다. 채널이 닫히면 해달 채널에서 I/O작업을 호출하려고 하면 닫힌 채널 예외가 발생한다.
- 일반적으로 채널은 인터페이스를 확장하고 구현하는 인터페이스 및 클래스의 사양에 설명된 대로 다중 스레드 접근을 위해 안전하도록 고안되었다.


| Channels | Description |
| Channel | A nexus for I/O operations |
| ReadablyByteChannel | Can read into a buffer |
| ScatteringByteChannel | can read into a sequence of buffers |
| WritableByteChannel | Can write from a buffer |
| GatheringByteChannel | Can write from a sequence of buffers |
| ByteChannel | Can read/write to/from a buffer |
| SeekableByteChannel | A ByteChannel connected to an entity that contains a variable-length sequence of bytes |
| AsynchronousChannel | Supports asynchronous I/O operations. |
| AsynchronousByteChannel | Can read and write bytes asynchronously |
| NetworkChannel | A channel to a network socket |
| MulticastChannel | Can join Internet Protocol(IP_ multiast groups |
| Channels | Utility methods for channel/stream interoperation |


```java
/*
 * 커스텀 애노테이션을 만들 때에는, @interface를 사용하여 만든다.
 */
 @Target(ElementType.METHOD)
 @Retention(RetentionPolicy.RUNTIME)
 public @interface Nicname {
  String[] value() default {};
 }
```
```java
@Nickname("Test")
public void test() {
}

@Nickname(value = "Test2")
public void test2() {
}
```

***

### @retention
- 내가 만든 애노테이션 정보를 얼마나 유지할 것인가를 정의(RetentionPolicy)
  - @Retention(RetentionPolicy.SOURCE) : 소스까지만 애노테이션 정보를 유지. SOURCE로 설정 시 컴파일하면 애노테이션이 사라진다.
  - @Retention(RetentionPolicy.CLASS) : CLASS는 클래스 파일까지 유지하겠다는 의미. 즉, 컴파일 -> 바이트코드 -> 클래스 파일 안에도 이 애노테이션 정보가 남아있다는 의미. 하지만 런타임 시에는 애노테이션 정보가 사라진다.
  - @Retention(RetentionPolicy.RUNTIME) : 런타임까지 애노테이션 정보를 유지하겠다는 의미.

```java
for(Season season : Season.values()) {
  System.out.println(season);
}
```

***

### @target
- 내가 만든 애노테이션을 자바의 엘리먼트(ElementType(필드, 메소드, 생성자 등)) 중 어디에 붙일 수 있는가를 정의.
```java
@Target({
        ElementType.PACKAGE, // 패키지 선언시
        ElementType.TYPE, // 타입 선언시(Interface, Class, Enum)
        ElementType.CONSTRUCTOR, // 생성자 선언시
        ElementType.FIELD, // 멤버 변수 선언시
        ElementType.METHOD, // 메소드 선언시
        ElementType.ANNOTATION_TYPE, // 어노테이션 타입 선언시
        ElementType.LOCAL_VARIABLE, // 지역 변수 선언시
        ElementType.PARAMETER, // 매개 변수 선언시
        ElementType.TYPE_PARAMETER, // 매개 변수 타입 선언시
        ElementType.TYPE_USE // 타입 사용시(자바 8 이상)
        ElementType.MODULE // 모듈 사용 시(자바 9 이상)
})
```

***

### @documented
- @Documented 애노테이션이 지정된 대상의 JavaDoc에 이 애노테이션의 존재를 표기하도록 지정.
```java
@Documented
public @interface MyAnnotation {}
```
```java
@MyAnnotation
public class MySuperCLass {...}
```

***

### 애노테이션 프로세서
- 일반적으로 애노테이션에 대한 코드베이스를 검사, 수정 또는 생성하는데 사용된다. 본질적으로 애노테이션 프로세서는 java 컴파일러의 플로그인의 일종이다. 애노테이션 프로세서를 적재적소에 잘 사용한다면 코드를 단순화 할 수 있다.
- 컴파일 시 소스코드에 있는 특정 애노테이션을 찾아서 소스코드의 AST(Abstract Syntax Tree)를 조작할 수 있게 해준다.
- 롬복도 애노테이션 프로세서를 사용하여 소스코드를 조작해준다.
- 커스텀 애노테이션 프로세서를 만들기 위해서는 Processor 또는 AbstractProcessor를 상속받아야 한다.

#### Annotation Processor는 어떻게 동작하는가?
애노테이션 처리를 여러 라운드에 걸쳐 수행된다.
1. 자바 컴파일러가 컴파일을 수행(자바 컴파일러는 애노테이션 프로세서에 대해 미리 알고 있어야 한다)
2. 실행되지 않은 애노테이션 프로세서들을 수행(각각의 프로세서는 모두 각자의 역할에 맞는 구현이 되어있어야 한다)
3. 프로세서 내부에서 애노테이션이 달린 Element(변수, 메소드, 클래스 등)들에 대한 처리를 한다(보통 이곳에서 자바 클래스를 생성)
4. 컴파일러가 모든 애노테이션 프로세서가 실행되었는지 확인하고, 그렇지 않다면 반복해서 위 작업을 수행한다.
![pic](https://user-images.githubusercontent.com/26809312/108018483-347eeb80-705b-11eb-89ed-2c716b6583c8.png)

***
