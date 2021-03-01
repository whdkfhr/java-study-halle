



## week15. 람다식

### 학습할 것
- 람다식 사용법
- 함수형 인터페이스
- variable Capture
- 메소드, 생성자 레퍼런스

***

### 람다식이란?
> 식별자 없이 실행가능한 함수
- 메소드를 하나의 식으로 표현하는 것이라고 볼 수 있다.
- 람다식으로 표현하면 return 이 없어지므로 람다식을 anonymous function(익명 함수)라고도 한다.

#### 장점
- 코드를 간결하게 만들 수 있다.
- 가독성이 향상된다.
- 멀티스레드 환경에서 용이하다.
- 함수를 만드는 과정 없이 한번에 처리하기에 생산성이 높아진다.

#### 단점
- 람다로 인한 무명함수는 재사용이 불가능하다.
- 디버깅이 까다롭다.
- 람다를 무분별하게 사용하면 코드가 클린하지 못하다.
- 재귀로 만들 경우 부적합하다.

#### 사용밥
```
(매개변수) -> 표현 바디
(매개변수) -> { 표현 바디 }
() -> { 표현 바디 }
() -> 표현 바디
...
```
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
- 어떤 타입도 받을 수 있도록 하기위해서 와일드 카드를 사용
```
<? super T> : 와일드 카드의 상한 제한. T와 그 자식들만 가능
<? extends T> : 와일드 카드의 하한 제한. T와 그 조상들만 가능
<?> : 제한없이 모든 타이비 가능. <? extends Object> 와 동일.
```
```java
public class Person {}
public class Arok extends Person {}
```
```java
List<? super Person> test1 = new ArrayList<Person>();
List<? super Person> test2 = new ArrayList<Arok>();   // 컴파일 에러

List<? extends Person> test3 = new ArrayList<Person>();
List<? extends Person> test4 = new ArrayList<Arok>(); // <? extends Person>을 통해 다형성 사용가능.
```

***

### 제네릭 메소드 만들기
- 제네릭 메소드란 메서드 선언부에 제네릭 타입이 선언된 메서드를 말한다.
```java
// 대표적인 제네릭 메소드
// Collections 클래스의 sort 메소드.
// 제네릭 클래스에 정의된 타입 변수 T와 메소드에서 사용된 타입 변수 T
public static <T> void sort(List<T> list, Comparator<? super T> c) {
  list.sort(c);
}
```

***

### Erasure
- Erasure란 원소 타입을 컴파일 타임에서만 검사를 하고, 런타임에는 해당 타입 정보를 알 수가 없다. 즉, 컴파일 상태에만 제약 조건을 적용하고, 런타임에는 타입에 대한 정보를 소거하는 프로세스.
```java
public static void main(String[] args) {
  List<String> list = new ArrayList<>();
}
```
```java
public static main([Ljava/lang/String;)V throws java/io/IOException 
   L0
    LINENUMBER 9 L0
    NEW java/util/ArrayList
    DUP
    INVOKESPECIAL java/util/ArrayList.<init> ()V
    ASTORE 1
   L1
    LINENUMBER 10 L1
    RETURN
   L2
    LOCALVARIABLE args [Ljava/lang/String; L0 L2 0
    LOCALVARIABLE list Ljava/util/List; L1 L2 1
    // signature Ljava/util/List<Ljava/lang/String;>;
    // declaration: list extends java.util.List<java.lang.String>
    MAXSTACK = 2
    MAXLOCALS = 2
```

***
