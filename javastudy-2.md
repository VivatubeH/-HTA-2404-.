기본자료형 vs 참조자료형
-------------------------------------

자바에서 자료형(데이터타입)은 변수에 적재할 데이터가 메모리에 어떤 방식으로 저장되고 프로그램에서 어떻게 처리 되어야 하는지 방식을 
명시적으로 알려주는 키워드가 된다. [ JavaScript에서는 자료형 명시없이 변수에 값을 저장할 때 자료형이 결정되지만, 자바는 컴파일 언어이기
때문에 자료형 명시를 컴파일 전에 미리 해줘야 한다. ]

- 기본자료형(prmitive type) 
 + 기본 자료형 타입의 변수에는 <strong>'실제 값'</strong>이 저장된다.
 + 모두 소문자로 시작되고, 객체 타입이 아니므로 null값은 가질 수 없다. ( 착각하기 쉬운데 String은 기본자료형이 아닌 객체이다. )
 + 변수의 선언과 동시에 메모리에 생성되며, 모든 기본자료형 값은 메모리의 스택영역에 저장된다. 
 + 예)
   ![image](https://github.com/user-attachments/assets/99fbf7c5-062d-466b-9998-3ed1024bfdef)

- 참조자료형(Reference Type)
  + 참조형 타입은 기본자료형 8가지를 제외한 나머지를 말한다.
  + 기본적으로 제공되는 클래스들, 프로그래머가 만든 클래스, 배열 및 열거 타입 모두 참조형에 해당된다.
  + 참조자료형은 값이 아닌 자료가 저장된 공간의 <strong>'주소'</strong>가 저장된다.
  + 실제 값은 다른 곳에 있지만 주소를 참조해서 값을 가져올 수 있다.
  + 실제 값은 heap에 저장되고, 주소값을 갖는 변수만 스택에 저장된다.
  + null 값을 가질 수 있다.
  + 예)
  ![image](https://github.com/user-attachments/assets/1d641a86-1bf5-43c7-bb42-f21f63bd751c)

- 기본자료형과 참조자료형이 생성되는 영역
  ![image](https://github.com/user-attachments/assets/bd40e78e-d522-442a-ae9b-eaa9ea0a39c5)

  String
  ----------------------------------------------
  앞에서 말했다시피, String은 기본자료형이 아닌 참조자료형이다.
  String을 문자열을 저장할 수 있는 클래스로만 알고 있는데 String이 가지는 더 중요한 특성과 주의점에 대해 알아보자.

  ```java
  public final class String implements java.io.Serializable, Comparable<String>, CharSequence {

    @Stable
    private final byte[] value;
    
    private final byte coder;

    private int hash; // Default to 0
    
    ...
    public boolean equals(Object anObject) {
      if (this == anObject) {
        return true;
      }
      if (anObject instanceof String) {
        String aString = (String)anObject;
        if (coder() == aString.coder()) {
          return isLatin1() ? StringLatin1.equals(value, aString.value)
                              : StringUTF16.equals(value, aString.value);
        }
      }
      return false;
    }
  }
  ```
  
  - 불변 자료형이다.(Immutable)
    + 위의 코드를 보면 알 수 있듯이 String 타입은 final 값, 즉 변하지 않는 상수값을 가지게 된다.
    + 상수라는 말의 정확한 의미는, '한 번 정해지면 변경이 불가능한'으로 해석하는 게 맞다.
    ```java
      String a = "안녕"; // 이 때 a가 가리키는 문자열은 안녕이지만,
      a = "def"; // 이 때는, "def"라는 새로운 객체가 생성된다.
    ```
    + 고로, String 타입의 객체 대신 String 타입을 변경해야 할 일이 많다면 StringBuilder나 StringBuffer 클래스를 사용하자.
   
  - 문자열을 비교할 땐 반드시 equals를 사용한다.
    + 위의 코드를 보면 알다시피, String 타입의 비교는 객체의 비교, 즉 주소값의 비교가 된다.
    ```java
      String a = "안녕"; 
      String b = "안녕";
      String c = new String("안녕"); 
    ```
    + 위의 코드에서 a와 b는 같은 객체를 가리키지만 ( 문자열은 한 번 생성되면 이미 생성된 문자열을 참조하도록 되어 있다. )
    + b와 c는 다른 객체를 가리킨다. ( 그러나 new String()은 다른 객체를 생성하게 된다. )
   
  동등성(equality) vs 동일성(identity)
  -----------------------------------------
  앞에서 말했듯, String 타입의 비교가 바로 동일성 vs 동등성의 차이가 잘 드러나는 예시가 된다.
   ```java
      String a = "안녕"; 
      String b = "안녕";
      String c = new String("안녕"); 
    ```

   + 동일성: 동일하다. 즉 두 객체가 완전히 같아야 한다. ( 주소값까지 같아야 한다. )
     - 앞의 예시에서 a와 b는 동일하다. (동일성)
       
   + 동등성: 동등하다. 즉 두 객체의 '정보'가 같아야 한다. ( 주소가 다르더라도 내용이 같으면 동등 )
     - 뒤의 예시에서 b와 c는 동등하다. (동등성)
    
  Object Class
  ---------------------------------
  Object 클래스는 모든 클래스의 조상인 클래스, 즉 상속계층도에서 가장 최상위에 속한 클래스이다.
  우리가 만든 클래스도 보이지는 않지만, Object 클래스를 상속하게 된다.

  ```java
  class Glass // 이렇게 적으면
  class Glass extends Object // 컴파일러가 자동으로 extends Object를 추가시킨다.
  ```

   근데, 클래스를 사용하려면 import 해야하는데 왜 Object는 import 하지 않을까?
  이는 java.lang 패키지에 속한 클래스이기 때문이다. java.lang 패키지의 클래스는 컴파일러에 의해 자동 import 된다.
   ```java
   import java.lang.*; // 이 코드가 숨어있다.
   ```

  equals의 주요 메소드 3개인 toString(), equals(), hashCode()에 대해 알아보자.
  - toString()
    + Object로부터 상속받아 재정의 할 수 있다. 재정의 하기 전에는 클래스명@해시코드값 [ 예: Book@16f65612 ] 이 출력된다.
    + 용도는 객체의 정보를 문자열로 형 변환하는 것이 용도이다.
   
  - equals()
    + Object로부터 상속받아 재정의 할 수 있다. 재정의 하기 전에는 두 객체의 주소값을 비교해서 true/false를 반환한다. 
    + 재정의 전에는 동일성 비교이지만, 재정의 후에는 보통 동등성 비교로 재정의한다.
   
  - hashCode()
     + 객체의 주소값을 해시 코드로 변환한 후 반환한다.
     + 해시코드는 엄밀히 말하면 주소값이 아닌, '주소값을 기반으로 만든 중복되지 않는 숫자값'이다.
     + equals를 재정의 할 때는 반드시 hashCode를 재정의해야 한다. 

  ![image](https://github.com/user-attachments/assets/b7d1cd5d-c4ba-40c6-9ae8-e5465eb31e1f)

 + equals만 재정의한다 -> hashCode값이 다르니 다른 객체로 판단. -> equals 값이 true여도 다른 객체로 판단되는 문제 발생.

 JCF(Java Collections Framework)
 -------------------------------------
- 컬렉션 : 여러 객체(데이터)를 담을 수 있는 데이터 그룹 또는 집합
- 프레임워크 : 표준화된 프로그래밍 방식 + 라이브러리

+ 따라서, 컬렉션 프레임워크라는 건 데이터를 다룰 수 있는 클래스들을 표준화해서 라이브러리 형태로 제공한 자료구조를 말한다.

#### 컬렉션 프레임워크의 장점
- 검증된 최적화된 API를 제공해준다. [ 품질 향상, 개발 시간 단축 ]
- 가변적인 저장 공간 제공 [ 배열의 특성과는 대비된다. ]

![image](https://github.com/user-attachments/assets/11bd9153-66e0-4d7b-843d-00f344435add)

#### 컬렉션 프레임워크의 핵심 컬렉션들
- List : 순서가 있는 데이터 집합이다. 중복은 허용한다. ( 예: ArrayList, LinkedList... )
- Map : Key - Value 쌍으로 이루어진 데이터 집합이다. 순서는 유지되지 않는다. 키는 중복 허용 x이지만 값은 중복이 허용된다.
  예) HashMap, TreeMap...
- Set : 순서가 없는 데이터 집합이다. 중복은 허용하지 않는다. ( 예: HashSet, LinkedHashSet, TreeSet ... )

Array vs List
-----------------------------
Array는 메모리 상에 연속적으로 데이터를 저장하지만, List는 메모리 상에 비연속적으로 데이터가 저장된다. 

+ Array
  ![image](https://github.com/user-attachments/assets/cffc93d4-9172-4000-9a23-ec97a9bbe97e)
- 순차적으로 저장된 데이터를 참조하므로 Index를 사용한다.
- 고정된 크기를 가지며, 논리적 저장 순서와 물리적 저장 순서가 일치하게 된다.

+ List
![image](https://github.com/user-attachments/assets/dda35c71-29c4-422c-a516-e05954dd1ee3)
+ 데이터가 순서대로 구성되어 있지만, 메모리 상에 연속적으로 저장되지는 않는다. ( 연결 리스트라 불린다. )
+ 가변적인 크기를 가진다.

# 그럼 ArrayList는 뭔데?
+ ArrayList는 List와 Array의 특성을 고루 가지고 있다.
+ 즉, 크기가 가변적인 Array(배열)인 것이다.

ArrayList vs LinkedList
--------------------------------
+ ArrayList와 LinkedList는 둘 다 List 인터페이스의 구현체이다.

![image](https://github.com/user-attachments/assets/91e2270b-05ed-4906-be93-263e4d31e163)
- ArrayList는 배열을 이용하므로 값을 삽입/삭제하려면 기존의 배열을 복사해서 이동해야한다.
- 이는 시스템의 성능 저하로 이어진다.

![image](https://github.com/user-attachments/assets/c93577d2-f471-467e-ab76-a34b55398bcc)
- LinkedList는 포인터를 이용하므로 값을 삽입/삭제 시 노드가 가리키는 포인터만 변경하면 된다.
- 이는 처리 속도의 향상으로 이어진다.
- 그러나 특정 자료를 탐색할 때 시간 소요가 많아진다. ( 어딨는지 찾으러 다녀야 함 )

Generic
-----------------------------
제네릭(Generic)은 데이터 타입을 외부에서 정해줄 수 있는 자바의 문법을 말한다.
![image](https://github.com/user-attachments/assets/e98f6f41-f4a7-4266-a56d-e62a23a6f09a)

- 제네릭은 변수 선언 시 타입을 지정해주는 것처럼, 객체에 대해서도 타입을 지정해주는 것이다.
- 제네릭을 사용하면 컴파일 시에 타입 검사를 하기 때문에 예외를 방지할 수 있다.
- Object로 타입을 선언하면 모든 클래스 타입을 다 받을 수 있지만, 에러는 런타임시에야 발견할 수 있다.

#### Generic 사용 시 주의점
- 제네릭 타입으로 객체를 생성할 수 없다. [ 객체 생성시에는 어떤 객체를 생성할 지 말해줘야 한다. ]
- static 멤버에 제네릭을 사용할 수 없다. [ static 멤버는 클래스가 로드될 때 생성되므로 자료 타입이 정해져 있어야만 한다. ]


Wrapper Class
-------------------------------------------------
기본 자료형을 객체로서 다루기 위해서 만든 클래스를 말한다.

![image](https://github.com/user-attachments/assets/65d5df30-3074-455b-b81d-15369eedcaf7)

- 박싱 : 기본 타입의 값으로 래퍼 객체를 만드는 것
- 언박싱 : 래퍼 객체에서 기본 타입의 값을 얻어내는 것.

- 래퍼 타입과 기본 타입간에는 오토 박싱, 오토 언박싱이 지원된다.
```
public class Wrapper_Ex {
    public static void main(String[] args)  {
        Integer num = 17; // 자동 박싱
        int n = num; //자동 언박싱
        System.out.println(n);
    }
}
```

- 래퍼 클래스도 내부의 값을 변환할 때는 equals 메소드를 사용해야만 한다. [ ==는 래퍼 객체들의 참조 주소를 비교하기 때문에 ]
- 그러나, 래퍼 클래스와 기본 자료형간의 비교는 ==로 가능하다. [ 컴파일러가 자동으로 처리해준다. ]
