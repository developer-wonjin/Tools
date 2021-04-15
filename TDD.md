# TDD

- 책 `테스트 주도 개발 시작하기` - 최범균 저

# 1장. 환경설정

## 1.1 이클립스 java project에서 JUnit 설정

1. File>New>JavaProject 생성

2. project 우클릭 > properties > 좌측탭 java build path  > libraries탭 > add Library버튼클릭

3. JUnit클릭 > Next > Junit5선택

4. 테스트코드 작성 (StrinngTest.java)

   ```java
   package chap01;
   
   import static org.junit.jupiter.api.Assertions.*;
   
   import org.junit.jupiter.api.Test;
   
   public class StringTest {
   
   	@Test
   	void substring() {
   		String str = "abcde";
   		assertEquals("cd", str.substring(2, 4));
   	}
   }
   ```

5. StrinngTest.java 파일 마우스 우클릭

6. Run > Run As > JUnit Test



## 1.2. 메이븐프로젝트 JUnit설정

pom.xml에 다음 의존성라이브러리 추가

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>tddb</groupId>
    <artifactId>chap01-maven</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.5.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>8</source>
                    <target>8</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.1</version>
            </plugin>
        </plugins>
    </build>
</project>
```

- 메이븐 프로젝트가 2.19.x버전이면 maven-surefire-plugin을 2.22.0 이상을 사용해야함



# 2장. TDD시작

## 2.1. TDD가 없었을 때

1. 한 번에 General한 코드를 작성하려고함.
2. 오류를 찾기 위해 많은 코드를 뒤져야함



## 2.2. TDD란?

1. 어떤 프로젝트건 기능명세가 항상 우선적으로 존재.
2. **[테스트코드 작성]** 기능에 대한 테스트코드를 먼저 작성(기능구현하기도 전에) 
   - 테스트코드에 컴파일에러 발생함(어쩔 수 없이 놔둠)
3. **[기능구현]** 테스트를 통과하기 위한 코드를 구현한다.
   - 컴파일 에러가 없어질 **정도로만 구현**
   - 테스트코드를 통과할 **정도로만 구현**
4. **[테스트]** 테스트코드를 실행한다.
5. 실패시 구현코드 수정후 다시 테스트
6. 성공시 다른 테스트코드 추가 이후 동일 작업.



## 2.3. 테스트/구현 코드 작성시 고려할 점.

**테스트코드**

- asserEquals(expect, result) 의 expect 부분은 별도의 Enum클래스의 상수로 표현하면 좋음.
- Enum 열거타입은 더 눈에 보기 좋게 표현함. 

**메서드 구현**

- 메서드이름은 뭐가 좋을까
- 파라미터 갯수, 파라미터 데이터타입, 리턴타입
- 정적메서드/ 인스턴스메스더 중 뭐가 더 좋을까
- 메서드가 소속된 클래스의 이름은 뭐가 좋을까

(리펙토링)

- 구현메소드의 로직을 복잡하게, 길게 만드는 요소는 메서드추출을 한다.
- 불필요한 boolean 선언은 없애고 if의 조건절에 집어넣는다.
- if문의 괄호 ( )안 조건이 길면 cnt 변수로 대체가능하지 확인할 것.
- 조건을 점진적으로 충족해 나가는 순으로 코드를 배치
- 구현하는 순서는 충족하는 조건이 많은 것부터





**구현순서**

- 모든 경우의 수를 나열하는것이 중요.

- 각 경우의 수의 특징을 파악하여 **범주화**한다.

- 가장 **경우의 수가 적은 범주부터 정복**해 나간다.

- 가장 **쉬운 경우먼저 구현** (경우의 수가 적은 범주와 동치)

  ```java
  public class Calculator {
      public static int plus(int a1, int a2) {
          return 3;
      }
  }
  ```

  이후 General하게 확장

  ```java
  public class Calculator {
      public static int plus(int a1, int a2) {
          return a1 + a2;
      }
  }
  ```

  







