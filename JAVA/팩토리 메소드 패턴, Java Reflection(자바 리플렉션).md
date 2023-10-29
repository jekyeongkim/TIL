## 팩토리 메소드 패턴, 자바 리플렉션

---
### 팩토리 메소드 패턴
**복잡한 생산 과정을 숨기고, 완성된 인스턴스만 반환한다.**<br/><br/>
**객체 생성을 대신해주는 클래스 == 팩토리**
1. Bus 클래스
```java
public class Bus {
    ...
}
```
2. BeanFactory 클래스
```java
public class BeanFactory {
    // 2. 자기 자신 인스턴스를 참조하는 static한 필드를 선언
    private static BeanFactory instance = new BeanFactory();
    
    // 1. private 생성자를 만든다. 외부에서 인스턴스를 생성하지 못한다.
    private BeanFactory() {
        
    }
    
    // 3. 2번에서 생성한 인스턴스를 반환하는 static한 메소드를 만든다.
    public static BeanFactory getInstance(){
        return instance;
    }
    
    // ✅ BeanFactory 클래스에서 내부적으로 Bus()를 만들어서 리턴해주는 메소드
    public Bus getBus() {
        return new Bus();
    }
}
```
3. BeanFactoryMain 클래스
```java
public class BeanFactoryMain {
    public static void main(String[] args) {
        BeanFactory beanFactory1 = BeanFactory.getInstance();
        BeanFactory beanFactory2 = BeanFactory.getInstance();
        if (beanFactory1 == beanFactory2) {
            System.out.println("beanFactory1 == beanFactory2");
        }
        
        // ✅ 생성과정을 BeanFactory한테 맡겨 Bus 인스턴스를 생성
        Bus b1 = beanFactory1.getBus();
        Bus b2 = beanFactory2.getBus();
        
        
        // Bus b3 = new Bus(); → 원래는 직접 new 연산자를 이용해 생성
    }
}
```
생성과정을 다른 객체(BeanFactory)에게 맡겨서 리턴받아서 사용</br>

---

### 클래스 로더를 이용한 인스턴스 생성
```java
Class class = Class.forName("클래스 풀네임");
Object obj = class.newInstance(); 
```
1. Bus 클래스
```java
public class Bus {
    public void a() {
        System.out.println("Bus a");
    }
    
    public void b() {
        System.out.println("Bus b");
    }
    
    public void c() {
        System.out.println("Bus c");
    }
}
```
2. SuperCar 클래스
```java
public class SuperCar {
    public void a() {
        System.out.println("Super a");
    }
}
```
3. ClassLoaderMain 클래스
```java
public class ClassLoaderMain {
    public static void main(String[] args) throws Exception {
        // 일반적인 인스턴스 생성 및 메소드 호출방법
        // Bus b1 = new Bus();
        // b1.a();
        
        
        // a() 메소드를 갖고 있는 클래스가 있는데 클래스 이름을 아직 모르는 경우 → 클래스로더 사용
        String className = "com.example.Bus";
        // className 에 해당하는 클래스 정보를 CLASSPATH 에서 읽어들이고 그 정보를 clazz가 참조하도록 함
        Class clazz = Class.forName(className);
        // clazz.getDeclaredMethods();를 통해 clazz가 갖고 있는 메소드의 정보들을 리턴
        Method[] declaredMethods = clazz.getDeclaredMethods();
        for (Method m : declaredMethods){
            System.out.println(m.getName());
        }
        
        System.out.println("--------------------");
        
        // 아래 세줄은 Object o1 = new SuperCar(); 를 나타내는 같은 코드
        String className = "com.example.SuperCar";
        Class clazz = Class.forName(className);
        Object o1 = clazz.newInstance(); // clazz 가 갖고 있는 정보(SuperCar)를 갖고 인스턴스를 생성
        
        // 형 변환도 가능
        SuperCar superCar = (SuperCar) o1;
        superCar.a();
    }
}
```
4. 출력 결과
```text
Bus b
Bus c
Bus a
--------------------
Super a
```

---

### 비슷한 다른 예시 (상속, 추상 클래스 활용)
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
3. SuperCar 클래스
```java
public class SuperCar extends Car {
    @Override
    public void a() {
        System.out.println("Super a");
    }
}
```
4. ClassLoaderMain 클래스
```java
public class ClassLoaderMain {
    public static void main(String[] args) throws Exception {
        
        // 아래 세줄은 Object o1 = new SuperCar(); 를 나타내는 같은 코드
        String className = "com.example.SuperCar";
        Class clazz = Class.forName(className);
        Object o1 = clazz.newInstance(); // clazz 가 갖고 있는 정보(SuperCar)를 갖고 인스턴스를 생성
        
        // SuperCar의 인스턴스가 만들어졌는데 부모타입인 Car를 ㄱ참조하게끔 형 변환
        Car car = (Car) o1;
        car.a();

        System.out.println("---------------");

        // 아래 세줄은 Object o2 = new Bus(); 를 나타내는 같은 코드
        String className = "com.example.Bus";
        Class clazz = Class.forName(className);
        Object o2 = clazz.newInstance(); // clazz 가 갖고 있는 정보(Bus)를 갖고 인스턴스를 생성

        // Bus의 인스턴스가 만들어졌는데 부모타입인 Car를 참조하게끔 형 변환
        Car car = (Car) o2;
        car.a();
    }
}
```
5. 출력 결과
```text
Super a
---------------
Bus a
```

---
### 위의 예시에서 상속을 받지 않는 메소드를 호출하고자 할 때?
1. MyHome 클래스 (Car 클래스는 존재하나 서로 관련 1도 없는 클래스)
```java
public class MyHome {
    public void a() {
        System.out.println("a 라는 메소드를 MyHome이 갖고 있습니다.");
    } 
}
```
2. ClassLoaderMain 클래스
```java
public class ClassLoaderMain {
    public static void main(String[] args) throws Exception {
        
        // 아래 세줄은 Object o1 = new MyHome(); 를 나타내는 같은 코드
        String className = "com.example.MyHome";
        Class clazz = Class.forName(className);
        Object o1 = clazz.newInstance(); // clazz 가 갖고 있는 정보(MyHome)를 갖고 인스턴스를 생성

        // a() 메소드 정보를 가지고 있는 메소드를 반환하라는 뜻
        Method m = clazz.getDeclareMethod("a", null); // 첫번째는 메소드 이름, 두번째는 파라미터 값 입력
        // Object o 가 참조하는 객체의 m 메소드를 실행하라는 뜻
        m.invoke(o1, null); // 첫번째는 인스턴스 변수 이름, 두번째는 파라미터 값 입력

        System.out.println("---------------");
        
        // 위와 동일한데 className 만 Bus로 변경
        String className = "com.example.Bus";
        Class clazz = Class.forName(className);
        Object o2 = clazz.newInstance();

        Method m = clazz.getDeclareMethod("a", null);
        m.invoke(o2, null);
    }
}
```
3. 출력 결과
```text
a 라는 메소드를 MyHome이 갖고 있습니다.
---------------
Bus a
```
***클래스 로더를 이용하면 클래스 이름이 정해져 있지 않더라도 문자열("com.example.~~~)로 받아들여 인스턴스를 만들 수도 있고 인스턴스가 갖고 있는 메소드를 호출할 수 있다.</br>***
***→ 리플렉트(reflect)***</br></br>
나중에 서블릿이나 스프링에서 내부적으로 많이 사용되기 때문에 알아두면 좋음!

<br/><br/>

>**Reference**
><br/>부부개발단 - 즐겁게 프로그래밍 배우기.