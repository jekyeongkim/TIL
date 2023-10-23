## final, 불변객체 String

---
### final
**부모가 될 수 없는 클래스**<br/><br/>
상속을 못하게 하려면 클래스를 정의할 때, final 키워드를 사용<br/>
```java
public final class 클래스명 {
    ...
}
```
대표적인 final 클래스는 String</br></br>
다음과 같이 String을 상속받는 클래스를 정의해보면 오류가 뜰 것임 (String이 final 클래스이기 때문에)
```java
public class MyString extends String{ // 오류
    
} 
```
---

### 불변객체 String
1. StringTest 클래스
```java
public class StringTest {
    public static void main(String[] args) {
        String str1 = "hello";
        String str2 = "hello";
        String str3 = new String("hello");
        String str4 = new String("hello");

        System.out.println(str1);
        System.out.println(str2);
        System.out.println(str3);
        System.out.println(str4);
        
        if(str1 == str2)
            System.out.println("str1 == str2");
        if(str1 == str3)
            System.out.println("str1 == str3");
        if(str3 == str4)
            System.out.println("str3 == str4");
    }
}
```
2. 출력 결과
```text
hello
hello
hello
hello
str1 == str2
```
다음과 같이 출력되는 이유?</br>

str1, str2는 new 인스턴스로 생성하지 않았기 때문에 상수처럼 취급</br>
따라서 str1과 str2는 hello 라는 문자열이 메모리에 올라갔을 때, 둘 다 **같은 것을 참조**</br></br>
str3, str4는 new 인스턴스로 객체를 생성했기 떄문에 **각각 다른 영역**을 차지하는 hello 라는 문자열이 메모리 상에 올라감</br></br>
**if(str1 == str2)는 같은 값을 참조하는지를 물어보는 것** (값이 같다라는 뜻 ❌, 값이 같은지를 표현할 때는 `equlas()` 사용)

따라서 String을 사용할 떄는 new를 사용하는 것은 바람직하지 않음. 메모리를 되도록이면 적게 쓰는 것이 좋기 떄문에

---
### 다른 예시

```java
public class StringTest {
    public static void main(String[] args) {
        String str1 = "hello";
        String str2 = new String("hello");
        
        if(str1.equals(str2)) { // 값이 같느냐
            System.out.println("str1과 str2는 값이 같다");
        }
        
        String s1 = str1.toUpperCase();
        System.out.println(s1);
        // String은 불변 클래스이기 떄문에 어떤 메소드를 호출해줘도 str1아 참조하는 String은 변하지 않음
        System.out.println(str1);
        
        String s2 = str1.substring(3); // 3번쨰 있는 값부터 짤라서 리턴
        System.out.println(s2);
        // String은 불변 클래스이기 떄문에 어떤 메소드를 호출해줘도 str1아 참조하는 String은 변하지 않음
        System.out.println(str1);
    }
}
```
```text
str1과 str2는 값이 같다
HELLO
hello
lo
hello
```

---

### StringBuffer 클래스, StringBuilder 클래스 (String과 비슷하지만 내부가 변하는 )
사용방법, 내부적으로 동작하는 원리에 대해 추가로 작성해볼 것!!!


<br/><br/>

>**Reference**
><br/>부부개발단 - 즐겁게 프로그래밍 배우기.