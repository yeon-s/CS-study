# JAVA


# JAVA란?

- 제임스 고슬링과 다른 연구원들이 개발한 객체 지향적 프로그래밍 언어. 기존의 WWW가 가지고 있던 한계 극복의 필요성으로 탄생. 초기의 자바는 가전 제품에 탑재할 프로그래밍 언어로 개발되었지만 지금은 스마트폰을 비롯, 각종 장비와 데스크탑에서 실행되는 애플리케이션을 개발하는 중추적인 언어가 되었다.
- 특징 : 자바 컴파일러는 자바 언어로 작성된 프로그램을 바이트 코드라는 특수한 바이너리 형태로 변환한다. 변환된 바이트 코드를 실행하기 위해서는  JVM이라는 특수한 가상머신이 필요하다. 이 가상머신은 자바 바이트코드를 어느 플랫폼에서나 동일한 형태로 실행시킨다. 때문에 자바로 개발된 프로그램은 CPU나 운영체제의 종류에 관계없이 JVM을 설치할 수 있는 시스템 어디서나 실행할 수 있다.

### java의 장단점

- 장점
    - 운영체제에 독립적이다.
        - JVM에서 동작하기 때문에, 특정 운영체제에 종속되지 않는다.
    - 객체지향 언어이다.
        - 객체지향적으로 프로그래밍 하기위해 여러 언어적 지원을 하고 있다.(캡슐화, 상속, 추상화, 다형성 등)
        - 객체지향 패러다임의 특성상 비교적 이해하고 배우기 쉽다.
    - 자동으로 메모리 관리를 해준다.
        - JVM에서 Garbage Collector라고 불리는 는 데몬 쓰레드에 의해 가비지 컬렉션이 일어난다. GC로 인해 별도의 메모리 관리가 필요 없으며 비즈니스 로직에 집중할 수 있다.
    - 오픈소스이다.
        - 정확히 말하면 OpenJDK가 오픈소스이다.
        - 많은 java개발자가 존재하고 생태계가 잘 구축되어있다. 덕분에 오픈소스 라이브러리가 풍부하며 잘 활용한다면 짧은 개발 시간 내에 안정적인 애플리케이션을 쉽게 구현할 수 있다.
    - 멀티스레드를 쉽게 구현할 수 있다.
        - 자바는 스레드 생성 및 제어와 관련된 라이브러리 API를 제공하고 있기 때문에 실행되는 운영체제에 상관없이 멀티 스레드를 쉽게 구현할 수 있다.
    - 동적 로딩을 지원한다.
        - 애플리케이션이 실행될 때 모든 객체가 생성되지 않고, 각 객체가 필요한 시점에 클래스를 동적 로딩해서 생성한다. 또한 유지보수시 해당 클래스만 수정하면 되기 때문에 전체 애플리케이션을 다시 컴파일할 필요가 없다. 따라서 유지보수가 쉽고 빠르다.
- 단점
    - 비교적 속도가 느리다
        - 자바는 한번의 컴파일링으로 실행 가능한 기계어가 만들어지지 않고 JVM에 의해 기계어로 번역되고 실행하는 과정을 거치기 때문에 C나 C++의 컴파일 단계에서 만들어지는 완전한 기계어보다는 속도가 느리다. 그러나 하드웨어의 성능 향상과 바이트 코드를 기계어로 변환해주는 JIT 컴파일러 같은 기술 적용으로 JVM의 기능이 향상되어 속도의 격차가 많이 줄어들었다.
    - 예외처리가 불편하다.
        - 프로그래머 검사가 필요한 예외가 등장한다면 무조건 프로그래머가 선언을 해줘야 한다.

[https://yolojeb.tistory.com/17](https://yolojeb.tistory.com/17)

# 자바 컴파일 과정을 설명

![Untitled](/image/java/Untitled.png)

1. .java 파일을 build하면 자바 컴파일러의 javac라는 명령어를 사용해서 .class파일을 생성한다.(.class파일은 바이트 코드로 아직 컴퓨터가 읽을 수 없는 반기계어이다.)
2. .class 파일은 Class Loader에 의해 JVM내로 로드된다.
3. 실행엔진(Execution Engine의 인터프리터 또는 JIT)을 이용해 byte code→binary code로 변환한다.
4. 변환된 binary code(기계어)는 메모리(Runtime Data Area)에 배치된다.
5. Runtime Data Area에 올라간 파일은 실행엔진(Execution Engine)에 의해 실행

이와 같은 과정을 통해 JAVA 파일이 컴파일되고, JVM에 의해 해당 OS에 맞게 변환시켜 컴퓨터가 읽을 수 있도록 만들어준다.

메모리 영역에 올라가는 것들

![Untitled](/image/java/Untitled1.png)

- Interpreter : 자바 바이트 코드를 한줄 씩 실행, 속도 느리다.
- JIT Compiler : Interpreter의 단점을 보완, 전체 바이트 코드를 컴파일, 속도가 느리지만 캐시를 사용해서 한번 컴파일 하면 다음에는 빠르게 수행된다.
- 한번만 실행하면 되는 코드는 인터프리팅 하는 것이 유리

• [https://aljjabaegi.tistory.com/387](https://aljjabaegi.tistory.com/387)

• [https://jeong-pro.tistory.com/148](https://jeong-pro.tistory.com/148)

# JVM, JRE, JDK의 차이

### JVM

자바 가상머신

JVM은 자바 소스코드로부터 만들어지는 자바 클래스 파일(.class)을 실행할 수 있다. JVM은 플랫폼에 의존적이다. 즉, 리눅스의 JVM과 윈도우의 JVM은 다르다. 단, 컴파일된 바이너리 코드는 어떤 JVM에서도 동작시킬 수 있다.

### JRE

자바 실행환경

JRE는 JVM이 자바 프로그램을 동작시킬 때 필요한 라이브러리 파일들과 기타 파일들을 가지고 있다. JRE는 JVM의 실행환경을 구현했다고 할 수 있다.

JRE에는 자바 클래스 라이브러리+ 자바 클래스 로더+ 자바 가상머신

### JDK

자바 개발도구

JDK는 JRE+개발을 위해 필요한 도구(javac,java 등)들을 포함한다.

[https://www.itworld.co.kr/news/110768](https://www.itworld.co.kr/news/110768)

# String, StringBuffer, StringBuilder

공통점 : String을 저장하고 관리하는 클래스

차이점 : 불변 ⇒ String, 가변 ⇒ StringBuffer, StringBuilder

### String

- 리터럴을 통해 생성되면 그 인스턴스의 메모리 공간은 절대 변하지 않는다.
    
    String str = “literal”   //리터럴로 생성하는 방식
    
- 문자열 변수에 변화를 주면 메모리 공간 내의 값이 변하는 것이 아니라 String Pool이라는 공간 안에 메모리를 할당 받아 새로운 String 클래스 객체를 만들어서 문자열을 나타낸다.
- 따라서 변화가 나타날 때마다 1. 기존 문자열은 가비지 컬렉터에 의해 제거되어야 하고 2. 계속해서 문자열 객체를 만드는 오버헤드가 발생해서 성능이 떨어진다.

### String 클래스가 적절한 경우

- String 클래스는 문자열 연산이 적고 자주 조회하는 경우 사용하면 좋다.
- 불변하기 때문에 멀티 쓰레드 환경에서 동기화를 신경쓸 필요 없다.

### StringBuffer, StringBuilder

- String과 다르게 문자열 연산시 클래스는 한번만 만들고(new) 메모리의 값을 변경시켜서 문자열을 변경한다.

### StringBuffer

- 멀티 쓰레드 환경에서 동기화 o. 즉, thread-safe하다.
- 내부적으로 append 등 모든 메소드에 대해 synchronized 키워드가 붙어있다.

### StringBuilder

- 동기화 x, 멀티쓰레드 환경에서 적합하지 않다.
- 싱글 쓰레드 환경에서 StringBuffer에 비해 연산처리가 빠르다.

[https://jeong-pro.tistory.com/85](https://jeong-pro.tistory.com/85)

# StringBuilder

## 내부 작동

- 가변 길이의 배열과 같이 작동
- StringBuilder 객체가 생성되면 일정크기(16)를 갖는 빈 문자열 객체가 생성된다.
- 여기에 사용자가 문자열을 대입하면 미리 준비된 배열 공간에 문자열이 들어간다.
- length() 메소드는 실제로 채워진 문자들의 개수를 돌려줌.
- capacity() 메소드는 StringBuilder 객체의 저장 용량을 돌려줌. 초기 저장 용량은 16이고 필요시 자동으로 증가된다. StringBuilder 클래스의 생성자를 이용해서 초기 저장 용량을 지정할 수 있다.

![Untitled](/image/java/Untitled2.png)

[https://m.blog.naver.com/itinstructor/100203105622](https://m.blog.naver.com/itinstructor/100203105622)

# OOP의 4가지 특징

캡상추다

- 캡슐화 : 특정 객체가 독립적으로 역할을 수행하기 위해 필요한 데이터와 기능을 하나로 묶은 것(모듈화), 객체 안의 정보에 직접 접근을 허용하지 않고 필요에 따라 확인할 수 있는 인터페이스를 외부에 공개한다.(은닉)
    - ex) 접근제어자
- 상속 : 상위 개념의 특징을 하위 개념이 물려받는 것
    - 기능의 일부분을 변경하는 경우 자식 클래스에서 상속받아 수정 및 사용
    - 상속은 캡슐화를 유지, 클래스의 재사용이 용이하도록 해준다.
- 추상화 : 객체들의 공통적인 특징(속성,기능)을 모아 하나의 클래스로 다룬다. 각 객체의 구체적인 개념에 의존하지 않고 추상적인 개념에 의존해야 설계를 유연하게 변경할 수 있다.
    - 인터페이스로 클래스들의 공통적인 특성(변수, 메소드)들을 묶어 표현한 것
- 다형성 : 하나의 타입으로 여러가지 참조 변수를 사용할 수 있는 것
    - 오버로딩, 오버라이딩

# 오버로딩과 오버라이딩의 차이

### Overloading

- 개념: 하나의 클래스에서 같은 이름의 메소드를 여러개 가질 수 있다. 단, 메서드의 파라미터(개수나 타입)가 달라야한다. (리턴타입은 상관없다)
- 오버로딩 있는 이유 : 유사한 일을 수행하면서 인자만 다른 메소드들을 각기 다른 이름으로 준다면 불편하다. 예를들어 키보드를 문서용 키보드, 게임용 키보드를 따로 만들면 비효율적이다.

### Overriding

- 개념 : 슈퍼클래스를 상속받은 서브 클래스에서 슈퍼 클래스의 메소드를 같은 이름, 같은 반환값, 같은 인자로 메소드 내의 로직들을 새롭게 정의하는 것. 오버라이딩을 이용해서 같은 이름이지만 구현하는 클래스마다 다른 역할을 하는 메소드를 정의한다. ex) 키보드라는 모양을 가졌지만 문서를 작성하고 게임도 한다.

[https://brunch.co.kr/@kd4/4](https://brunch.co.kr/@kd4/4)

# HashMap과 TreeMap의 차이

### Map

- key와 value를 가진 집합으로, 중복허용x

| 기능 | HashTable | HashMap | ConcurrentHashMap |
| --- | --- | --- | --- |
| key,value 
null 허용 여부 | X | O | X |
| 여러 쓰레드 안전 여부 | O
(put,get과 같은 주요 메소드에 
syncronized 키워드가 선언되어 있다 | X | O(map 전체에 lock을 걸지 않는다. 부분적으로 걸기 때문에 효율이 더 좋다.) |

[https://jdm.kr/blog/197](https://jdm.kr/blog/197)

| HashMap | TreeMap |
| --- | --- |
| 순서 유지 x | key 값에 따라 정렬 |
| Hashing으로 내부 구현 | Red-Black 트리로 내부 구현 |
| key: null 1개 허용,
value: 허용 | key : null 허용 x,
value : 허용 |
| O(1) get,put 같은 기본 연산에 일정한 시간 성능 | O(log n) 시간 |
| TreeMap보다 빠르다 | HashMap 보다 느리다 |
| Map 인터페이스 구현 | NavigableMap 인터페이스 구현 |
- 특별한 이유가 없다면 검색성능이 좋은(O(1)) HashMap을 사용하자
- 키 값이 일정한 수준대로 iterate 하려고 한다면 TreeMap을 사용하자. 하지만 랜덤 접근에 대해서는 O(lonN)의 성능을 지니므로 많은 데이터를 넣을 경우 좋지 않은 성능이 나올 수 있다.
- 입력 순서가 중요하면 LinkedHashMap을 사용하자. 하지만 랜덤 접근에 대해 O(n)의 성능을 지니므로 많은 데이터를 입력할 경우 사용하지 않는 것이 좋다.

[https://genie247.tistory.com/11](https://genie247.tistory.com/11)

# 자바의 메모리구조

![Untitled](/image/java/Untitled3.png)

### Method Area(메소드 or 스태틱 영역)

- 모**든 스레드가 공유하는 영역으로 JVM이 시작될때 생성, 프로그램 종료시 해제.**
- **static 변수, final class변수 등이 생성**
- JVM이 읽어 들인 각각의 클래스와 인터페이스에 대한 런타임 상수 풀, 멤버 변수(필드)와 메서드 정보, static 변수, 메서드의 바이트코드 등을 보관한다.
- class의 구조 정보가 들어간다.
- 멤버변수(static), 데이터 타입, 접근제어자 같은 필드 정보
- 메소드 이름, 리턴 타입, 파라미터, 접근 제어자 같은 **메서드 정보**
- type정보(interface나 class), Contant Pool(상수 풀:문자 상수, 타입, 필드, 객체참조)

### Runtime Constant Pool

- 각 클래스와 인터페이스의 상수뿐만 아니라, 메서드와 필드에 대한 모든 레퍼런스까지 담고 있는 테이블이다.
- 즉, 어떤 메서드나 필드를 참조할 때 JVM은 런타임 상수 풀을 통해 해당 메서드나 필드의 실제 메모리상 주소를 찾아서 참조한다.

### Heap Area

- **new 키워드로 생성된 객체와 배열을 저장**
- 메소드 영역에 로드된 클래스만 생성가능하다.
- 데이터가 동적으로 생성,소멸
- 참조하는 변수나 필드가 없으면 의미 없는 객체가 되어 **GC의 대상이 된다.**

### Perm 영역

- JDK 8부터 Permanent Heap 영역이 제거되었다.
    - 대신 Metaspace 영역이 추가되었다.
    - Perm은 JVM에 의해 크기가 강제되던 영역이다.
- Metaspace는 Native Memory영역으로 OS가 자동으로 크기를 조절한다.
    - 옵션으로 Metaspace의 크기를 줄일 수도 있다.
- 그 결과 기존과 비교해 큰 메모리 영역을 사용할 수 있게 되었다.
    - Perm 영역 크기로 인한 java.lang.OutOfMemoryError를 더 보기 힘들어진다.
- Class의 Meta정보, Method의 Meta정보, Static 변수와 상수 정보들이 저장되는 공간으로 흔히 메타데이터 저장 영역이라고도 한다. java8부터는 Native 영역으로 이동하여 Metaspace 영역으로 변경되었다.(다만, 기존 Perm 영역에 존재하던 Static Object는 Heap 영역으로 옮겨져서 GC의 대상이 최대한 될 수 있도록 하였다.)

### Stack Area

- 지역변수, 파라미터(매개변수), 리턴 값, 연산에 사용되는 임시 값이 생성되는 영역
- Method 정보, 메소드 호출 시 사용하는 지역변수 데이터 등을 저장한다. {}가 끝나는 동안 유지된다.
- **기본 타입 변수는 스택영역에 직접 값을 가진다.**
- ex) int a=10;    스택영역에 이름이 a인 공간을 생성, 10입력
- 참조타입 변수는 힙 영역이나 메소드 영역의 객체 주소를 가진다.
- ex) Person p = new Person();     Person p는 스택에 생성, new로 생성된 인스턴스는 heap에 생성
    - 스택에 생성된 p값으로 heap영역에 생성된 객체를 참조한다.(가리킨다)
- 각 쓰레드 마다 존재, 쓰레드가 시작될때 할당

### PC 레지스터

- 쓰레드가 생성될때마다 생성되는 영역
- **현재 쓰레드가 실행되는 부분의 주소와 명령을 저장하고 있다.**
- 이것을 이용해서 쓰레드를 돌아가면서 수행할 수 있다.
- 쓰레드가 시작될 때 생성, 현재 수행중인 JVM의 명령의 주소를 가진다.

### Native Method Stack

- 자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역
- 보통 c/c++등의 코드를 수행하기 위한 스택

### 쓰레드가 생성되었을 때

- Method Area, Heap Area : 모든 쓰레드가 공유
- Stack, PC 레지스터, Native Method Stack : 각각의 쓰레드마다 생성, 공유X

[https://hoonmaro.tistory.com/19](https://hoonmaro.tistory.com/19)

[https://jeong-pro.tistory.com/148](https://jeong-pro.tistory.com/148)

[https://steady-snail.tistory.com/67](https://steady-snail.tistory.com/67)

# Main 메소드에 static을 사용하는 이유

- static은 java 프로그램이 실행하기 전에 static 함수나 static 변수를 첫 단계로 메모리에 올려 프로그램을 실행시킨다. (static이 실행시 1순위)
- 프로그램이 종료될 때까지 사라지지 않는다.
- main함수가 실행되기 위해서는 메모리에 미리 올라가야한다.
- 메모리에 올라가 있지 않으면 시작점인 main() 메소드를 호출하려고 하는데 메모리에는 main이 없기 때문에 실행을 할 수가 없다.
- 그래서 main 메소드는 누군가 호출하기 전에 미리 메모리에 있어야 하기 때문에 static을 붙이는 것이다.

# Equals(), ==

- equals()는 **객체간의 내용(값)**을 비교할 수 있는 **‘메소드’**이다.
- ==는 **객체의 주소값**을 비교하는 **‘연산자’**이다.

# 접근 제어자의 종류와 특징

| 제어자 | 같은 클래스 | 같은 패키지 | 자손 클래스 | 전체 |
| --- | --- | --- | --- | --- |
| public | O | O | O | O |
| protected | O | O | O |  |
| default | O | O |  |  |
| private | O |  |  |  |

| 접근 제어자 | 설명 |
| --- | --- |
| public | 프로젝트 안에서 자유롭게 사용가능하다(제한이 전혀 없다) |
| protected | 같은 패키지 내, 다른 패키지의 자손 클래스에서 접근 가능 |
| default | 같은 패키지 내에서만 접근 가능 |
| private | 같은 클래스 내에서만 접근 가능 |

| 대상 | 사용 가능한 접근 제어자 |
| --- | --- |
| 클래스 | public, (default) |
| 메서드 | public, protected, (default), private |
| 멤버변수 | public, protected, (default), private |
| 지역변수 | 없음 |

### 접근 제어자를 이용한 캡슐화

- 접근 제어자를 사용하는 이유는 클래스의 내부에 선언된 데이터를 보호하기 위해서.
- 데이터가 유효한 값을 유지하도록 또는 외부에서 변경하지 못하도록 하기 위해서는 외부로부터의 접근을 제한하는 것이 필요.
- **객체지향 개념에서는 캡슐화라고 한다.**

### 제어자를 조합해서 사용할 때 주의사항

1. 메소드에 static과 abstract를 함께 사용할 수 없다.
- static 메소드에는 몸통이 있는 메소드에사만 사용할 수 있기 때문에
1. 클래스에 abstract와 final을 동시에 사용할 수 없다.
- 클래스에 사용되는 final은 클래스를 확장할 수 없다는 의미이고, abstract는 상속을 통해서 완성되어야 한다는 의미이므로 모순
1. abstract 메소드의 접근제어자가 private일 수 없다.
- abstract 메소드는 자손클래스에서 구현하기 위해 접근해야 하기 때문
1. 메소드에 private와 final을 같이 사용할 필요는 없다.
- 접근제어자가 private인 메소드는 오버라이딩 될 수 없기 때문. 둘 중 하나만 사용해도 의미가 충분하다.

[https://csw7432.tistory.com/entry/Java-접근제어자-Access-Modifier](https://csw7432.tistory.com/entry/Java-%EC%A0%91%EA%B7%BC%EC%A0%9C%EC%96%B4%EC%9E%90-Access-Modifier)

# 멤버 변수 & 지역 변수

### 멤버 변수

- 클래스 블록 영역에 선언되는 변수

### 멤버 변수 - static

- 클래스 변수, 공통변수, 정적변수, 전역변수
- 컴파일시에 메모리 할당, 프로그램 종료시 메모리 해제
- 클래스 전체에서 사용 가능
- 클래스의 모든 인스턴스가 같은 저장공간을 가리킨다.
- 저장공간: Method Area(영역)
- 생성 시기 : JVM이 클래스를 로드할 때

### 멤버 변수 - static X

- 인스턴스 변수(주소값이 할당된 상태)
- 객체 생성시마다 따로 저장되는 변수
- 저장 공간: Heap 영역
- GC가 관리한다.
- 생성 시기 : new 생성자()

### 지역 변수

- 메소드 블록 영역에 선언되는 변수
- 저장공간 : Stack 영역

[https://easywebs.tistory.com/29](https://easywebs.tistory.com/29)

[https://sublivan.tistory.com/5](https://sublivan.tistory.com/5)

# non-static 멤버와 static멤버의 차이

- static은 메서드나 변수에 붙을 수 있다.
- static 사용의 이점

전역적으로 쉽게 재사용하는 멤버나 잘 변하지 않는 변수나 메서드를 사용할 때 주로 사용.

만들어 놓고 클래스 호출, 객체 생성을 따로 할 필요없이 바로바로 사용할 수 있기 때문에 사용성이 좋다.

- static 사용의 단점

static은 메모리 자원을 할당해놓고 사용하는 것이기 때문에 너무 많이 사용하면 메모리를 많이 차지하게 되어 프로그램이 무거워진다.

### static 멤버

- 공간적 특성 : 멤버는 클래스당 하나가 생성된다.
    - 멤버는 객체 내부가 아닌 별도의 공간에 생성된다.
    - 클래스 멤버라고 부른다.
- 시간적 특성 : 클래스 로딩 시에 멤버가 생성된다.
    - 객체가 생기기 전에 이미 생성된다.
    - 객체가 생기기 전에도 사용이 가능하다(즉, 객체를 생성하지 않고도 사용할 수 있다.)
    - 객체가 사라져도 멤버는 사라지지 않는다.
    - 멤버는 프로그램이 종료될 때 사라진다.
- 공유의 특성 : 동일한 클래스의 모든 객체들에 의해 공유된다.

### non-static  멤버

- 공간적 특성: 멤버는 객체마다 별도로 존재한다.
    - 인스턴스 멤버라고 부른다.
- 시간적 특성 : 객체 생성 시에 멤버가 생성된다.
    - 객체가 생길 때 멤버도 생성된다.
    - 객체 생성 후 멤버 사용이 가능하다.
    - 객체가 사라지면 멤버도 사라진다.
- 공유의 특성 : 공유되지 않는다.
    - 멤버는 객체 내에 각각의 공간을 유지한다.

## static의 활용

### 1. 전역 변수와 전역 함수를 만들 때 활용

- 캡슐화 원칙
    - 자바에서는 c/c++과 달리 어떤 변수나 함수도 클래스 바깥에 존재할 수 없으며 클래스의 멤버로 존재하여야한다.
- 그러나 응용프로그램 작성 시 모든 클래스에서 공유하는 전역변수나 모든 클래스에서 언제든지 호출할 수 있는 전역 함수를 만들어 사용할 필요가 생긴다.
    - static은 이런 문제의 해결책이다.
    - ex) java.lang.math 클래스
        - jdk와 함께 배포되는 클래스
        - 객체를 생성하지 않고 바로 호출할 수 있는 수학 계산용 상수와 메소드를 제공

### 2. 공유 멤버를 만들고자 할 때 활용

static으로 선언된 필드나 메서드는 모두 이 클래스의 각 객체들의 공통 멤버가 되며 객체들 사이에서 공유된다.

## static 메서드의 제약 조건

### 1. static 메서드는 오직 static 멤버만 접근할 수 있다.

static 메서드는 객체가 생성되지 않은 상황에서도 사용이 가능하므로 객체에 속한 인스턴스 메서드,인스턴스 변수 등을 사용할 수 없다.

- static 멤버들만 사용이 가능하다.
- 그러나 인스턴스 메서드는 static 멤버들을 모두 사용할 수 있다.

### 2. static 메서드에서는 this 키워드를 사용할 수 없다.

this는 호출 당시 실행중인 객체를 가리키는 레퍼런스이다.

- 따라서 객체가 생성되지 않은 상황에서도 클래스 이름을 이용하여 호출이 가능한 static 메서드는 this를 사용할 수 없다.

[https://gmlwjd9405.github.io/2018/08/04/java-static.html](https://gmlwjd9405.github.io/2018/08/04/java-static.html)

# final 키워드(final/finally/finalize)

### final 키워드

- 변수나 메서드 또는 클래스가 **‘변경 불가능’** 하도록 만든다.
    - 원시 변수에 적용시
        - 해당 변수의 값 변경X
        - final int number =1;
    - 참조 변수에 적용시
        - 참조 변수가 힙 내의 다른 객체를 가리키도록 변경X
    - 메소드에 적용시
        - 해당 메서드를 오버라이딩 X
    - 클래스에 적용 시
        - 해당 클래스를 상속받을 수 X

### finally 키워드

- try/catch 블록이 종료될 때 항상 실행될 코드 블록의 정의하기 위해 사용
    - finally는 선택적으로 try 혹은 catch 블록 뒤에 사용
    - finally 블록은 예외가 발생하더라도 항상 실행
        - 단, JVM이 try블록 실행 중에 종료되는 경우는 제외
    - finally 블록은 종종 뒷마무리 코드를 작성하는 데 사용

### finalize() 메소드

**GC가 더이상의 참조가 존재하지 않는 객체를 메모리에서 삭제하겠다고 결정하는 순간 호출**

Object 클래스의 finalize()메서드를 오버라이드해서 맞춤별 GC를 정의할 수 있다.

```
protected void finalize() throws Throwable {
/* 파일 닫기, 자원 반환 등등 */
}
```

개발자가 마음대로 오버라이딩 하면 안좋다

[https://gmlwjd9405.github.io/2018/08/06/java-final.html](https://gmlwjd9405.github.io/2018/08/06/java-final.html)

# 인터페이스와 추상 클래스의 차이(Interface vs Abstract Class)

## 추상 메서드

- abstract 키워드와 함께 원형만 선언되고 코드는 작성되지 않는 메소드

```
public abstract String getName(); // 추상메서드
	public abstract int getAge() {return 11; }; // 추상 메서드 x
```

## 추상 클래스

- 개념: abstract 키워드로 선언된 클래스
    - **추상 메서드를 최소 한개 이상** 가지고 abstract로 선언된 클래스
        - 최소 한개의 추상 메서드를 포함하는 경우 **반드시 추상 클래스로 선언**
    - **추상 메서드가 없어도 abstract로** 선언된 클래스
        - 추상 메서드가 없어도 **추상 클래스 선언 가능**
- 추상 클래스 구현
    - 추상 클래스를 일반 클래스에서 상속 받는다면 슈퍼 클래스의 모든 추상메서드를 구현해야한다.
    - 추상 클래스를 추상 클래스에서 상속 시 모든 추상 메서드 구현 안해도 된다.
- 추상 클래스 목적
    - 객체를 생성하기 위함X, 상속을 위한 부모 클래스로 활용하기 위한 것
    - 여러 클래스들의 **공통된 부분을 추상화** 하여 상속받는 클래스에서 구현을 강제화 하기 위함(메서드의 동작을 구현하는 자식 클래스로 책임을 위임)
    - 즉, 추상 클래스의 추상 메서드를 자식 클래스가 구체화하여 **기능을 확장하는 데 목적**이 있다.

## 인터페이스

- 개념: 추상 메서드와 상수만을 포함, interface 키워드 사용하여 선언
- 특징: 모든 메서드는 추상 메서드로서, abstract public 속성이며 생략 가능하다. 상수는 public static final 속성이며, 생략가능하다.
- 인터페이스 구현
    - 일반 클래스: 인터페이스를 상속하고 **추상 메서드 모두 구현**
    - 추상 클래스: 일부만 구현하면 abstract 사용하여 추상클래스로 구현
    - 인터페이스: 인터페이스를 상속받아 새로운 인터페이스 구현
    - implements 키워드 사용하여 구현
- 인터페이스 목적
    - **목적: 구현 객체의 같은 동작을 보장**
    - 즉, 서로 관련없는 클래스에서 공통적으로 사용하는 방식이 필요하지만 기능을 각각 구현할 필요가 있는 경우 사용

## 공통점

- 인스턴스(객체)는 생성할 수 없다.
- 선언만 있고 구현 내용이 없다.
- 자식 클래스가 메서드의 구체적인 동작을 구현하도록 책임을 위임한다.

## 차이점

- 서로 다른 목적을 가지고 있다.
    - 추상 클래스는 추상 메서드를 자식 클래스가 구체화하여 그 기능을 확장하는 데 목적이 있다. (상속을 위한 부모 클래스)
    - 인터페이스는 서로 관련이 없는 클래스에서 공통적으로 사용하는 방식이 필요하지만 기능을 각각 구현할 필요가 있는 경우에 사용한다. (구현 객체의 같은 동작을 보장)
- 추상 클래스는 클래스이지만 인터페이스는 클래스가 아니다.
- 추상 클래스는 단일 상속이지만 인터페이스는 다중 상속이 가능하다.
- 추상 클래스는 ‘is a kind of’, 인터페이스는 ‘can do this’
    - ex) 추상 클래스: Appliance - TV, Refrigerator
    - ex) 인터페이스: Flyable - Plane, Bird

[https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-java.html](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-java.html)

[https://loustler.io/languages/oop_interface_and_abstract_class/](https://loustler.io/languages/oop_interface_and_abstract_class/)

# Set, List, Map의 차이와 각각의 인터페이스 구현체의 종류

![Untitled](/image/java/Untitled4.png)

## Map

- 검색할 수 있는 인터페이스
- 데이터를 삽입할 때 Key와 Value의 형태로 삽입되며, Key를 이용해서 Value를 얻을 수 있다.

## Collection

- List
    - 순서가 있는 Collection
    - 데이터를 중복해서 포함할 수 있다.
- Set
    - 집합적인 개념의 Collection
    - 순서의 의미가 없다
    - 데이터를 중복해서 포함할 수 없다

## Collections Framework 선택 과정

- Map과 Collection 인터페이스 중 선택
    - Collection 선택 시 사용 목적에 따라 List와 Set 중에 선택
- 사용 목적에 따라 Map, List, Set 각각의 하위 구현체를 선택
    - Map: HashMap, LinkedHashMap, HashTable, TreeMap
    - List: ArrayList, LinkedList
    - Set: HashSet, TreeSet

## Map 인터페이스 구현체의 종류

- HashMap
    - Entry<K,V>의 배열로 저장되며 배열의 index는 내부 해쉬 함수를 통해 계산된다.
    - 내부 해쉬값에 따라서 키순서가 정해지므로 특정 규칙없이 출력된다
    - key와 value에 null 허용
    - 비동기 처리
    - 시간복잡도: O(1)
- LinkedHashMap
    - HashMap을 상속받으며 LinkedList로 저장된다.
    - 입력 순서대로 출력된다.
    - 비동기 처리
    - 시간 복잡도: O(n)
- TreeMap
    - s내부적으로 레드 블랙트리로 저장된다.
    - 키 값이 기본적으로 오름차순 정렬되어 출력된다.
    - 키 값에 대한 Comparator 구현으로 정렬 방법을 지정할 수 있다.
    - 시간 복잡도: O(log n)
- HashTable
    - single lock
    - .모든 메서드에 대해 동기 처리
    - key와 value에 null 허용하지 않는다.

## Set 인터페이스 구현체

- HashSet
    - 저장 순서를 유지하지 않는 데이터의 집합이다.
    - 해시 알고리즘을 사용하여 검색 속도가 매우 빠르다.
    - 내부적으로 HashMap 인스턴스를 이용하여 요소를 저장한다.
- LinkedHashSet
    - 저장 순서를 유지하는 HashSet
- TreeSet
    - 데이터가 정렬된 상태로 저장되는 이진 탐색 트리의 형태로 요소를 저장한다.
    - 이진 탐색 트리 중에 성능을 향상시킨 레드-블랙 트리로 구현되어 있다.
    - Comparator 구현으로 정렬방법을 지정할 수 있다.

## List 인터페이스 구현체

- ArrayList
    - 단방향 포인터 구조로 각 데이터에 대한 인덱스를 가지고 있어 데이터 검색에 적합하다.
    - 데이터의 삽입, 삭제 시 해당 데이터 이후 모든 데이터가 복사되므로 삽입, 삭제가 빈번한 데이터에는 부적합하다.
- LinkedList
    - 양방향 포인터 구조로 데이터의 삽입, 삭제 시 해당 노드의 주소지만 바꾸면 되므로 삽입, 삭제가 빈번한 데이터에 적합하다.
    - 데이터의 검색 시 처음부터 노드를 순회하므로 검색에는 부적합하다.
    - 스택, 큐, 양방향 큐 등을 만들기 위한 용도로 쓰인다.
- Vector
    - 내부에서 자동으로 동기화 처리가 일어난다.
    - 성능이 좋지 않고 무거워 잘 쓰이지 않는다.

[https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-java.html](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-java.html)

# 오버 플로우

- 오버 플로우 : 데이터마다 크기가 있는데 최대값이 넘어가면 발생, 음수로 넘어간다.
- 언더 플로우 : 최소값에서 더 작아지면 발생, 최대값으로 넘어간다.
- 아웃 오브 메모리
- 힙 오버플로우
- 스택 오버플로우
    - 스택에서 스택경계를 넘어설때
    - 하나 이상의 메소드가 순환 형식으로 상호 호출하면서 스택 내에 함수 호출 수가 끊임없이 증가할 때 발생한다.

# Java 8과 Java 7, 11

### 7부터 지원되는 것들

1. Diamond Operator
- 제네릭스에서 타입 추론 가능

```
 // Before Java 7
  ArrayList<Integer> arr = new ArrayList<Integer>();

  // In Java 7
  ArrayList<Integer> arr = new ArrayList<>();
```

1. Switch문에 String 자료형 사용 가능
2. 사용한 리소스를 .close()를 이용해 수동으로 관리하던 것을 try문에 선언하면 자동으로 관리 가능

### 8부터 지원되는 것들

1. Lamda 표현식

```
Arrays.asList("a", "b", "c").forEach( e->{
	System.out.println(e);
});
```

1. Stream API
- 배열이나 컬렉션 사용시 반복문이나 iterator를 사용했었다.
- 이렇게 되면 코드가 길어져서 가독성 저하, 이를 해결

```
String[] arr = new String[]{"넷", "둘", "셋", "하나"};

// 배열에서 스트림 생성
Stream<String> stream1 = Arrays.stream(arr);
stream1.forEach(e -> System.out.print(e + " "));
```

1. Interface에서 default method 생성 가능
2. new Date, Time API
3. Permanent 영역이 제거되었다.(대신 Metaspace 영역이 추가됨)

### 11

[java11](https://futurecreator.github.io/2018/09/29/java-11-released/)

# this 키워드

- this는 자기 자신을 의미하는 키워드
- this.은 class내의 변수와 메소드의 매개변수가 동일할 때 class내의 멤버임을 명확하게 해준다.
- this()는 자신의 생성자를 호출할 때 사용, 호출하는 곳의 첫번째 문장에 사용해야 한다.

# 리틀 엔디안, 빅 엔디안

- endian: 메모리에 연속된 대상을 배열하는 방법
- Little Endian
    - 메모리의 첫 주소에 하위 데이터(데이터의 맨 오른쪽)부터 저장
    - 연산이 적다
    - 인텔이 리틀엔디안 사용
- Big Endian
    - 메모리의 첫 주소에 상위 데이터(데이터의 맨 왼쪽)부터 저장
    - 사람이 읽는 방법
    - 디버그는 빅 엔디안이 편하다
    - 서버쪽에서 많이 사용하는 것
    - IBM이 빅 엔디안 사용

# Reflection

- 정의: 객체를 통해 클래스의 정보를 분석해 내는 프로그램 기법
- 리플렉션은 compiler를 무시한 채 runtime 상황에서 메모리에 올라간 클래스,메소드 등의 정의를 동적으로 찾아 조적할 수 있는 행위
- 즉, 동적인 언어가 가진 특징
- 프레임워크에서 유연성있는 동작을 위해 자주 사용한다.
- 자바 API, getClass: 코드 작성시점에서 클래스나 메소드가 뭔지 모를 때 사용한다.

# OOP 5대 원칙

## SOLID 원칙

- SRP(단일 책임 원칙) - 응집도는 높고 결합도는 낮은 프로그램
- OCP(개방-폐쇄 원칙)
    - 기존의 코드를 변경하지 않고(closed) 기능을 수정하거나 추가할 수 있도록(open) 설계
    - 설계할 때 변경되는 것이 무엇인지에 초점을 맞춘다.
    - 이를 위해 interface가 자주 사용된다
- LSP(리스코프 치환 원칙)
    - 자식 클래스는 부모 클래스에서 가능한 행위를 수행할 수 있어야 한다.
- DIP(의존 역전 원칙)
    - 의존 관계를 맺을 때, 변하기 쉬운것(구체적인 것, 구체화 된 클래스)보다 변하기 어려운 것(추상적인 것, 추상클래스,인터페이스)에 의존해야 한다.
- ISP(인터페이스 분리 원칙)
    - 클래스에서 자신이 사용하지 않는 인터페이스는 구현하지 않아야 한다.
    - 따라서 하나의 일반적인 인터페이스 보다는, 여러개의 구체적인 인터페이스가 낫다.

[https://dev-momo.tistory.com/entry/SOLID-원칙](https://dev-momo.tistory.com/entry/SOLID-%EC%9B%90%EC%B9%99)

# 직렬화

- 자바에서 입출력할 때는 스트림이라는 데이터 통로를 통해 이동한다.
- 하지만 객체는 그렇지 않아서 스트림을 통해 저장이나 네트워크로 전송하는 것이 불가능
- 따라서 **객체를 스트림으로 입출력하기 위해 바이트 배열로 변환하는 것**이 직렬화이다.
- 역직렬화: 스트림→객체

# 제네릭과 와일드카드

### Generics(제네릭스)

- 클래스를 선언할 때 타입을 결정하지 않음. 객체를 생성할 때 유동적인 타입으로 재사용하기 위한 것
- 형변환을 할 필요없고, 타입 에러가 발생할 확률이 없어진다.
- 타입추론은 메서드를 호출하는 코드에서 타입인자가 정의한대로 제대로 쓰였는지 살펴보는 컴파일러의 능력이다.
- 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일시에 타입체크를 해주는 기능
- 컴파일 후에 제네릭 타입이 제거되고 원시 타입으로 지정된다. (.class 파일에는 제네틱 타입에 대한 정보가 없다.)

### Generics 장점

- 타입의 안정성 제공 (의도하지 않은 타입의 객체가 저장되는 것을 막고 저장된 객체를 꺼내올 때 원래의 타입과 다른 타입으로 잘못 형변환되어 발생하는 오류를 막는다.)
- 타입체크와 형변환을 생략하여 코드가 간결해진다.
- ex) ArrayList는 다양한 종류의 객체를 담는다. 꺼낼때마다 타입체크를 하고 형변환 하는 것은 불편하다.

### Collection 클래스에서 제네릭을 사용하는 이유

- 컬렉션 클래스에서 제네릭을 사용하면 컴파일러는 특정 타입만 포함될 수 있도록 컬렉션을 제한한다.
- 컬렉션 클래스에 저장하는 인스턴스 타입을 제한하여 런타임에 발생할 수 있는 잠재적인 모든 예외를 컴파일 타임에 잡아낼 수 있도록 도와준다.

### Generics 주의할 점

- static 멤버에 타입변수 T를 사용하면 안된다.
    - 왜> static 멤버는 타입변수에 지정된 타입, 즉 대입된 타입의 종류에 관계없이 동일한 것이어야 하기 때문이다.
    - Box.item과 Box.item이 다른것이면 안된다는 뜻
- 참조변수와 생성자에 대입된 타입이 일치해야 한다.

### 제한된 Generics

- 제네릭 타입에 extends를 사용하면, 특정 타입의 자손들만 대입할 수 있다.
- ex)

```
// Fruit의 자손만 타입으로 지정가능
class FruitBox<T extends Fruit>{
  ArrayList<T> list = new ArrayList<T>();
}
FruitBox<Apple> appleBox = new FruitBox<Apple>(); // o
FruitBox<Toy> toyBox = new FruitBox<Toy>(); // x

FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
fruitBox.add(new Apple()); // o, apple은 Fruit의 자손
fruitBox.add(new Orange());// o, orange는 Fruit의 자손
```

- 만일 클래스가 아니라 인터페이스를 구현해야 한다는 제약이 필요하면 extends를 사용한다.(implements 아니다)
- ex)

```
interface Eatable {}
class FruitBox<T extends Eatable & Fruit> {...}
```

### 와일드 카드

- 특정 타입을 지정할 때 제한을 둘 수 있다.

```
<? extends T> : 와일드 카드의 상한 제한, T와 그 자손들만 가능
<? super T> : 와일드 카드의 하한 제한, T와 그 조상들만 가능
<?> : 제한 없음. 모든 타입가능 <? extends Objects>와 동일
```

# Extends, Implements

[https://velog.io/@hkoo9329/자바-extends-implements-차이](https://velog.io/@hkoo9329/%EC%9E%90%EB%B0%94-extends-implements-%EC%B0%A8%EC%9D%B4)
