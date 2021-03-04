



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

#### 사용법
```
(매개변수) -> 표현 바디
(매개변수) -> { 표현 바디 }
() -> { 표현 바디 }
() -> 표현 바디
...
```

***

### 함수형 인터페이스(Functional Interfaces)
> 함수형 인터페이스는 java.util.function 에 있으며, 람다 표현식 및 메소드 참조에 대한 대상 유형을 제공하는 개발자의 요구를 충족시킨다. 이러한 각 인터페이스는 일반적이고 추상으로 거의 모든 람다 표현식에 쉽게 적용할 수 있고, 새로운 함수형 인터페이스를 만들기 전에 패키지를 살펴봐야 한다.
```java
@FunctionalInterface
public interface Foo {
  String method (String string);
}
```
- 인터페이스가 1개의 함수만을 갖도록 제한하기 때문에, 여러 개의 함수를 선언하면 컴파일 에러가 발생할 것이다.
- 
#### 자바가 제공하는 함수형 인터페이스
```
Supplier <T>  // 매개변수 없이 반환값 만을 갖는 함수형 인터페이스
```
```java
@FunctionalInterface
public interface Supplier<T> {
  T get();
}

Supplier<String> supplier = () -> "Hello Lambda!";
System.out.println(supplier.get());
```

```
Consumer<T> // 객체를 인자로 받고 리턴값은 없다.
```
```java
public interface Consumer<T> {
  void accept(T t);
  
  default Consumer<T> andThen(Consumer<? superT> after) {
    Objects.requireNonNull(after);
    return (T t) -> {accept(t); after.accept(t);};
  }
}

Consume<String> printString = text -> System.out.println("Hello " + text);
printString.accept("Lambda!");  // Hello Lambda!
Consumer<String> printString2 = text -> System.out.println("Bye Lambda!");
printString.andThen(printString2).accept("Lambda");   // Hello Lambda!
                                                      // Bye Lambda!
```

```
Function <T, R>   // T 타입의 인자를 받고, R 타입의 객체를 리턴
```
```java
public interface Function<T, R> {
  R appliy(T t);
  
  default <V> Function<V, R> compse(Function<? super V, ? extends T> before) {
    Objects.requireNonNull(before);
    return (V v) -> apply(before.apply(v));
  }
  
  default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
    Objects.requireNonNull(before);
    return (T t) -> after.apply(apply(t));
  }
  
  static <T> Function<T, T> identity() {
    return t -> t;
  }
}

Function<Integer, Integer> add = (value) -> value + 10;
Integer result = add.apply(5);
System.out.println(result);   // 15
```

```
Predicate<T>    // T 타입 인자를 받고 결과를 boolean 반환
```
```java
public interface Predicate<T> {
  boolean test(T t);
	
	default Predicate<T> and(Predicate<? super T> other) {
		Objects.requireNonNull(other);
		return (t) -> test(t) && other.test(t);
  }
 
  default Predicate<T> negate() {
    return (t) -> !test(t);
  }
  
  default Predicate<T> or (Predicate<? super T> other) {
    Objects.requireNonNull(other);
    return (t) -> test(t) || other.test(t);
  }
  
  static <T> predicate<T> isEqual(Object targerRef) {
    return (null == targetRef) ? Objects::isNull
      : object -> targetRef.equals(object);
  }
}

Predicate<Integer> isBigger = num -> num > 5;
System.out.print(isBigger.test(10)) // ture
Predicate<String> isEqual = Predicate.isEqual("Google");
isEqual.test("Google"); // true
```
***

### Variable Capture
> Java 람다식은 특정 상황에서 람다 함수 본문 외부에 선언된 변수에 접근이 가능하다. Java8 이전에서는 익명의 내부 클래스가 이를 둘러싼 컴파일러가 만족할 수 있도록 로컬 변수 앞에 final 키워드를 추가했어야만 했다.

#### 지역 변수
- 참조되는 변수는 "effectively final" 이다. 즉, 할당된 후에도 값을 변경하지 않는다. 나중에 myString 변수의 값이 변경되면 컴파일러는 람다 본체 내부에서 myString 변수에 대한 참조에 대해 complain
```java
public interface MyFactory {
  public String create(char[] chars);
}

String myString = "Test";
MyFactory mf = (chars) -> {return myString + " : " + new String(chars);};
```

#### Instance Variables
- 동봉된 EventConsumerImpl 객체의 name 인스턴스 변수를 캡처한다
- 람다는 __자체 인스턴스 변수를 가질 수 없으므로, 항상 둘러싸는 개체를 가리킨다__
```java
public class EventConsumerImpl {
  private String name = "MyConsumer";
  
  public void attach(MyEventProducer eventProducer) {
    eventProducer.listen ( e -> {
      Ststem.out.println(this.name);
    });
  }
}
```

#### Static Variables
```java
public class EventConsumerImpl {
  private static String someStaticVar = "some txt";
  
  public void attach(MyEventProducer eventProducer) {
    eventProducer.listen( e -> }
      System.out.println(someStaticVar);
    });
  }
}
```

***

### 메소드, 생성자 레퍼런스
> Java 람다는 메소드를 생성할 수 있는 간결한 방법을 제공

#### 메소드 레퍼런스
- `static Method References` : 메소드 참조는 static method를 직접적으로 가리킬 수 있다.
```java
public interface Finder {
  public int find(String s1, String s2);
}

public class MyClass {
  public static int doFind(String s1, String s2) {
    return s1.lastIndexOf(2);
  }
}

Finder finder = MyClass::doFind;
```

- `Parameter Method Referece`
```java
Finder finder = String::indexOf;;
Finder finder = (s1,  s2) -> s1.indexOf(s2);
```

- `Instance Method Referece` : 특정 인스턴스의 메소드를 참조할 수 있다
```java
public interface Deserializer {
  public int deserialize(String v1);
}

public class StringConverter {
  public int converterToInt(String v1) {
    return Integer.valueOf(v1);
  }
}

StringConverter sc = new StringConverter();
Deserializer des = sc::convertToInt;
```

#### 생성자 레퍼런스
- `MyClass::new`
```java
public interface Factory {
  public String create(char[] val);
}

Factory factory = String::new;    // (==) Factory = chars -> new String(chars); 
```
***

