# 3. JDBC 개발

# 3. JDBC 개발

## JDBC를 이용한 CRUD Repository 작성

앞서 만들었던 DBConnectionUtil이라는 클래스를 이용해 Connection을 얻어와서 CRUD를 하는 Repository를 작성하고 테스트까지 처리한다.

## DB Table을 만든다.

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled.png)

DB에 테이블을 만들어준다. JPA와 다르게 DriverManager는 Table을 자동으로 DDL 해주지 않기 때문이다. 

## Repository를 만든다.

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled%201.png)

- Repository의 Save 메소드 구현
    - 파라미터 바인딩을 사용하기 위해 PreparedStatement 사용
    - PreparedStatement에 와일드 카드를 사용해주고 추후 setString+setInt 등을 이용해 파라미터 바인딩을 할 수 있다.
    - PreparedStatement에는 SQl이 들어가고 이 SQL을 실행시키면 실제로 DB에 쿼리가 나가게 된다.
- 사용한 자원(Connection, PreparedStatement, ResultSet)은 반드시 회수해야한다.
    - 예외가 발생하든, 하지 않든 반드시 해야한다. 따라서 Finally 구문에 주의해서 작성해야한다.
    - 이부분을 처리하지 않을 경우, 커넥션이 끊어지지 않고 계속 유지되는 문제가 발생한다.

## Repository 클래스의 getConnection() / Close() 메서드 만들기

### getConnection() 메서드

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled%202.png)

- getConnection()은 앞서 만들었던 DBConnectionUtil.getConnection()을 바로 호출한다.
- DBConnectionUtil은 DriverManager에게 설정 정보를 넘겨주고 Connection을 받아온다.

### Close() 메서드

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled%203.png)

- 자원은 반드시 사용한 것의 반대 순서로 Release 해야한다.
    - 얻을 때는 Connection → PreparedStatement → ResultSet순으로 얻었다.
    - 해제할 때는 Resultset → PreparedStatement → Connection 순으로 해제한다.
- 사용한 자원은 반드시 해제해야한다. 그런데 각 자원을 해제할 때마다 Exception이 발생할 수 있기 때문에 Try~catch로 처리한다.

## 테스트 코드 작성

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled%204.png)

- 아직까지 정상적으로 들어갔는지 조회를 할 수 없는 상황이기 때문에 JUnit을 이용한 테스트 검사는 할 수 없다.

## PreparedStatement는 왜 사용하는가?

**SQL Injection 공격을 당하지 않기 위해.**

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled%205.png)

- 위와 같은 구문이 있을 경우, Statement를 사용하면 values의 memberId 자리에 앞에 있는 memberId가 저장된 select 쿼리가 직접 들어오게 된다. 즉, 원하지 않는 SQL Injection이 들어와서 의도치 않게 DB에 있는 모든 정보가 탈취될 수 있다.
- 반면 PreparedStatement에서 ?,?는 SQL 구문이 아닌 단순한 파라미터로 인지되기 때문에 SQL Injection이 되지 않는다.

## Member 조회 기능 추가

### Member 조회 코드

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled%206.png)

- DriverManager로 얻은 Connection으로 다음과 같은 코드를 작성할 수 있다.
- 결과는 ResultSet으로 돌려받는다. ResultSet은 DataTable과 같은 구조를 가지고 있으며, 내부적으로 Cursor가 가리키는 Column값을 불러올 수 있다.
- 커서 초기값은 0이기 때문에 최초로 데이터를 가리키는 곳으로 이동이 필요하다. 따라서 rs.next()를 통해 다음 커서를 설정한다.

### Member 조회 테스트 코드

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled%207.png)

- Member를 저장한 후, Repository에서 Member를 찾아와 같은 객체인지 확인한다.
- 이때, Member와 findMember는 Hash상으로는 같은 객체지만 주소 상으로는 다른 객체여야한다. 따라서 equals만 True를 만족한다.

## ResultSet 자료구조

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled%208.png)

- ResultSet은 “select member_id, money”로 지정하면 ‘meber_id→money’ 컬럼 순서대로 결과가 들어간다.
- ResultSet은 내부적으로 커서를 사용한다. 커서를 이동해서 다음 데이터를 조회한다.
- rs.next()
    - 내부의 커서를 움직이는 메서드다.
    - boolean 타입을 반환한다. True: 현재 값 존재, False: 현재 값 존재하지 않음
    - 커서는 최초에는 데이터를 가리키지 않는다. 사용하기 위해 한번 커서를 옮겨야 한다.
- rs.getString(), rs.getInt() → rs.getString(”member_id”)
    - 특정 컬럼으로 조회해서 현재 커서가 가리키는 값을 불러온다.

## Member 수정 기능 개발

### Member 수정 코드

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled%209.png)

- memberId를 통해 Member를 찾아 그 Member의 Money 컬럼을 업데이트한다.
- excute()는 영향받은 row값을 리턴한다.

### Member  수정 테스트 코드

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled%2010.png)

## Member 삭제 기능 구현

### Member 삭제 코드

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled%2011.png)

- 특정 MemberId를 가지는 Member 삭제

### Member 삭제 테스트 코드

![Untitled](3%20JDBC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%202fa79466a8054d49b7bf9dcd0de8d8c7/Untitled%2012.png)

- 검증은 Member가 지워지고 그걸 조회했을 때 NoSuchElementException이 발생하는 것을 이용했다.

## 정리

- JDBC 인터페이스를 구현한 DriverManager를 이용해 Connection을 얻어와 crud 구현
- 자원은 Connection → PreparedStatement → ResultSet 순으로 얻게 된다. 리소스는 반드시 해제해야하는데 이때, 얻은 순서의 반대로 해제해야한다.
- 리소스를 해제하기 위해서는 Try/Catch 구문으로 처리해야한다. 왜냐하면 하나라도 자원해제를 하던 과정에 실패하게 되면 나머지 자원이 해제되지 않는다.
- Connection이 해제되지 않은 경우, 계속 유지된다. 이 경우, DB 세션이 유지되기 때문에 계속 이런 상태가 지속될 경우 DB 커넥션이 마를 수 있다.