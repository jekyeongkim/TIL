## Enum

---
### Enum
자바의 Enum은 Enumeration의 약자로 JDK 5부터 지원하는 기능

JDK 5 이전에 어떤 상수(항상 변하지 않는 값)를 표현하고자 하면 다음과 같이 작성했었음.</br>
`final static` 이 붙어있는 변수는 다 상수라 보면 됨
```java
package com.example.enumtype;

public class DayType {
    public final static int SUNDAY = 0;
    public final static int MONDAY = 1;
    public final static int TUSEDAY = 2;
    public final static int WEDNESDAY = 3;
    public final static int THURSDAY = 4;
    public final static int FRIDAY = 5;
    public final static int SATURDAY = 6;
}
```
DayType 클래스는 `final static int` 로 정의된 상수를 6개 갖고 있음
```java
int today = DayType.SUNDAY;
```
today는 SUNDAY 상수값을 가지게 되니 0이라는 숫자값을 가지게 된다.</br>
today는 요일에 대한 값을 갖도록 하니까 0부터 6까지의 값을 갖기를 희망함.

아래와 같이 일요일인지 검사할 수도 있음.
```java
if (today == DayType.SUNDAY){
    System.out.println("일요일 입니다.");
}
```

문제는 요일을 나타내는 값이 int 형이라는 것.</br>
today는 0부터 6사이의 값을 갖게 하고 싶은데, 정수이다 보니까 아래와 같이 다른 값을 갖게할 수도 있다는 문제가 생김
```java
int today = 100;
```
정해진 값만 변수에 할당할 수 없다는 문제점이 있다. 이러한 것을 타입에 안전하지 않다(No-Type-Safety)고 말한다.</br>

**➤ 이러한 문제를 해결하기 위해 등장한 문법이 Enum**


---

### Enum 사용하기
* 클래스를 생성하는 것과 같은 방식으로 Enum을 생성한다.
* com.example.enumtype 패키지를 생성한다.
* 생성된 패키지 아래에 Day enum을 생성한다.
```java
package examples.enumtype;

public enum Day {
    SUNDAY,
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY
}
```
* Day 안에 상수를 나타내는 값을 적는다. 보통 모두 대문자로 표현하는데, 상수와 상수는 컴마(,)로 구분한다.
* Enum 타입을 필드로 가지는 Today 클래스는 다음과 같이 작성한다.

```java
package com.example.enumtype;

public class Today {
    // Enum 타입의 Day 타입의 day 변수 선언
    // day는 Day라는 Enum 타입이 가질 수 있는 값만 가질 수 있음
    private Day day;

    public Day getDay() {
        return day;
    }

    public void setDay(Day day) {
        this.day = day;
    }
}
```
* Today 클래스의 필드로 Day 타입 day가 선언된 것을 볼 수 있다.
* 이제 Today를 사용하는 TodayTest 클래스를 다음과 같이 작성한다.
```java
package com.example.enumtype;

public class TodayTest {
    public static void main(String[] args) {
        Today today = new Today();
        today.setDay(Day.SUNDAY);
        System.out.println(today.getDay());
    }
}
```
위의 코드를 실행하면 다음과 같음.
```text
SUNDAY
```
```java
today.setDay(Day.SUNDAY);
```
* today의 setDay() 메소드에는 Enum 타입인 Day가 전달되어야 한다.
* 이 경우, 정수로 선언된 상수와 다르게 Day 안에 선언된 상수만 값으로 전달할 수 있다.
```java
today.setDay(100);
```
위와 같은 코드는 사용할 수 없음. 이러한 것을 타입에 안전한다(Type-Safety)하다고 말한다.
```java
System.out.println(today.getDay());
```
* today가 가지고 있는 Day타입의 상수값을 출력한다. 상수 이름이 그대로 출력된다 (SUNDAY)

***특별한 값을 갖게 하고싶다거나 할 때는 Enum을 활용하는 것이 좋음!!!***

---

### Enum 타입의 특징
* **Enum은 타입에 대해 안전하다. 미리 정의된 Enum 변수 안의 상수만을 대입할 수 있다.**
```java
Day day = Day.SUNDAY;
```
위와 같은 코드는 되지만
```java
Day day = 5;
```
위와 같은 코드는 사용할 수 없음! (정수 타입이나 문자열 같은 타입의 값들은 할당해줄 수가 없음)

<br/><br/>

>**Reference**
><br/>부부개발단 - 즐겁게 프로그래밍 배우기.