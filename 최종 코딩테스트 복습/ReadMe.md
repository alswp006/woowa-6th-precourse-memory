## 생각해볼 점
- 크루 클래스를 추상 부모 클래스로 만들고 WeekDayCrew와 DayOffCrew를 클래스로 싱글턴 자식클래스로 두어 객체를 바꿔가며 사용하는 방법
    - Crew : WeekDayCrew DayOffCrew 추상 부모 클래스 또는 인터페이스
    - WeekDayCrew : WeekDayCrew의 데이터를 저장하고 크루를 반환하며 회전
    - DayOffCrew : DayOffCrew의 데이터를 저장하고 크루를 반환하며 회전
    - CrewRepo : Crew의 객체를 만들고 WeekDayCrew, DayOffCrew 번갈아가며 사용하여 크루들의 데이터 저장
- 테스트 코드를 쉽게 만들 수 있을만한 방법
- 여러 디자인 패턴 생각해보기
- 효율적인 크루 데이터 저장 방법 생각해보기
- 효율적인 날짜들의 저장 방법 생각해보기

[👉복습 브랜치](https://github.com/alswp006/java-oncall-6-alswp006/tree/practice)
