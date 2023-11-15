- 원래 이 과정은 프로그램 구현의 고민들이라는 페이지에 있었는데 내용이 길어져 따로 옮겼습니다!

# Discount 클래스 리팩토링하기

## Discount

- 현재 Discount 클래스에는 사용자에게 적용되는 할인 내용을 저장해놓은 List를 반환해주는 getter 메소드가 있습니다.
    
    ```java
    public List<String> getDiscountDetails() {
            return discountDetails;
        }
    ```
    
- 이를 어떻게 없애고 기능을 구현한 메소드로 반환할까라는 고민을 해보았습니다!
- 현재 저의 프로그램은 할인 내역을 추가하는 메소드가 총 두 개 있습니다.
    - 첫 번째는 금액에 대한 할인을 적용해주는 메소드
    - 두 번째는 증정품에 대한 혜택을 적용해주는 메소드
- 이 두 가지는 현재 Discount 클래스에서 적용되는 할인 또는 혜택 내용을 List<String>에 저장해주고 있습니다.
- 그래서 제 생각은 증정품에 대한 혜택을 적용해주는 메소드가 현재 혜택 내역에서 마지막으로 적용되는 로직이니 여기서 List를 반환해주면 getter를 사용하지 않아도 될 것 같다! 라는 생각을 가지고 코드를 수정하였습니다.

### 수정 전

```java
public void champagneFree(int totalPrice) {
        String benefitMessage = "증정 이벤트: -25,000원";
        int applyBenefitPrice = 120000;

        if (totalPrice >= applyBenefitPrice) {
            discountDetails.add(benefitMessage);
        }
    }
```

### 수정 후

```java
public List<String> apliedAllEvent(int totalPrice) {
        String benefitMessage = "증정 이벤트: -25,000원";
        int applyBenefitPrice = 120000;

        if (totalPrice >= applyBenefitPrice) {
            discountDetails.add(benefitMessage);
        }

        return discountDetails;
    }
```

- 수정을 한 후 저는 문득 이런 생각이 들었습니다.
- “이렇게 되면 appliedAllEvent는 증정 이벤트를 적용하는 기능과 discountDetails를 반환하는 기능 두 가지를 가지게 되는 것인가? 아니면 discountDetails를 반환하는 기능은 getter의 역할이지만 이를 다른 메소드에 넘겨줌으로 getter 메소드가 사라졌으니 좋은건가..?
- 이를 고민하다가 제 머리를 관통하는 하나의 생각이 들었습니다. “아..일급 컬렉션 잘 쓰면 될거같은데..쓰읍..하…” 이를 해결하기 위한 방법으로 Discount의 클래스를 바꿔보려 합니다. 현재의 Discount클래스는 아래와 같이 구성되어 있습니다.

```java
public class Discount {
    private DiscountStrategy discountStrategy;
    private final List<String> discountDetails = new ArrayList<>();

		//...
    public void setDiscountStrategy(DiscountStrategy discountStrategy) {
        this.discountStrategy = discountStrategy;
    }

    public int applyDiscounts(Map<String, Integer> menus, int date) {
        int totalDiscount = 0;
        int discountAmount = discountStrategy.applyDiscount(menus, date);

        if (discountStrategy != null && discountAmount != 0) {
            String discountAmountStr = String.format(": -%,d원", discountAmount);
            discountDetails.add(discountStrategy.getDiscountName() + discountAmountStr);
            totalDiscount += discountAmount;
        }

        return totalDiscount;
    }

    public List<String> apliedAllEvent(int totalPrice) {
        String benefitMessage = "증정 이벤트: -25,000원";
        int applyBenefitPrice = 120000;

        if (totalPrice >= applyBenefitPrice) {
            discountDetails.add(benefitMessage);
        }

        return discountDetails;
    }
		//...

}
```

- 만약 값을 반환해주는 것까지 기능으로 보면 applyDiscounts는 discountDetails에 할인 내역을 추가해주는 기능을 하며 totalDiscount까지 계산하여 반환해주는 기능을 하고 있었습니다. 저는 처음에는 이를 “discountDetails와 totalDiscount를 계산하는 것을 한 번에 해주니 일타이피다..!”라고 생각하였으나 객체 지향적으로 생각해봤을 때 메소드는 한개의 기능만 담당해야하니 보다보니 좋은 기능이 아닌 것 같다라는 생각을 하게 되었습니다..
- 그래서 아래와 같이 메소드들을 구성해보기로 했습니다.
    - 할인 내역을 discountDetails에 넣어주는 메소드
    - 증정 내역을 discountDetails에 넣어주는 메소드
    - 위의 두 메소드를 실행하고 discountDetails를 반환해주는 메소드
- 위와 같은 방법은 Lotto에서 일급 컬렉션을 구현할 때 사용했던 방법인데 이 때까지 생각하지 못했던 제 머리에게 안타까움을 느끼며 한번 구현해보려 합니다!

## 메소드 분리하기

- 우선 현재 discountDetails에 할인 내역을 넣어주는 기능과 totalDiscount 금액을 계산하는 기능을 분리해줍니다. discountDetails() 메소드는 void로 두고 totalDiscount는 반환값을 int로 두어 총 할인 값을 계산하게 합니다.
- 하지만 여기서 문제가 발생합니다. 원래의 이 메소드는 컨트롤러 부분에서 Stream을 통한 반복을 사용하여 메소드를 여러 번 실행시켰는데 둘을 분리하여 discountDetails() 메소드와 증정 내역을 넣어주는 메소드를 함께 실행시키려면 다른 방법을 모색해야 합니다!
    - 원래의 controller에서 discountDetails()를 사용하던 방법
    
    ```java
    private int discountProcess(int totalPrice){
            int minimumPrice = 10000;
    
            if (totalPrice < minimumPrice) {
                return 0;
            }
    
            List<DiscountStrategy> applicableStrategies = discount.getApplicableStrategies(date);
    
            return applicableStrategies.stream()
                    .mapToInt(strategy -> {
                        discount.setDiscountStrategy(strategy);
                        return discount.applyDiscounts(orderMenu, date);
                    })
                    .sum();
        }
    ```
    
- 그래서 저는 그냥 discountDetails() 메소드 안에서 적용할 수 있는 모든 할인을 반복하여 할인 금액이 0이 아니라면 바로 discountDetails 리스트에 넣어주는 것입니다!
    - 마침 적용할 수 있는 할인을 담아놓는 메소드도 사용하던 중이어서 이를 활용하여 사용하기로 했습니다.
    - 적용 가능 할인을 LIst<DiscountStrategy>에 담는 메소드
    
    ```java
    public List<DiscountStrategy> getApplicableStrategies(int date) {
            return strategies.stream()
                    .filter(strategy -> strategy.isApplicable(date))
                    .collect(Collectors.toList());
        }
    ```
    
- totalDiscount 금액을 구하는 메소드에서도 이를 똑같이 적용하여 사용하였습니다.

### 메소드 분리 결과

- 이를 적용한 결과 메소드들은 아래와 같이 수정되었습니다.

```java
private void applyDiscounts(Map<String, Integer> menus, int date) {

        for (DiscountStrategy strategy : getApplicableStrategies(date)) {
            int discountAmount = strategy.applyDiscount(menus, date);

            if (discountAmount != 0) {
                String discountAmountStr = String.format(": -%,d원", discountAmount);
                discountDetails.add(strategy.getDiscountName() + discountAmountStr);
            }
        }

    }

    public int totalDiscount(Map<String, Integer> menus, int date){
        int totalDiscountPrice = 0;

        for (DiscountStrategy strategy : getApplicableStrategies(date)){
            int discountAmount = strategy.applyDiscount(menus, date);
            
            totalDiscountPrice += discountAmount;
        }

        return totalDiscountPrice;
    }
```

- 이렇게 구현함으로써 원래 discountProcess에서 사용하기 위해 만들어졌던 setter메소드도 사라지게 되고 메소드의 기능도 분리되었습니다.
- 이제 applyDiscounts와 원래 사용하던 샴페인 증정 여부를 체크하는 메소드를 묶어서 실행시키는 메소드와 Controller에서 이들을 사용할 코드만 변경하면 됩니다!!
- 원래의 의도인 할인 내역을 discountDetails에 넣어주는 메소드, 증정 내역을 discountDetails에 넣어주는 메소드를 묶어서 실행시키는 메소드를 만들면 될 것 같습니다.
    
    ```java
    public List<String> discountDetails(Map<String, Integer> menus, int date, int totalPrice){
            applyDiscounts(menus, date);
            applyBenefitEvevnt(totalPrice);
    
            return discountDetails;
    ```
    
- 그리고 이를 사용한 discountProcess는 이러합니다.
    
    ```java
    private int discountProcess(int totalPrice){
            int minimumPrice = 10000;
    
            if (totalPrice < minimumPrice) {
                return 0;
            }
    
            int totalDiscount = discount.totalDiscount(orderMenu, date);
        }
    }
    //...
    List<String> discountDetails = discount.discountDetails(orderMenu, date, totalPrice);
    //...
    ```
    
- 위와 비교하면 코드가 많이 줄어든 것을 확인할 수 있습니다.
- 하지만 이렇게 두면 평일에 디저트를 안시키는 경우나 주말에 메인 메뉴를 안시키는 경우 아래와 같은 출력을 보일 수 있습니다.
    
    ```java
    평일 할인: -0원
    
    or
     
    주말 할인: -0원
    ```
    
- 그래서 이 예외 처리는 아주 편리하게 discountDetails에서 하기로 했습니다.
- 이 예외 처리를 해주고 discountProcess에서 totalPrice가 10000원 보다 작으면 0을 반환하는 조건은 이제 필요없으니 저것도 제거하면 discountProcess()메소드를 소거할 수 있습니다!!

## 최종 결과

### Discount

```java
public class Discount {
    private final List<String> discountDetails = new ArrayList<>();

    //...
    public List<String> discountDetails(Map<String, Integer> menus, int date, int totalPrice){
        if (totalPrice >= 10000){
            applyDiscounts(menus, date);
            applyBenefitEvevnt(totalPrice);
        }

        return discountDetails;
    }
    private void applyDiscounts(Map<String, Integer> menus, int date) {

        for (DiscountStrategy strategy : getApplicableStrategies(date)) {
            int discountAmount = strategy.applyDiscount(menus, date);

            if (discountAmount != 0) {
                String discountAmountStr = String.format(": -%,d원", discountAmount);
                discountDetails.add(strategy.getDiscountName() + discountAmountStr);
            }
        }

    }

    public int totalDiscount(Map<String, Integer> menus, int date){
        int totalDiscountPrice = 0;

        for (DiscountStrategy strategy : getApplicableStrategies(date)){
            int discountAmount = strategy.applyDiscount(menus, date);

            totalDiscountPrice += discountAmount;
        }

        return totalDiscountPrice;
    }

    private void applyBenefitEvevnt(int totalPrice) {
        String benefitMessage = "증정 이벤트: -25,000원";
        int applyBenefitPrice = 120000;

        if (totalPrice >= applyBenefitPrice) {
            discountDetails.add(benefitMessage);
        }
    }

    private List<DiscountStrategy> getApplicableStrategies(int date) {
        return strategies.stream()
                .filter(strategy -> strategy.isApplicable(date))
                .collect(Collectors.toList());
    }
}
```

- setter와 getter가 없고 메소드도 한 가지의 기능만 하는 클래스가 잘 구현되었다고 생각합니다!!!

## 회고

우선 글을 적을 때는 문제 상황에 대해 술술 풀어간 것처럼 적어놨지만 사실 정말 오래 고민하고 오류를 겪으며 진행한 과정입니다… 이 쪽 코드에서 문제를 해결하면 출력이 다르게 나오고 오류가 나고 어떤 부분은 수정하다가 더 최적화할 방법이 생각나서 적용해보면 뭔가 한 개가 안맞고… 꽤나 고난의 연속이었다고 생각합니다.😢 (객체 지향 프로그래밍의 중요성을 더 꺠달아버린…)
하지만 이번 최적화 과정에서 정말 많은 고민을 하며 메소드를 분리하고 getter를 없앤 것은 물론. 어쩌다보니 setter를 소거할 수 있게 되고 Controller클래스의 discountProcess() 메소드를 소거할 수 있게 되니 정말 소름이 끼칠 정도로 재밌었던 것 같습니다.
이번 최적화 과정은 다 끝내고 난 뒤 엄청난 쾌감과 성취감을 느낀 과정이었던 것 같습니다. 정말 좋은 기분이었고 이런 기분을 꼭 다시 느껴보기 위해 노력할 것입니다…!!🔥👊
