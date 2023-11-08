- 저는 우선 코드 구현을 마쳤습니다..
- 하지만 코드가 너무 난잡한 것처럼 느껴지고 record, MVC 패턴의 분리 등을 공부하였는데 적용하려니 어디서부터 손을 대야할지 잘 모르겠어서 우선 제 코드에 대한 정리를 한 뒤 이를 바탕으로 재설계하고 새로 배운 것을 적용해보려 합니다!!

# 현재 코드 목차

- [Controller](#controller)
    - [PlayGame](#playgame)
- [Model](model)
    - [Lotto](#lotto)
    - [Result](#result)
    - [Setting](#setting)
    - [UserLotto](#userlotto)
    - [Winning](#winning)
- [View](#view)
    - [PrintInput](#printinput)
    - [PrintOutput](#printoutput)

# Controller

## PlayGame

### play()

- Setting 클래스의 purchase_amount()메소드로 구입 금액 입력받기
- UserLotto 클래스의 userLottosCreate(lottoAmount)메소드를 통해 유저의 로또 번호 생성하기
- Setting 클래스의 getWinningNumbers()메소드를 통해 당첨 번호 입력받기
- Setting 클래스의 getBonusNumber()메소드를 통해 보너스 번호 입력받기
- 현재 클래스의 matchResult(randomLottoNumbers, setting.getBonusNumber())를 통해 당첨된 복권 Result 클래스의 EnumMap인 result에 저장
- WinningData 클래스의 printResult(lottoAmount * 1000)메소드를 통해 당첨 통계 및 수익률 출력

### matchResult()

- Lotto클래스의 객체인winningNumbers의matchNumber(userLottoNumbers, bonusNumber)메소드를 사용하여 일치 갯수와 보너스 볼 포함 여부를 반환받고 이를 Result클래스의 winningResult()에 넘겨준다.
    
    →이 메소드는 비즈니스 로직으로 넘겨주면 인자 3개를 전달해줘야할 것 같아서 남겨두었는데 비즈니스 로직으로 옮겨주겠습니다!
    

# Model

## Lotto

### 생성자

- 현 클래스의 로또 크기, 중복, int타입 유효성 검사를 하고 List<Integer> numbers에 저장, 유효성에서 에러 발생시 throw로 던져줌

### size_check(List<Integer> numbers)

- 로또 번호의 수가 6개가 아닐 시 에러 발생

### number_check(List<Integer> numbers)

- 로또 번호의 수가 1~45인지 체크

### duplication_check(List<Integer> numbers)

- 로또 번호가 중복되었는지 확인

### check_bonusNumber(int bonusNumber)

- 보너스 볼이 당첨 번호에 포함되어 있으면 에러 발생

### matchNumber(Lotto lottoNumbers, int bonusBall)

- 입력받은 lottoNumbers와 bonusBall이 당첨 번호와 일치하는 갯수 반환

### toString()

- 생성자에서 저장한 numbers 반환

## Result

- EnumMap<Winning, Integer>타입인 result 변수 new EnumMap<>(Winning.class)으로 초기화

### 생성자

- 반복문을 통해 Winning 클래스의 상수 갯수만큼 result에 (상수, 0) put

### winningResult(int match, int bonusBall)

- Winning클래스의 result(int matchNumber, int checkBonusBall)메소드를 통해 일치하는 수에 맞는 상수의 result.value를 +=1 해준다.

### printResult(int purchasePrice)

- totalPrice를 0으로 초기화
- Winning클래스의 values()를 반복문으로 반복한다
    - totalPrice += winning.getWinningPrice() * result.get(winning)으로 현재 반복되고 있는 winning 상수의 value값 * 당첨 등수에 따른 상금을 해준다.
    - 현 클래스의 printMatch(winning)메소드를 호출하여 당첨 통계를 출력한다.
- 총 수익률을 출력한다.

### printMatch(Winning winning)

- winning.getMatchNumber(), winning.getWinningPrice(), result.get(winning)를 사용하여 당첨 통계를 출력한다.

→ Result 클래스는 record를 사용하여 DTO 패키지에 넣어주고 위 메소드들은 (클래스 명을 ResultService?, LottoResult?) Domain단으로 옮겨주면 될 것 같습니다!

## Setting

- Lotto winningNumbers 선언

### getBonusNumber()

- bonusNumber를 while문으로 에러가 안날 때까지 반복하여 입력받음
- 현 클래스의 input_bonusNumber()을 통해 유효성 검사

### getWinningNumbers()

- winningNumber를 while문으로 에러가 안날 때까지 반복하여 입력받음
- Lotto생성자와 현 클래스의 stringToList(String input)를 통해 유효성 검사

### input_bonusNumber()

- inputToInt(input), checkNumber(bonusNumber),winningNumbers. check_bonusNumber(bonusNumber)를 통해 int 타입, 1~45의 범위, 당첨번호에 포함되는지에 대한 유효성 검사

### inputToInt(String input)

- input이 Integer.parseInt가 실행된다면 이를 반환

### checkNumber(int number)

- 수가 1~45인지 유효성 검사

### purchase_amount()

- inputToInt(input), check_money(input) 입력 금액에 대한 유효성 검사 후 입력금액 반환

### check_money(String money)

- 입력 금액이 1000의 배수인지 검사

### stringToList(String input)

- String을 “,”를 기준으로 잘라서 inputToInt()메소드로 유효성 검사를 해준 뒤 List<Integer>에 추가
- List를 반환

→ 우선 Setting 클래스의 역할은 당첨번호 입력받아 List로 변환, 보너스 번호 입력받기, 유효성 검사가 있습니다.

→ 입력을 받는 클래스와 변환, 유효성 검사를 구성하는 클래스를 분리하려고 합니다.

## UserLotto

### userLottosCreate(int amount)

- amount값만큼 사용자의 로또 번호를 생성하고 정렬한 뒤 반환

## Winning

- *FIFTH*(3, 5000, 0)
- *FOURTH*(4, 50000, 0)
- *THIRD*(5, 1500000, 0)
- *SECOND*(5, 30000000, 1)
- *FIRST*(6, 2000000000, 0)
- *No_Match*(0, 0, 0)
- private final int matchNumber
- private final int winningPrice
- private final int checkBonusBall

### 생성자

- matchNumber, winningPrice, checkBonusBall를 set

### getMatchNumber()

- matchNumber 반환

### getWinningPrice()

- winningPrice 반환

### result(int matchNumber, int checkBonusBall)

- 전달된 인자와 일치하는 상수를 반환

# View

## PrintInput

- public static final String *INPUT_MONEY* = "구입금액을 입력해 주세요."
- public static final String *WINNING_NUMBER* = "당첨 번호를 입력해 주세요."
- public static final String *BONUS_NUMBER* = "보너스 번호를 입력해 주세요.";

## PrintOutput

- public static final String *NUMBER_OF_LOTTO* = "개를 구매했습니다."
- public static final String *RESURLT_LOTTO* = "당첨 통계"

### printAmount(int lottoAmount)

- lottoAmount를 출력

### printUserLotto(List<Lotto> randomLottoNumbers)

- 사용자의 로또 번호들을 출력

→ Error 메시지들을 다 넣어둘까 생각중입니다.

# DTO, DAO, VO

## DAO

- 데이터베이스에 접근하여 조회, 삭제, 삽입하는 역할을 합니다.

## DTO

- 순수한 데이터 객체만 가진 클래스입니다.
- 로직을 가지지 않고 getter와 setter만 가지고 있습니다.
- 계층간 데이터 교환을 위한 자바 빈즈(JAVA Beans)를 의미합니다.
    - JAVA Beans : Java로 작성된 소프트웨어 컴포넌트를 지칭하는 단어, 비즈니스 로직 부분을 담당하는 Java 프로그램 단위

## VO

- 오직 getter만 사용하여 읽기만 가능합니다. Read-Only
- 단순히 값 타입을 표현하기 위해 불변 클래스를 만들어 사용합니다.

# 리팩토링

- 리팩토링할 내용들을 정리해보면 아래와 같습니다.
- stream의 적용은 글이 길어질 것 같아 따로 빼두었습니다.
    - [stream을 적용](https://github.com/alswp006/woowa-6th-precourse-memory/blob/main/3%EC%A3%BC%EC%B0%A8/Stream%20%EC%A0%95%EB%B3%B5%EA%B8%B0.md)한다.
    - [x]  Controller의 matchResult메소드를 비즈니스 로직으로 옮긴다.
        - 현재 matchResult메소드와 연관되어 있는 메소드
            - Lotto의 matchNumber : 당첨 번호와 일치하는 갯수 반환
            - PlayGame에서 결과를 구하기 위해 사용
        - 현재 matchResult가 연관되어 있는 것
            - 사용자 로또 번호를 List<Lotto>로 저장한 randomLottoNumbers
            - 당첨 번호를 List<Integer>로 저장한 winningNumbers
            - setting.getBonusNumber()을 통해 구한 bonusBall
            - result의 객체인 winningData
        
        → **enum클래스인 Winning 클래스의 필드에서 bonusBall을 체크하는 곳을 int타입에서 boolean타입으로 변경 후 for문으로 메소드 실행**
        
    
    - [x]  Result 클래스를 DTO로 넣어줍니다.(가능하다면 record를 사용)
        - 나머지 메소드들은 새로운 클래스를 만들어(클래스 명을 ResultService?, LottoResult?) Domain단으로 옮겨줄 생각입니다.
        - Result 클래스는 현재 EnumMap<Winning, Integer> result를 선언해주고 있습니다.
            - 그리고 생성자로 Winning 상수의 갯수만큼 result를 세팅해줍니다.
            - 메소드는 로또 번호가 일치하는 갯수에 따른 결과를 result에 넣어주는 winningResult메소드,
            - 수익률을 출력해주고 printMatch메소드를 호출해주는 printResult메소드,
            - 당첨 통계를 출력해주는 printMatch메소드가 있습니다.
        - 생성자와 winningResult메소드를 DTO인 Result에 넣어주고 나머지는 새로운 LottoResult라는 클래스를 만들어 넣어줄 생각입니다.
    - [x]  Setting 클래스를 입력을 받는 클래스와 변환, 유효성 검사를 구성하는 클래스를 분리합니다.
    - [x]  Error 메시지들을 다 묶어둡니다.(PrintOutput클래스나 ErrorMessage클래스를 따로 만들어 관리할 생각입니다.)
