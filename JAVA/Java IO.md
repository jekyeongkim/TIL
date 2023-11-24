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
        // InputStreamReader(InputStream in) → 장식 ✅
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        String line = null; // line 이라는 변수 선언
        while ( (line = br.readLine()) != null) {// 한 줄 입력받아서 line 변수에 넣어주는데 null이 아닐 때까지 반복하라 command + D 프로그램 종료
          System.out.println("읽어들인 값 :" + line);
        }
    }
}
```
키보드는 System.in → InputStream 타입이며</br>
InputStreamReader(InputStream in) 은 InputStream을 받아들이므로</br>
BufferedReader 의 생성자 안에 들어갈 수가 있다!
![img_10.png](img_10.png)
* 순서

![img_11.png](img_11.png)
값을 입력하면 System.in 이 읽어들이고, 읽어들인 것을 InputStreamReader한테 전달하며 이걸 또 BufferedReader에게 전달하며 문자열로 리턴해주는 순서로 구성

***Java IO 가 제공해주는 클래스를 적절하게 이용하게 되면 적절하게 다양한 곳에 유용하게 쓸 수가 있다!!***

---

### File 클래스

* java.io.File 클래스는 파일의 크기, 파일의 접근 권한, 파일의 삭제, 이름 변경 등의 작업을 할 수 있는 기능을 제공
* 주의해야할 사항은 디렉토리(폴더) 역시 파일로써 취급된다는 점

---

### File 클래스 생성자
| 클래스 생성자                           | 설명                                       |
|-----------------------------------|------------------------------------------|
| File(File parent, String child)   | parent 디렉토리에 child 라는 파일에 대한 File 객체를 생성 |
| File(String child)                | child 라는 파일에 대한 File 객체를 생성                     |
| File(String parent, String child) | parent 디렉토리에 child 라는 파일에 대한 File 객체를 생성                      |
* 파일 인스턴스를 만들었다고 해서 실제 폴더에 파일이 저장되는 것은 아니다!

---

### File 클래스의 중요 메소드
| File 클래스 메소드             | 설명                                            |
|--------------------------|-----------------------------------------------|
| boolean canRead()        | 파일이 읽기 가능할 경우(파일 커미션) true, 그렇지 않으면 false를 반환 |
| boolean canWrite()       | 파일이 쓰기 가능할 경우(파일 커미션) true, 그렇지 않으면 false를 반환 |
| boolean createNewFile()  | 지정한 파일이 존재하지 않을 경우 파일을 생성                     |
| boolean delete()         | 파일을 삭제. 디렉토리일 경우 비어있는 경우 삭제가 된다.              |
| void deleteOnExit()      | JVM(자바 가상 머신)이 종료될 때, 파일을 삭제                  |
| boolean exist()          | 파일이 존재할 경우 true, 그렇지 않을 경우 false를 반환          |
| String getAbsolutePath() | 파일의 절대 경로를 문자열로 반환                            |
| String getCanonicalPath() | 파일의 전체 경로를 문자열로 반환                            |
| String getName()         | 파일이나 디렉토리의 이름을 반환                             |
| String getParent()       | 부모 경로에 대한 경로명을 문자열로 반환                        |
| File getParentFile()     | 부모 디렉토리를 File의 형태로 반환                         |
| String getPath()         | 파일의 경로를 문자열의 형태로 반환                           |
| boolean isDirectory()    | 디렉토리일 경우 true, 그렇지 않을 경우 false를 반환            |

### 예제
1. FileInfo 클래스
```java
import java.io.File;
import java.io.IOException;

public class FileInfo {
    public static void main(String[] args) {
        if (args.length != 1) {
            System.out.println("사용법 : Java FileInfo 파일이름");
            System.exit(0); // return; 으로 적어줘도 됨
        } // if end

        File f = new File(args[0]);
        if (f.exist()) { // 파일이 존재할 경우
            System.out.println("length : " + f.length());
            System.out.println("canRead : " + f.canRead());
            System.out.println("canWrite : " + f.canWrite());
            System.out.println("getAbsolutePath : " + f.getAbsolutePath());
            
            try {
                System.out.println("getCanonicaPath : " + f.getCanonicaPath());
            } catch (IOException e) {
                System.out.println(e);
            }
            
            System.out.println("getName : " + f.getName());
            System.out.println("getParent : " + f.getParent());
            System.out.println("getPath : " + f.getPath());
        } else { // 파일이 존재하지 않을 경우
            System.out.println("파일이 존재하지 않습니다.");
        }
    } // main end
}
```
2. 출력 결과
```text
"사용법 : Java FileInfo 파일이름"
```
현재는 아규먼트가 0 이기 때문에 맨 위의 코드가 실행됨

여기서 아규먼트 값으로 . 을 넣어주게 되면
![img_12.png](img_12.png)

다음과 같이 출력이 되는 것을 볼 수 있다.
![img_13.png](img_13.png)
FileInfo 뒤에 . 이 붙은 것을 볼 수 있는데 여기서 . 은 현재 자바가 실행되는 경로를 말함.</br>
현재 리렉토리를 의미하므로 읽고 쓸 수 있다는 반환값 trueㅇ 출력하는 것을 볼 수 있다.


---

### Byte Stream
![img_14.png](img_14.png)

---

InputStream, OutputStream
* 추상클래스
* byte 단위 입출력 클래스는 InputStream, OutputStream의 후손이다.

---

### InputStream 이 가지는 중요 메소드
| InputStream 클래스 메소드                        | 설명                                                                        |
|--------------------------------------------|---------------------------------------------------------------------------|
| int available() throws IOException         | 현재 읽을 수 있는 바이트 수를 반환                                                      |
| void close() throws IOException            | 입력 스트림을 닫음                                                                |
| int read() throws IOException              | 입력 스트림에서 한 바이트를 읽어서 int 값으로 반환. 더 이상 읽어들일 내용이 없을 경우 -1을 반환                |
| int read(byte buf[]) throws IOException    | 입력 스트림에서 buf[] 크기만큼을 읽어 buf에 저장하고 읽은 바이트 수를 반환. 더 이상 읽어들일 내용이 없을 경우 -1을 반환 |
| int skip(long numBytes) throws IOException | numBytes로 지정된 바이트를 무시하고, 무시된 바이트 수를 반환                                    |
<br/><br/>

>**Reference**
><br/>부부개발단 - 즐겁게 프로그래밍 배우기.