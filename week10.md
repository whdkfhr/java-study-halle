
## week10. 멀티쓰레드 프로그래밍

### 학습할 것
- Thread 클래스와 Runnable 인터페이스
- 쓰레드의 상태
- 쓰레드의 우선순위
- Main 쓰레드
- 동기화
- 데드락

***

## Thread 클래스와 Runnable 인터페이스
- 스레드를 구현하는 방법은 Thread 클래스를 상속받는 방법과 Runnable 인터페이스를 구현하는 방법이 있다.
- Thread 클래스를 상속받으면 다른 클래스를 상속받을 수 없기 때문에, Runnable 인터페이스를 구현하는 방법이 일반적이다.
```java
/*
 * Thread 상속 받는 방법
 */
public class SingleThread extends Thread {
  private int[] tmp;
  
  public SingleThread(String threadName) {
    super(threadName);
    tmp = new int[10];
    for(int i = 0; i < tmp.length; i++) {
      tmp[i] = i;
    }
  }
  
  public void run() {
    for(int i : tmp) {
      try {
        Thread.sleep(1000);
      } catch(InterruptedException ie) {
        ie.printStackTrace();
      }
      System.out.println("# threadName : " + currentThread().getName());
      System.out.println("# tmp : " + i);
    }
  }
  
  public static void main(String[] args) {
    SingleThread st = new SingleThread("first");
    st.start();
  }
}
```
```java
/*
 * Runnable 구현 방법
 */
public class SingleThread implements Runnable {
  private int[] tmp;
  
  public SingleThread() {
    tmp = new int[10];
    for(int i = 0; i < tmp.length; i++) {
      tmp[i] = i;
    }
  }
  
  @Override
  public void run() {
    for(int i : tmp) {
      try {
        Thread.sleep(1000);
      } catch(InterruptedException ie) {
        ie.printStackTrace();
      }
      System.out.println("# threadName : " + Thread.currentThread().getName());
      System.out.println("# tmp : " + i);
    }
  }
  
  public static void main(String[] args) {
    SingleThread st = new SingleThread();
    Thread t= new Thread(st, "second");
    t.start();
  }
}
```

***

### 쓰레드의 상태
![pic](https://user-images.githubusercontent.com/26809312/104680935-6d623280-5734-11eb-9bd2-86e93a1481a1.png)
- java.lang.Throwable 클래스가 Java Exception의 root 클래스이다. hierarchy은 크게 두개로 나뉘는데 Exception과 Error로 나뉜다.
- Exception도 Checked Exception과 Unchecked Exception으로 나뉜다.

#### Types of Java Exceptions
- Unchecked Exception
    - RuntimeException을 상속받은 Exception.
    - 일반적으로 발생을 예측할 수 없고 복구할 수 업는 예외.
    - 주로 programming errors나 외부 API 사용 때문에 일어난다.
- Checked Exception
    - RuntimeException을 제외한 Exception을 상속받은 Exception.
    - 예상 가능한 예외이며, 복구할 수 있다.
- Error
    - Error 클래스의 하위 클래스들이 Error에 속한다. Error는 예상할 수 없고, 복구할 수도 없다.

#### Built-in Exceptions
- ArithmeticException
    - 산술 연산에서 예외 조건이 발생했을 때 발생.
```java
class ArithmetixExceptionTest {
    public static void main(String[] args) {
        try {
            int a = 2, b = 0;
            int c = a / b;      // 0으로 나눌 수 없음.
            System.out.println("c = " + c);
        } catch(ArithmeticException e) {
            System.out.println("ArithmeticException : " + e.getMessage());
        }
    }
}
```
- ArrayIndexOutOfBoundException
    - 잘못된 인덱스로 Array에 액세스했을 때 발생.
    - 인덱스가 음수이거나 배열 크기보다 크거나 같을 때 발생.
```java
public static void main(String[] args) {
    try {
        int[] a = new int[3];
        a[5] = 1;               // a[] 의 size는 3으로 잘못된 인덱스로 array에 엑세스
    } catch(ArrayIndexOutOfBoundException e) {
        System.out.println("ArrayIndexOutOfBoundException : " + e.getMessage());
    }
}
```
- ClassNotFoundException
    - 정의한 클래스를 찾을 수 없을 때 발생.
```java
public static void main(String[] args) {
    Object o = class.forName(args[0]).newInstance();
    System.out.println("Class created for " + o.getClass().getName());
}
```
- FileNotFoundException
    - 파일에 액세스할 수 없거나 열리지 않을 때 발생
```java
public static void main(String[] args) {
    try {
        File file = new File("test.txt");
        FileReader fr = new FileReader(file);
    } catch(FileNotFoundException e) {
        System.out.println("FileNotFoundException : " + e.getMessage());
    }
}
```
- IOException
    - 입출려겨 작업이 실패하거나 중단될 때 발생.
```java
public static void main(String[] args) {
    FileInputStream f = null;
    f = new FileInputStream("test.txt");
    int i;
    while((i = f.read()) != -1) {
        System.out.print((char)i);
    }
    f.close();
}
```
- InterruptedException
    - Thread가 waiting, sleeping 또는 어떤 처리를 하고 있을 때 interrupt가 되면 발생하는 예외.
```java
static class TestInterruptingThread extends Thread {
    public void run() {
        try {
            Thread.sleep(1000);
            System.out.println("task");
        } catch(InterruptedException e) {
            System.out.println("InterruptedException : " + e.getMessage());
        }
        System.out.println("Thread is running...");
    }
}
public static void main(String[] args) {
    TestInterruptingThread t = new TestInterruptingThread();
    t.start();
    t.interrupt();
}
```
- NoSuchMethodException
    - 찾을 수 없는 메소드에 액세스할 때 발생.
```java
public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException{
    Class c = Class.forName("NoSuchMethodExceptionTest");
    c.getDeclareMethod("test");
}
```
- NullPointerException
    - null 객체의 멤버를 참조할 때 
```java
public static void main(String[] args) {
    try {
        String s = null;
        System.out.println(a.charAt(0));
    } catch(NullPointerException e) {
        System.out.println("NullPointerException : " + e.getMessage());
    }
}
```
- NumberFormatException
    - 메소드가 문자열을 숫자 형식으로 변환할 수 없는 경우 발생
```java
public static void main(String[] args) {
    try {
        int num = Integer.parseInt("Test");
        System.out.println(num);
    } catch(NumberFormatException e) {
        System.out.println("NumberFormatException : " + e.getMessage());
    }
}
```
- StringIndexOutOfBoundsException
    - 문자열에 액세스하는 인덱스가 문자열보다 큰 경우거나 음수일 때 발생
```java
public static void main(String[] args) {
    try {
        String s = "string";
        chat c = a.charAt(10);
    } catch(StringIndexOutOfBoundsException e) {
        System.out.println("StringIndexOutOfBoundsException : " + e.getMessage());
    }
}
```

***

### Exception과 Error의 차이
- Exception이란
    - 오라클에서는 Exception을 Event라고 한다. 프로그램의 정상적인 흐름을 방해시키는 이벤트이다.
- Exception 처리 흐름
    - 이런 이벤트가 발생하면 메소드에서 에러 정보가 담긴 Exception Object를 만들고 런타임 시스템에 전달한다.
    - 그러면 런타임 시스템은 이 Exception Handler를 찾으려고 한다. 이 Exception Handler를 찾으려는 집합은 메소드를 호출한 집합인 Method Call Stack이다.
    - 먼저 에러가 발생한 메소드부터 찾고 거기서 찾지 못하면 이 메소드를 호출한 메소드에서 찾는식으로 진행.
    - 이렇게 해서 찾으면 런타임 시스템은 Exception Object를 Exception Handler에 전달.
    - Exception Handler는 Exception Object를 보고 자신이 처리할 수 있는 타입인지 판단하고 처리.
    - 만약 Exception Handler를 찾지 못한다면 런타임 시스템은 종료된다.
- Exception vs Error
    - 둘 다 최상위 예외 클래스인 Throwable 클래스의 하위 클래스이다.
    - Error는 주로 시스템 리소스 부족으로 인해 발생하는 문제를 나타내며, 이러한 유형의 문제는 어플리케이션 외부의 문제로 잡을 수 없다. 주로 확인되지 않은 유형에 속하며 런타임 시점 때 확인되고 컴파일 시점에는 알 수 없다.(ex 시스템 충돌 오류, 메모리 부족 오류 등)
    - Exception은 어플리케이션 내부 문제로 확인할 수 있다. 컴파일 시점과 런타임 시점 모두 발생할 수 있고 Checked, UnChecked으로 나뉜다.

***

### RuntimeException과 RE가 아닌 것의 차이
- Exception에는 크게 2가지 종류가 있다. __컴파일 시점에 발생하는 Exception__, __프로그램 실행시에 발생하는 RuntimeException__. 즉, 예외가 발생하는 시점에 프로그램이 실행 전 후 상태에 따라 이를 구분하면 된다.
- RuntimeException은 UnCheckedException, 아닌 것은 CheckedException으로 분류.
- 가장 큰 차이는 CheckedException의 경우 예외를 예측하고 복구할 수 있기 때문에 예외를 처리하는 Exception Handler가 강제된다. 즉, try-catch-finally block을 만들어야 한다. 하지만 RE는 그렇지 않다.
- 또한, Checked Exception 종류의 사용자 정의 예외 클래스를 만들려면 java.lang.Exception 클래스를 상속받으면 되고 UnCheckedException 종류의 사용자 정의 예외 클래스를 만들려면 java.lang.RuntimeException 클래스를 상속받으면 된다.

***

### 커스텀한 예외 만드는 방법
- Java platform에서는 자신만의 예외 클래스를 만들 수 있다.
- 다음 조건에 부합된다면 만들어서 사용하고, 그렇지 않다면 기존의 자바에서 제공하는 예외를 사용하면 된다.
    - Do you need an exception type that isn't represented by those in the Java platform?
    - If they could differentiate your exception from those thrown by classes written by other vendors?
    - Does your code throw more than one related exception?
    - If you use someone else's exceptions, will users have access to those exceptions?

#### Custom Checked Exception
```java
try (Scanner file = new Scanner(new File(fileName))) {
    if(file.hasNextLine()) return file.nextLine();
} catch(FileNotFoundException e) {
    ...
}
```
- FileNotFoundException 은 정확한 예외의 원인을 알 수 없다. 파일의 이름이 유효하지 않으르 수 있고, 파일이 존재하지 않을 수도 있다 -> java.lang.Exception 클래스를 상속받은 커스텀 클래스를 만들어 해결할 수 있다.
```java
// 에러 메시지를 담을 생성자와, 근본 원인을 담을 생성자를 추가해서 예외를 만들 수 있다.
public class IncorrectFileNameException extends Exception {
    public IncorrectFileNameException(String errorMessage) {
        super(errorMesage);
    }
    
    public IncorrectFileNameException(String errorMessage, Throwable err) {
        super(errorMessage, err);
    }
}
```
```java
try (Scanner file = new Scanner(new File(fileName))) {
    if(file.hasNextLine()) return file.nextLine();
} catch(FileNotFoundException e) {
    // 커스텀 예외를 사용해 정확한 원인을 알 수 있다.
    if(!isCorrectFileName(fileName)) {
        throw new IncorrectFileNameException("Incorrect filename : " + fileName, e);
    }
}
```

#### Custome UnChecked Exception
- 이 경우 오류는 런타임에서 감지되므로 Unchecked Exception이며 사용자 정의 예외가 필요하다.
- java.lang.RuntimeException 클래스를 상속받는다.
```java
public class IncorrectFileExtensionException extends RuntimeException {
    public IncorrectFileExtensionException(String errorMessage, Throwable err) {
        super(errorMessage, err);
    }
}
```
```java
try(Scanner file = new Scanner(new File(fileName))) {
    if(file.hasNextLine()) {
        return file.nextLine();
    } else {
        throw new IllegalArgumentException("Non readable file");
    }
} catch(FileNotFoundException e) {
    if(isCorrectFileName(fileName)) {
        throw new IncorrectFileNameException("Incorrect filename : " + fileName, e);
    }
} catch(IllegalArgumentException e) {
    if(!containsExtension(fileName)) {
        throw new IncorrectFileExtensionException(
          "Filename does not contain extension : " + fileName, e);
    }
}
```

***
