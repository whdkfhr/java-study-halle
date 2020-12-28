
## week2. 자바 데이터 타입, 변수 그리고 배열

### 학습할 것
- 프리미티브 타입 종류와 값의 범위 그리고 기본 값
- 프리미티브 타입과 레퍼런스 타입
- 리터럴
- 변수 선언 및 초기화하는 방법
- 타 변환, 캐스팅 그리고 타입 프로모션
- 1차 및 2차 배열 선언하기
- 타입 추론, var

***

### 프리미티브 타입 종류와 값의 범위 그리고 기본 값
![pic](https://user-images.githubusercontent.com/26809312/103190330-bc762e00-4913-11eb-978f-fd4044664e60.png)

***

### 프리미티브 타입과 레퍼런스 타입
- 프리미티브 
  - 총 8가지의 기본형 타입을 미리 정의하여 제공.
  - 기본값이 있기 때문에 NULL이 존재하지 않는다. 만약 기본형 타입에 NULL을 넣고 싶다면 Wrapper Class를 활용.
  - 실제 값을 저장하는 공간으로 Stack 메모리에 저장.
  - 만약 컴파일 시점에 담을 수 있는 크기를 벗어나면 컴파일 에러 발생
- 참조형 타입
  - 기본형 타입을 제외한 타입들이 모두 참조형 타입이다.
  - 빈 객체를 의미하는 NULL이 존재.
  - 값이 저장되어 있는 곳의 주소값을 저장하는 공간으로 Heap 메모리에 저장.
  - 문법상으로는 에러가 없지만 실행시켰을 때 에러가 나는 런타입 에러 발생.

***

### 리터럴
- 그 자체로 데이터인 것을 '리터럴(literal)'이라고 한다.
  - 예) 'A', "abc", 123 등.
- 사실 리터럴은 상수(constant)와 의미가 같지만 프로그래밍에서는 상수를 '값을 한번 지정하면 변경할 수 없는 저장공간'으로 정의하기 때문에 이와 구분하기 위해 '리터럴'이라는 용어를 사용한다.
- 접미사는 대소문자를 구분하지 않는다.
- long 타입의 리터럴에는 접미사 'L' 또는 'l'을 반드시 붙여야 한다. 리터럴에 접미삭 붙어있지 않으면 int타입으로 간주.
- float 타입에는 접미사 'f'가 사용되고, double 타입에는 'd'가 사용된다.
- 정수형에서는 int가 기본 자료형이고 실수형에서는 double이 기본 자료형이다.

***

#### 변수 선언 및 초기화하는 방법
- 변수 타입과 함께 값이 가지는 의미를 지닌 변수 이름을 지정해준다.
- 관례로 변수 이름을 지을 때, camel case를 사용한다.
- 자바에서 이미 예약어로 사용중인 이름은 변수 이름으로 사용할 수 없다.
```java
// 변수타입 변수이름;
// 정수형 변수 number를 선언.
int number;
```
```java
// 정수형 변수 number를 선언하고, 변수의 값을 10으로 초기화
int number = 10;
```

***

#### 변수의 스코프와 라이프타임
- 인스턴스 변수 스코프
    - 인스턴스가 생성될 때 생성. 때문에 인스턴스를 먼저 생성해줘야 한다.
- 클래스 변수 스코프
    - 프로그램이 실행될 때 메모리에 딱 한번만 올라가고, 종료될 때까지 메모리에 살아있다.
- 지역 변수 스코프
    - 지역 변수는 메소드나 블록 내에서 선언되어 사용된다.
    - 지역 변수의 스코프는 해장 지역 변수가 선언된 메소드나 블록 내에서만 유지된다.
- 반복문 변수 스코프
    - 반복문에 선언한 변수는 반복문 내에서만 스코프 유지.
```java
public class ScopeTest {
  
  // 인스턴스 변수
  int iv = 1;

  // 클래스 변수
  static int cv = 1;
  
  void method() {
    // 지역 변수
    int localScope = 1;
  }
}
```    
```java
void method() {
  int i = 0;                // 변수 i 선언.
  while(i <= 10) {
    int j = 0;              // 변수 j 선언.
    i++;
  }                         // j 스코프 끝.
  System.out.println(i);
}                           // i 스코프 끝.
```

***

### 타입 변환, 캐스팅 그리고 타입 프로모션
- 타입변환(명시적 형변환, 캐스팅)
    - 변수 또는 리터럴의 타입을 다른 타입으로 변환하는 것
- 형변환 방법
    - 기본형과 참조형 모두 형변환이 가능하지만, 기본형과 참조형 사이에는 형변환이 성립되지 않는다.
```java
int a = (int) 1.23;   // double형 값을 int형으로 변환하여 a에 1이 저장.
byte b = (byte) a;    // a에 저장된 값을 byte형으로 변환하여 byte에 저장.
```
- 타입 프로모션(자동 형변환)
    - 원칙적으로는 모든 형변환에 캐스트 연산자를 이용한 형변환이 이루어져야 하지만, 값의 표현범위가 작은 자료형에서 큰 자료형의 변환은 값의 손실이 없으므로 캐스트 연산자를 생략하는 것을 허용.
    - 그렇다고 해서 형변환이 이루어지지 않는 것은 아니고, 캐스트 연산자를 생략한 경우에도 JVM의 내부에서 자동적으로 형변환이 수행된다.
```java
byte a = 12;
short b = 34;
b = a;  // 가능
a = b;  // 컴파일 에러 발생.
```
    
- 기본형의 자동 형변환이 가능한 방향
    - byte -> short, char -> int > long -> float -> double
    - 왼쪽에서 오른쪽으로의 변환은 캐스트 연산자를 사용하지 않아도 자동 형변환이 되며, 그 반대 방향으로의 변환은 반드시 캐스트 연산자를 이용한 형변환을 해야 한다.
    
***

### 1차 및 2차 배열 선언하기
- 원하는 타입의 변수를 선언하고 변수 또는 타입에 배열임을 의미하는 대괄호[]를 붙이면 된다.
- 1차원 배열
```java
// 타입[] 변수이름;
int[] intArr = new int[10];       // 10개의 int 값을 저장할 수 있는 배열 생성
String[] strArr = new String[10];
```
- 2차원 배열
```java
// 타입[][] 변수이름;
int[][] intArr = new int[10][5];       // 10행 5열의 int 값을 저장할 수 있는 배열 생성
```

***

### 타입 추론, var
- 타입이 정해지지 않은 변수의 타입을 컴파일러가 유추하는 기능이다. 자바에서는 일반 변수에 대해 타입 추론을 지원하지 않기 때문에 자바에서의 타입 추론은 제네릭과 람다에 대한 타입 추론을 말한다.
- 타입 추론 개선 내역
```java
// Java 5 : 제네릭 메서드 타입 추론
List<String> list = Collections.<String>emptyList();

// Java 7 : 다이아몬드 연산자(<>)
Map<String, Integer> map = new HashMap<>();

// Java 8 : 람다식 인자 타입
Predicate<String> predicate = (String x) -> x.length() > 0;
```
- Java10에서 var를 도입하여 암시적 타이핑을 지원하게 됨. var는 keyword(ex.abstract)가 아니라 reserved type name이라서 변수, 함수 이름으로도 사용할 수 있다. 추가로 var의 도입으로 dynamic type을 지원하는 것은 아니다. 컴파일러가 알아서 타입을 추론해서 컴파일해주는 것이다.
```java
// Java 10 이전
Map<User, List<String>> map = new HashMap<>();

// Java 10
var map = new HashMap<User, List<String>>();
```
