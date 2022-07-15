# 1. JDBC와 ORM의 필요성

# 1. JDBC와 ORM의 필요성

## 어플리케이션 서버와 DB - 일반적인 사용방법

![Untitled](/image/database/lecture/1/Untitled.png)

1. 커넥션 연결: 서버와 DB는 TCP/IP로 커넥션을 연결한다.
2. SQL 전달: 어플리케이션 서버는 DB가 이해할 수 있는 SQL을 연결된 커넥션을 통해 DB에 전달한다.
3. 결과응답: DB는 전달된 SQL을 수행하고, 그 결과를 응답한다. 어플리케이션 서버는 응답 결과를 활용한다.

## 어플리케이션 서버와 DB- DB변경

![Untitled](/image/database/lecture/1/Untitled%201.png)

예전에는 각각의 DB마다 사용법이 달랐다. 따라서 커넥션을 연결하는 방법 / SQL을 전달하는 방법 / 결과를 응답 받는 방법이 모두 달랐다. 따라서 예전에는 크게 두가지의 문제점이 있었다.

1. DB를 다른 종류의 DB로 변경하려면 어플리케이션에 사용된 코드도 변경되어야한다.
2. 개발자가 각 DB를 사용하는 방법을 다시 학습해야한다.

**이런 문제를 해결하기 위해 JDBC라는 표준이 등장했다.**

## JDBC 표준 인터페이스

![Untitled](/image/database/lecture/1/Untitled%202.png)

어플리케이션은 JDBC 표준 인터페이스에 의존한다. 그리고 JDBC는 아래 3가지 기능을 표준 인터페이스로 정의해서 제공한다. 

- java.sql.Connection : DB와의 커넥션
- java.sql.Statement : SQL을 담은 내용
- java.sql.ResultSet : SQL 요청 응답

위 JDBC 인터페이스를 각 DB 회사에서 구현해서 제공해준다. 이런 구현체를 JDBC 드라이버라고 한다. 예를 들어 MySQL DB에 접근할 수 있는 것은 MySQL JDBC 드라이버다.

![Untitled](/image/database/lecture/1/Untitled%203.png)

위와 같이 변경 시, MySQL을 사용하든, 오라클을 사용하든 어플리케이션은 JDBC 인터페이스에만 의존하게 된다. 그리고 실제 사용하는 시점에서는 그 DB에 맞는 구현체만 가져다 쓰면 된다.

## JDBC 필요성 정리

JDBC 등장으로 다음 2가지 문제가 해결됨.

1. DB마다 사용법이 달라, DB 변경 시 어플리케이션 코드를 변경해야 했던 점 ⇒ 어플리케이션은 이제 JDBC 인터페이스에만 의존한다. 그리고 공통된 메서드를 이용해 접근한다. 따라서 다른 종류의 DB로 변경해도 어플리케이션 코드는 변경되지 않는다.
2. DB마다 사용법이 달라, DB 변경 시 각 DB 사용을 위한 사용법을 새로 학습해야 했던 점 ⇒ 개발자는 JDBC 인터페이스 사용법만 공부하면 된다. 나머지는 DB 구현체가 직접 해준다.

## JDBC 표준화 한계

JDBC는 일반적인 부분만 공통화 했기 때문에 각 DB의 모든 일반화를 커버하지 못한다. 예를들어 페이징 처리를 위한 SQL은 각 DB마다 사용법이 다르다. 결국 DB를 변경하면 JDBC 코드는 변경하지 않지만, 페이징 처리와 관련된 코드는 변경해야 한다는 한계점이 존재한다.

## JDBC 최신 데이터 접근 기술

JDBC를 편리하게 사용하도록 SQL Mapper와 ORM이 제공된다.

## SQL Mapper

![Untitled](/image/database/lecture/1/Untitled%204.png)

- 장점 : JDBC를 편리하게 사용하도록 도와준다.
    - SQL 응답 결과를 객체로 편리하게 반환해준다.
    - JDBC의 반복 코드를 제거해준다.
- 단점 : 개발자가 SQL을 직접 작성해야한다.
- 대표 기술 : 스프링 JDBC Template, MyBatis

## ORM 기술

![Untitled](/image/database/lecture/1/Untitled%205.png)

- ORM은 객체를 RDBMS에 매핑해주는 기술이다.
- ORM 기술은 개발자 대신에 SQL을 동적으로 만들어 실행해준다. 추가로 각각의 DB마다 다른 SQL을 사용하는 문제도 중간에서 해결해준다.
- 대표 기술 : JPA, 하이버네이트

## SQL Mapper vs ORM

- SQL Mapper는 직접 SQL만 작성하면 나머지 번거로운 일은 SQL Mapper가 처리해준다. SQL Mapper는 SQL만 작성할 줄 알면 진입 장벽이 낮다.
- ORM 기술은 SQL을 직접 작성하지 않아 개발 생산성이 올라간다. 편리한 반면 진입 장벽이 높다.

## 정리

SQL Mapper와 ORM은 결국 JDBC를 사용하고 있다. 따라서 JDBC가 어떻게 돌아가는지 알아야 트러블 슈팅이 가능하다. 따라서 JDBC의 학습이 필요하다.

[https://ojt90902.tistory.com/878?category=967948](https://ojt90902.tistory.com/878?category=967948)
