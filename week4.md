
## week4. 제어문

### 학습할 것
- 선택문
- 반복문

### 과제(옵션)
- JUnit5
- live-study 대시 보드를 만드는 코드 작성
- LinkedList 구현
- Stack 구현
- 앞서 만든 ListNode를 사용해서 Stack 구현
- Queue 구현
    - 배열 사용
    - ListNode 사용

***

### 선택문
- if-then
```java
/*
* if 조건문이 true일 경우에만 실행문을 실행
*/
if(조건문) {
  실행문
}
```
- if-then-else
```java
/*
* 위의 if 문에서 else를 붙이면 if 조건문이 false일 경우에 실행할 실행문을 추가 가능.
*/
if(조건문) {
  실행문       // true일 때
} else {
  실행문       // false일 때
}
```
- switch
```java
/*
* switch문으로도 조거네 따라 실행할 실행문을 분기시킬 수 있다.
* primitive type, wrapper class(Character, Byte, Integer, Short) 사용 가능
*/
switch(조건식) {
  case 값1 :
    // 조건식의 결과가 값1일 경우
    실행문
    break;
  case 값2 :
    // 조건식의 결과가 값2일 경우
    실행문
    break;
  defalut :
    // 조건식의 결과와 일치하는 case문이 없을 경우
}
```

***

### 반복문
- for
```java
/*
* 변수를 초기화하여 초기값을 주고, 변수의 값이 조건식의 조건을 만족하는 동안 계속 변수를 증감시키면서 실행문을 반복.
* 조건식을 만족하지 않게 되면 반복문을 빠져나간다.
*/
for(초기화; 조건식; 증감식) {
  실행문
}
```

```java
for(int i = 1; i <= 10; i++) {
  System.out.println("count = " + i);
}
```

- while
```java
/*
* 조건식이 만족하는 동안 계속 실행문을 반복해서 실행
*/
while(조건식) { 
  실행문
}
```
count값이 10보다 작거나 같을 동안, 계속해서 값을 증가시킨다.
10보다 크게되면 반복문을 빠져나간다.
```java
int count = 1;
while(count <= 10) {
  count++;
}
```

- do-while
```java
/*
* 실행문을 무조건 최초 1번은 실행하게 한다.(조건식에 맞지 않더라도 무조건 1번은 실행문을 실행)
* 최초 1번은 무조건 실행문을 실행한 이후에 조건식을 만족하는 동안 실행문을 계속 반복적으로 실행.
*/
do {
  실행문
} while(조건식);
```
```java
int i = 3;
do {
  System.out.println("count = " + i);
  i++;
} while(i < 3);
```

***

### 과제0. JUnit5
- JUnit은 Java 에코 시스템에서 가장 인기있는 __단위 테스트 프레임워크__ 중 하나이다. JUnit5 버전에는 __Java8 이상의 새로운 기능을 지원__ 하고 다양한 스타일의 테스트를 가능하게 하는 것을 목표로하는 흥미로운 혁신이 많이 포함되어 있다.
- JUnit 5.x.0 설정
    - pom.xml에 다음 종속성을 추가
```xml
<dependency>
  <groupid>org.junit.jupiter</groupid>
  <artifactid>junit-jupiter-engine</artifactid>
  <version>5.1.0</version>
  <scope>test</scope>
</dependency>
```
- 아키텍처
    - JUnit 플랫폼 : JVM에서 테스트 프레임 워크를 시작하는 역할. JUnit과 빌드 도구와 같은 클라이언트 간의 안정적이고 강력한 인터페이스를 정의. 최족 목표는 클라이언트가 테스트를 발견하고 실행할 때 JUnit과 쉽게 통합되는 방법이다.
    - JUnit 목성 : JUnit5 에서 테스트를 작성하기위한 새로운 프로그래밍 및 확장 모델이 포함되어 있다.
    - JUnit 빈티지 : JUnit5 플랫폼에서 JUnit3 및 JUnit4 기반 테스트 실행을 지원한다.
- 기본 애노테이션
<br/>
`@BeforeAll, @BeforEach : 주요 테스트 케이스 전에 실행되는 코드`
```java
// @BeforeAll 애노테이션이 있는 메소드는 static이어야 한다.
@BeforeAll
static void setup() {
    log.info("@BeforeAll - executes once before all test methods in this class");
}

@BeforeEach
void init() {
    log.info("@BeforeEach - executes before each test method in this class");
}
```
`@DisplayName, @Disabled : 표시 이름을 변경하거나, 메소드를 비활성화`
```java
@DisplayName("Single test Successful")
@Test
void testSingleSuccessTest() {
    log.info("success");
}

@Test
@Disabled("Not implemented yet")
vod testShowSomething() {
}
```
`@AfterEach, AfterAll : 테스트 실행 후 작업에 연결된 메서드`
```java
@AfterEach
void tearDown() {
    log.info("@AfterEach - executed after each test method");
}

// @AfterAll - static 메소드이이어야 한다
@AfterAll
static void done() {
    log.info("AfterAll - executed after all test methods");
}
```
- 주장과 가정
    - JUnit5는 Java8의 새로운 기능, 특히 람다 표현식을 최대한 활요하려 한다.
`assertion : org.junit.jupiter.api.Assertions 로 이동 되었으며 크게 개선됨. 앞서 언급했듯 이제 assertion에서 람다를 사용할 수 있다`
```java
@Test
void lambdaExpressions() {
    assertTrue(Stream.of(1,2,3)
    .stream()
    .mapToInt(i->i)
    .sum()>5, ()->"Sum should be greater than 5);
}
```
MultipleFailuresError로 그룹 내에서 실패한 주장을 보고하는 assertAll()을 사용하여 주장을 그룹화 할 수도 있다. 즉, 오류의 정확한 위치를 찾을 수 있으므로 더 복잡한 주장을 하는 것이 더 안전하다
```java
@Test
void groupAssertions() {
    int[] nums = {0, 1, 2, 3, 4};
    assertAll("numbers",
    ()->assertEquals(nums[0],1),
    ()->assertEquals(nums[3],3),
    ()->assertEquals(nums[4],1)
    );
}
```
`가정`
특히 조건이 충족되는 경우에만 테스트를 실행하는 데 가정이 사용된다. 이는 일반적으로 테스트를 제대로 실행하는데 필요하지만 테스트 대상과 직접 관련이 없는 외부 조건에 사용된다. 가정은 assumeTrue(), assumeFalse() 및 assumingThat()으로 선언할 수 있다. 가정이 실패하면 TestAbortedException이 발생하고 테스트를 건너 뛴다.
```java
@Test
void trueAssumption() {
    assumeTrue(5>1);
    assertEquals(5+2, 7);
}

@Test
void falseAssumption() {
    assumeFalse(5 < 1);
    assertEquals(5+2, 7);
}

@Test
void assumptionThat() {
    String someStr = "just a string";
    assumingThat(
        someStr.equals("just a string"),
        ()->assertEquals(2+2, 4)
    );
}
```
- 예외 테스트
JUnit5에는 두 가지 예외 테스트 방법이 있다. 둘 다 assertThrows() 메소드를 사용하여 구현할 수 있다.
```java
@Test
void shouldThrowException() {
    Throwable exceptin = assertThrows(UnsupportedOperationException.class, ()->{
        throw new UnsupportedOperationException("Not supported");
    });
}

@Test
void assertThrowsException() {
    String str = null;
    assertThrows(IllegalArgumentsException.class, () -> {
        Integer.valueOf(str);
    });
}
```
- Test Suites
JUnit5의 새로운 기능을 계속하기 위해 test suites에서 여러 테스트 클래스를 집계하는 개념을 파악하여 함께 실행할 수 있다. JUnit5는 @SelectPackages 및 @SelectClasses 라는 두 가지 주석을 제공하여 test suites를 만든다. 이 초기 단계에서 대부분의 IDE는 이러한 기능을 지원하지 않는다.
```java
// @SelectPackage는 test suites를 실행할 때 선택할 패키지 이름을 지정하는데 사용된다. 여기서는 모든 테스트를 실행.
@RunWith(JUnitPlatform.class)
@SelectPackages("me.arok")
public class AllUnitTest{}
```
```java
// @SelectClasses는 test suites를 실행할 때 선택할 클래스를 지정하는데 사용된다.
@RunWith(JUnitPlatform.class)
@SelectClasses({AssertionTest.class, AssumptionTest.class, ExceptionTest.class})
public class AllUnitTest{}
```

*** 

#### 과제1. live-study 대시 보드를 만드는 
```java
// 사용자 아이디 및 암호를 통해 연결
GitHub gitHub = new GitHubBuilder().withPassword("my_user", "my_password").build();

// 깃헙 레포 연결
GHRepository ghRepository = gitHub.getRepository("whdkfhr/java-study-halle");

// 참여자 저장소 생성
Map<String, int[]> participantMap = new HashMap<>();

// 참여여부 체크
for(int issueNum = 1; issueNum < 19; issueNum++) {
    GHIssue ghIssue = ghRepository.getIssue(issueNum);
    PagedIterable<GHIssueComment> ghIssueComments = ghIssue.listComments();
    for(GHIssueComment comment : ghIssueComments) {
        String participant = comment.getUser().getName();
        if(participantMap.containsKey(participant)) {
            int[] assignments = participantMap.get(participant);
            assignments[issueNum] = 1;
        } else {
            int[] assignments = new int[19];
            assignments[issueNum] = 1;
            participantMap.put(participant, assignments);
        }
    }
}
for(String key : participantMap.keySet()) {
    int[] weeks = participantMap.get(key);
    System.out.print(Arrays.toString(weeks) + " ");
    System.out.println(String.format("%.2f", ((float) Arrays.stream(weeks).sum()/18) * 100) + "%");
}
```

***

#### 과제2. LinkedList 구현
```java
class LinkedList {

    private Node head;
    private Node tail;
    private int size = 0;
    
    public class Node {
    
        private Object data;
        private Node next;
        
        public Node(Object data) {
            this.data = data;
            this.next = null;
        }
        
        public String toString() {
            return String.valueOf(this.data);
        }
    }
    
    // 추가, 삭제를 위한 node 탐색
    Node node(int index) {
        Node x = head;
        for(int i = 0; i < index; i++)
            x = x.next;
        return x;
    }
    
    // 시작에 데이터 추가
    public void addFirst(Object data) {
        Node newNode = new Node(data);
        newNode.next = head;
        head = newNode;
        size++;
        if(head.next == null)
            tail = head;
    }
    
    // 끝에 추가
    public void addLast(Object data) {
        Node newNode = new Node(data);
        if(size == 0) {
            addFirst(data);
        } else {
            tail.next = newNode;
            newNode = tail;
            size++;
        }
    }
    
    // 중간에 추가
    public void add(int k, object data) {
        if(k == 0) {
            addFirst(data);
        } else {
            Node tmp1 = node(k - 1);
            Node tmp2 = tmp1.next;
            Node newNode = new Node(data);
            tmp1.next = newNode;
            newNode.next = tmp2;
            size++;
            if(newNode.next == null)
                tail = newNode;
        }
    }
    
    // 처음 노드 삭제
    public Object removeFirst() {
        Node tmp = head;
        head = tmp.next;
        
        Object returnData = tmp.data;
        tmp = null;
        size--;
        return returnData;
    }
    
    // 중간 노드 삭제
    public Object remove(int k) {
        if(k == 0)
            return removeFirst();
        
        Node tmp = node(k - 1);
        Node todoDeleted = tmp.next;
        tmp.next = tmp.next.next;
        Object returnData = todoDeleted.data;
        if(todoDeleted.data == null) {
            tail = tmp;
        }
        todoDeleted = null;
        size--;
        return returnData;
    }
    
    // 엘리먼트의 크기
    public int size() {
        return size;
    }
    
    // 엘리먼트 가져오기
    public Object get(int k) {
        Node tmp = node(k);
        return tmp.data;
    }
    
    // 탐색
    public int indexOf(Object data) {
        Node tmp = head;
        int index = 0;
        while(tmp.data != data) {
            tmp = tmp.next;
            index++;
            if(tmp == null) return -1;
        }
        return index;
    }
    
    public static void main(String[] args) {
        LinkedList linkedList = new LinkedList();
        linkedList.addLast(10);
        linkedList.addLast(20);
        linkedList.addLast(30);
        System.out.println(linkedList.indexOf(10));
        System.out.println(linkedList);
    }
}
```

***

### 과제3. Stack 구현
```java
public class Stack {

    Object[] stack;
    int size;
    int top;
    
    public Stack(int size) {
        top = -1;
        stack = new int[size];
        this.size = size;
    }
    
    public Object peek() {
        return stack[top];
    }
    
    public Object pop() {
        Object returnData = stack[top];
        stack[top] = 0;
        top--;
        return returnData;
    }
    
    public void push(Object data) {
        stack[++top] = data;
    }
}
```
    
***

### 과제4. 앞서 만든 ListNode를 사용해서 Stack 구현
```java
public class StackByListNode {
    
    class Node<T> {
        private T data;
        private Node<T> next;
        
        public Node(T data) { 
            this.data = data;
        }
    }
    
    private Node<T> top;
    
    public void push(T data) {
        Node<T> node = new Node<>(data);
        node.next = top;
        top = node;
    }
    
    public T pop() {
        if(top == null)
            throw new EmptyStackException();
        T data = top.data;
        top = top.next;
        return data;
    }
    
    public T peek() {
        if(top == null)
            throw new EmptyStackException();
        return top.data;
    }
    
}
```
***

### 과제5. Queue 구현
- 배열 사용
```java
public class QueueByArray<T> {
    private T[] q;
    private int rear, front, size;
    
    public QueueByArray() {
        q = (T[]) new Object[2];
        front = rear = size = 0;
    }
    
    public int size() {
        return size;
    }
    
    public boolean isEmpty() {
        return size == 0;
    }
    
    public void resize(int length) {
        Object[] q2 = new Object[length];
        for(int i = 1, j = front + 1; i < size + 1; i++, j++) {
            q2[i] = q[j % q.length];
        }
        front = 0;
        rear = size;
        q = (T[]) q2;
    }
    
    public void add(T data) {
        if((rear + 1) % q.length == front) {
            resize(2 * q.length);
        }
        rear = (rear + 1) % q.length;
        q[rear] = data;
        size++;
    }
    
    public T remove() {
        if(isEmpty())
            throw new NoSuchElementException();
        front = (front + 1) % q.legnth;
        T returnData = q[front];
        q[front] = null;
        size--;
        
        if(size > 0 && size == q.length / 4) {
            resize(q.length / 2);
        }
        return returnData;
    }
}
```
- ListNode 사용
```java
public class QueueByListNode<T> {
    private Node<T> top;
    private int size;
    
    public QueueByListNode() {
        top = null;
        size = 0;
    }
    
    public int size() {
        return size;
    }
    
    public boolean isEmpty() {
        return size == 0;
    }
    
    public void push(T data) {
        top = new Node<T>(data);
        size++;
    }
    
    public T peek() {
        if(isEmpty())
            throw new NoSuchElementException();
        return top.data;
    }
    
    public T pop() {
        if(isEmpty())
            throw new NoSuchElementException();
        T returnData = top.data;
        top = top.next;
        size--;
        return returnData;
    }
}
```

***
