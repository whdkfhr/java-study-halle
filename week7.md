
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
- `java.lang`
        - 기본적인 언어 클래스 패키지
- `java.util`
        - 다양항 유틸 클래스
- `java.io`
        - 입출력 클래스
- `java.net`
        - 네트워킹 클래스
- `java.lang.reflect`
- `java.util.regex`
- `javax`
        - 오라클에 의해 표준화된 자바 플랫폼 확장 버전
- `org.w3c`
- `org.omg`

***

### import 키워드
- 자바 코드에서 클래스 또는 인터페이스를 참조할 때 사용한다.
- import를 사용할 때에는 기본적으로 패키지 이름을 포함한 완전한 유형의 이름을 사용해야 한다.
- 만약 파일을 조작하는 코드를 작성 중이고 java.io 패키지의 File클래스를 사용해야 할 경우, __java.io.File__ 을 입력해야 한다.
                - 예외적으로 패키지 java.lang은 매우 중요하고 일반적으로 사용되므로 항상 패키지명 없이 클래스명으로만 사용할 수 있다.
                - 같은 패키지 내의 클래스끼리는 패키지명 없이 클래스명으로 참조할 수 있다.
                - import로 한번 선언해 두면 해당 클래스 내에서는 import된 클래스를 참조할 때 더이상 패키지명을 쓰지 않아도 된다.
```java
import java.io.File;
import java.il.*;               // File분 아니라 java.io 패키지내의 모든 클래스를 참조하도록 하고 싶은 경우
```
```java
// 클래스명이 중복될 경우에는 충돌이 일어나서 컴파일 에러 발생
import java.util.List;
import java.awt.List;
```
```java
// 정적 멤버도 참조할 수 있다.
import static java.lang.System.out;
public class Main {
        public static void main(String[]args ){
                out.println();
        }
}
```

***

### 클래스패스
- Classpath는 사용자 정의 클래스 및 API 패키지의 경로를 지정하는 JVM 또는 Java 컴파일러의 매개 변수이다.
- 매개 변수는 명령 행 또는 환경 변수를 통해 설정할 수 있다.

***

### CLASSPATH 환경변수
- JAVA_HOME : Java가 설치된 경로를 지정
- CLASSPATH : Java 클래스가 있는 경로들을 지정
- PATH : Java 실행파일이 있는 경로를 추가
- MacOS
- `cd /Library/Java/JavaVirtualMachines/jdk1.8.0_192.jdk/Contents/Home`
- `vi ~/.bash_profile`
-
```bash
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_192.jdk/Contents/Home
export PATH=${PATH}:$JAVA_HOME/bin
```
- `source ~/.bash_profile`
- `echo $PATH`

***

#### -classpath 옵션
- 컴파일에 필요한 클래스들이 있는 곳의 경로 지정한다.
```bash
$ javac -classpath hello.jar:. Hello.java
```

***

### 접근지시지
- public
                - 모든 접근을 허용.
- protected
                - 같은 패키지에 있는 객체와 상속관계(다른 패키지도 상속 가능)에 있는 객체들만 허용
- default
                - 같은 패키지에 있는 객체들만 허용.
- private
                - 현재 객체 내에서만 허용(클래스는 public, defualt만 사용 가능)

***
