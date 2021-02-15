

## week11. Enum

### 학습할 것
- enum 정의하는 방법
- enum이 제공하는 메소드(values(), valueOf())
- java.lang.Enum
- EnumSet

***

### enum 정의하는 방법
- enum은 변수가 미리 정의된 상수 집합이 되도록 하는 데이터 유형이다.
- enum 변수는 enum에 정의된 값 중 하나이어야 한다.
- 나침반 방향(동/서/남/북) 혹은 요일(월,화,수,목,금,토,일)과 같이 관련이 있는 상수들의 집합으로 정의한다.
- 상수이기 때문에 enum의 필드들은 대문자로 정의한다.
```java
enum Seaon {
  SPRING, SUMMER, FALL, WINTER
}

public class People {
  public String name;
  public String favoriteSeason;
  
  public static void main(String[] args) {
    People people1 = new People();
    people1.name = "arok";
    people1.favoriteSeason = Season.SPRING;
    
    System.out.println("이름 : " + people1.name);
    System.out.println("좋아하는 계절 : " + people1.favoriteSeason);
  }
}
```

***

### enum이 제공하는 메소드(values(), valueOf())
#### values()
- enum에 정의한 필드를 배열로 반환해준다.
```java
for(Season season : Season.values()) {
  System.out.println(season);
}
```
#### valueOf()
- enum에 정의된 필드 중 특정 필드를 가져올 수 있다.
```java
Season fall = Season.valueOf("FALL");
```

***

### java.lang.Enum
- 모든 enum은 암시적으로 java.lang.Enum 클래스를 상속받는다.
- 자바는 다중상속을 지원하지 않으므로 enum은 다른 클래스를 상속 받을 수 없다.
- java.lang.Enum을 상속받기 때문에 java.lang.Enum에 정의되어 있는 메소드들을 사용할 수 있다.
```java
System.out.println(Season.SPRING.name());                     // SPRING
System.out.println(Season.SPRING.compareTo(Season.WINTER));   // -1
System.out.println(Season.SPRING.equals(Season.SPRING));      // true
System.out.println(Season.SPRING.ordinal());                  // 0
System.out.println(Season.SPRING.toString());                 // SPRING
```

***

### EnumSet
![pic](https://user-images.githubusercontent.com/26809312/107907497-367f7680-6f97-11eb-8e5d-d30dc8c00fca.png)
- enum을 컬렉션의 Set을 상속받는 EnumSet으로 만들어서 사용할 수 있다.
```java
/*
 * of : enum값들로 EnumSet을 만든다
 */
 System.out.println("of : " + EnumSet.of(Season.SPRING, Season.FALL));
 
 
/*
 * complementOf : 전달한 enum값들 중 없는 enum값들로 EnumSet을 만든다
 */
 System.out.println("complementOf : " + EnumSet.complementOf(EnumSet.of(Season.SPRING, Season.FALL)));
 
 /*
 * allOf : enum으로 만든 클래스로 EnumSet을 만든다.
 */
 System.out.println("allOf : " + EnumSet.allOf(Season.class));
 
 
 /*
 * range : enum에 정의된 필드의 범위를 전달하여 EnumSet을 만든다.
 */
 System.out.println("range : " + EnumSet.range(Season.SUMMER, Season.WINTER));
 
 
 /*
 * iterator : enum으로 만든 클래스를 Iterator 클래스로 만들어서 사용할 수 있다.
 */
 Iterator<Season> iter = EnumSet.allOf(Season.class).iterator();
 System.out.print("iterator : ");
 while(iter.hasNext()) {
  System.out.print(iter.next() + " ");
 }
```

***
