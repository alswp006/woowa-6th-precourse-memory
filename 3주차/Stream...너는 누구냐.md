# Stream이란

- 기존 컬렉션 데이터의 처리는 for나 foreach문을 사용하며 요소들을 하나씩 다뤘습니다. 이러한 방법은 복잡한 처리가 필요하거나 컬렉션의 크기가 커지면 성능 저하를 일으키게 되므로 이를 해결할 수 있는 것이 Stream입니다.
- Stream을 사용하면 복잡한 loop문을 사용하지 않아도 되며 중첩 loop문을 사용해야하는 안좋은 경우도 없어집니다. 또한 병렬처리를 별도의 멀티 스레드를 구현하지 않아도 쉽게 구현할 수 있습니다.
- 

# 사용 방법

## 배열 스트림

- Stream을 사용하기 위해서는 먼저 생성을 해야 합니다.

```java
String[] str = new String[]{"1","2","3"};
Stream<String> stream = Arrays.stream(str,1,3);
```

## 컬렉션 스트림

- 컬렉션 타입도 Stream을 만들 수 있습니다.

```java
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream = list.stream();
```

## Stream.generator()

```java
Stream<String> stream = Stream.generate(() -> "kim").limit(5);
stream.forEach(s -> System.out.print(s.toString() + "\t"));
//출력
kim	kim	kim	kim	kim
```

## Stream.iterator()

```java
Stream<Integer> stream = Stream.iterate(30, n -> n+2).limit(5);
stream.forEach(s -> System.out.print(s.toString() + "\t"));
//출력
30	32	34	36	38
```

- 첫 번째 요소는 2가 더해지지 않고 30 그대로 출력되는 것을 확인할 수 있습니다.

## Stream.filter()

- Stream 각 요소에 대해 조건식을 실행하고 조건에 맞는 Stream이 반환됩니다.

```java
List<String> name = Arrays.asList("kim", "min", "je");
Stream<String> stream = name.stream().filter(s -> s.contains("m"));
stream.forEach(s -> System.out.print(s.toString() + "\t"));
//출력
kim	min
```

## Stream.Map()

- Stream에 있는 요소 값들이 특정 로직을 거친 후 반환됩니다.

```java
List<String> name = Arrays.asList("kim", "min", "je");
Stream<String> stream = name.stream().map(s -> s.toUpperCase());
stream.forEach(s -> System.out.print(s.toString() + "\t"));
//출력
KIM	MIN	JE
```

## Sorting

- 수열을 받아서 정렬한 후 출력할 수 있습니다.

```java
List<Integer> name = IntStream.of(5, 12, 42, 31, 92, 1)
        .sorted()
        .boxed()
        .toList();
System.out.println(name);
//출력
[1, 5, 12, 31, 42, 92]
```

- 리스트를 받아서 역순으로 정렬할 수도 있습니다.

```java
List<String> name = Arrays.asList("kim", "min", "je", "java", "python", "javaScript");
name = name.stream()
        .sorted(Comparator.reverseOrder())
        .collect(Collectors.toList());
System.out.println(name);
//출력
[python, min, kim, je, javaScript, java]
```

## Calculator

- stream을 통해 여러가지 연산들도 할 수 있습니다.

```java
List<Integer> name = Arrays.asList(1,2,3,4,5,6,7);
System.out.println(name.stream().count());
//출력
7
```

- 또한 filter를 통해 조건을 줄 수도 있습니다.

```java
List<Integer> name = Arrays.asList(1,2,3,4,5,6,7);
System.out.println(name.stream().filter(s -> s == 2).count());
//출력
1
```

- 아래와 같이 다양한 기능들을 사용할 수 있습니다.

```java
System.out.println(IntStream.of(1,2,3,4,5).sum());
System.out.println(IntStream.of(1,2,3,4,5).max());
System.out.println(IntStream.of(1,2,3,4,5).min());
System.out.println(IntStream.of(1,2,3,4,5).filter(s -> s > 2).sum());
//출력
15
OptionalInt[5]
OptionalInt[1]
12
```

- stream이 빈 경우 sum과 count같은 경우는 0을 출력하면 되지만 max나 min은 0을 출력하면 안되기 때문에 OptionalInt를 출력합니다.
- stream이 빈 경우를 알아보겠습니다.

```java
System.out.println(IntStream.of().sum());
System.out.println(IntStream.of().max());
System.out.println(IntStream.of().min());
System.out.println(IntStream.of().filter(s -> s > 2).sum());
//출력
0
OptionalInt.empty
OptionalInt.empty
0
```

## ::

- 왼쪽 클래스 또는 객체의 오른쪽 메소드를 사용한다는 뜻입니다.
- 람다 표현식의 축약형으로 사용됩니다.
- 아래와 같이 사용될 수 있습니다.
    - **`Integer::parseInt`**
    - **`System.out::println`**
    - **`String::length`**
    - `**numbers::contains**`
        - List인 numbers의 contains 메소드를 사용합니다.

## findAny(), findFirst()

- findAny()는 스트림의 요소 중에서 임의의 요소를 반환합니다. 병렬 스트림에서는 첫 번째 요소를 찾는다는 보장이 없기 때문에 다른 요소를 반환하는 경우가 종종 있습니다.
- findFirst()는 스트림의 첫 번째 요소를 반환합니다. 순차적인 처리가 보장되며, 병렬 스트림에서도 항상 스트림의 첫 번째 요소를 반환합니다.
- 둘 다 반환값은 Optional이며, 스트림이 비어있는 경우에는 Optional.empty가 반환됩니다.
- 저는 이번 로또 미션에서 findAny를 아래와 같이 사용해줬습니다.

<img width="706" alt="image" src="https://github.com/alswp006/woowa-6th-precourse-memory/assets/102672547/0ae23cca-ee63-4d6d-ac9f-00024b0c0610">

- winning의 values()를 받아서 filter()에서 조건에 만족하는 요소를 반환하고 없다면 NO_MATCH를 반환합니다.
- 현재 기능에서는 요소의 순서가 중요하지 않기 때문에 findAny를 사용했고 findAny옆에 뜨는 메시지를 보면 Optional타입으로 반환되는 것을 확인할 수 있습니다.

## orElse()

- 조건에 맞는 Optional객체의 값이 없다면 실행되는 메소드입니다!
