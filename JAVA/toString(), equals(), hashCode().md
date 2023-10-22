## Object가 제공하는 3가지 오버라이딩 메소드

---
### `toString()`
Object 클래스의 `toString()` 메소드는 해당 인스턴스에 대한 정보를 문자열로 반환한다.
1. Car 클래스
```java
public class Car {
    public void run() {
        System.out.println("자동차가 달리다");
        
    }
    
    @Override
    public String toString() {
        return "자동차!!!";
    }
}
```
2. CarTest 클래스
```java
public class CarTest {
    public static void main(String[] args) {
        Car c1 = new Car();
        System.out.println(c1); // println(Object x) - Object를 참조할 수 있는 무엇이든 받을 수 있다.
    }
}
```
3. 출력 결과
```java
자동차!!!
```
뒤에 `toString()` 메서드를 붙여주지 않고 변수만 출력해도 메서드를 붙인것과 똑같은 값이 출력됨<br/>
System.out.println(c1.toString()); == System.out.println(c1);</br>
이는 컴파일러가 객체만 출력할 경우 자동으로 `toString()`을 붙여주고 컴파일 하기 때문<br/>

---

### `equals()`
**두 참조 변수의 값이 같은지 비교하는 메소드 (같은 참조 x)**<br/>

---

### `hashCode()`

**나중에 컬렉션 프레임워크, 자료구조와 관련된 객체들을 공부할 때 사용 (`hashSet`, `hashMap`)**<br/>
**이 객체들을 사용할 때, `equals()`, `hashCode()` 라는 메소드를 잘 오버라이딩 해줘야 함**<br/>

---

***Obect가 제공하는 3가지 메소드 `toString()`, `equals()`, `hashCode()`는 반드시 오버라이딩 하여 사용해야 한다!!!***</br></br>
>**Reference**
><br/>부부개발단 - 즐겁게 프로그래밍 배우기.