- 보통 상수는 static final 키워드를 사용하여 정의합니다.
- 하지만 이는 여러 문제점을 야기시킨다고 합니다.
- 이러한 문제점들을 해결하기 위한 Enum 클래스를 알아봅시다.

# Enum Class

- Enum은 "Enumeration"의 약자이며 Enumeration은 열거, 목록 등의 뜻을 가지고 있습니다.
- 정해진 데이터의 묶음을 Enum으로 묶어주면 보다 구조적인 프로그래밍을 할 수 있습니다.
- 열거 타입은 서로 연관된 상수 데이터들의 집합입니다. 따라서 배열처럼 나열하여 사용할 수 있습니다.
- 우선 예를 들어 알아봅시다.
- 일주일을 나타내는 변수 week에 요일을 나타내는 상수를 넣어서 사용하는 상황이라고 생각해봅시다.

```java
public class Week {
    public static final String MONDAY = "MONDAY";
    public static final String SUNDAY = "SUNDAY";

    public static void main(String[] args) {
        String week ;
        week = MONDAY;
        week = SUNDAY;
        // week에 알맞은 값들이 들어감

        week = "kimminje";
        // week와 전혀 상관없는 값이 들어가도 프로그램은 알지 못함
    }

}
```

- 위와 같은 코드가 구성될 것입니다.
- 위와 같이 week는 상수를 넣어서 사용하지만 week와 전혀 상관없는 값이 들어가도 프로그램은 알지 못합니다. 이와 같은 상황이 오면 우리는 week 값이 요일에 해당하는 값이 맞는지 검증하는 로직을 또 구현해야 합니다.
- 이러한 상황을 해결해주는 것이 enum 클래스입니다.
- 쉽게 말해 초기적인 문제를 바로 잡겠다!라는 것이 enum 클래스입니다.
- enum 클래스는 아래와 같이 사용됩니다.

```java
enum Week {
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
}

public class TTest {
    public static void main(String[] args) {
        Week week ;
        week = Week.MONDAY;
        week = Week.SUNDAY;
        System.out.println(week);
        // week에 알맞은 값들이 들어감

        week = "kimminje";
        // incompatible types 에러 발생
    }
}
```

- 위와 같이 저희에게 발생했던 문제를 해결해주는 것을 확인해볼 수 있습니다.
- 그럼 enum 클래스에 대해 더 자세하게 알아봅시다!

## Enum의 특징

- DB의 코드 값과 많이 활용될 수 있습니다.
- **primitive(원시) 타입이 아닌 referece(참조) 타입으로 분류**되므로 enum 상수값은 힙 영역에 저장되게 됩니다.
    - 위에서 예를 들면 MONDAY와 SUNDAY라는 객체가 힙 영역에 생성되고 monday와 sunday 변수는 객체의 번지를 참조하는 값을 가지게 됩니다.
- 위의 예와 같이 사용자가 올바른 값을 사용했는지에 대한 기능을 구현하지 않고도 판단할 수 있습니다.
- enum 타입 객체도 하나의 데이터 타입이므로 변수를 선언하고 사용할 수 있습니다.

## Enum의 장점

- 코드가 단순해지며 가독성이 좋아집니다.
- 인스턴스 생성과 상속을 방지하여 상수의 타입 안정성(Type-Safety)가 보장됩니다.
- 구현 의도가 열거임을 확실하게 알 수 있다.

## Enum의 메소드

- 모든 Enum 타입은 컴파일 시에 java.lang.Enum 클래스를 상속하게 되어있기 때문에, java.lang.Enum 에 선언된 메소드를 이용할 수 있습니다.
- Enum은 다음과 같은 메소드들이 있습니다.

| 메소드 | 설명 | 리턴 타입 |
| --- | --- | --- |
| name() | 열거체 상수의 이름을 리턴합니다. | String |
| ordinal() | 해당 열거체 상수의 열거체 순번(0부터 시작)을 리턴합니다. | int |
| compareTo() | 열거체 상수들을 비교하여 순번 차이를 리턴합니다. | int |
| valueOf(String name) | 문자열을 입력받아서 일치하는 열거체 상수를 리턴합니다. | enum |
| values() | 모든 열거 객체들을 enum배열로 리턴합니다. | enum[] |
- 이 외의 메소드를 더 보고싶다면 [여기](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Enum.html)로👈👈

## Enum 매핑

- 또한 Enum의 상수를 가져왔을 때 상수의 고유값을 가져오고 싶으면 매핑하여 사용할 수도 있다.

```java
public enum Week {
    MONDAY("월요일"),
    TUESDAY("화요일"),
    WEDNESDAY("수요일"),
    THURSDAY("목요일"),
    FRIDAY("금요일"),
    SATURDAY("토요일"),
    SUNDAY("월요일");

    private String week;
    private Week(String week) {
        this.week = week;
    }

    public String getDay(){
        return week;
    }
}

public class Test {
    public static void main(String[] args) {
        Week week = Week.SUNDAY;
        System.out.println(week);
        System.out.println(week.getDay());
    }
}

//출력
SUNDAY
일요일
```

- 이와 같이 상수를 (” ”)으로 고유값을 주고 getter, setter 메소드를 만들어주면 위와 같이 사용할 수 있다.

## **데이터의 그룹화 및 관리**

- 또한 enum은 데이터의 그룹화 및 관리에 용이합니다.
- 데이터들을 enum을 통해 한 클래스 내에서 관리할 수 있습니다.
    - 예를 들어 반에 대한 학생들 명단을 다룬다고 한다면, 반도 다르고 같은 반에서 학생들도 다르니 반 클래스와 학생 클래스를 나누어 두 클래스가 연관되게 코드를 구성해야할 것입니다.
    - 하지만 enum을 이용하면 한눈에 보기 쉽게 구성이 가능합니다. 그리고 적절한 메소드 로직을 enum 객체 내 안에 구현해준다면 강력한 상수 클래스를 구현할 수 있게 됩니다.

```java
public enum Class {
    CLASSA("A반", Arrays.asList("kim", "min", "je")),
    CLASSB("B반", Arrays.asList("kim", "seoung", "hyun"));

    private final String Class;
    private final List<String> name;

    Week(String Class, List<String> name) {
        this.Class = Class;
        this.name = name;
    }

    String getClass(String names) {
        return Arrays.stream(Class.values())
                .filter(classes -> classes.equals(names))
                .findFirst()
                .orElseThrow(Exception::new);
    }
}
```

## Enum의 확장

- enum 매핑 기능과 람다를 활용하여 상태와 행위를 한 곳에서 관리가 가능합니다.
- 이를 이용하면 아래와 같이 간단한 사칙연산 로직을 구현할 수 있습니다.

```java
// 함수형 인터페이스 임포트
import java.util.function.DoubleBinaryOperator;

enum Operation {
    PLUS("+", (x, y) -> x + y),
    MINUS("-", (x, y) -> x - y),
    TIMES("*", (x, y) -> x * y),
    DIVIDE("/", (x, y) -> x / y);

    private final DoubleBinaryOperator op; // 람다식을 저장할 필드

    private final String symbol;

    Operation(String symbol, DoubleBinaryOperator op) {
        this.symbol = symbol;
        this.op = op;
    }

    @Override
    public String toString() { return symbol; }

    public double apply(double x, double y) {
        return op.applyAsDouble(x, y);
    }
}
```

## 참고

- **[☕ 자바 Enum 열거형 타입 문법 & 응용 💯 정리](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%97%B4%EA%B1%B0%ED%98%95Enum-%ED%83%80%EC%9E%85-%EB%AC%B8%EB%B2%95-%ED%99%9C%EC%9A%A9-%EC%A0%95%EB%A6%AC#recentEntries)**
- 그 외 구글링👍
