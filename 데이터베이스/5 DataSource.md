# 5. DataSource

# 5-1 DataSource

## 커넥션을 얻는 방법의 추상화

DB와 통신을 하는데 있어서 반드시 커넥션이 필요하다는 것을 확인했다. 그리고 그 커넥션을 얻기 위해 DriverManager를 통해서 얻는 방법, 커넥션 풀을 통해서 얻는 방법이 있는 것도 확인했다. 그리고 커넥션 풀 인터페이스를 구현한 수많은 커넥션 풀 구현체들이 있다. 이것은 **커넥션을 얻는 방법이 너무나 다양하다는 것이다.** 

**커넥션을 얻는 방법이 여러가지기 때문에 각 방법마다 커넥션을 얻기 위해 다른 코드가 요구된다.** 이런 경우 내가 사용하고 있는 기술이 조금만 변경되어도 매번 코드를 수정해야 하는 일이 발생한다. 이것을 개선하기 위해 커넥션을 얻는 방법의 추상화가 필요하다.

## DataSource

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled.png)

- DriverManager를 통해 커넥션을 생성해서 가져온다.
- 커넥션 풀에서 커넥션을 조회해서 사용한다.

위의 두가지 방법은 커넥션을 얻기 위한 과정도 다르고, 그 과정이 다르기 때문에 코드까지 달라진다. 즉, 사용하는 기술이 바뀌게 되면 어플리케이션에서 커넥션을 얻기 위한 코드까지 수정이 되어야 한다는 것이다. 이처럼 커넥션을 획득하는 방법의 추상화가 필요하다.

## DataSource : 커넥션 획득 방법의 추상화

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%201.png)

커넥션 획득 방법의 추상화가 필요했다. 그래서 자바는 ‘DataSource’라는 인터페이스를 도입해서 커넥션 획득 방법을 추상화시켰다. 어플리케이션 로직은 DataSource 인터페이스를 의존성 주입받고 getConnection()이라는 메서드를 이용해 커넥션을 받아오는 방법을 추상화시킨 것이다.

## 정리

1. 대부분의 커넥션 풀은 DataSource 인터페이스를 이미 구현해두었다. 따라서 개발자는 DataSource 인터페이스에 의존하면 된다.
2. DriverManager는 DataSource를 구현하지 않았다. 이 문제를 해결하기 위해 스프링은 DriverManagerDataSource라는 구현 클래스를 제공해준다.
3. 자바는 DataSource 인터페이스를 제공해 커넥션을 얻는 방법을 추상화했다. 이제 커넥션을 이용하는 방법을 바꾸더라도 어플리케이션 로직에 코드를 변경하지 않아도 된다.

# 5-2 DataSource 적용

## DriverManager와 DataSource

### DriverManager 살펴보기

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%202.png)

- DriverManager는 커넥션을 요청할 때마다 생성해서 전달해준다.
- DriverManager를 통해 커넥션을 얻을 때마다 설정값을 전달해줘야한다.(URL/ USERNAME / PASSWORD)

### DataSource 살펴보기

**DataSource는 각 DB마다 불러오는 커넥션을 얻는 방법이 다르다는 것에 착안해서, 커넥션을 얻는 방법을 추상화한 인터페이스다.** 이 인터페이스는 Connection을 얻어오는 방법에 따라 나눠지며, DriverManagerDataSource, HikariDataSource 등의 구현체로 나눠진다.

### DriverManagerDataSource

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%203.png)

- DriverManagerDataSource는 DriverManager를 이용해 DB에서 Connection을 얻어오는 클래스다.

### HikariDataSource

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%204.png)

- HikariDataSource는 Hikari Connection Pool의 DataSource 인터페이스 구현체다.
- 풀 사이즈 등을 설정할 수 있다.

## DriverManger / DriverManagerDataSource / HikariDataSoure 비교

### DriverManger 테스트 코드

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%205.png)

- DriverManger 테스트 코드를 통해 Connection을 얻어오는 방법이다.
- 매번 DriverManager에 설정 정보를 전달해서 DB 커넥션을 얻는다 →매번 얻기 때문에 쓸데없는 시간 낭비가 존재한다.

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%206.png)

- 실행 결과에서도 DriverManager가 요청마다 커넥션을 새로 만들어주는 것을 알 수 있다.
- 각 Connection은 요청 때마다 새로 만들어지기 때문에 conn 번호가 0-3까지 변경된다.

### DriverManagerDataSource

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%207.png)

- DriverManager에게 Connection을 얻는 방법을 DataSource를 통해 추상화했다.
- DriverManager에게 요청할 때마다, Connection을 얻는 방식으로 동작한다.

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%208.png)

코드 실행 결과 Connection이 요청될 때마다 만들어지는 것을 볼 수 있다.

### HikariDataSource

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%209.png)

- HikariDataSource는 Hikari ConnectionPool이 Connection을 얻는 방법을 DataSource 인터페이스로 추상화 해둔 것이다.
- Connection Pool은 I/O Bound Job이다. 따라서 오래걸리는 일이 많다. 따라서, 커넥션 풀을 만들기 위한 쓰레드가 하나 따로 만들어져서 처리가 된다. 즉, 병렬 프로그래밍으로 처리된다.
- 이런 이유 떄문에 sleep()을 이용한다. 테스트 환경만 아니면 Sleep을 사용하지 않아도 된다. 왜냐하면 커넥션 풀이 다 만들어지자마자 끝이 나기 때문에 커넥션 풀에서 정상적으로 커넥션을 얻어온 것이 잘 보이지 않는다.

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%2010.png)

실행 코드를 봤을 때 현재 0~3번 커넥션이 배정된 것으로 확인된다. 왜냐하면 4개의 요청이 왔고 4개의 요청이 커넥션을 반납하지 않았기 때문이다. 따라서 커넥션풀의 커넥션은 4개가 대여된 것이고, 아직도 사용하고 있는 중이다.

## DataSource와 DriverManager를 사용했을 때의 차이(설정과 사용의 분리)

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%2011.png)

DriverManager와 DataSource를 사용하는 것은 실제 코드에서 큰 차이를 보여준다. 바로 ‘설정 부분’과 ‘사용 부분’의 분리가 이루어진다는 것이다. 이렇게 설정과 사용이 분리되게 되면 코드의 유지보수 관점에서 큰 장점이 존재한다.

### 설정과 사용 분리

- 어플리케이션을 개발할 때 주로 설정은 한 부분에서만 처리가 된다.
    - 설정: DataSource를 만들고 필요한 속성들을 설정하는 부분이다.
- 어플리케이션을 개발할 때 사용은 어플리케이션 전반에서 사용된다.
    - 사용 : 설정은 신경쓰지 않고 getConnection()을 호출해서 사용하면 된다.

위 관점에서 바라본다면 설정하는 부분이 한 지점으로 모이게 되는 것이다. 변경 지점이 줄어들기 때문에 만약 코드르 변경해야하는 일이 발생한다면, 딱 그 부분만 수정을 하면된다. 따라서 유지보수 관점에서 코드의 개선이 가능해진다.

## Connection Pool 로그 살펴보기

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%2012.png)

**MyPool Connection Adder**

- 커넥션 풀을 채우기 위해서 별도로 동작하는 쓰레드다. 이 쓰레드는 커넥션 풀에 커넥션을 최대 풀 수까지 채워준다.
- 커넥션 풀을 메인 쓰레드가 아닌, 자식 쓰레드가 채워준다.

**Pool stats**

- 현재 커넥션 풀의 상태를 알려준다. total은 커넥션 풀의 전체 커넥션 수, Active는 현재 사용중인 커넥션 개수, Idle은 현재 대기중인 커넥션의 개수이다.

**HikariProxyConnection**

- Hikari Connection Pool에 커넥션을 요청하면 HikariProxyConnection을 반환해준다.
- **Hikari Connection Pool은 커넥션 요청을 받으면 커넥션을 새로 생성한 HikariProxyConnection으로 한번 감싸서 전달해준다.**
    - 따라서 HikariProxyConnection의 주소는 매번 다르지만 실제 커넥션은 같은 번호를 가진다.

**JDBCUtils**

- 커넥션 풀은 사용이 끝난 커넥션 및 자원을 반환해줘야 한다.
- JDBCUtils.Close()를 통해 각각의 자원을 Release해주면 커넥션은 파괴되는 것이 아니라 Connection Pool로 반환된다.

## DataSource 적용한 코드 (DriverManager → DataSource로 리팩토링, 기존 V0 코드에 참조 추가)

### 코드 변경

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%2013.png)

다음과 같이 내부적으로 DataSource를 참조하도록 하고, DI 해준다.

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%2014.png)

- DataSource를 사용할 때 / 사용하지 않을 때 커넥션을 받아오는 것이 다른 것을 확인했다. 위의 코드에서 볼 수 있듯이 DriverManager를 사용할 때는 커넥션을 불러오기 위해 설정값을 넣는다.
- DataSource를 이용하면 커넥션을 불러오기 위해 초기 셋팅만 하고 추후에는 설정과 관계없이 getConnection()만 해주는 방식으로 리팩토링이 가능하다.

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%2015.png)

- DriverManager를 사용할 때는 Null인지를 따지고, Exception이 발생하는지를 따져서 Try/Catch를 처리해줘야 했다. 따라서 코드 복잡도가 올라갔다.
- DataSource를 활용할때 JDBCUtils를 이용해서 자원을 손쉽게 Release할 수 있다. 이 때 반납된 커넥션은 다시 커넥션 풀로 들어간다.

## DataSource 적용한 코드(DriverManagerDataSource→HikariDS)

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%2016.png)

dataSource라는 인터페이스는 커넥션을 얻어오는 방법을 추상화했다. 그리고 현재 MemberRepositoryV1은 dataSource 인터페이스에 의존한다.

### DataSource DI 수정 코드

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%2017.png)

DriverManagerDataSource와 HikariDataSource 모두 DataSource 인터페이스를 구현한 구현체다. 따라서 같은 DataSource 인터페이스를 구현한 구현체기 때문에 DriverManagerDataSource에서 HikariDataSource를 이용하기 위해서는 의존성 주입 코드만 변경해주면 된다. 즉, 변경점이 매우 최소화된다. 스프링 컨테이너를 사용하는 경우는 코드 변경이 아예 없을 수 있다.

## Hikari Connection Pool 적용한 테스트 코드+ 테스트 코드 결과

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%2018.png)

- 다음과 같이 DriverManager를 이용하던 테스트 코드를 DI만 수정해서 사용할 수 있다.

![Untitled](5%20DataSource%2046ea23ea921f4d2cb42de132437fc8ce/Untitled%2019.png)

- 커넥션 풀에서 얻은 ProxyConnection 객체의 주소는 매번 바뀐다. 하지만 커넥션은 항상 Conn0이다. 이유는?
- 테스트 코드에서는 커넥션을 사용하고 Close를 한다. 커넥션 풀에서 얻은 커넥션을 JDBCUtils로 Close()하게 되면 이 커넥션은 커넥션 풀로 반환된다.
- Conn은 항상 ‘conn0’으로 들어오는데 HikariProxyConnection의 주소는 항상 바뀌는 것을 볼 수 있다. 이것은 Hikari Connection Pool에 값을 달라고 했을 때, Hikari 커넥션 풀은 DB 커넥션을 HikariProxy 객체를 하나 생성해서 감싸서 전달해준다. DB 커넥션(세션 관점)에서는 동일한 객체다.

## 정리

- DI 관점에서 DriverManagerDataSource를 Hikari 커넥션 풀로 변경해도 MemberRepositoryV1의 코드르 바꿀 필요가 없었다. 왜냐하면 DataSource를 사용했기 때문이다.
- DataSource는 커넥션을 얻어오는 방법에 대한 추상화 인터페이스고 MemberRepositoryV1이 추상화 인터페이스에 의존하기 때문이다.
- 커넥션 풀에서 얻어진 커넥션은 Close하면 커넥션 풀로 반환된다.
- 커넥션 풀에서 제공되는 커넥션은 ProxyConnection 객체로 한번 감싼 다음에 제공된다.