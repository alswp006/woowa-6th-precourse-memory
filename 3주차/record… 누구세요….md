- 저는 다른 분들의 저번 2주차 미션 자동차 경주 코드를 보다가 record라는 것을 발견했습니다.
- 이는 JDK 16에서 정식으로 포함되었기 때문에 잘 사용한다면 JDK 17을 사용하는 프리코스 환경에서도 아주 유용하게 쓸 수  있겠다!!라는 생각이 들어 공부해보고 적용해보려 합니다!

# record란?

- Java 16에서 정식 기능이 된 데이터를 표현하기 위한 새로운 형태의 타입입니다.
- Record는 필드에 대한 getter(accessor), equals(), hashCode(), toString() 메소드를 자동으로 생성해줍니다
- Record는 단순히 데이터를 표현하기 위한 용도로 사용됩니다.
- Record는 final 클래스이기 때문에 다른 클래스나 레코드가 상속할 수 없습니다.

# 코드부터 알아봅시다!

- ~~프로그래머는 코드로 말하는 법…!~~
- 제가 이것저것 찾아본 결과 우선 코드를 보면 왜 record를 쓰는지 확실히 이해할 수 있을 것 같아서 정리해보겠습니다.

## 일반적인 코드

```java
public class Datas{
		private final Long data;
		
		public Datas(final Long data){
				this.data = data;
		}

		public Long getData{
				return data;
		}
```

- 위와 같은 일반적인 데이터를 저장하는 클래스가 있습니다.
- 여기에 record를 적용하면 아래와 같이 됩니다.

## record 적용

```java
public record Datas(Long data){}
```

- ??? 뭔가 엄청 짧아졌죠..
- record는 기능 상의 이점이 있다기보다 코드를 줄여주는 용도가 강하다고 합니다!
- Entity 또는 dto와 같이 데이터를 묶어두는 용도로 사용하는 클래스에 사용하기 좋을 것 같습니다!
- 비슷한 기능으로는 외부 라이브러리인 Lombok이 있습니다.

## Lombok 적용

```java
@AllArgsConstructor
@Getter
public class Datas{
		private final Long data;
}
```

- Lombok은 외부 라이브러리로 getter, setter, toString 등의 메서드 작성 코드를 줄여주는 코드 다이어트 라이브러리입니다.
    - Lombok을 사용하려면 dependency를 추가해주고 class에서 import를 하여 사용할 수 있습니다.

## record의 장점

- 우선 코드 길이의 이점이 상당한게 눈에 보입니다!
    - 필드의 수가 많아질수록 더 효율적이겠네요!
- 비슷한 기능을 제공하는 Lombok은 번거로운 과정이 있고 외부 라이브러리이지만 record는 내부 기능이라서 바로 사용할 수 있고 코드도 더 짧은 것이 특징입니다!
    - 내부 라이브러리인만큼 속도에도 장점이 있을까 궁금하네요..
- equals, hashcode, toString도 자동으로 오버라이드해줍니다.

- 사실 EnumMap을 담는 DTO의 Result 클래스에 적용해보고 싶었으나 Result의 메소드가 EnumMap의 값을 계속 바꿔줘야해서 적용해보지 못했습니다...
