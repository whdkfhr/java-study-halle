

## week12. 애노테이션

### 학습할 것
- 애노테이션 정의하는 방법
- @retention
- @target
- @documented
- 애노테이션 프로세서

***

### 애노테이션 정의하는 방법
- 애노테이션은 자바 프로그램에 주석을 다는 것이다. 애노테이션 자체가 무엇을 실행하는 것은 아니다.
- 예를 들어, @override 의 경우 부모클래스의 메소드를 오버라이딩한 메소드임을 의미한다. 이 애노테이션은 컴파일러와 ide에게 유용한 힌트를 제공한다. 만약, 오버라이딩할 부모클래스의 메소드 이름과 다를 경우 경고 표시를 해준다.
- 모든 애노테이션들은 java.lang.annotation.Annotation을 상속 받는다.
```java
/*
 * 커스텀 애노테이션을 만들 때에는, @interface를 사용하여 만든다.
 */
 @Target(ElementType.METHOD)
 @Retention(RetentionPolicy.RUNTIME)
 public @interface Nicname {
  String[] value() default {};
 }
```
```java
@Nickname("Test")
public void test() {
}

@Nickname(value = "Test2")
public void test2() {
}
```

***

### @retention
- 내가 만든 애노테이션 정보를 얼마나 유지할 것인가를 정의(RetentionPolicy)
  - @Retention(RetentionPolicy.SOURCE) : 소스까지만 애노테이션 정보를 유지. SOURCE로 설정 시 컴파일하면 애노테이션이 사라진다.
  - @Retention(RetentionPolicy.CLASS) : CLASS는 클래스 파일까지 유지하겠다는 의미. 즉, 컴파일 -> 바이트코드 -> 클래스 파일 안에도 이 애노테이션 정보가 남아있다는 의미. 하지만 런타임 시에는 애노테이션 정보가 사라진다.
  - @Retention(RetentionPolicy.RUNTIME) : 런타임까지 애노테이션 정보를 유지하겠다는 의미.

```java
for(Season season : Season.values()) {
  System.out.println(season);
}
```

***

### @target
- 내가 만든 애노테이션을 자바의 엘리먼트(ElementType(필드, 메소드, 생성자 등)) 중 어디에 붙일 수 있는가를 정의.
```java
@Target({
        ElementType.PACKAGE, // 패키지 선언시
        ElementType.TYPE, // 타입 선언시(Interface, Class, Enum)
        ElementType.CONSTRUCTOR, // 생성자 선언시
        ElementType.FIELD, // 멤버 변수 선언시
        ElementType.METHOD, // 메소드 선언시
        ElementType.ANNOTATION_TYPE, // 어노테이션 타입 선언시
        ElementType.LOCAL_VARIABLE, // 지역 변수 선언시
        ElementType.PARAMETER, // 매개 변수 선언시
        ElementType.TYPE_PARAMETER, // 매개 변수 타입 선언시
        ElementType.TYPE_USE // 타입 사용시(자바 8 이상)
        ElementType.MODULE // 모듈 사용 시(자바 9 이상)
})
```

***

### @documented
- @Documented 애노테이션이 지정된 대상의 JavaDoc에 이 애노테이션의 존재를 표기하도록 지정.
```java
@Documented
public @interface MyAnnotation {}
```
```java
@MyAnnotation
public class MySuperCLass {...}
```

***

### 애노테이션 프로세서
- 일반적으로 애노테이션에 대한 코드베이스를 검사, 수정 또는 생성하는데 사용된다. 본질적으로 애노테이션 프로세서는 java 컴파일러의 플로그인의 일종이다. 애노테이션 프로세서를 적재적소에 잘 사용한다면 코드를 단순화 할 수 있다.
- 컴파일 시 소스코드에 있는 특정 애노테이션을 찾아서 소스코드의 AST(Abstract Syntax Tree)를 조작할 수 있게 해준다.
- 롬복도 애노테이션 프로세서를 사용하여 소스코드를 조작해준다.
- 커스텀 애노테이션 프로세서를 만들기 위해서는 Processor 또는 AbstractProcessor를 상속받아야 한다.

#### Annotation Processor는 어떻게 동작하는가?
애노테이션 처리를 여러 라운드에 걸쳐 수행된다.
1. 자바 컴파일러가 컴파일을 수행(자바 컴파일러는 애노테이션 프로세서에 대해 미리 알고 있어야 한다)
2. 실행되지 않은 애노테이션 프로세서들을 수행(각각의 프로세서는 모두 각자의 역할에 맞는 구현이 되어있어야 한다)
3. 프로세서 내부에서 애노테이션이 달린 Element(변수, 메소드, 클래스 등)들에 대한 처리를 한다(보통 이곳에서 자바 클래스를 생성)
4. 컴파일러가 모든 애노테이션 프로세서가 실행되었는지 확인하고, 그렇지 않다면 반복해서 위 작업을 수행한다.
![pic](https://user-images.githubusercontent.com/26809312/108018483-347eeb80-705b-11eb-89ed-2c716b6583c8.png)

***
