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




<br/><br/>

>**Reference**
><br/>부부개발단 - 즐겁게 프로그래밍 배우기.