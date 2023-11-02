- 이번 2주차 미션에서 추가된 요구 사항에는 “JUnit 5와 AssertJ를 이용하여 본인이 정리한 기능 목록이 정상 동작함을 테스트 코드로 확인한다.”라는 항목이 있었습니다!
- 그래서 JUnit5와 AssertJ를 공부해본 뒤 이번에 테스트 도구를 공부하라고 제공해주신 test/java/study 클래스와 이번에 제공된 테스트 코드까지 살펴보고 실제 필요한 테스트 코드를 직접 작성해보겠습니다!
- 우선 JUnit5부터 알아보겠습니다!

# JUnit5

- JUnit은 Java에서 많이 사용되는 유닛 테스트 프레임워크라고 합니다!
- 이와 같은 특징이 있습니다
    - **assert 메소드**로 테스트 케이스의 수행 결과를 판결한다.
    - JUnit4부터는 **어노테이션**을 통해 개발자가 간결하게 사용할 수 있도록 지원하고 있다.
    - **Junit5는 JAVA8 버전 이상부터 사용 가능하며 스프링 부트 2.2버전 이상부터는 기본으로 제공**하고 있다.
    - 테스트 결과를 확인하는 것 이외에도 **최적화된 코드를 유추해내는 기능**도 제공한다.

## JUnit5의 구성

- JUnit5는 **JUnit Platform, JUnit Jupiter, JUnit Vintage**로 구성되어 있습니다.
    1. **JUnit Platform** : J**VM에서 돌아가는 테스트 프레임워크**로, 테스트를 발견하고 테스트 계획을 생성하는 TestEngine 인터페이스를 가지고 있습니다. (TestEngine을 통해서 테스트를 발견하고 ,실행하고 ,결과를 보고합니다.)
    2. **JUnit Jupiter** : JUnit5 TestEngine의 실제 구현체입니다. Junit5 기반의 테스트를 실행시키기 위한 TestEngine을 Platform에 제공합니다.JUnit에서의 테스트 코드 작성을 위한 새로운 프로그래밍 모델, 확장 모델입니다.
    3. **JUnit Vintage** : JUnit3 및 JUnit4 버전으로 작성한 테스트 코드를 실행할 수 있도록 해주는 테스트 엔진입니다.

## JUnit5의 어노테이션(Annotation)

- JUnit의 어노테이션은 이와 같은 것들이 있습니다.

| Annotation | 내용 |
| --- | --- |
| @Test | 테스트 메소드임을 선언합니다 |
| @DisplayName | 테스트 클래스 또는 테스트 메소드의 이름을 선언할 때 사용합니다. |
| @BeforeEach | 모든 테스트 실행 전에 실행할 테스트 앞에 사용합니다. |
| @AfterEach | 모든 테스트 실행 후에 실행할 테스트 앞에 사용합니다. |
| @BeforeAll | 현재 클래스를 실행하기 전 가장 먼저 실행할 테스트에 사용합니다. 이는 static로 선언해야합니다. |
| @AfterAll | 현재 클래스의 종료 후 실행할 테스트에 사용합니다. 이는 static으로 선언해야합니다. |
| @Timeout | 테스트 실행 시간을 선언 후 초과되면 실패하도록 설정합니다 |
- 위와 같은 어노테이션들이 있고 어노테이션을 더 알아보고 싶다면 아래의 링크에서 보시면 됩니다!
- [JUnit5 Annotation 더 보기](https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations)

## 테스트 클래스와 메소드

## 테스트 클래스와 메소드의 규칙

- 테스트 클래스는 하나 이상의 테스트 메소드를 포함하는 최상위 클래스, static 멤버 클래스, 또는 중첩 클래스이어야 합니다.
- 테스트 클래스는 추상 클래스로 정의되면 안 되며, 단일 생성자만 가져야 합니다.
- 테스트 메소드는 '@Test, @RepeatedTest, @ParameterizedTest, @TestFactory, @TestTemplate' 중 하나의 어노테이션으로 선언된 메소드를 말합니다.
- 생명 주기 메소드는 '@BeforeAll, @AfterAll, @BeforeEach, @AfterEach' 중 하나의 어노테이션으로 선언된 메소드를 말합니다.
- 테스트 메소드와 생명 주기 메소드는 추상 메소드로 정의되면 안 되며, 값을 반환해서도 안 됩니다. 그리고 두 메소드는 non-static인 인스턴스 메소드이어야 합니다.

# AssertJ

- AssertJ란 많은 assertion을 제공하는 자바 라이브러리입니다.
- 주요 목표
    - 에러 메세지와 테스트 코드의 가독성을 매우 높이는 것과 테스트 유지관리를 쉽게 만드는 것이 주요 목표라고 합니다!
- JUnit5와 함께 사용하기 좋다고 합니다.
- [기존 JUnit의 Assertion을 AssertJ로 쉽게 변환하는 가이드](https://joel-costigliola.github.io/assertj/assertj-core-converting-junit-assertions-to-assertj.html)까지 있습니다!

## AssertJ 사용하기

### AssertJ 메소드 정적 import

- AssertJ를 사용하려면 정적 import를 해야한다는데요.
- 아래와 같이 한다고 합니다.
    
    ```java
    import static org.assertj.core.api.Assertions.*;
    ```
    
- 또한 여느 import문과 같이 필요한 메소드만 따로 가져올 수도 있습니다.
    
    ```java
    import static org.assertj.core.api.Assertions.assertThat;  // main one
    import static org.assertj.core.api.Assertions.atIndex; // for List assertions
    import static org.assertj.core.api.Assertions.entry;  // for Map assertions
    ```
    

### assertThat().

- assertThat(테스트 개체).비교 메소드의 형식을 통해 테스트를 해볼 수 있습니다.
    
    ```java
    assertThat(objectUnderTest). // code completion -> assertions specific to objectUnderTest
    ```
    

## AssertJ 사용해보기

### contains()

- 배열애 기대값이 포함되어있는지 확인합니다.

```java
@Test
public void containTest(){
    String[] name = {"kim", "min", "je"};

    assertThat(name).contains("kim");
}
```

<img width="708" alt="image" src="https://github.com/alswp006/woowa-6th-precourse-memory/assets/102672547/639feef0-f4cb-49fb-a9d3-b16306c53827">

- 테스트가 잘 성공하는 모습을 볼 수 있습니다

### containsExactly()

- 배열에 기대값만 포함되어있는지 확인합니다.

```java
@Test
public void containsExactly(){
    String[] name = {"kim", "min", "je"};

    assertThat(name).containsExactly("kim");
}
```

<img width="708" alt="image" src="https://github.com/alswp006/woowa-6th-precourse-memory/assets/102672547/1f22538e-fa38-4961-8c16-886856527b19">

- 테스트가 의도한대로 실패하는 것을 볼 수 있습니다

### isEqualTo()

- 테스트 대상이 기대값과 같은지 확인합니다.

```java
@Test
public void isEqualTo(){
    String name = "kimminje";

    assertThat(name).isEqualTo("kimminje");
}
```

<img width="708" alt="image" src="https://github.com/alswp006/woowa-6th-precourse-memory/assets/102672547/a56d829b-8904-47b4-a2e5-f76402578030">

- 테스트가 잘 성공하는 것을 확인할 수 있습니다.

### ****assertThatThrownBy()****

- 예외가 발생하는 케이스를 테스트할 때 예상한 예외가 나왔는지 확인할 때 사용합니다

```java
@Test
public void containsExactly() throws Exception{
    String name = "kimminje";

    assertThatThrownBy(() -> name.charAt(name.length()))
            .isInstanceOf(StringIndexOutOfBoundsException.class);
}
```

<img width="708" alt="image" src="https://github.com/alswp006/woowa-6th-precourse-memory/assets/102672547/b5b18ced-9a07-41e6-b306-e6a0fa9c31f0">

- name.charAt(name.length())을 통해 string의 인덱스 범위를 넘어간 값을 주었고 예외가 잘 발생하여 테스트가 통과한 것을 확인할 수 있습니다.
- 그리고 또한 assertThatThrownBy의 특징으로는 람다식을 사용하여 인자를 전달해준다는 점이 있습니다.
- 그렇다면 예외가 발생하지 않는다면 어떻게 될까요??

```java
@Test
public void containsExactly() throws Exception{
    String name = "kimminje";

    assertThatThrownBy(() -> name.charAt(name.length()-1))
            .isInstanceOf(StringIndexOutOfBoundsException.class);
}
```

<img width="708" alt="image" src="https://github.com/alswp006/woowa-6th-precourse-memory/assets/102672547/31cf01e4-ad36-44db-a3be-c34207d7c3b7">

- 당연히 테스트가 통과되지 않은 것을 확인할 수 있네요!

### 정리

- 여기까지 JUnit5와 AssertJ에 대하여 알아보았는데요!
- AssertJ의 메소드들을 위의 것들로 공부한 이유가 있었습니다!
- 사실 위의 메소드들이 프리코스 2주차 미션에서 제공된 공부를 위한 study테스트 코드에서 사용된 메소드들입니다.
- 이들을 통해 테스트코드를 공부해보았으니 이제 프리코스에서 제공하는 테스트 코드를 분석해보고 직접 테스트 코드를 만들어보겠습니다!
- 이들은 글의 가독성을 위한 객체 지향으로 새페이지에 작성해보겠습니다!
