- 저는 [2주차에서 Enum에 대해서 공부한 적](https://github.com/alswp006/woowa-6th-precourse-memory/blob/main/2%EC%A3%BC%EC%B0%A8/Enum.md)이 있습니다.
- 하지만 적용시켜보지는 못했었는데 이번 3주차 미션에서 추가된 요구 사항에 “Java Enum을 적용한다.”라는 사항이 있습니다!
- 그래서 이번 미션에서 “Enum 하나는 내가 불타오르는 한이 있어도 확실하게 가져가야겠다!”라는 생각으로 이를 다시 공부해보며 정리하고 적용해보려 합니다!

# Enum 간단한 사용법

- Enum으로 정해진 데이터를 묶어주면 보다 구조적인 프로그래밍을 할 수 있다고 공부했었습니다.
- 우선 예를 들어 일주일을 나타내는 변수 week에 요일을 나타내는 상수를 넣어서 사용하는 상황이라고 생각해봅시다.

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

- 위와 같이 week는 상수를 넣어서 사용하지만 week와 전혀 상관없는 값이 들어가도 프로그램은 알지 못합니다. 이와 같은 상황이 오면 우리는 week 값이 요일에 해당하는 값이 맞는지 검증하는 로직을 또 구현해야 합니다.
- 이러한 상황을 해결해주는 것이 enum 클래스입니다.

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

- Enum은 위와 같이 정의하여 사용할 수 있습니다.
- 또한 상수 고유한 값을 매핑하여 사용할 수도 있습니다.

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

- 이와 같이 상수를 (” ”)으로 고유값을 주고 getter, setter 메소드를 만들어주면 위와 같이 사용할 수 있습니다.
- 데이터의 그룹화 및 관리로 아래와 같이 사용할 수도 있습니다.

```java
public enum Class {
    CLASSA("A반", Arrays.asList("kim", "min", "je")),
    CLASSB("B반", Arrays.asList("kim", "seoung", "hyun"));

    private final String Class;
    private final List<String> name;

	  Class(String Class, List<String> name) {
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

## Ordinal을 사용하지 마라..!

- 저는 enum타입을 통해 로또 번호의 당첨 결과 값들을 저장하고 ordinary를 배열의 인덱스로 활용하여 결과값들을 저장해주려 했습니다!
- 그리하여 ordinal을 검색해본 결과…

<img width="706" alt="image" src="https://github.com/alswp006/woowa-6th-precourse-memory/assets/102672547/3eaa1bf4-2724-4f55-bb4f-3d1ac4054456">

- 파도파도 괴담뿐인 ordinal때문에 다른 것을 사용하기로 했습니다!!
- 우선 ordinal을 사용하면 안되는 이유에 대해서 한번 알아보겠습니다.

## Ordinal 왜 쓰면 안돼…

- enum 타입의 상수를 선언할 때 순서를 지키면서 정의해야 한다.
    - 말 그대로 Enum 타입에서 상수를 선언할 때 ordinary는 배열의 인덱스와 같이 순서에 따른 수를 반환해주기 때문에 순서를 지키면서 정의해줘야 합니다.
    - 이는 유지보수가 필요할 때나 처음에 선언할 때도 많은 불편함이 있겠죠???
- 같은 수는 여러 개 선언할 수 없다.
    - 말 그대로 같은 수를 선언할 수 없다는 것인데요.
    - 제가 지금 구현하고 있는 우테코 프리코스 3주차 로또 미션같은 경우 수를 아래와 같이 정의하고 있는데요.
    
    ```java
    FIFTH(3, 5000, 0),
    FOURTH(4, 50000, 0),
    THIRD(5, 1500000, 0),
    SECOND(5, 30000000, 1),
    FIRST(6, 2000000000, 0),
    No_Match(0, 0, 0);
    ```
    
    - 이를 ordinary로 할 경우 일치하는 수가 5개일 경우 보너스 번호 때문에 헷갈리는 부분이 많이 생길 수도 있습니다!!

## 그래서 뭘 써야하는데..

- 우선 찾아본 첫 번째 방법은 필드로 선언하는 것입니다.
    - 하지만 저는 이미 필드의 값을 3개나 줘버려서 너무 많은 필드의 값을 주면 안될 것 같다는 생각이 들어 다른 방법을 찾아보기로 했습니다!!
- 그래서 제가 사용하기로 한 것은 EnumMap!!

## EnumMap이 뭐야 그게 왜 써..?

- EnumMap은 Map 인터페이스에서 키를 특정 enum 타입만을 사용하도록 하는 구현체 입니다.
- EnumMap은 내부에 데이터를 Array에 저장하기 때문에 HashMap 처럼 해시를 만들고 해시 충돌(Hash Collision)에 대응하는 작업 자체가 필요없어져서 HashMap보다 빠릅니다.
- 정리해보면 성능이 우수하고 key 값을 Enum 타입으로 제한할 수 있다정도가 되겠네요!
- 사용 방법은 제 Lotto 미션에 적용해보며 익혀보겠습니다!

## 그거 어떻게 쓸건데..

- 그러면 EnumMap을 통해서 제가 지금 해야할 것들을 정리해보겠습니다.
    1. EnumMap<Winning, Integer>를 통해 EnumMap을 구현하고 각각의 Winning에 대한 Integer를 0으로 초기화해줍니다
        
        → 아마 stream과 forEach를 쓰면 되지 않을까 싶습니다..
        
        → 나중에 로또 일치 갯수에 맞는 Winning의 Integer에 1씩 더해줄 생각입니다!!
        
    2. 랜덤 생성 수들을 하나씩 반복하며 당첨 번호와 비교하여 타입에 맞는 EnumMap의 Interger부분에 1씩 더해줍니다!
        
        → 이건 감이 잘 안잡히는 부분인데요.. 우선은 Model에서 따로 비교해주는 클래스를 만들거나 랜덤 수들을 생성할 때부터 비교를 하여 더해줄까 생각중입니다!
        
        → 하나의 메소드는 하나의 일만 담당해야하니 두 번째 방법은 패스!
        
        → 랜덤 수들이 현재는 이중 리스트로 구현이 되어있는데 이를 일급 컬렉션으로 바꾼 뒤 그 쪽에 비교 및 일치 수들을 반환하는 메소드를 만드는 건 어떨까요.. 우선 이 방법 채택!
        
        → 우선 로직은 이렇게 할까 싶습니다.
        
        - 랜덤 수를 하나씩 꺼내어 당첨 번호에 있는지 contains로 확인
        - 있으면 count+=1
        - 다 끝나면 count에 맞는 EnumMap을 찾아 Integer+=1
    3. EnumMap의 순서에 따라 결과 값 출력
        - toString 같은 것을 Winning안에 메소드로 구현해놓고 EnumMap을 반복하여 실행해주면 어떨까 싶네요!!

# EnumMap 적용기..

- 우선 제rk Enum 클래스인 Winning 클래스입니다.

```java
public enum Winning {
    FIFTH(3, 5000, 0),
    FOURTH(4, 50000, 0),
    THIRD(5, 1500000, 0),
    SECOND(5, 30000000, 1),
    FIRST(6, 2000000000, 0),
    No_Match(0, 0, 0);

    private final int matchNumber;
    private final int winningPrice;
    private final int checkBonusBall;

    public int getMatchNumber(){
        return matchNumber;
    }

    public int getWinningPrice(){
        return winningPrice;
    }

    Winning(int matchNumber, int winningPrice, int checkBonusBall) {
        this.matchNumber = matchNumber;
        this.winningPrice = winningPrice;
        this.checkBonusBall = checkBonusBall;
    }

    public static Winning result(int matchNumber, int checkBonusBall){
        if (matchNumber == SECOND.matchNumber && checkBonusBall == SECOND.checkBonusBall){
            return SECOND;
        }
        for (Winning winning : values()){
            if (winning.matchNumber == matchNumber){
                return winning;
            }
        }
        return No_Match;
    }

}
```

- 생성자를 통해 필드값을 넣어주고 result()메소드를 통해 각 일치 숫자에 맞는 상수값을 부여해주고 있습니다.
- 그리고 제가 최종적으로 구현한 EnumMap의 코드입니다.

```java
import lotto.View.PrintOutput;
import java.util.EnumMap;

public class Result {
    private final EnumMap<Winning, Integer> result = new EnumMap<>(Winning.class);

    public Result() {
        for (Winning winning : Winning.values()) {
            result.put(winning, 0);
        }
    }

    public void winningResult(int match, int bonusBall) {
        Winning winning = Winning.result(match, bonusBall);
        result.put(winning, result.get(winning) + 1);
    }

    public void printResult(int purchasePrice) {
        int totalPrice = 0;
        System.out.println("\n" + PrintOutput.RESURLT_LOTTO + "\n---");
        for (Winning winning : Winning.values()) {
            totalPrice += winning.getWinningPrice() * result.get(winning);
            printMatch(winning);
        }
        System.out.printf("총 수익률은 %,.1f%%입니다.", (double) totalPrice / purchasePrice * 100);
    }

    public void printMatch(Winning winning) {
        String message = String.format(
                "%d개 일치 (%s원) - %d개",
                winning.getMatchNumber(),
                String.format("%,d", winning.getWinningPrice()),
                result.get(winning));
        if (winning != Winning.No_Match) {
            System.out.println(message);
        }
        if (winning == Winning.SECOND) {
            message = message.replace("일치", "일치, 보너스 볼 일치");
            System.out.println(message);
        }
    }
}
```

- 우선 EnumMap을 통해 위의 Enum클래스인 Winning을 넣어주고 value값으로는 로또 번호의 일치 갯수에 따른 Enum타입의 갯수를 카운트한 값을 넣어주려고 했습니다.
- 생성자에서는 Winning의 상수 갯수만큼 EnumMap에 상수를 key로 value에는 0으로 초기화해주고 있습니다.
- winningResult는 result에 번호와 보너스볼의 일치 갯수에 맞는 key의 value 값을 1씩 증가시켜 줍니다.
- 그리고 printResult와 printMatch메소드는 result를 차례로 꺼내어 요구하는 출력값의 형태대로 출력하는 메소드입니다!

# 정리

- 사실 이번 미션의 요구사항들을 Enum으로 적용하려하니 감이 잘 안잡혀서 Enum을 적용하는 데에만 하루하고 반나절정도가 소요된 것 같습니다..
- Enum과 EnumMap에 대하여 공부할 때는 어떻게 사용해야할지 감이 잘 안잡혔는데 테스트 클래스를 만들어 혼자서 정말 여러가지 테스트도 해보고 오류도 많이 겪으며 사용법을 익히고 나니 좀 더 깊은 이해를 하게된 것 같습니다!
- 마음이 정말 급해져서 많은 테스트와 오류를 정리해가며 공부하면 기능 구현을 마치지 못할 것 같아 일단 문제를 해결해야한다는 생각에 여러가지를 몸으로 부딪히며 해결하여 이러한 부분들을 필기하며 정리하지 못한 것은 아쉽습니다..
- 제 머릿속으로만 정리된 부분도 있고 여기서 정리한 부분들도 있는데 Enum을 더 많이 사용하다 보면 정리가 더 잘 될 것 같습니다!
