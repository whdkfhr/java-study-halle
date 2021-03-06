## week1. JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가.

### 학습할 것
- JVM이란 무엇인가
- 컴파일 하는 방법
- 실행하는 방법
- 바이트코드란 무엇인가
- JIT 컴파일러란 무엇이며, 어떻게 동작하는지
- JVM 구성 요소
- JDK와 JRE의 차이

***

### JVM이란 무엇인가
- 바이트코드를 OS가 이해할 수 있도록 해석하고 실행하는 가상 머신.
- JVM은 OS에 종속적이지만, 자바는 OS에 상관없이 JVM만 있으면 자바 코드를 변경하지 않고 사용할 수 있다.
- JVM은 바이트코드를 실행하는 것일 뿐 자바에 종속적이지 않다.(코틀린, 스칼라, 그루비 등 바이트코드로 변환만 할 수 있으면 JVM에서 실행할 수 있다.

***

### 컴파일 하는 방법
![pic](https://user-images.githubusercontent.com/26809312/103145751-bc92f400-4782-11eb-9a7f-758aa6af2ed6.png)
<br/>
 출처 : [https://cocomo.tistory.com/499](https://cocomo.tistory.com/499)
 <br/>
- JDK에 들어있는 javac를 사용하여 자바 소스를 바이트코드(클래스파일)로 컴파일 한다.
- 커맨드창을 실행해서 직접 명령을 주거나 IDE를 사용해서 컴파일 할 수 있다.

```bash
$ javac [자바 소스코드 파일]
```

```bash
$ javac Hello.java
```
***

### 실행하는 방법
- 'java'명령어로 클래스명을 주어 실행.<br/>
```bash
$ java [클래스명]
```
```bash
$ java Hello
```
#### 실행 옵션
- 명령어로 'java'만 실행하면  java의 명령 옵션을 확인할 수 있다.
```bash
$ java
```
#### 실행 옵션 종류
```bash
$ java -cp <classpath>      // 참조할 클래스들이 있는 곳의 경로 지정.

$ java -D<property=value>   // 자바 시스템 프로퍼티 설정.

$ java -jar                 // 실행가능한 jar 실행.

$ java -agentlib:<agent>    // 주로 성능 검토 및 모니터링을 위한 에이전트를 사용하기 위해 지정.
$ java -agentpath:<path to agent> 

$ java -Xms<size>           // JVM의 최소 힙 사이즈

$ java -Xmx<size>           // JVM의 최대 힙 사이즈

$ java -verbose             // 디버깅에 유용할 수도 있는 추가적인 정보를 출력.
```
***
### 바이트코드란 무엇인가
- 가상머신이 해석할 수 있는 중간 코드
- 자바에서 JIT컴파일러가 생기기 전까지는 자바 바이트코드를  JVM의 인터프리터가 한 줄씩 기계어로 번역한 후 실행했기 때문에 무겁고 속도가 느리다는 문제점이 있었다
***

### JIT 컴파일러란 무엇이며, 어떻게 동작하는지
- 런타임 시, 바이트 코드를 실행하는 시점에 동작한다.
- JIT 컴파일러가 바이트코드를 기계어로 변환하여 캐싱해 두었다가 같은 함수가 또 나오면 새로운 기계어를 생성하지 않고, 이전에 캐싱해둔 기계어를 사용하도록 한다.
- JIT 컴파일러로 인한 최적화 덕분에 성능이 크게 향상되었다.

***

### JVM 구성 요소
![pic]https://user-images.githubusercontent.com/26809312/103146103-e8fd3f00-4787-11eb-9da3-9c1abc1156d6.png
<br/>
 출처 : [https://sjh836.tistory.com/64](https://sjh836.tistory.com/64)
***

### JDK와 JRE의 차이
- JRE : 자바 애플리케이션을 실행할 수 있는 핵심 라이브러리를 가지고 있다.
- JDK : JRE에 개발 툴이 더 포함되어 있다.
- 자바11부터 JRE를 따로 제공하지 않는다.(JDK로만 제공)
