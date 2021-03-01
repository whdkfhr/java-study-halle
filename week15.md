



## week15. 람다식

### 학습할 것
- 람다식 사용법
- 함수형 인터페이스
- variable Capture
- 메소드, 생성자 레퍼런스

***

### 제네릭이란?
- JDK 1.5 부터 추가된 기능.
- 타입을 generalize(일반화)한다는 것을 의미하며 데이터 타입을 컴파일 시에 미리 지정하는 방법.
- 제네릭을 사용하지 않으면 여러 타입을 사용하는 클래스나 메소드에서는 보통 Obejct 타입을 인자값이나 리턴값으로 사용한다. 이러한 경우 Object 타입의 객체를 알맞는 타입에 맞게 형변환을 해줘야 하는데, 이때 에러가 생길 가능성이 높다.
- 장점
  - 형변환이나 타입체크를 해주지 않아도 된다.
  - 객체의 Type Safety

#### 제네릭을 사용해야하는 이유
- __잘못된 타입이 사용될 수 있는 문제를 컴파일 과정에서 제거__
- 자바 컴파일러는 코드에서 잘못 사용된 타입 때문에 발생하는 문제점을 제거하기 위해 제네릭 코드에 대한 강한 타입 체크를 한다.
- __실행 시 타입 에러가 나는 것보다 컴파일 시에 미리 타입을 강하게 체크해서 에러를 사전에 방지__
- 제네릭 코드를 사용하면 타입을 국한하기 때문에 요소를 찾아올 때 __타입 변환을 할 필요가 없어 프로그램 성능이 향상되는 효과__ 를 얻을 수 있다.

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
