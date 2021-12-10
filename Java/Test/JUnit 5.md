#JUnit 5
> [JUnit User Guide](https://junit.org/junit5/docs/current/user-guide/) 를 읽고 정리한 자료입니다. 
---

# 1. JUnit이란
- JUnit은 자바 프로그래밍 언어용 **단위(Unit) 테스트 프레임워크**이다.

## JUnit 4, JUnit 5 차이점

구분|JUnit4|JUnit5
:---|:---|:---
Architecture|단일 jar|3개의 하위 모듈
JDK 버전|Java 5 이상|Java 8 이상

- JUnit 5는 JUnit Platform, JUnit Jupiter, JUnit Vintage 모듈로 구성되어 있으며, **Java 8 이상**부터 제공한다.
- `JUnit Platform`
  - JVM에서 **테스트 프레임워크를 시작하기 위한 기반 역할**을 한다.
  - 플랫폼에서 실행되는 테스트 프레임워크를 개발하기 위한 **TestEngine API를 정의**한다.
- `JUnit Jupiter`
  - JUnit 5에서 테스트 및 확장을 작성하기 위한 새로운 프로그래밍 모델과 확장 모델의 조합이다.
  - Jupiter 기반 **테스트를 실행하기 위한 TestEngine을 제공**한다.
- `JUnit Vintage`
  - **하위 호환성**을 위해 플랫폼에서 JUnit 3 및 JUnit 4 기반 테스트를 실행하기 위한 TestEngine을 제공한다.

## 적용 방법
- Spring Boot 2.2 버전부터 JUnit 5가 기본으로 채택되어 의존성이 추가된다.
- Spring Boot 프로젝트가 아닌 경우에는 다음과 같이 의존성을 추가하면 된다.
```java
//https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-engine

//Gradle - build.gradle인 경우 적용 방법
testImplementation 'org.junit.jupiter:junit-jupiter-engine:5.8.1'

//Maven - pom.xml인 경우 적용 방법
<dependency>
<groupId>org.junit.jupiter</groupId>
<artifactId>junit-jupiter-engine</artifactId>
<version>5.8.1</version>
<scope>test</scope>
</dependency>
```

# 2. 테스트 작성하기
- 다음 예제는 JUnit Jupiter에서 테스트를 작성하기 위한 최소 요구 사항을 보여준다.
```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import example.util.Calculator;

import org.junit.jupiter.api.Test;

class MyFirstJUnitJupiterTests {

    private final Calculator calculator = new Calculator();

    @Test
    void addition() {
        assertEquals(2, calculator.add(1, 1));
    }
    
}
```

## 어노테이션
- 달리 명시되지 않는 한 모든 핵심 어노테이션은 junit-jupiter-api 모듈의 org.junit.jupiter.api 패키지에 있다.

어노테이션|설명
:---|:---
@Test|테스트 메소드임을 나타낸다.
@ParameterizedTest|매개변수가 있는 테스트 메소드임을 나타낸다.
@RepeatedTest|반복되는 테스트임을 나타낸다.
@TestFactory|동적 테스트임을 나타낸다.
@TestTemplate|등록된 공급자가 반환한 호출 컨텍스트의 수에 따라 여러 번 호출되도록 설계된 테스트 템플릿임을 나타낸다.
@TestClassOrder|테스트 클래스 실행 순서를 구성하는데 사용된다.
@TestMethodOrder|테스트 메소드 실행 순서를 구성하는데 사용된다.
@TestInstance|테스트 인스턴스 생명 주기를 구성하는데 사용된다.
@DisplayName|테스트 클래스 또는 메소드에 대한 사용자 정의 이름을 선언한다.
@DisplayNameGeneration|테스트 클래스에 대한 사용자 정의 이름 생성기를 선언한다.
