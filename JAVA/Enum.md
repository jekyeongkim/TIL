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

* **Enum은 switch문에서 사용 가능하다. (JDK 7 이상부터는 switch문에서 String도 사용 가능)**
```java
package com.example.enumtype;

public class DaySwitchTest {
    public static void main(String[] args) {
        // Day 타입의 변수를 선언하고 SUNDAY라는 상수값을 갖게 함
        Day day = Day.SUNDAY;
        
        switch (day){
            case SUNDAY : // case 부분에는 Day가 갖고 있는 상수만 적여줘야 함 Day.Sunday 이렇게 적으면 ❌
                System.out.println("일요일입니다.");
                break; 
            case MONDAY :
                System.out.println("월요일입니다.");
                break;
            default :
                System.out.println("그 밖의 요일");
        }
    }
}
```
* day가 어떤 상수냐에 따라서 알맞은 case 부분이 실행된다.
* 이 때 조심해야 할 것은 case 다음에는 Day가 가지고 있는 상수의 이름이 나와야 한다는 것이다.
* case 다음에 Day.SUNDAY 라고 사용하면 컴파일 오류가 발생한다.</br>

실행 결과
```text
일요일입니다.
```

* **Enum은 생성자를 가질 수 있다. 단 생성자는 private 해야 한다.**
* **Enum의 생성자는 내부에서만 호출 가능하다.**
```java
package examples.enumtype;

public enum Gender {
    // 상수 뒤에 () 가 추가된 형태, 내부적인 생성자를 호출
    // XY, XX 값이 전달돼서 염색체에 해당하는 chromosome 필드의 값이 초기화 되는 것 → 내부적으로 문자열 값을 갖게 되는 것
    MALE("XY"),
    FEMALE("XX");
    
    private String chromosome; // 염색체
    
    private Gender(String chromosome){
        this.chromosome = chromosome;
    }
}
```
* Gender Enum 타입은 MALE과 FEMALE 2가지 상수를 가진다.
* 앞의 예제와는 다르게 상수 뒤에 ("XY"), ("XX") 가 붙어 있다.
* 상수 뒤에 괄호 열과 닫고 기호가 있으면 Enum의 생성자를 호출하게 된다.
* 생성자가 호출되며 chromosome 가 초기화 된다.

```java
package com.example.enumtype;

public class GenderTest {
    public static void main(String[] args) {
        Gender gender = Gender.MALE;

        System.out.println(gender);
    }
}
```
* Gender 타입의 변수 gender에는 Gender.MALE 이나 Gender.FEMALE 값만 할당할 수 있다.
* 해당 gender를 출력하면 상수 이름이 그대로 출력되는 것을 알 수 있다.

실행결과
```text
MALE
```
여기서 MALE, FEMALE 상수 값이 아닌 염색체 XY, XX 값을 출력하고 싶다면 ❓

---


### Enum에 메소드와 변수 선언하기

* Enum 안에 선언된 메소드나 변수를 가질 수 있다.
* 또한 Object 가 가지고 있는 메소드를 오버라이딩 할 수도 있다.
* Gender Enum을 생성할 때 chromosome 필드를 작성했었다.
* 이번엔 Gender Enum에 Object 가 가지고 있는 toString() 메소드를 오버라이딩 한다.

```java
package com.example.enumtype;

public enum Gender {

    MALE("XY"),
    FEMALE("XX");

    private String chromosome; // 염색체

    private Gender(String chromosome){
        this.chromosome = chromosome;
    }
    
    // toString() 메소드 오버라이딩
    @Override
    public String toString() {
        return "Gender{" +
                "chromosome='" + chromosome + '\'' +
                '}';
    }
    
    // print() 메소드 추가
    public void print(){
        System.out.println("염색체 정보 : " + chromosome);
    }
}
```
```java
package com.example.enumtype;

public class GenderTest {
    public static void main(String[] args) {
        Gender gender = Gender.MALE;

        System.out.println(gender);
        
        gender.print();
    }
}
```
실행 결과
```text
Gender{chromosome='XY'}
염색체 정보 : XY
```

---

### Enum 값끼리 비교할 때는 ==를 사용한다.
```java
Day day1 = Day.MONDAY;
Day day2 = Day.MONDAY;

if(day1 == day2){
    System.out.println("같은 요일입니다.");
}
```

---

### EnumMap
EnumMap은 Enum타입을 키(key)로 사용할 수 있도록 도와주는 클래스이다.
```java
package com.example.enumtype;

import java.util.EnumMap;

public class EnumMapTest {
    public static void main(String[] args) {
        EnumMap emap = new EnumMap(Day.class); // key 값으로 Day에 정의한 상수만 사용한다는 의미
        emap.put(Day.SUNDAY, "일요일은 잠자는 것이 최고."); // 각각의 key 값으로 문자열 값 지정
        emap.put(Day.FRIDAY, "불금!!");
        emap.put(Day.MONDAY, "월요병");

        System.out.println(emap.get(Day.SUNDAY));
    }
}
```
```text
일요일은 잠자는 것이 최고.
```

---

### EnumSet
EnumSet은 Enum 상수를 Set 자료구조로 다루기 위한 유용한 메소드를 제공하는 클래스이다.</br>
상수값을 가지는데 중복된 값을 허용하지 않는다.
```java
package com.example.enumtype;

import java.util.EnumSet;
import java.util.Iterator;

public class EnumSetTest {
    public static void main(String[] args) {
        EnumSet eset = EnumSet.allOf(Day.class); // allOf(Day.class) → Day가 갖고 있는 모든 상수들을 자동으로 EnumSet에 넣어줌
        
        Iterator<Day> dayIter = eset.iterator();
        
        while (dayIter.hasNext()) {
            Day day = dayIter.next();
            System.out.println(day);
        }
        
        System.out.println("---------------------------------");
        
        EnumSet eset2 = EnumSet.range(Day.MONDAY, Day.WEDNESDAY); // range(Day.MONDAY, Day.WEDNESDAY) → Day 상수가 정의되어 있는 사이값에 있는 모든 것을 포함시켜라
        Iterator<Day> dayIter2 = eset2.iterator();
        while (dayIter2.hasNext()) {
            Day day = dayIter2.next();
            System.out.println(day);
        }
    }
}
```
```text
SUNDAY
MONDAY
TUSEDAY
WEDNESDAY
THURSDAY
FRIDAY
SATURDAY
---------------------------------
MONDAY
TUSEDAY
WEDNESDAY
```
<br/><br/>

>**Reference**
><br/>부부개발단 - 즐겁게 프로그래밍 배우기.