## 예외(Exception)처리

---
### Error의 종류
* 컴파일 에러 
  * 컴파일 시 발생하는 에러
* 런타임 에러
  * 실행 시 발생하는 에러 

---

### 자바에서 실행 시 발생하는 2가지 형태의 오류
* Error : 수습할 수 없는 심각한 오류
* Exception (예외) : 예외 처리를 통해 수습할 수 있는 덜 심각한 오류
* 메모리 부족, 스택오버플로우(stack overflow) 등이 발생하여 프로그램이 죽는 것은 프로그래머가 제어할 수 없다.

다음과 같은 클래스를 실행시켜보면
```java
public class Exception1 {
    public static void main(String[] args) {
        ExceptionObj1 exobj = new ExceptionObj1();
        int value = exobj.divide(10, 0);
        System.out.println(value);
    }
}
```
```java
public class ExceptionObj1 {
    public int divide(int i, int k){
        int value = 0;
        value = i / k;
        return value;
    }
}
```
이런 식으로 오류가 뜸 ( i(10)를 k(0)로 나누는데 0으로 나눌 수 없기 때문)</br>
JVM이 0으로 나눌 수 없기 때문에 `ArithmeticException`라는 객체를 생성하여 프로그램을 멈춤
```text
Exception in thread "main" java.lang.ArithmeticException ~~~~
at ExceptionObj1.divide(Exception1.java:12)
at Exception1.main(Exception1.java:4)
```

---

### 예외 처리하기 (`try-catch`)
`try` 블럭에 코드를 적고, 코드에서 어떤 Exception이 발생하게 되면 이 Exception을 처리해주는 로직을 `catch` 블럭에 적어주면 됨
```java
try{
    코드1
    코드2
        .......    
} catch(Exception클래스명1 변수명1){
        Exception을 처리하는 코드
} catch(Exception클래스명2 변수명2){
        Exception을 처리하는 코드
}
```

---

### 위의 예시에 `try-catch` 적용
```java
// B 사용자가 작성, A 사용자가 만든 ExceptionObj1을 이용한다.
public class Exception1 {
    public static void main(String[] args) {
        ExceptionObj1 exobj = new ExceptionObj1();
        int value = exobj.divide(10, 0);
        System.out.println(value);
    }
}
```
```java
// A 사용자가 작성
public class ExceptionObj1 {
    public int divide(int i, int k){
        int value = 0;
        try {
          value = i / k;    
        }catch (ArithmeticException ex){
          System.out.println("0으로 나눌 수 없습니다.");
          System.out.println(ex.toString());
        }
        
        return value;
    }
}
```
에러가 발생하지 않았기 때문에 `value`는 처음에 0으로 선언했던 0을 리턴
```text
0으로 나눌 수 없습니다.
java.lang.ArithmeticException: / by zero
0
```

### 위 클래스에서 찾아볼 수 있는 2가지 문제점 🤔
1. 오류는 발생하지 않았지만 0으로 나눴을 때 0이 나오면 안되는데 0을 리턴하게끔 하는 심각한 오류를 야기할 수 있는 점
2. 사용자(B)가 원하지 않는 메세지를 출력하게 한 점

❓ 여기서 10은 실제로 0으로 나눌 수가 없는데 0을 출력하도록 하는게 맞는 것인가 ❓</br>
❌ 잘못된 값을 리턴해준 것이므로 try-catch 적용 전 예시처럼 예외를 발생시키고 프로그램이 종료되게끔 하는 것이 좋음</br>

그리고 A라는 사용자는 `ExceptionObj1`이라는 클래스를 재사용되길 원했다면
```text
System.out.println("0으로 나눌 수 없습니다.");
System.out.println(ex.toString());
```
이런 식으로 아무렇게나 값을 출력하도록 하는 것은 바람직하지 않음!!

---

### 예외 떠넘기기(`throws`)
```java
리턴타입 메소드명( 아규먼트 리스트 ) throws Exception클래스명1, Exception클래스명2 ....{
        코드1 
        코드2
        .......
}
```

### 예외 떠넘기기(`throws`) 적용
`value = i / k;` </br>
이 부분에서 Exception(`ArithmeticException`)이 발생하는데 여기서 발생하는 Exceptino을 `divide()` 메소드에서 처리하지 않고 `divide()` 메소드를 호출하는 쪽에 떠넘길 수가 있음.</br>
그리고 `divide()` 메소드를 사용하는 쪽에서 `try-catch` 문을 사용하여 Exception을 처리해줘야 한다.
```java
// B 사용자가 작성, A 사용자가 만든 ExceptionObj1을 이용한다.
public class Exception1 {
    public static void main(String[] args) {
        ExceptionObj1 exobj = new ExceptionObj1();
        // ✅ 메소드를 사용하는 쪽에서 Exception 처리
        try {
          int value = exobj.divide(10, 0);
          System.out.println(value);
        }catch (ArithmeticException ex){
          System.out.println("0으로 나눌 수 없습니다.");
        }
    }
}
```
```java
// A 사용자가 작성, 예외 떠넘기기를 이용하면 JavaDoc 주석문도 사용이 가능
public class ExceptionObj1 {
  /**
   * i를 k로 나는 나머지를 반환한다.
   * @param i
   * @param k
   * @return
   * @throws ArithmeticException
   */
    public int divide(int i, int k) throws ArithmeticException{ // ✅ 메소드를 호출하는 쪽에 throws
        int value = 0;
        value = i / k;
        return value;
    }
}
```
<br/><br/>

>**Reference**
><br/>부부개발단 - 즐겁게 프로그래밍 배우기.