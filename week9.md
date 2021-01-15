


## week9. 예외 처리

### 학습할 것
- 자바에서 예외 처리 방법(try, catch, throw, finall)
- 자바가 제공하는 예외 계층 구조
- Exception과 Error의 차이
- RuntimeExceptino과 RE가 아닌 것의 차이
- 커스텀한 예외 만드는 방법

***

### 자바에서 예외 처리 방법(try, catch, throw, finall)
- 예외 처리는 갑작스러운 예외 Exception 발생으로 인해 시스템 및 프로그램이 불능상태에 빠지지 않고 시스템 및 프로그램이 정상실행 되도록 유지시켜 준다.
- try 블록 : __실제 코드가 들어가는 곳으로써__ 예외 Exception이 발생할 가능성이 있는 코드
- catch 블록 
```java
[접근제어자] interface [인터페이스 이름] {   
}
```
- 인터페이스의 바디는 추상 메소드를 포함하고 있지만 인터페이스 내의 모든 메소드들이 추상적이기 때문에 abstract 키워드가 필요하지 않다. 인터페이스의 모든 메소드들은 암묵적으로 public 이다.
```java
public interface Predator {
    void chasePrey();
    void eatPrey();
}
```

***

### 인터페이스 구현하는 방법
- 클래스는 인터페이스를 구현할 수 있다.
```java
class class Lion implements Predator {
    @Override
    public void chasePrey() {
        System.out.println("사자가 먹이를 사냥한다.");
    }
    
    @Override
    public void eatPrey() {
        System.out.println("사자가 먹이를 먹는다.");
    }
}
```
- 클래스들은 여러 인터페이스들을 구현할 수 있다.
```java
public class Frog implements Predator, Prey { ... }
```

***

### 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
- 위의 코드에서 만든 인터페이스의 레퍼런스로 구현체의 개게를 만들어서 사용할 수 있다.
```java
public class Main {
    Predator lion = new Lion();
    Predator frog = new Frog();
    lion.chasePrey();               // "사자가 먹이를 사냥한다" 출력
    frog.chasePrey();               // "개구리가 먹이를 사냥한다" 출력
}
```
- 이와 가이 인터페이스를 사용하면 다형성을 구현할 수 있다.
- 아래와 같은 예제를 보면 Washable 인터페이스를 구현하는 구현체는 반드시 wash()를 구현해야 하며, Main에서 모든 클래스에 대해 같은 wash()라는 기능(씻는 기능)을 수행하지만 어떤 구현체의 객체를 던져주느냐에 따라서 다른 wash()를 수행하게 된다.
```java
public interface Washable {
    void wash();
}
```
```java
public class Cup implements Washable {
    @Override
    public void wash() {
        System.out.println("Washing a Cup.");
    }
}
```
```java
public class Animal {
}
```
```java
public class Dog extends Animal implements Washable {
    @Override
    public void wash() {
        System.out.println("Washing a Dog.");
    }
}
```
```java
public class Fish extends Animal {
}
```
```java
public class Main {
    public static void wash(Object object) {
        if(object instanceof Washable) {
            ((Washalbe)object).wash();
        } else {
            System.out.println("Can't wash this.");
        }
    }
    public static void main(String[] args) {
        wash(new Cup());
        wash(new CoffeeCup());
        wash(new Dog());
        wash(new Fish());
    }
}
```
- 이 외에 구현체 클래스를 만들지 않고, 인터페이스 레퍼런스 생성 시 바로 구현체를 생성해서 사용하는 방법도 있다.
```java
public class Main {
    public static void main(String[] args) {
        Predator lion = new Predator() {
            @Override
            public void chasePrey() {
                System.out.println("사자가 먹이를 사냥한다.");
            }
            
            @OVerride
            public void eatPrey() {
                System.out.println("사자가 먹이를 먹는다.");
            }
        };
        lion.chasePrey();
    }
}
```

***

### 인터페이스 상속
- 인터페이스는 다른 인터페이스를 부모로 하여 상속받을 수 있다.
```java
[접근제어자] interface [인터페이스 이름] extends [부모 인터페이스 이름] {
}
```
- 아래와 같이 여러 인터페이스들을 상속받을 수도 있다.
```java
public interface VenomousPredator extends Predator, Venomous {
    ...
}
```
***

### 인터페이스의 기본 메소드(Default Method), Java8
- 자바 8부터는 인터페이스에 default 메소드를 정의할 수 있다.
```java
public interface TimeClient {
    void setTime(int hour, int minute, int second);
    void setDate(int day, int month, int year);
    void setDateAndTime(int day, int month, int year, int hour, int minute, int second);
    LocalDateTime getLocalDateTime();
    
    default ZonedDateTime getZonedDateTime(String zoneString) {
        return ZonedDateTime.of(getLocalDateTime(), getZonedId(zoneString));
    }
}
```

***

### 인터페이스의 private 메소드, Java9
- 자바 9부터는 인터페이스에 private 메소드를 정의할 수 있다.
```java
public interface CustomCalculator {
    // 짝수의 합
    default int addEvenNumbers(int... nums) {
        return add(n -> n % 2 == 0, nums);
    }
    
    // 홀수의 합
    default int addOddNumbers(int... nums) {
        return add(n -> n% 2 != 0, nums);
    }
    
    // 더하기
    private int add(IntPredicate predicate, int... nums) {
        return IntStream.of(nums)
            .filter(predicate)
            .sum();
    }
}
```
```java
public class Main implements CustomCalculator {
    public static void main(String[] args) {
        int sumOfEvenNumbers = calculator.addEvenNumbers(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        System.out.println("Sum of even numbers = " + sumOfEvenNumbers;
        
        int sumOfOddNumbers = calculator.addOddNumbers(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        System.out.println("Sum of odd numbers = " + sumOfOddNumbers;
    }
}
```

***
