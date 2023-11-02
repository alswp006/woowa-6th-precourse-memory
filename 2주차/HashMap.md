- 저는 이번 프리코스 2주차 미션 자동차 경주 게임에서 원래는 Car라는 클래스를 만들어 자동차 객체를 리스트로 생성하여 데이터를 입력받았었습니다.
- 하지만 Car클래스를 통해 객체를 생성하면 getter와 setter를 사용하게 되고 이는 객체지향 생활체조 원칙에 위배되기 때문에 이를 해결하고자 데이터를 Map형태로 받으려 합니다.
- 제가 지금 생각하고 있는 것은 Map<String,int>형태로 받는 것입니다.
- 그러면 Map을 먼저 공부해보겠습니다.

# Map

- API에서는 “키를 값에 매핑하는 객체입니다. 맵에는 중복 키가 포함될 수 없습니다. 각 키는 최대 하나의 값에 매핑될 수 있습니다.”라고 합니다.
- Map은 리스트나 배열처럼 인덱스로 요소 값을 구하는 것이 아닌 key를 통해 value값을 얻는 자료형입니다.
- key는 고유한 값이기 때문에 중복을 허용하지 않고 value는 중복을 허용합니다.
- Map은 인터페이스이며 Map 인터페이스를 구현한 자료형으로는 HashMap, LinkHashMap, TreeMap 등이 있습니다.
- [Map 공식 API 문서](https://www.notion.so/HashMap-3a3fef3c380a4ecdb4887b126a910b56?pvs=21)
- 그러면 제가 사용해보려 하는 HashMap에 대하여 알아보겠습니다.

# HashMap

- HashMap은 위에서도 설명했듯 Map 인터페이스를 구현한 Collction입니다.
- HashMap도 Map 인터페이스를 구현하고 있기 때문에 <key, value> 구조를 가집니다.
- HashMap은 해싱(Hashing)을 사용하기 때문에 많은 양의 데이터를 검색하는데 있어서 뛰어난 성능을 가집니다.
    - 해싱은 해시 함수에 문자열 입력값을 넣어서 특정한 값으로 추출하는 것을 의미합니다.
- 또한 공식 API 문서에서는 “이 구현은 모든 선택적 맵 작업을 제공하고 `null`값과 `null`키를 허용합니다”, “이 클래스는 맵의 순서를 보장하지 않습니다. 특히, 시간이 지나도 순서가 일정하게 유지된다는 보장은 없습니다.” 라고 합니다.
- 그럼 HashMap의 사용법을 알아보겠습니다.

## HashMap 사용법

### HashMap 선언

```java
HashMap<String, Integer> hashMap = new HashMap<String, Integer>();
```

### 데이터 추가

```java
HashMap<String, Integer> hashMap = new HashMap<String, Integer>();
hashMap.put("kim", 1);
hashMap.put("kim", 2); 
hashMap.put("min", 3);
hashMap.put("je", 4);
```

- 위와 같이 key값이 중복된 경우 새로 입력된 value 값으로 갱신이 됩니다.

### 데이터 삭제

```java
hashMap.remove("kim");
hashMap.clear();
```

- hashMap.remove(key) : key에 해당하는 데이터를 삭제합니다.
- hashMap.clear() : hashMap의 데이터를 모두 삭제합니다.

### 데이터 추출

```java
hashMap // hashMap 전체
hashMap.keySet() //hashMap의 모든 key
hashMap.values() //hashMap의 모든 value
hashMap.get(key) //해당 key에 해당하는 value값
hashMap.entrySet() //hashMap에 key와 value값이 결합된 데이터를 Set으로 반환
```

### enhanced for문 응용

```java
for (String key : hashMap.keySet())
for (int value : hashMap.values())
for (Entry<String, Integer> item : hashMap.entrySet())
```

- HashMap은 잘 사용하면 속도적인 측면에서 Array나 List보다 유용하게 사용될 수도 있을 것 같습니다.
- [HashMap 공식 API 문서](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html)

## LinkHashMap

- 여기서 끝인줄 알았으나 제가 한가지 간과한 부분이 있었습니다…
- 그것은 바로 HashMap은 순서를 보장해주지 않는다는 점..
- HashMap을 구현하여 미션에서 자동차의 이름과 이동횟수의 데이터를 저장해주었으나 순서가 입력한 순서대로 나오지 않는 문제를 겪었고 이를 해결하기 위해 LinkHashMap을 공부하여 사용하려고 합니다..!
- LinkHashMap은 Map에 입력된 순서를 기억하는 자료구조입니다.
- 공식 API의 설명으로는 “Hash table and linked list implementation of the `Map` interface, with well-defined encounter order”, “잘 정의된 만남 순서를 사용하여 Map 인터페이스의 해시 테이블 및 연결 목록 구현”이라고 합니다.
    - 즉, HashTable과 LinkedList를 사용하여 만들어진 자료구조인 것 같습니다.
- 기본적인 사용법은 HashMap과 동일하고 순서가 있다는 점만 다른 것 같습니다.
- 그럼 이를 다시 적용하여 2주차 미션인 자동차 경주 게임에 적용해보겠습니다!
