## 먼저!!!

> 1. 깃허브: **Fork**하기 => **Clone**하기 (깃허브 desktop) => **SolvingDot 브랜치** 만들기 => **_initial commit 하고 확인_** 꼭!!! (Pull
     Request 보내지는지도!!)
> - 꼭!!!!! Application, ApplicationTest 둘 다 돌아가는지 부터 확인!!!
> 2. 설계에 공들이자, 계속 고치는 대공사보다 설계에 공들이는게 더 빠르다
> 3. 라이브러리는 알려준 고대로 쓰자! List<String>을 shuffle하라 했으면 그모양 그대로
> 4. 너무 강박적으로 하지 말자!!!

## 목차

- [SPEEDY](#speedy)
- [Controller 로직](#로직-controller)
- [Enum 관리](#enum)
- [Decimal Format](#Decimal Format)
- [재입력 로직](#재입력 로직)
- [FileReader 사용법](#file-reader)
- [검증로직들](#검증로직들)
- [깃허브](#깃허브)

## speedy

### START

### 기능 목록을 작성할 때

- 복잡한 문제라면 **플로우차트**를 그린다
- 어떤 클래스가 어떤 데이터를 담아야 하는지 **코드 짜기 _전에_ 생각한다!!!**
- 가능하면 출력해야 하는 내용을 **`view`에 복붙**하면서 작성하자!!! => 집착은 X

### 기능목록 형식 (복붙해서 수정하기)

```
# 프로젝트: 12월 이벤트 플래너

## 프로젝트 소개
- 12월 이벤트 플래너를 구현하는 프로젝트입니다.

## 12월 이벤트 플래너 설명

### 플래너 사용 절차 요약
1. 사용자는 예상 방문 날짜를 입력한다.
2. 사용자는 주문할 메뉴와 개수를 입력한다.
3. 프로그램은 날짜, 메뉴, 메뉴 개수를 토대로 적용되는 이벤트 혜택을 다음과 같이 보여준다.
  - 주문한 메뉴를 보여준다.
  - 할인 전 총 주문 금액을 보여준다.
  - 증정 메뉴를 보여준다.
  - 이벤트 혜택 내역을 보여준다.
  - 총 혜택 금액을 보여준다.
  - 할인 적용 후 예상 결제 금액을 보여준다.
  - 12월 이벤트 배지를 보여준다.

## 기능 목록

### 핵심 기능

- 한줄 요약: 사용자가 날짜와 메뉴를 입력하면 이벤트 혜택 적용 결과를 알려준다.

## 기능 목록

### 핵심 기능

- 한줄 요약: 한 주 동안의 점심 메뉴를 중복되지 않게 추천한다.

#### 코치 이름(입력1)
- [x] 빈값일 때 예외 발생.
- [x] 코치 이름은 쉼표로 구분한다.
  - [x] 구분했을 때 빈값이 있으면 예외 발생.
  - [x] 구분했을 때 코치 이름이 2명 미만, 5명 초과일 경우 예외 발생.
  - [x] 코치 이름에 빈칸(띄어쓰기)이 있을 경우 예외 발생.
  - [x] 코치 이름이 2글자 미만, 4글자 초과일 경우 예외 발생.

#### 못 먹는 메뉴(입력2)
- [x] 각 코치는 최소 0개, 최대 2개의 못 먹는 메뉴가 있다. (`,` 로 구분해서 입력한다.)
    - [x] 먹지 못하는 메뉴가 없으면 빈 값을 입력한다.
    - [x] 3개 이상을 입력했을 때 예외 발생.
- [x] 메뉴에 없는 입력은 예외 발생.

#### 메뉴 카테고리

- 카테고리를 무작위로 정한다.
  - 1에서 5까지의 무작위 숫자를 생성한다.
  - 요일별로 반복한다.
  - 3회 이상 중복이면 다시 임의의 숫자를 생성한다.
  - 무작위 값이 1이면 일식, 2면 한식, 3이면 중식, 4면 아시안, 5면 양식이다.

#### 메뉴 추천

- 메뉴를 추천하는 과정은 아래와 같이 이뤄진다.
  - [x] 월요일 카테고리를 가져온다.
    - [x] 해당 카테고리의 메뉴를 섞어서 첫번째 코치에게 추천한다.
      - [x] 추천한 메뉴가 못 먹는 메뉴이면 다시 섞어서 추천한다.
      - [x] 추천한 메뉴가 중복이면 다시 섞어서 추천한다.
    - [x] 다음 코치에게 같은 과정을 반복한다.
  - [x] 화, 수, 목, 금요일에 대해 같은 과정을 반복한다.

## 개발 과정

#### <Programming process>

---
    Write small but useful program everday.
---

1. 핵심 기능을 한 문장으로 정의하기.
2. 핵심 기능이 동작 가능한 가장 작은 버전부터 만들기.
- 작은 기능을 커밋 메시지 용으로 미리 적어놓기.
3. 작은 기능을 테스트하기.
4. 커밋 메시지를 달성했다면 커밋하기.
```

#### 패키지 나누기

- `controller` `model` `util` `view` 패키지 생성
- `util` 패키지 안에 `Util` 클래스 생성 (여러번 사용되는 것들)
- `util` 패키지 안에 `validator` 패키지 생성

---

###### 그 다음, `view`부터 만들자!!!

#### OutputView

```
public class OutputView {

  private static final OutputView instance = new OutputView();

  public static OutputView getInstance(){
      return instance;
  }
  private OutputView() {
  }
    
  public void printGameStart() {
    System.out.println(Message.OUTPUT_GAME_START.message);
    System.out.println(); // 출력하고 한줄 띄우기 (선택)
  }

  private enum Message {
        OUTPUT_GAME_START("게임을 시작합니다.");

        private final String message;

        Message(String message) {
            this.message = message;
        }
    }
}
```

### InputView

```
public class InputView {

    private static final InputView instance = new InputView();

    public static InputView getInstance(){
        return instance;
    }
    private InputView() {
    }

    public String readMainOption() {
        System.out.println(Message.INPUT_MAIN_OPTION.message);
        String input = Console.readLine();
        System.out.println(); // 입력 받고 한줄 띄우기
        return input;
    }


     private enum Message {
        INPUT_MAIN_OPTION("메인 옵션");

        private final String message;

        Message(String message) {
            this.message = message;
        }
    }
}
```

---

# Controller & Application

#### Application

```
public class Application {
    public static void main(String[] args) {
        MainController mainController = new MainController(InputView.getInstance(), OutputView.getInstance());
        mainController.play();
    }
}
```

#### MainController

- 일단은 전체 `MainController`에 만들고 나중에 필요하면 다른 Controller를 만들어서 분리하자
- 게임에 필요한 다른 변수들이 많으면 `MainVariable` 클래스 생성을 고려한다.

##### 아무런 리팩터링도 고려하지 않은 간단 ver.

```
public class MainController {
    private final InputView inputView;
    private final OutputView outputView;

    public MainController(InputView inputView, OutputView outputView) {
        this.inputView = inputView;
        this.outputView = outputView;
    }

    public void play() {
    }
}
```

---

### 출력 메세지 처리

#### ExceptionMessage

```
public enum ErrorMessage {

    INVALID_NOT_NUMERIC("자연수만 입력 가능합니다."),
    INVALID_OUT_OF_INT_RANGE("입력 범위를 초과하였습니다.");

    public static final String BASE_MESSAGE = "[ERROR] %s";
    private final String message;

    ErrorMessage(String message) {
        this.message = String.format(BASE_MESSAGE, message);
    }

    public String getMessage() {
        return message;
    }
}
```

- 예외를 던지는 곳에서
  `throw new IllegalArgumentException(ErrorMessage.~~.getMessage());`

## Util

- 필요한 것만 골라다 쓰자!

```
public class Util {

    public static String removeSpace(String input) {
        return input.replaceAll(Regex.SPACE.regex, Regex.NO_SPACE.regex);
    }

    public static String removeDelimiters(String input) {
        return input.replace(Regex.SQUARE_BRACKETS_START.regex, Regex.NO_SPACE.regex)
                .replace(Regex.SQUARE_BRACKETS_END.regex, Regex.NO_SPACE.regex);
    }

    public static List<String> splitByComma(String input) {
        return Arrays.asList(input.split(Regex.COMMA.regex));
    }

    public static List<String> formatProductInfo(String input) {
        return Util.splitByComma(Util.removeDelimiters(Util.removeSpace(input)));
    }


    private enum Regex {
        SPACE(" "), NO_SPACE(""),
        SQUARE_BRACKETS_START("["), SQUARE_BRACKETS_END("]"),
        COMMA(",");

        private final String regex;

        Regex(String regex) {
            this.regex = regex;
        }
    }

    private Util() {
    }
}
```

## Validation
- 각 클래스에서 담당하도록 먼저 접근해보고, 너무 많이 겹치면 리팩토링 때 Validation 클래스로 묶어보자.

#### Validator 추상메서드 생성

```
public abstract class Validator {

    private static final Pattern NUMBER_REGEX = Pattern.compile("^[0-9]*$");

    abstract void validate(String input) throws IllegalArgumentException;

       void validateNumeric(String input) {
        if (!NUMBER_REGEX.matcher(input).matches()) {
            throw new IllegalArgumentException(ErrorMessage.INVALID_NOT_NUMERIC.getMessage());
        }
    }

    void validateRange(String input) {
        try {
            Integer.parseInt(input);
        } catch (NumberFormatException exception) {
            throw new IllegalArgumentException(ErrorMessage.INVALID_OUT_OF_INT_RANGE.getMessage(), exception);
        }
    }

    void validateNumberRange(String input) {
        int number = Integer.parseInt(input);
        if (number < Range.MIN_RANGE.value || number > Range.MAX_RANGE.value) {
            throw new IllegalArgumentException();
        }
    }
    private enum Range{
        MIN_RANGE(3), MAX_RANGE(20);

        private final int value;

        Range(int value) {
            this.value = value;
        }
    }
}
```

## Constants (최대한 X)

- 최대한 클래스 내에서 해결
- 다양한 자료형의 상수가 모여있다면 `Enum`을 활용하기 어려움
- 한 클래스가 아니라 **여러 클래스에서 사용**되는 상수의 경우 따로 클래스를 만들자!

```
public class Constants {

    public static final int NUMBER_COUNT = 6;
    public static final int MIN_RANGE = 1;
    public static final int MAX_RANGE = 45;
    public static final int LOTTO_PRICE = 1000;

    private Constants() {
    }
}

```

### `MapPractice` 을 key, value (키, 값) 순으로 출력하기 `Map.Entry`

```
    Map<Integer, Integer> map = new HashMap<>();
    map.put(100, 1);
    map.put(200, 2);
    for (Map.Entry<Integer, Integer> element : map.entrySet()) {
        System.out.println(String.format("%d원 - %d개", element.getKey(), element.getValue()));
    }
```


## enum

## Enum 관리

``` 
public enum FoodCategory {
    JAPANESE("일식", 1, List.of("규동", "우동", "미소시루", "스시", "가츠동", "오니기리", "하이라이스", "라멘", "오코노미야끼")),
    KOREAN("한식", 2, List.of("김밥", "김치찌개", "쌈밥", "된장찌개", "비빔밥", "칼국수", "불고기", "떡볶이", "제육볶음")),
    CHINESE("중식", 3, List.of("깐풍기", "볶음면", "동파육", "짜장면", "짬뽕", "마파두부", "탕수육", "토마토 달걀볶음", "고추잡채")),
    ASIAN("아시안", 4, List.of("팟타이", "카오 팟", "나시고렝", "파인애플 볶음밥", "쌀국수", "똠얌꿍", "반미", "월남쌈", "분짜")),
    WESTERN("양식", 5, List.of("라자냐", "그라탱", "뇨끼", "끼슈", "프렌치 토스트", "바게트", "스파게티", "피자", "파니니"));

    private final String category;
    private final int number;
    private final List<String> menu;

    FoodCategory(String category, int number, List<String> menu) {
        this.category = category;
        this.number = number;
        this.menu = menu;
    }

    public String getCategory() {
        return category;
    }

    public int getNumber() {
        return number;
    }

    public List<String> getMenu() {
        return menu;
    }

    public static String getCategoryByNumber(int number) {
        for (FoodCategory foodCategory : FoodCategory.values()) {
            if (foodCategory.getNumber() == number) {
                return foodCategory.getCategory();
            }
        }
        throw new IllegalArgumentException(ErrorMessage.MISMATCH_NUMBER.getMessage());
    }

    public static List<String> getMenuByCategory(String category) {
        for (FoodCategory foodCategory : FoodCategory.values()) {
            if (foodCategory.getCategory().equals(category)) {
                return foodCategory.getMenu();
            }
        }
        throw new IllegalArgumentException(ErrorMessage.MISMATCH_CATEGORY.getMessage());
    }
}
```

#### 조건에 일치하는 값 반환하는 get함수 예시

```
    // 주어진 이름과 일치하는 메뉴를 찾아 해당 가격 반환하기
    public static int getPriceByName(String menuName) {
        for (Menu menu : Menu.values()) {
            if (menu.getName().equals(menuName)) {
                return menu.getPrice();
            }
        }
        throw new IllegalArgumentException();
    }

    // 주어진 이름과 일치하는 메뉴를 찾아 해당 타입을 반환하기
    public static String getTypeByName(String menuName) {
        for (Menu menu : Menu.values()) {
            if (menu.getName().equals(menuName)) {
                return menu.getType();
            }
        }
        throw new IllegalArgumentException();
    }
    
    public static String getCategoryByNumber(int number) {
        for (FoodCategory foodCategory : FoodCategory.values()) {
            if (foodCategory.getNumber() == number) {
                return foodCategory.getCategory();
            }
        }
        throw new IllegalArgumentException(ErrorMessage.MISMATCH_NUMBER.getMessage());
    }

    public static List<String> getMenuByCategory(String category) {
        for (FoodCategory foodCategory : FoodCategory.values()) {
            if (foodCategory.getCategory().equals(category)) {
                return foodCategory.getMenu();
            }
        }
        throw new IllegalArgumentException(ErrorMessage.MISMATCH_CATEGORY.getMessage());
    }
    
    MISMATCH_NUMBER("숫자와 일치하는 카테고리가 없습니다."),
    MISMATCH_CATEGORY("일치하는 카테고리가 없습니다.");

```

## Decimal Format

- 숫자를 특정 형태의 문자열로 출력할 때
- OutputView에서 적용

#### 가격(원화)으로 출력할 때 (예: 50,000원 or -4,000원)

```
    private static final String FORMAT_PRICE = "###,###원";
    private static final String FORMAT_DISCOUNT = "-###,###원";

    private String applyPriceFormat(int price) {
        DecimalFormat decimalFormat = new DecimalFormat(FORMAT_PRICE);
        return decimalFormat.format(price);
    }

    private String applyDiscountFormat(int discountAmount) {
        DecimalFormat decimalFormat = new DecimalFormat(FORMAT_DISCOUNT);
        return decimalFormat.format(discountAmount);
    }
```

#### 소수점 자리, 반올림 적용

``` 
    private final String YIELD_DECIMAL_PLACE = "0.0"; // 소수점 첫째 자리까지
    private final String YIELD_PERCENT_SIGN = "%";
    
    public void printYield(double yield) {
        DecimalFormat decimalPlace = new DecimalFormat(YIELD_DECIMAL_PLACE);
        decimalPlace.setRoundingMode(RoundingMode.HALF_UP); // 소수점 둘째 자리에서 반올림해서 첫째 자리까지 표시
        String decimalPointYield = decimalPlace.format(yield);
        System.out.println(String.format(OutputText.YIELD, decimalPointYield, YIELD_PERCENT_SIGN));
    }
```

## 재입력 로직

- 컨트롤러의 private 메서드로 작성.

```
    private List<String> readNames() {
        Coach coach = new Coach();
        while (true) {
            try {
                return coach.readNames(inputView.readName());
            } catch (IllegalArgumentException e) {
                System.out.println(e.getMessage());
            }
        }
    }
    
    // 모든 예외 발생에 대해 공통 에러 메시지를 지정하고 싶을 때
    private int readDate() {
        Date date = new Date();
        while (true) {
            try {
                return date.read(inputView.readDate());
            } catch (IllegalArgumentException e) {
                System.out.println(ErrorMessage.INVALID_DATE.getMessage());
            }
        }
    }
```

## file reader

#### 주의사항

1. try, catch로 꼭 `IOException` 잡아줘야함 => 안잡으면 에러
2. 파일 경로 조심 + 오타 조심
3. 한줄씩 읽을거면 `BufferedReader` 아니면 다른거 (구글링)

``` 
try {
            File backendCrews = new File("src/main/resources/backend-crew.md");
            File frontendCrews = new File("src/main/resources/frontend-crew.md");

            BufferedReader backendCrewsReader = new BufferedReader(new FileReader(backendCrews));
            BufferedReader frontendCrewsReader = new BufferedReader(new FileReader(frontendCrews));

            String backendCrew;
            while ((backendCrew = backendCrewsReader.readLine()) != null) {
                System.out.println(backendCrew);
            }
            System.out.println();

            String frontendCrew;
            while ((frontendCrew = frontendCrewsReader.readLine()) != null) {
                System.out.println(frontendCrew);
            }
        } catch (IOException exception) {
            outputView.printExceptionMessage(exception);
            throw new RuntimeException(exception);
        }
  ```

## 검증로직들

- 정규식으로 숫자만 받기

```
private static final Pattern MONEY_REGEX = Pattern.compile("^[0-9]*$");
if (!MONEY_REGEX.matcher(input).matches()) {
            throw new IllegalArgumentException(ERROR_PURCHASE_TYPE);
        }
```

- 1부터 9까지의 자연수

```
"^[1-9]+$"
```

- R, Q 외의 문자는 안됨

```
String moveCommandRegex = "^([RQ])$";

      if (!Pattern.matches(moveCommandRegex, restartCommand)) {
          throw new IllegalArgumentException(InputErrorText.ERROR_RESTART_COMMAND.errorText());
      }
```

- 369 게임에서 3, 6, 9가 들어간 글자 골라내기

```
private static int calculateCurrentNumberClaps(int currentNumber) {
      int originalLength = String.valueOf(currentNumber).length();
      return originalLength - String.valueOf(currentNumber).replaceAll("[369]", "").length();
  }
```

- 중복 검사

```
public static void isDistinct(String input) {
  long count = input.chars()
      .distinct()
      .count();
  if (count != ConstVariable.SIZE.getValue()) {
      throw new IllegalArgumentException();
  }
}
 ```

## 깃허브

1. Fork 후에 `eunkeeee/프로젝트이름`을 local로 `clone`하기

```
cd {저장할 경로 입력}
git clone https://github.com/eunkeeee/java-baseball.git
```

2. 브랜치 생성 및 전환

```
git branch {생성할 브랜치 이름}
git checkout {전환할 브랜치 이름}
git checkout -b {생성 후 전환할 브랜치 이름}
```

3. 정상적 commit & push

```
// 모든 파일 스테이징
git add . 

// 스테이징된 파일 전부 커밋하기
git commit -m "feat(GameController): 기능 잘 구현

git push origin eunkeeee
```

4. add 취소

```
git reset HEAD README.md  // README 파일을 Unstaged 상태로 변경
git reset // 모든 파일을 스테이징 취소
git checkout . // 커밋되지 않은 모든 로컬 변경 사항을 되돌림
git checkout [some_dir|file.txt] // 커밋되지 않은 변경 사항을 특정 파일이나 디렉터리로만 되돌림
```

5. commit 취소

```
// [방법 1] commit을 취소하고 해당 파일들은 staged 상태로 워킹 디렉터리에 보존
$ git reset --soft HEAD^

// [방법 2] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에 보존
$ git reset --mixed HEAD^ // 기본 옵션
$ git reset HEAD^ // 위와 동일
$ git reset HEAD~2 // 마지막 2개의 commit을 취소

// [방법 3] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에서 삭제
$ git reset --hard HEAD^
```

- reset 옵션
  – soft : index 보존(add한 상태, staged 상태), 워킹 디렉터리의 파일 보존. 즉 모두 보존.
  – mixed : index 취소(add하기 전 상태, unstaged 상태), 워킹 디렉터리의 파일 보존 (기본 옵션)
  – hard : index 취소(add하기 전 상태, unstaged 상태), 워킹 디렉터리의 파일 삭제. 즉 모두 취소.

6. commit 메세지 변경

```
git commit --amend
```

7. commit 로그와 수정된 파일 함께 보기

```
git log 
// q를 눌러 나가기
```

8. 특정 파일만 commit & push

```
// 작업한 파일 목록 확인하기
git status

// 기존파일의 변경내역 확인하기
git diff

// 특정 파일 Staging하기
git add <경로 및 file명> <경로 및 file명> <경로 및 file명> ...

// 특정 파일 Commit하기
git commit -m "커밋 메세지" {파일이름}

// 위의 두 개 한꺼번에 하기
git commit -am "커밋 메세지" {파일이름}

// 특정 파일 unstaged 상태 만들기
git restore --staged {파일이름}
```
