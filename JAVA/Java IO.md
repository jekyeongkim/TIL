## Java IO

---
### IO
* Input & Output
* 입출력
* 임력은 키보드, 네트워크, 파일 등으로 부터 받을 수 있다.
* 출력은 화면, 네트워크, 파일 등에 할 수 있다.

---

### Java IO도 객체
* Java IO 에서 사용되는 객체는 자바 세상에서 사용되는 객체이다.
* Java IO 가 제공하는 객체는 어떤 대상으로 부터 읽어들여, 어떤 대상에게 쓰는 일을 한다.

---
### Java IO는 조립되어 사용되도록 만들어짐
* Decorator 패턴(디자인 패턴 중 하나) 으로 만들어졌다.

![img_6.png](img_6.png)

* Component 라는 부모클래스, 이를 상속받고 있는 ConcreteComponent(주인공), Decortator(장식)
* 초록색 화살표로 되있는 것 → Decortator는 component 역할을 수행하는 것들을 가질 수 있다 라는 뜻 </br>
( = Component 클래스를 상속받고 있는 것들도 가질 수 있다)

### 주인공과 장식을 구분할 수 있어야 함
* 장식은 InputStream, OutputStream, Reader, Writer를 생성자에서 받아들인다.</br>
  (위 4개 클래스는 Decorator 패턴에서 Component 역할을 수행, 추상클래스로 new로 인스턴스 생성 불가)
* 주인공은 어떤 대상에게서 읽어들일지, 쓸지를 결정한다.
* 주인공은 1byte 또는 byte[] 단위로 읽고 쓰는 메소드를 가진다.
* 주인공은 1char 또는 char[] 단위로 읽고 쓰는 메소드를 가진다.
* 장식은 다양한 방식으로 읽고 쓰는 메소드를 가진다.

---

### Java IO의 특수한 객체
* System.in : 표준 입력 (InputStream)
* System.out : 표준 출력 (PrintStream)
* System.err : 표준 에러 출력 (PrintStream)

--- 

### Java IO의 클래스 상속도
![img_7.png](img_7.png)

* InputStream, OutputSteram, Reader, Writer 4가지 추상 클래스가 중요!
* 4가지 추상 클래스를 각각 상속받고 있는 클래스들로 구성되어 있음
* 이 중에선 주인공 역할을 수행하는 것이 있고 장식 역할을 수행하는 것이 있다.

___

### Java IO 클래스 이름이 중요
| 클래스                    | 설명                                                                                         |
|------------------------|--------------------------------------------------------------------------------------------|
| Stream로 끝남             | byte 단위 입출력 클래스                                                                            |
| InputStream로 끝남        | byte 단위로 입력을 받는 클래스                                                                        |
| OutputStream로 끝남       | byte 단위로 출력을 하는 클래스                                                                        |
| Reader로 끝남             | 문자 단위로 입력을 받는 클래스                                                                          |
| Writer로 끝남             | 문자 단위로 출력을 하는 클래스                                                                          |
| File로 시작 (File 클래스 제외) | File로부터 입력이나 출력을 하는 클래스                                                                    |
| ByteArray로 시작          | 입력 클래스의 경우 byte 배열로부터 읽어들이고, 출력 클래스의 경우 클래스 내부의 자료구조에 출력을 한 후 출력된 결과를 byte 배열로 반환하는 기능을 가짐 |
| CharArray로 시작          | 입력 클래스의 경우 char 배열로부터 읽어들이고, 출력 클래스의 경우 클래스 내부의 자료구조에 출력을 한 후 출력된 결과를 char 배열로 반환하는 기능을 가짐 |
| Filter로 시작             | Filter로 시작하는 입출력 클래스는 직접 사용하는 것보다 상속받아 사용을 하며, 사용자가 원하는 내용만 필터링할 목적으로 사용됨                  |
| Data로 시작               | 다양한 데이터 형을 입출력 할 목적으로 사용. 특히 기본형 값(int, float, double 등)을 출력하는데 유리                         |
| Buffered로 시작           | 프로그램에서 Buffer라는 말은 메모리를 의미. 입출력 시에 병목현상을 줄이고 싶을 경우 사용                                      |
| RandomAccessFile       | 입력이나 출력을 모두 할 수 있는 클래스로써, 파일에서 임의의 위치의 내용을 읽거나 쓸 수 있는 기능을 제                                |

---

### Java IO 클래스는 생성자가 중요!!
* 장식은 InputStream, OutputStream, Reader, Writer를 생성자에서 받아들인다.

Java api java.io 검색 후 공식문서 들어가서 Frames 클릭 → java.io 패키지 접속</br>
![img_8.png](img_8.png)
BufferedInputStream 들어가 보면 InputStream을 상속받는 것을 알 수 있음
![img_9.png](img_9.png)
생성자를 보면 InputStream을 받아들이는 것을 알 수 있다.</br>
**InputStream, OutputStream, Reader, Writer를 생성자에서 받아들이는 클래스는 전부 다 장식 역할**</br>
**InputStream, OutputStream, Reader, Writer를 생성자에서 받아들이지 않는 클래스는 전부 다 주인공 역할**</br>
File 클래스는 Java IO에서 File 클래스 자체일 뿐 읽고 쓰기 위한 클래스가 아님 (Decorator 패턴 적용 ❌)

***Java IO는 상속관계와 생성자를 잘 보는 것이 중요하다!!***

---

### Java IO를 잘 다루려면, API를 한번쯤은 읽어봐야 함

문제) 키보드로부터 한 줄씩 입력받아 화면에 한 줄씩 출력하시오.

다음과 같은 문제를 해결하려면❓

```java
public class KeyboardIOExam {
    public static void main(String[] args) throws Exception{
        // 이렇게 JVM에게 Exception을 떠넘기는 건 좋지 않지만 코드를 간단하게 적기 위해 작성 
        
        // 키보드로부터 한 줄씩 입력받고, 한 줄씩 화면에 출력하시오.
        // 키보드 : System.in (InputStream 주인공)
        // 화면에 출력 : System.out (OutputStream 주인공)
        // 키보드로 입력받는다는 것은 문자를 입력받는 것 : char 단위 입출력
        // char 단위 입출력 클래스 : Reader, Writer
        // 한 줄 읽기 : BufferedReader 라는 클래스는 readLine 이라는 메소드를 가지고 있다.
        //            더 이상 읽어들일 것이 없으면(EOF) null을 반환
        //            장식!
        // 한 줄 쓰기 : PrintStream, PrintWriter


        // BufferedReader는 장식이기 때문에 생성자로 Reader를 받아들여야 함 → Reader는 추상클래스라 그냥 받지 못하고 자손클래스를 받아야 함❗️
        
        // BufferedReader 생성자로 올 수 있는 목록 (
        // BufferedReader → 🚫
        // CharReader → 문자로부터 읽어들이므로 🚫
        // FilterReader → 장식이라 Reader를 넣어줘야 하므로 🚫
        // InputStreamReader(InputStream in) → 장식 ✅ㅊ
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        String line = null; // line 이라는 변수 선언
        while ( (line = br.readLine()) != null) {// 한 줄 입력받아서 line 변수에 넣어주는데 null이 아닐 때까지 반복하라
          System.out.println("읽어들인 값 :" + line);
        }
    }
}
```
키보드는 System.in → InputStream 타입이며</br>
InputStreamReader(InputStream in) 은 InputStream을 받아들이므로</br>
BufferedReader 의 생성자 안에 들어갈 수가 있다!
<br/><br/>

>**Reference**
><br/>부부개발단 - 즐겁게 프로그래밍 배우기.