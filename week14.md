


## week14. 제네릭

### 학습할 것
- 제네릭 사용법
- 제네릭 주요 개념(바운디드 타입, 와일드 카드)
- 제네릭 메소드 만들기
- Erasure

***

### 제네릭이란?
- JDK 1.5 부터 추가된 기능.
- 타입을 generalize(일반화)한다는 것을 의미하며 데이터 타입을 컴파일 시에 미리 지정하는 방법.
- 제네릭을 사용하지 않으면 여러 타입을 사용하는 클래스나 메소드에서는 보통 Obejct 타입을 인자값이나 리턴값으로 사용한다. 이러한 경우 Object 타입의 객체를 알맞는 타입에 맞게 형변환을 해줘야 하는데, 이때 에러가 생길 가능성이 높다.
- 장점
  - 형변환이나 타입체크를 해주지 않아도 된다.
  - 객체의 Type Safety

***

### 제네릭 사용법
- 꺽쇠 기호'< >'를 사용해 그 안에 사용할 타입을 정의
- JDK1.7 부터 생성자 뒤로 오는 꺽쇠의 타입은 생략 가능.
```java
ArrayList<String> list = new ArrayList<String>();
Map<String, Integer> map = new HashMap<>(); // < > 안에 타입 생략 가능
```

#### 제네릭 명명규칙

| 타입 | 의미 |
| :---: | :--- |
| T | Type|
| E | Element|
| N | Number |
| K | Key |
| V | Value |
| S, U, V ... | 2nd, 3rd, 4th types |

***

### 제네릭 주요 개념(바운디드 타입, 와일드 카드)

#### 바운디드 타입
- 바운디드 타입은 제네릭으로 사용될 타입 파라미터의 값을 제한할 수 있는 방법
```java
// ex) 정수든 실수든 숫자값이라면 모두 사용하고 싶을 때
public class GenericClass<T extends Number> { }
```
```java
GenericClass<Integer> integerGC = new GenericClass<>();
GenericClass<Float> floatGC = new GenericClass<>();
GenericClass<Long> longGC = new GenericClass<>();

GenericClass<String> stringGC = new GenericClass<>();   // Number를 상속받은 객체가 아니므로, 컴파일 에러
```

#### 와일드 카드
- 제네릭을 사용하면 타입이 제한된다.

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
