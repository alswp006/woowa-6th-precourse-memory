# 테스트 코드에서의 객체의 static과 non-static 생성의 차이
- 저는 우테코 최종 코딩 테스트에서 평일 비상 근무자와 주말 비상 근무자들을 저장하는 클래스를 싱글턴 패턴으로 만들어 주었습니다.
    
    ```dart
    public class DayOffCrew implements Crew{
        List<String> dayOffCrew = new ArrayList<>();
        private static final DayOffCrew DAY_OFF_CREW = new DayOffCrew();
    
        private DayOffCrew(){}
    
        public static DayOffCrew getInstance(){
            return DAY_OFF_CREW;
        }
    ...
    }
    
    public class WeekDayCrew implements Crew{
        private List<String> weekDayCrews = new ArrayList<>();
        private static final WeekDayCrew WEEK_DAY_CREW = new WeekDayCrew();
        private WeekDayCrew(){}
    
        public static WeekDayCrew getInstance(){
            return WEEK_DAY_CREW;
        }
    ...
    }
    ```
    
- 하지만 이렇게 사용했을 때 테스트 코드가 각각은 실행되지만 2개가 같이 실행되지는 않는 문제가 발생하였습니다.
    - 2개의 테스트 코드가 있을 때 잘 작동하지 않는 모습
    <img width="668" alt="image" src="https://github.com/alswp006/woowa-6th-precourse-memory/assets/102672547/f670d3ba-64b6-444d-a5cd-f6ab11115ef1">
    
    - 테스트 코드를 따로 실행하면 잘 작동하는 모습
    <img width="668" alt="image" src="https://github.com/alswp006/woowa-6th-precourse-memory/assets/102672547/9724cbab-a31f-43a1-88c9-4968fa29d5fa">
    
    - 위와 같은 문제가 발생하였습니다,
- 여러 개의 테스트 케이스를 한 번에 돌릴 때 static으로 객체를 생성하면 그 안에 상태를 가진 값들은 매번 생성되지 않고 한 번만 생성되어 저장된 값을 계속 사용합니다.

  <img width="668" alt="image" src="https://github.com/alswp006/woowa-6th-precourse-memory/assets/102672547/98e2231e-9b1f-49c8-abec-5befaf5e47b9">
    
- 그래서 위 결과를 보면 첫 번째 테스트 케이스가 폴로에서 끊기고 그 다음 주말 비상 근무자인 스캇과 홍고가 다음 테스트 케이스에서 시작 멤버로 들어간 것을 확인해볼 수 있습니다.
- 싱글턴 패턴을 사용하지 않고 객체를 Controller에서 매번 만들어 사용하니 테스트 케이스가 잘 작동하는 것을 확인할 수 있습니다.
    <img width="668" alt="image" src="https://github.com/alswp006/woowa-6th-precourse-memory/assets/102672547/752a4b73-d0bd-42b6-bda4-2ff310403004">
    
- 싱글턴 패턴을 사용하는 것도 좋지만 이런 상태를 저장하는 객체의 경우에는 잘 생각하여 코드를 구현해야할 것 같습니다.
- 결론 : static과 non-static의 특성을 잘 이해하자! 테스트 코드라고 각각 프로그램이 실행되는건 아니다!
