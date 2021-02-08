
## week10. 멀티쓰레드 프로그래밍

### 학습할 것
- Thread 클래스와 Runnable 인터페이스
- 쓰레드의 상태
- 쓰레드의 우선순위
- Main 쓰레드
- 동기화
- 데드락

***

### Thread 클래스와 Runnable 인터페이스
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
![pic](https://user-images.githubusercontent.com/26809312/107176415-af6c5480-6a12-11eb-9302-7e5b8f9d4098.png)
- Runnalbe (준비 상태)
- Running (실행 상태)
- Dead (종료 상태)
- Blocked (지연 상태)
  - CPU 점유권을 상실한 상태
  - 후에 특정 메소드를 실행시켜서 Runnable로 전환
  - wait() 메소드에 의해 Blocked 상태가 된 스레드는 notify() 메소드가 호출되면 Runnable 상태로 된다.
  - sleep() 메소드에 의해 Blocked 상태가 된 스레드는 지정된 시간이 지나면 Runnable 상태가 된다

***

### 쓰레드의 우선순위
- 스레드는 우선순위라는 멤버변수를 가지고 있는데, 이 우선순위의 값에 따라 스레드를 실행할 수 있다. 우선순위 범위는 1 ~ 10 이며, 높을수록 우선순위가 높다.
```java
public static final int MIN_PRIORITY = 1;
public static final int NORM_PRIORITY = 1;
public static final int MAX_PRIORITY = 1;
```
```java
class ThreadDemo extends Thread {
  public void run() {
    System.out.println("Inside run method");
  }
  
  public static void main(String[] args) {
    ThreadDemo t1 = new ThreadDemo();
    ThreadDemo t2 = new ThreadDemo();
    ThreadDemo t3 = new ThreadDemo();
    
    // default 5
    System.out.println("t1 thread priority : " + t1.getPriority());
    System.out.println("t2 thread priority : " + t2.getPriority());
    System.out.println("t3 thread priority : " + t3.getPriority());
    
    t1.setPriority(2);
    t2.setPriority(5);
    t3.setPriority(8);
    
    System.out.println("t1 thread priority : " + t1.getPriority());
    System.out.println("t1 thread priority : " + t2.getPriority());
    System.out.println("t1 thread priority : " + t3.getPriority());
  }
}
```

***

### Main 스레드
![pic](https://user-images.githubusercontent.com/26809312/107176890-d5462900-6a13-11eb-86f9-95315a36e8ab.png)
- 자바 프로그램이 시작될 때 하나의 스레드가 즉시 실행된다. 이 스레드를 프로그램의 메인 스레드라고 한다.
- 다중 스레드를 만들 때 메인 스레드가 자식 스레드를 만든다.
- 모든 스레드의 실행이 완료하는 마지막 스레드도 메인 스레드이다.
    - 산술 연산에서 예외 조건이 발생했을 때 발생.
```java
public static void main(String[] args) {
  Thread t = Thread.currentThread(); // 현재 실행중인 메인 스레드.
  ...
}
```

***

### 동기화
- 한 스레드가 진행 중인 작업을 다른 스레드가 간섭하지 못하도록 막는 것.
1. synchronized를 이용한 동기화
```java
1. 메서드 전체를 임계영역으로 지정.
  public synchronized void something() {
    ...
  }

2. 특정한 영역을 임계영역으로 지정.
  synchronized(객체의 참조변수) {
    ...
  }
```
- 스레드는 synchronized 메서드가 호출된 시점부터 해당 메서드가 포함된 객체의 lock을 얻어 작업을 수행하다가 메서드가 종료되면 lock을 반환한다.
- lock의 획득과 반납이 모두 자동으로 이루어지므로, 개발자는 그저 임계영역만 설정해주면 된다.
- 모든 객체는 lock을 하나씩 가지고 있고, 해당 객체의 lock을 가지고 있는 스레드만 임계영역의 코드를 수행할 수 있다.
- 때문에 다른 스레드들은 lock을 얻을 때까지 기다리게 되므로, 가능하면 메서드 전체에 lock을 거는 것보다 synchronized 블럭으로 임계영역을 최소화하는 것이 좋다.

2. lock과 Condition을 이용한 동기화
- 동기화할 수 있는 방법은 synchronized 블럭 외에도 java.util.concurrent.locks 패키지가 제공하는 lock 클래스들을 이용하는 방법이 있다. 같은 메서드 내에서만 lock을 걸 수 있는 synchronized 블럭의 제약이 불편할 때 lock 클래스를 사용한다.
  - synchronized의 경우 기본적으로 스레드간의 락을 획득하는 순서를 보장해주지 않는다. 이러한 것을 불공정 방법이라고 하는데 ReentrantLock은 불공정 방법 뿐만 아니라 메소드를 이용해 순서를 보장해 주도록(공정 방법)으로 설정할 수 있다.
  - synchronized와 같은 단일 블록의 형태를 넘어 여러가지의 컬렉션이 얽혀 있을 때 명시적으로 락을 실행시킬 수 있다.
  - 대기 상태의 락에 대한 인터럽트를 걸어야 할 경우 사용할 수 있다.
  - 락을 획득하려고 대시중인 스레드들의 상태를 받아야 할 경우 사용할 수 있다.

- 정리
  - 락을 모니터링 해야할 때
  - 락을 획득하려는 스레드의 개수가 많을 때 (4개 이상)
  - 위에서 이야기한 복잡한 동기화 코드를 작성해야 할 때

***

### 데드락
- 하나의 자원을 가지고 두 개 이상의 스레드가 서로 자원을 획득하기 위해 무한정 대기하고 있는 상태를 말한다.
- 해결 방법?
    - 교착상태를 방지하거나 회피하는 방법은 있지만 해결 방법은 중지하는 ㄴ것.

***
