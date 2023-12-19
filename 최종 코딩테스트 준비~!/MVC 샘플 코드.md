### OutputView

```java
public enum OutputView {
    INSTANCE;

    public static OutputView getInstance(){
        return INSTANCE;
    }

		public void printList(List<String> list){
        list.forEach(System.out::println);
    }
}
```

### InputView

```java
public enum InputView {
	    INSTANCE;

    public static InputView getInstance(){
        return INSTANCE;
    }

		public List<Integer> stringToList(){
        String input = Console.readLine();

        return Arrays.stream(input.split(",")).
                map(Integer::parseInt).
                toList();
    }
}
```

### Controller

```java
public class Controller {
    private final InputView inputView = InputView.getInstance();;
    private final OutputView outputView = OutputView.getInstance();

    public void play() {
    }
}
```

### Validator → Util

```java
public abstract class Validator {
    private final String ERROR_HEAD = "[ERROR]" ;

    protected void throwError(String errorMessage){
        System.out.println(ERROR_HEAD + errorMessage);
        throw new IllegalArgumentException();
    }

    public abstract void validate(String input);
}
```

### EachValidator

```java
public class MenuValidator extends Validator{
    private static final MenuValidator MENU_VALIDATOR = new MenuValidator();

    private MenuValidator() {
    }
    public static MenuValidator getInstance(){return MENU_VALIDATOR;}

    @Override
    public void validate(String input) {
    }
}
```

### Util

```java
public class ErrorMessage {
    private static final String ERROR_HEAD = "[ERROR]" ;

    public static void throwError(String errorMessage){
        System.out.println(ERROR_HEAD + errorMessage);
        throw new IllegalArgumentException();
    }
}
```

## 여러 옵션 선택해야할 때

### Controller

```java
public class Controller {
    private final InputView inputView = InputView.getInstance();
    Map<String, Controllable> controllers = new HashMap<>();
    List<PairRepo> pairRepos = new ArrayList<>();

    public Controller(){
        controllers.put("1", new MatchingController());
        controllers.put("2", new InquiryController());
        controllers.put("3", new InitController());
        controllers.put("Q", new EndControllable());
    }

    public void run() throws IOException {
        String menu = "0";
        while (!menu.equals("Q")){
            menu = inputView.inputMenu();
            service(menu);
        }
    }

    public void service(String input) throws IOException {
        Controllable controllable = controllers.get(input);
        controllable.process(pairRepos);
    }
}
```

### Controllable

```java
public interface Controllable {
    void process(List<PairRepo> pairRepos);
}
```

## 클래스 갈아끼우기

```java
public class Crew {
    Course course;
    String level;
    String mission;
    List<String> crews;

    Crew(Course course, String level, String mission, List<String> crews) {
        this.course = course;
        this.level = level;
        this.mission = mission;
        this.crews = crews;
    }
}
```

```java
public class BackEndCrew extends Crew {
    public BackEndCrew(String level, String mission){
        super(Course.BACKEND, level, mission, backEndCrews());
    }
}
```
