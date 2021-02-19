


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

Channels | Description |
|:--:|:--:|
|Channel | A nexus for I/O operations|
|ReadableByteChannel | Can read into a buffer |
|ScatteringByteChannel | Can read into a sequence of buffers|
|WritableByteChannel | Can write from a buffer |
|GatheringByteChannel | Can write from a sequence of buffers |
|ByteChannel | Can read/write to/from a buffer|
| SeekableByteChannel	| A ByteChannel connected to an entity that contains a variable-length sequence of bytes|
|AsynchronousChannel| Supports asynchronous I/O operations. |
|AsynchronousByteChannel| Can read and write bytes asynchronously |
|NetworkChannel	|A channel to a network socket |
|MulticastChannel | Can join Internet Protocol (IP) multicast groups|
|Channels| Utility methods for channel/stream interoperation |

- Channel  인터페이스에 지정된 대로, 개발되거나 폐쇄될 수 있으며 비동기식으로 닫히거나 인터럽트할 수 있다. 

***

### InputStream과 OutputStream

#### InputStream
- Java InputStream Class는 모든 InputStream Class의 최상위 추상 클래스이다.
- InputStream의 하위 클래스들은 항상 다음 입력 바이트를 반환하는 메소드를 제공해야 한다.
```
read() : 입력 스트림으로부터 1byte를 읽고, 읽은 바이트를 리턴하는 메소드
read(byte[] b) : 입력 스트림으로부터 읽은 바이트들을 byte[] b에 저장하고 실제로 읽은 바이트 수를 리턴하는 메소드.
read(byte[] b, int off, int len) : 입력 스트림으로부터 len byten만큼 읽어 byte[] b의 b[off]부터 len 개까지 저장한 후 읽은 byte 수인 len개를 리턴한다. 만약 len개보다 적은 byte를 읽는 경우 실제 읽은 byte수를 리턴하는 메소드.
close() : 사용한 시스템 리소스를 반납 후 입력 스트림을 닫는 메소드.
```

#### OutputStream
- OutputStream은 InputStream과 마찬가지로 출력 스트림들 중 최상위 클래스로 추상 클래스이다.
- Byte기반으로 출력되는 스트림은 OutputStream 클래스를 상속받아 생성된다.
```
write(byte[] b) : 출력 스트림으로부터 주어진 byte[] b의 모든 byte를 보내는 메소드.
write(byte[] b, int off, int len) : 출력 스트림으로부터 byte[] b의 b[off]부터 len개까지의 byte를 보내는 메소드.
flush() : 버퍼에 남아있는 모든 byte를 출력하는 메소드.
close() : 사용한 시스템 리소스를 반납 후 출력 스트림을 닫는 메소드.
```

***

### Byte와 Character Stream

#### ByteStream
- 자바의 스트림은 기본적으로 __Byte 단위로 스트림을 전송__ 하며 입출력 대상에 따라 제공하는 클래스가 다르다.
- __그림, 멀티미디어, 문자 등 모든 종류의 데이터를 주고받을 수 있다는 특징__
```java
// 입출력 스트림 클래스
FileInputStream / FileOutputStream : 파일 입출력 대상
ByteArrayInputStream / ByteArrayOutputStream : 메모리 입출력 대상
PipedInputStream / PipedOutputStream : 프로세스 입출력 대상
AudioInputStream / AudioOutputStream : 오디오 장치 입출력 대상
```

#### CharacterStream
- 자바에서 가장 작은 타입인 char형이 2byte이므로, 1byte씩 전송되는 바이트 기반 스트림으로는 원활한 처리가 힘든 경우가 있다. 이러한 경우를 해결하기 위해 자바는 __문자 기반 스트림__ 을 지원한다.
- 문자 기반 스트림은 __오직 문자 데이터를 주고받기 위해 존재하는 스트림__ 으로 문자 데이터를 입출력 할 때 사용하는 스트림이다.
- Reader와  Writer 클래스를 상속받아 사용한다.
```java
// 입출력 스트림 클래스
FileReader / FileWriter : 파일 입출력 대상
CharArrayReader / charArrayWriter : 메모리 입출력 대상
PipedReader / PipedWriter : 프로세스 입출력 대상
StringReader / StringWriter : 문자열 
```

***

### 표준 스트림
- 자바에서는 콘솔과 같은 표준 입출력 장치를 위해 System이라는 표준 입출력 클래스를 정의한다.
- System 클래스는 java.lang 패키지에 포함되어 있다.
- 표준 입출력 스트림은 자바에서 기본적으로 생성하기 때문에 별도로 생성할 필요가 없다.

#### System.in
- 콘솔로부터의 입력을 받는 표준 스트림을 가리키는 상수. 
- 해당객체는 JVM이 메모리에 올라갈 때 생성. 
- InputStream으로 byte단위로 입력받을 수 있다.

#### System.out
- 콘솔로 출력을 하는 표준 스트림을 가리키는 상수.
- ex) System.out.println()

#### System.err
- 표준 에러 출력 장치를 가리키는 상수.

***

### 파일 읽고 쓰기

#### 파일 읽기
```java
public class FileReadTest {
  public static void main(String[] args) {
    // 파일 객체 생성
    File file = new File(" ... ");
    try {
      // 입력 스트림, 입ㄹ력 버퍼 생성
      // FileInputStream, FileReader를 사용해서 파일을 입력 받을 수 있는데,
      // BufferedInputStream or BufferedReader로 감싸
      // 파일의 용량이 큰 경우 효율적으로 파일을 읽을 수 있다.
      FileReader fr = new FileReader(file);
      BufferedReader br = new BufferedReader(fileReader);
      String line = "";
      
      // 한 줄씩 읽기
      while((line = br.readLine()) != null) {
        System.out.println(line);
      }
      br.close();
    } catch(IOException e) {
      e.printStackTrace();
    }
  }
}
```

#### 파일 쓰기
```java
public class FileWriteTest {
  public static void main(String[] args) {
    try {
      // 파일 객체 생성
      File file = new File(" ... ");
      BufferedWriter bw = new BufferedWriter(new FileWriter(file));
      
      if(file.isFile() && file.canWrite()) {
        // 쓰기
        bw.write("hello");
        bw.newLine();
        bw.write("world");
        bw.newLine();
        bw.close();
      }
    } catch(IOException e) {
      e.printStrackTrace();
    }
  }
}
```

***
