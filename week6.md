

## week5. 클래스

### 학습할 것
- 클래스 정의하는 방법
- 객체 만드는 방법(new 키워드 이해하기
- 메소드 정의하는 방법
- 생성자 정의하는 방법
- this 키워드 이해하기

### 과제(옵션)
- int 값을 가지고 있는 이진 트리를 나타내는 Node 라는 클래스 정의하기
    - int value, Node left, right를 가지고 있어야 함
- BinaryTree 클래스 정의히ㅏ고 주어진 노드를 기준으로 출력하는 bfs(Node node)와 dfs(Node node) 메소드 구현
- dfs는 왼쪽, 루트, 오른쪽 순으로 순회

***

### 클래스 정의하는 방법
```java
[접근 제한자] class [클래스 이름] {
}
```
- 클래스 이름은 파스칼 케이스(단어 첫글자 대문자)를 사용한다
```java
public class Car {
}
```
- 다른 클래스를 부모로 하고 상속받을 수 있다.(하나의 부모 클래스만 상속받을 수 있다.)
```java
[접근 제한자] class [클래스 이름] extends [부모 클래스] {
}
```
- 인터페이스를 구현할 때에는 implements를 쓴다.(상속은 하난의 부모 클래스만 상속받을 수 있지만, 인터페이스 구현은 여러개의 인터페이스를 구현할 수 있다.)
```java
public class Circle {
  // 클래스 변수
  // 클래스 변수는 클래스가 메모리에 올라갈 때 메모리에 올라간다.
  public static final double PI = 3.141592;
  
  // 인스턴스 변수
  // 인스턴수 변수는 해당 클래스의 인스턴스가 생성될 때 힙 메모리에 올라간다.
  public double radius;
  
  // 메소드
  public double area() {
    // 지역변수
    // 지영ㄱ변수는 메소드가 실행될 때 스택에 올라간다. 메소드 실행이 끝나면 제거된다.
    String name = "원의 넓이";
    return PI * radius * radius;
  }
}
```
***

### 객체 만드는 방법(new 키워드 이해하기)
- new 키워드와 함께 클래스의 생성자를 호출하면 클래스의 객체가 만들어진다.(객체가 힙 메모리에 만들어진다.)
```java
public static void main() {
  Circle circle = new Circle();
  circle.radius = 5;
  circle.area();
}
```

***

### 메소드 정의하는 방법
```java
[접근 제한자] [반환 타입] [메소드 이름] ([인자1, 인자2, ... ]) {
  return [반환 값];
}
```
- 메소드 이름은 카멜 케이스(첫단어의 첫글자는 소문자)를 사용.
```java
public int plus(int a, int b) {
    return a + b;
}
```
- 메소드 이름은 인자의 타입이나 개수가 다른 경우(오버로딩)를 제외하고 한번만 사용할 수 있다.
```java
public int plus(int a, int b) {
    return a + b;
}
public int plus(int a, int b, int c) {
    return a + b + c;
}
```

***

### 생성자 정의하는 방법
- 기본적으로 아무런 생성자를 만들지 않으면 디폴트 생성자가 숨어있다.
- 생성자에는 super()가 숨어있다. super()는 부모의 생성자를 호출한다.
- 모든 클래스는 java.lang.Object를 암묵적으로 상속받고 있는데, 이 super()는 Object의 생성자까지 거슬러 올라간다.
```java
public class Circle {
    // public Circle() {    // 디폴트 생성자
    //      super();        // 부모의 생성자를 호출하는 super()도 숨어있다.
    // }
}
```
- 만약 인자를 받는 생성자를 추가하면 추가한 생성자가 디폴트 생성자를 대신하게 된다.
```java
public class Circle {
    public double radius;

    public Circle(double radius) {
        // super();
        this.radius = radius;
    }
}
```

***

### this 키워드 이해하기
- this는 현재 힙에 올라간 클래스의 객체 중 자기 자신을 의미한다.
- 원래 자기 자신의 인스턴스 변수나 메소드를 호출할 때 명시적으로 this를 사용하지 않으면 this가 숨겨져 있다.
- 그런데 인스턴스 변수 이름이 메소드의 인자 이름과 같을 경우에는 메소드의 인자로 간주해서 this가 붙지 않는다.(어떤 것이 인스턴스 변수이고 어떤 것이 메소드 인자인지 구분할 수 없으므로)
- 이럴 경우 인스턴스 변수에 직접적으로 this를 명시한다.
```java
public class Circle {
    public double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    public static void main(String[] args) {
        Circle circle = new Circle(5);
        System.out.prntln(circle.radius);
    }
}
```

*** 

### 과제. int 값을 가지고 있는 이진 트리를 나타내는 Node 클래스 정의.
```java
public class Node {
    int value;
    Node left;
    Node right;
    
    public Node(int value) {
        this.value = value;
        this.left = null;
        this.right = null;
    }
}
```
```java
public class BinaryTree {
    public Node addNode(Node node, int value) {
        if(node == null) {
            node = new Node(value);
            return node;
        }
        if(value < node.value) {
            node.left = addNode(node.left, value);
        } else if(value > node.value) {
            node.right = addNode(node.right, value);
        }
        return node;
    }
    
    public void bfs(Node node) {
        Queue<Node> queue = new LinkedList<>();
        queue.add(node);
        while(!queue.isEmpty()) {
            Node cur = queue.poll();
            System.out.print(cur.value + " ");
            if(cur.left != null) {
                queue.add(cur.left);
            }
            if(cur.right != null) {
                queue.add(cur.right);
            }
        }
    }
    
    public void dfs(Node node) {
        if(node != null) {
            dfs(node.left);
            System.out.print(node.value + " ");
            dfs(node.right);
        }
    }
}
```

***
