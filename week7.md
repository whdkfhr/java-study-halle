
## week7. 패키지

### 학습할 것
- package 키워드
- import 키워드
- 클래스패스
- CLASSPATH 환경변수
- -classpath 옵션
- 접근지시자

***

### package 키워드
- 하나의 package는 클래스, 인터페이스 또는 다른 레퍼런스 타입들의 모은 것의 이름이다.
- 패키지는 관련된 클래스 그룹과 그 클래스들이 담고 있는 내용을 의미하는 네임스페이스로 지정한다.
- 자바에서는 자바 플랫폼의 핵심 클래스는 이름이 __java__ 로 시작하는 패키지에 있다.
`java.lang'
`java.util`
`java.io`
`java.net`
`java.lang.reflect`
`java.util.regex`
`javax`
`org.w3c`
`org.omg`

public class Employee {
    public static int base = 10000;

    int salary() {
        return base;
    }
}

public class Manager extends Employee {
    String position() {
        return "Manager";
    }
}
```

***

### super 키워드
- 상속관계에서 자식이 부모의 필드나 메소드를 호출할 때 사용한다.
```java
class A {
    int i = 1;
    int f() {
        return i;
    }
}

class B extends A {
    int i;
    int f() {
        i = super.i + 1;        // 부모의 i 값을 가져와서 더한다.
        return super.f() + i;   // 부모의 f()를 호출하여 더한다.
    }
}
```

***

### 메소드 오버라이딩
- 상속관계에서 부모가 제공하는 메소드를 자식이 재정의 할 수 있다.
```java
public class Employee {
    public static int base = 10000;;
    int salary() {
        return base;
    }
}

public class Mangager extends Employee {
    int salary() {              // 부모의 salary() 메소드를 재정의
        return base + 20000;
    }
}

public class Clerk extends Employee {
    int salary() {              // 부모의 salary() 메소드를 재정의
        return base + 10000;
    }
}
```

***

### Dynamic Method Dispatch
- 메소드를 재정의하는 오버라이딩은 자바가 런타임 시 다형성을 지원하는 방법 중 하나이다.
- 다이나믹 메소드 디스패치는 컴파일 타임이 아닌 런타임 시 재정의 된 메소드 호출이 결정되는 메커니즘이다.
- 오버라이드 된 메소드가 부모 클래스를 통해 호출되면 자바는 호출이 발생될 때 참조되는 객체의 타입에 따라 부모 클래스와 자식 클래스 중 어떤 메소드가 실행될 것인지를 런타임 시 결정한다.(동적 바인딩은 런타임 시 수행되지만 private, final 및 static은 정적 바인딩을 사용하고, 정적 바인딩은 컴파일 타임에 수행된다)
- 다이나믹 메소드 디스패치는 자바의 추상화 및 캡슐화의 핵심이다.

#### Double Dispatch
- 런타임 시 인자의 타입에 따라 실행할 메소드를 선택한다.
```java
public interface Visitable<V> {
    void accept(V visitor);
}

public interface OrderVisitor {
    void visit(Order order);
    void visit(SpecialOrder order);
}

public class Order implements Visitable<OrderVisitor> {
    @Override
    public void accept(OrderVisitor visitor) {
        visitor.visit(this);
    }
}

public class SpecialOrder extends Order {
    @Override
    public void accept(OrderVisitor visitor) {
        visitor.visit(this);
    }
}

public class HtmlOrderViewCreator implements OrderVisitor {
    private String html;

    public String getHtml() {
        return html;
    }
    
    @Override
    public void visit(Order order) {
        html = "<p>Regular Order</p>";
    }
    
    @Override
    public void visit(SpecialOrder order) {
        html = "<h1>Special Order</h1>";
    }
}
```

***

### 추상 클래스
- 추상 클래스는 abstract로 선언된 클래스이다.
- 추상 클래스는 추상 메소드를 포함하거나 포함하지 않을 수 있다.
- 추상 클래스를 인스턴스화 할 수는 없지만 추상 클래스를 상속받는 하위 클래스를 만들 수 있다.
```java
abstract class GraphicObject {
    int x, y;
    
    void moveTo(int newX, int newY) {
        ...
    }
    
    abstract void draw();
    abstract void resize();
}

class Circle extends GraphicObject {
    void draw() {
        ...
    }
    
    void resize() {
        ...
    }
}

class Rectangle extends GraphicObject {
    void draw() {
        ...
    }
    
    void resize() {
        ...
    }
}
```
- 자바 8부터는 인터페이스에 static, default 메소드를, 자바 9부터는 인터페이스에 private 메소드를 구현할 수 있게 되어 추상 클래스는 거의 사용하지 않게 되었다.
- 상속은 하나의 클래스만 상속 받을 수 있기 때문에 추상 클래스를 상속 받으면 다른 클래스를 상속 받을 수 없다.
- 인터페이스는 여러개의 인터페이스를 구현 가능하다는 장점이 있다.

***

### final 키워드
- final 키워드를 메소드에 정의하면 해당 메소드는 오버라이딩 할 수 없다.
- 수정되면 안되는 메소드에 사용한다.
```java
class ChessAlgorithm {
    enum ChessPlayer { WHITE, BLACK }
    ...
    final ChessPlayer getFirstPlayer() {
        return ChessPlayer.WHITE;
    }
    ...
}
```
- 클래스에 선언하면 해당 클래스를 상속 받을 수 없다.
- 예를 들어, 자바의 java.lang.String이 있다. 때문에 우리는 String을 상속받아서 재구현 할 수 없다.
```java
public final class String ... {
}
```

*** 

### Object 클래스
- 자바의 모든 클래스는 암시적으로 java.lang.Object 클래스를 상속받고 있다.
- 때문에 우리는 Object에 있는 기능들을 사용할 수 있다.
- `protected Object clone() throws CloneNotSupportedException`
    - 객체를 복사해서 반환해 준다.
- `public boolean equals(Object obj)`
    - 현재 객체와 같은(==) 객체인지 여부를 반환
- `protected void finalize() throws Throwable`
    - GC 발생 시 GC가 호출하여 현재 이 객체를 참조하는 곳이 더이상 없음을 확인한다. 자바 9 버전부터 deprecated 되었다.
- `public final Class getClass()`
    - 현재 객체의 클래스를 반환한다.
- `public int hashCode()`
    - 현재 객체의 해시코드 값을 반환한다.
- `public String toString()`
    - 현재 객체를 문자열로 반환한다.

***
