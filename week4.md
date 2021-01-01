
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
    - @BeforeAll, @BeforEach : 주요 테스트 케이스 전에 실행되는 코드
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
<br/>
    - @DisplayName, @Disabled : 표시 이름을 변경하거나, 메소드를 비활성화
    
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
- 아키텍처애
***

#### 관계 연산자(Relational operator, 비교연산자)
- 두 피연산자의 상대적인 크기를 판단하는 연산자.
- 왼쪽의 피연산자와 오른쪽의 피연산자를 비교하여, 어느쪽이 더 큰지, 작은지, 또는 같은지를 판단.
- 결합방향은 왼쪽에서 오른쪽.
```java
/*
* == 같으면 true.
* != 다르면 true.
* > 왼쪽이 크면 true
* >= 왼쪽이 오른쪽보다 크거나 같으면 true.
* < 오른쪽이 크면 true.
* <= 오른쪽이 왼쪽보다 크거나 같으면 true.
*/
class RelationalOperator {
  public static void main(String[] args) {
    int a = 1, b = 2;
    System.out.println(a == b);   // false
    System.out.println(a < b);   // true
  }
}
```

***

#### 논리 연산자(Logical operator)
- 논리식으로 true / false 판단.
```java
/*
* && 논리식이 모두 참이면 true(논리 AND 연산)
* || 논리식 중에 하나라도 참이면 true(논리 OR 연산)
* ! 논리식이 true 이면 false, false 이면 true(논리 NOT 연산)
*/
class LogicalOperator {
  public static void main(String[] args) {
    int a = 1, b = 2;
    boolean c = true, d = false;
    if(a == b){       // false
      ...
    }
    if(a != b) {      // true
      ...
    }
    if(a > b) {       // false
      ...
    }
    if(a < b) {       // true
      ...
    }
    if(c && d) {      // false
      ...
    }
    if(c || d) {      // true
      ...
    }
  }
}
```

***

### instanceof
- 참조 변수가 참조하고 있는 인스턴스의 실제 타입을 반환.
- 해당 객체가 어떤 클래스(부모 클래스 포함)나 인터페이스로부터 생성되었는지 판별.
```java
class Instanceof {
  class Parent {}
  class Child extends Parent implements MyInterface {}
  interface MyInterface {}

  public static void main(String[] args) {
    Parent parent = new Parent();
    Parent child = new Child();
    
    System.out.println("parent instanceof Parent = " + (parent instanceof Parent));   // true
    System.out.println("parent instanceof Child = " + (parent instanceof Child));     // false
    System.out.println("parent instanceof MyInterface = " + (parent instanceof MyInterface));   // false
    System.out.println("child instanceof Parent = " + (child instanceof Parent));   // true
    System.out.println("child instanceof Child = " + (child instanceof Child));     // true
    System.out.println("child instanceof MyInterface = " + (child instanceof MyInterface));   // true
  }
}
```
    
***

### assignment(=) operator
- 오른쪽의 값을 왼쪽 피연산자에 할당
```java
int i = 1;
char c = 'a';
```
- 또는, 객체 참조를 할당하기 위해 사용.
```java
Member member = new Member(1L, "java");
Point point = new Point(0, 10);
```
- 'a = b = c'의 경우, 오른쪽에서 왼족으로 수행. 'a = (b = c)'
- 5개의 산술 연산자, 6개의 비트 및 shift 연산자와 결합해서 사용할 수 있다.
```java
/*
* +=, -=, *=, /=, %= (산술 연산자와 결합해서 사용)
* &=, |=, ^= (비트 연산자와 결합해서 사용)
* <<=, >>=, >>>= (shift 연산자와 결합해서 사용)
*/
a += 2;       // a를 2 증가해서 저장
b -= 5;       // c를 5 빼서 저장
flags |= f;   // 플래그 값들(flags) 중에서 특정 프래그 값 f를 세팅해서(비트 OR 연산)
flags &= ~f;  // 플래그 값들 중에서 특정 플래그 값 f를 세팅하지 않도록(f의 보수로 비트 AND 연산)
```
***

### 화살표(->) 연산자
- 자바 8 이상의 람다식에서 익명함수를 만들 때 사용.
- 코드가 간결해 진다는 장점.
- 화살표 왼쪽에 인자, 오른쪽에 실제 표현식을 쓴다,
```java
// 2 개의 숫자 중 큰 수 출력
@FunctionalInterface
public interface MyFI {
  int getMax(int n1, int n2);
}
public static void main(String[] args) {
  MyFI myFI = (n1, n2) -> (n1 >= n2) ? n1 : n2;
  System.out.println(myFI.getMax(3, 5));        // 5
}
```
- Java10에서 var를 도입하여 암시적 타이핑을 지원하게 됨. var는 keyword(ex.abstract)가 아니라 reserved type name이라서 변수, 함수 이름으로도 사용할 수 있다. 추가로 var의 도입으로 dynamic type을 지원하는 것은 아니다. 컴파일러가 알아서 타입을 추론해서 컴파일해주는 것이다.
```java
// Java 10 이전
Map<User, List<String>> map = new HashMap<>();

// Java 10
var map = new HashMap<User, List<String>>();
```

***

### 3항 연산자(Ternary operator)
- if-else 구문을 간결하게 만든 표현식.
- 3개의 피연산자가 물음표(?) 와 콜론(:)로 나눠져 있는 형태로 표현.
```java
<조건> ? <true이리 경우 반환되는 값> : <false일 경우 반환되는 값>
```
- 3개의 피연산자가 물음표(?) 와 콜론(:)로 나눠져 있는 형태로 표현.
```java
x > y ? x : y
```
- 두번째와 세번째 연산자는 같은 타입으로 변환할 수 있어야 한다.
- 3개의 피연산자가 물음표(?) 와 콜론(:)로 나눠져 있는 형태로 표현.
```java
int weekCount = 3;
System.out.println(weekCount >= 14) ? "스프링 공부" : "자바 공부");
```

***

### 연산자 우선 순위
- 괄호의 우선순위가 제일 높고, 산술 > 비교 > 논리 > 대입의 순서이며, 단항 > 이항 > 삼항의 순서이다.
![pic](https://user-images.githubusercontent.com/26809312/103265352-bc982b80-49f0-11eb-84a2-4b37655b84d5.png)

***

### (optional) Java 13. switch operator
- switch 내에서 라벨이 일치하는 경우, ｀case -> A｀와 같은 형식으로 표현이 가능.
- 단일 수행 또는 블록 수행이 가능.
- switch 블록 내에서 계산된 값을 반환하느 ｀yield｀라는 키워드가 생김.
- 여러 조건을 쉼표로 구분하여 한 라인으로 처리할 수 있음.
```java
enum Day { SUN, MON, TUE, WED, THU, FRI, SAT; }
```
```java
// traditional switch expression
static void test(Day day) {
  switch(day) {
    case MON:
    case FRI:
    case SUN:
      System.out.println(6);
      break;
    case TUE:
      System out.println(7);
      break;
    case THU:
    case SAT:
      System.out.println(8);
      break;
    case WED:
      System.out.println(9);
      break;
  }
}
```
```java
// new switch expression
static void test(Day day) {
  System.out.println(
    swtich(day) {
      case MON, FRI, SUN -> 6;
      case TUE -> 7;
      case THU, SAT -> 8;
      case WED -> 9;
    }
  );
}
```
- switch의 반환 yield
    - `yield`는 쉽게 말해 함수의 return과 비슷하다고 할 수 있다.
    - switch블록에 특정 값을 switch의 결과값으로 반환하는 것이다.
```java
static void test(Day day) {
  int cnt = switch(day) {
    case MON -> 0;
    case TUE -> 1;
    case WED -> {
      int k = day.toString().length();
      int result = k + 5;
      yield result;
      // break result; <--- Java 12 switch expression
    }
    default -> 0;
  };
}
```
```java
// yield 키워드는 항상 switch 블록 내부에서만 사용
static void test(Day day) {
  int cnt = switch(day) {
    case MON -> 0;
    case TUE -> yield 1;    // error, yield는 block 안에서만 유효
    case WED -> {
      yield 2;              // ok
    }
    default -> 0;
  };
}
```
- default
    - switch의 반환값이 따로 필요하지 않은 경우나, case가 switch로 들어오는 모든 인자를 커버하는 경우에는 `default` 항목을 따로 넣어주지 않아도 된다. 하지만 그렇지 않은 경우에는 `default -> { ...; }`를 꼭 작성해야 한다.
