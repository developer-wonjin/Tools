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

**대략적인 순서**

1. 어떤 프로젝트건 기능명세가 항상 우선적으로 존재.
2. **[테스트코드 작성]** 기능에 대한 테스트코드를 먼저 작성(기능구현하기도 전에) 
   - 테스트코드에 컴파일에러 발생함(어쩔 수 없이 놔둠)
3. **[기능구현]** 테스트를 통과하기 위한 코드를 구현한다.
   - 컴파일 에러가 없어질 **정도로만 구현**
   - 테스트코드를 통과할 **정도로만 구현**
4. **[테스트]** 테스트코드를 실행한다.
5. 실패시 구현코드 수정후 다시 테스트
6. 성공시 다른 테스트코드 추가 이후 동일 작업.



**TDD구성요소(TDD는 설계과정의 일부가 된다.)**

입력과 결과에 유의하여 기능명세를 (입력+결과)로 이해할 줄 알아야 한다.

1. 테스트할 기능실행
   - 클래스, 메서드, 함수   -   네이밍
   - 파라미터
2. 결과검증
   - 리턴값



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







<hr>

**구현순서Tip**

- step1. 모든 경우의 수를 나열하는것이 중요.

- step2. 각 경우의 수의 특징을 파악하여 **범주화**한다.

- step3. 가장 **경우의 수가 적은 범주부터 정복**해 나간다.(가장 쉬운 경우먼저 구현)

  이떄 테스트가 통과할 만큼만 작성하는 것이 중요! 

  - step3-1.단순하게 상수 리턴

  ```java
  public class Calculator {
      public static int plus(int a1, int a2) {
          return 3;
      }
  }
  ```

  - step3-2.이후 If문등 조건절의 값비교가 필요한 테스트를 늘려라

  ```java
  public class Calculator {
      public static int plus(int a1, int a2) {
          return a1 + a2;
      }
  }
  ```

- step4. step3과정을 통해 구현을 일반화한다.



<hr>



## 2.4 테스트 예(암호 검사기)

[검사규칙]

- 길이가 8글자 이상
- 0~9 사이의 숫자를 포함
- 대문자를 포함

[암호강도 결정기준]

- 강함: 위 세 규칙 모두를 충족
- 보통: 위 세 규칙중 2개규칙 충족
- 약함: 위 세 규칙중 1개규칙 충족 + 하나도 충족하지 않은 경우



**[1단계: 가능한 경우의 수 나열하기]**

| 번호 | 길이 | 숫자 | 대문자 | 구현순서 |
| ---- | ---- | ---- | ------ | -------- |
| 1    | O    | O    | O      | 1        |
| 2    | X    | O    | O      | 2        |
| 3    | O    | X    | O      | 2        |
| 4    | O    | O    | X      | 2        |
| 5    | O    | X    | X      | 3        |
| 6    | X    | O    | X      | 3        |
| 7    | X    | X    | O      | 3        |
| 8    | X    | X    | X      | 4        |
| 9    | null | null | null   | 예외상황 |

- 모든 규칙을 충족하는 경우: 1번
- 모든조건을 충족하지 않은 경우: 2번 ~ 8번
  - 두개 규칙을 충족하는 경우(2,3,4)
  - 한개 규칙을 충족하는 경우(5,6,7,8)



**[2단계: 단순한 케이스가 무엇인지 파악하기]**

> 조건 충족의 갯수를 기준으로 하나씩 늘려가는 순으로 코드를 배치한다.
>
> 그러나, 구현순서는 역순으로 진행한다.
>
> 즉, 구현은 조건충족 갯수가 가장많은 경우부터 시작한다.

**단순한 케이스**

- 모든 조건을 충족하는 경우
- 단순히 상수를 리턴한다.(if문 로직없이)



**[3단계: 점점 복잡한 경우도 테스트에 추가한다.]**

> 2,3,4 경우의 테스트를 추가하여 구현하고
>
> 5,6,7 경우의 테스트를 추가하여 구현한다.

**[4단계: 예외처리]**

구현 중간에 예외를 처리해줘도 된다

입력값이 null일 경우 또는 "" 비어있는String일 경우를 대처해준다.

(예외사항은 초반에 발견하여 처리하는것이 유리하다

이유. 예외상황을 처리할 때 if~else구조가 미리 만들어지기 때문에 코드구조가 뒤에 바뀔 가능성을 줄여준다.)

**[5단계: 리펙토링]**

리펙토링의 목적: 변경하기 쉬운 구조(유지보수하기 편한 구조)로 만들기

중간 중간에 작은 리펙토링 거리를 찾으면 바로 리펙토링하라.

못할 만큼 너무 복잡하면 하지말고 구현에만 집중할 것.

구현흐름이 한눈에 들어오면 리펙토링을 시작해라.







# 3. JUnit 기본

## 3.1. 주요 단언메소드

1. 값비교(primitive, object)
   - assertEquals
   - assertNotEquals
2. 주소비교
   - assertSame
   - assertNotSame
3. Boolean비교
   - assertTrue
   - asserNotTrue
4. Null비교
   - assertNull
   - assertNotNull
5. 실패처리
   - fail











