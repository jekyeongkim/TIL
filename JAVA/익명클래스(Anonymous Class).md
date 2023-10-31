## 익명 클래스(Anonymous Class)

---
### 익명 클래스
객체가 재사용할 일이 없거나

```java
new 생성자 () {...}
```
생성자 뒤에 중괄호가 나오고 코드를 오버라이딩 하여 보통 구현
```java
Car car = new Car() {
    public void run() {
        System.out.println("Car를 상속받는 이름없는 객체가 run메소드를 오버라이딩함");
    }
}
```

---

### 예시

1. Car 클래스 (추상 클래스)
```java
public abstract class Car {
    public abstract void a();
}
```
2. Bus 클래스
```java
public class Bus extends Car {
    @Override
    public void a() {
        System.out.println("Bus a");
    }
    
    @Override
    public void b() {
        System.out.println("Bus b");
    }
    
    @Override
    public void c() {
        System.out.println("Bus c");
    }
}
```
3. CarExam 클래스
```java
public class CarExam {
    public static void main(String[] args) {
        // Car c1 = new Car(); → Car 클래스는 추상클래스이기 때문에 인스턴스가 될 수 없음. 오류가 발생
        Car c2 = new Bus(); // → Bus 클래스는 Car를 상속받고 있는 클래스이기 떄문에 가능
        
        // ✅ Car를 상속받고 있긴 하지만 클래스를 만들고 싶지 않을 때 ❓
        Car c3 = new Car() {
            @Override
            public void a() {
                System.out.println("이름없는 객체의 a() 메소드 오버라이딩");
            }
        };
        
        c3.a();
    }
}
```
4. 출력 결과
```text
이름없는 객체의 a() 메소드 오버라이딩
```

---

### 다른 예시 1
1. MyRunnable 인터페이스 클래스
```java
public interface MyRunnable {
    public void run();
}
```
2. MyRunnableMain 클래스
```java
public class MyRunnableMain {
    public static void main(String[] args) {
// 인터페이스는 인스턴스가 될 수 없기 때문에 new 인터페이스명 적게되면 인텔리제이가 이름없는 객체를 자동으로 생성해줌
        MyRunnable r = new MyRunnable() {
            @Override
            public void run() {
                System.out.println("myrunnalbe run!!!");
            }
        };
        
        r.run();
    }
}
```
3. 출력 결과
```text
myrunnalbe run!!!
```

---

### 다른 예시 2
1. MyRunnable 인터페이스 클래스
```java
public interface MyRunnable {
    public void run();
}
```
2. RunnableExecute 클래스
```java
public class RunnableExecute {
    // MyRunnable 인터페이스 타입을 받아들이는 메소드
    public void execute(MyRunnable myRunnable) {
        myRunnable.run();
    }
}
```
3. MyRunnableMain 클래스 (1)
```java
public class MyRunnableMain {
    public static void main(String[] args) {
// 인터페이스는 인스턴스가 될 수 없기 때문에 new 인터페이스명 적게되면 인텔리제이가 이름없는 객체를 자동으로 생성해줌
        MyRunnable myRunnable = new MyRunnable() {
            @Override
            public void run() {
                System.out.println("hello!!!");
            }
        };
        
        
        RunnableExecute runnableExecute = new RunnableExecute();
        runnableExecute.execute(myRunnable); // 파라미터로 MyRunnable 타입이 들어가야 함
    }
}
```
3. MyRunnableMain 클래스 (2) (익명 객체 생성하는 코드를 파라미터로 바로 넣어버림)
```java
public class MyRunnableMain {
    public static void main(String[] args) {
        
        RunnableExecute runnableExecute = new RunnableExecute();
        runnableExecute.execute(        new MyRunnable() {
            @Override
            public void run() {
                System.out.println("hello!!!");
            }
        });
    }
}
```
3. MyRunnableMain 클래스 (3) (람다)
```java
public class MyRunnableMain {
    public static void main(String[] args) {
        
        RunnableExecute runnableExecute = new RunnableExecute();
        runnableExecute.execute(() -> {
                System.out.println("hello!!!");
            }
        ); // 파라미터로 MyRunnable 타입이 들어가야 함
    }
}
```
4. 출력 결과
```text
hello!!!
```

---
### 람다(Lambda)
이름 없는 객체를 간략화하여 표현한 것</br>
람다(lambda) 인터페이스는 메소드를 1개 가지고 있다.</br>
나중에 스트림(Stream) api와 엮여서 쓰이게 되면 편리하게 작성할 수 있다.
```java
MyRunnable myRunnable = new MyRunnable() {
            @Override
            public void run() {
                System.out.println("hello!!!");
            }
        };
```
```java
() -> {
    System.out.println("hello!!!");
}
```
위 식을 람라로 표현하면 다음과 같음<br>
`() ->` 는 `run()` 메소드까지를 의미

---

<br/><br/>

>**Reference**
><br/>부부개발단 - 즐겁게 프로그래밍 배우기.