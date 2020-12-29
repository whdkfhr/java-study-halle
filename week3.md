## week3. 연산자

### 학습할 것
- 산술 연산자
- 비트 연산자
- 관계 연산자
- 논리 연산자
- instanceof
- assignment(=) operator
- 화살표(->) 연산자
- 3항 연산자
- 연산자 우선 순위
- (optional) Java 13. switch operator

***

### 산술 연산자(Arithmetic operator)
- 수학적 계산에 사용되는 연산자.
- 두 개의 피연산자를 가지는 이항 연산자.
- 피연산자들의 결합 방향은 왼쪽에서 오른쪽.
```java
/*
* + 더하기
* - 빼기
* * 곱하기
* / 나누기
* % 나머지
*/
class ArithmeticOperator {
  public static void main(String[] args) {
    int result = 1 + 2;
    System.out.println("1 + 2 = " + result);  // 3
    
    result = result - 1;
    System.out.println("result - 1 = " + result);  // 2
    
    result = result * 3;
    System.out.println("result * 3 = " + result);  // 6
    
    result = result / 2;
    System.out.println("result / 2 = " + result);  // 3
    
    result = result % 2;
    System.out.println("result % 2 = " + result);  // 1
  }
}
```

***

### 단항 연산자(Unary Operator)
```java
/*
* + 양수
* - 음수
* ++ 1증가
* -- 1감소
* ! 반대(논리 연산자)
*/
class UnaryOperator {
  public static void main(String[] args) {
    int result = +3;
    System.out.println(result);  // 3
    
    result--;
    System.out.println(result);  // 2
    
    result++
    System.out.println(result);  // 3
    
    result = -result;
    System.out.println(result);  // -3
    
    boolean b = true;
    System.out.println(b);      // true
    System.out.println(!b);     // false
  }
}
```

***

### 비트 연산자(Bitwise operator)
- 비트(bit) 단위로 논리 연산을 할 때 사용하는 연산자.
```java
/*
* & 대응되는 비트가 모두 1이면 1을 반환(AND 연산)
* | 대응되는 비트 중에서 하나라도 1이면 1을 반환(OR 연산)
* ^ 대응되는 비트가 서로 다르면 1을 반환(XOR 연산)
* ~ 비트를 1이면 0, 0이면 1로 반전(NOT 연산, 1의 보수)
* << 명시된 수만큼 비트들을 모두 왼쪽으로 이동(left shift 연산)
* >> 명시된 수만큼 비트들을 모두 오른쪽으로 이동(right shift 연산)
* >>> 명시된 수만큼 비트들을 모두 오른쪽으로 이동, 새로운 비트는 0으로 채움(unsigned right shift 연산)
*/
class BitwiseOperator {
  public static void main(String[] args) {
    byte b = ~12;             // ~00001100
    System.out.println(b);    // -13
    
    b = 10 & 7;               // 00001010 & 00000111 => 00000010 또는 2
    System.out.println(b);    // -13
    
    b = 10 | 7;               // 00001010 | 00000111 => 00001111 또는 15
    System.out.println(b);    // 15
    
    b = 10 ^ 7;               // 00001010 ^ 00000111 => 00001101 또는 13
    System.out.println(b);    // 13
    
    b = 10 << 1;               // 00001010 << 1 => 00010100 또는 20 = 10 * 2
    System.out.println(b);     // 20
    
    b = 10 << 1;              // 00001010 >> 1 => 00000101 또는 5 = 10 /2
    System.out.println(b);    // 20
    
    b = 0xff >>> 4;           // 11111111 >>> 4 = 00001111 또는 15 = 255 / 16
    System.out.println(b);    // 15
  }
}
```

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

