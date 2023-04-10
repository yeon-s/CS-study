# MongoDB

- 세명이 기존 데이터베이스의 단점 보완을 위해 커스텀 데이터 스토어를 개발
- 확장성과 신속성에서 자주 문제가 발생
- 데이터베이스 개발
- 처음에 텐젠이라는 이름으로 쓰다가 몽고디비로 변경

개발자들이 몽고디비를 좋아하는 이유?

- 스키마가 자유롭다.
- HA와 스케일아웃 솔루션을 자체적으로 지원해 확장이 쉽다.
- 세컨더리 index(프라이머리 키에 생성된 인덱스 이외의 모든 인덱스)를 지원하는 nosql이다.
- 다양한 종류의 인덱스를 제공한다. ⇒ 다양하게 조회가 필요한 서비스의 요구사항을 만족시킬수 있다.
- 응답 속도가 빠르다.
- 배우기 쉽고 간편하게 개발이 가능하다.

**몽고 디비는 유연하고 확장성 높은 오픈소스 Document 지향 데이터베이스다.**

## sql vs nosql

sql 장점

- 데이터 중복을 방지할 수 있다.
- 조인의 성능이 좋다.
- 복잡하고 다양한 쿼리가 가능하다.
- 스키마가 엄격해 잘못된 입력을 방지할 수 있다.

단점

- 하나의 레코드르 확인하기 위해 여러 테이블을 조인하여 가시성이 떨어진다. ⇒ 조인 성능이 좋은데도 불구하고 너무 많은 테이블을 조인하여 성능이 떨어지는 문제도 발생할 수 있다.
- 스키마가 엄격해 변경에 대한 공수가 크다.

관계형 데이터베이스는 스케일 아웃이 가능하긴한데, 설정이 어렵다.

mysql 같은 경우 자체적으로 지원하는 스케일링 솔루션이 없어서 서드파티 솔루션을 사용하거나 어플리케이션 단에서 처리하는 경우가 많다.( 애플리케이션 단에서 처리하면 스케일링 할때마다 변경이 필요하기 때문에 개발자가 번거롭다…) 실제로 스케일링 때문에 mysql에서 몽고디비로 서비스 데이터를 마이그레이션하는 경우가 있다. 

nosql 

nosql은 document(MongoDB), key-value(Redis), wide-column(hbase), graph 등 여러 형태로 데이터를 저장한다.

몽고 디비 장점

![스크린샷 2023-01-27 오후 3.39.25.png](MongoDB%206a80cf1ddbe8427db2072183b97296ad/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-01-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.39.25.png)

- 데이터 접근성과 가시성이 좋다.
- 조인 없이 조회가 가능해 응답속도가 일반적으로 빠르다.
- 스키마가 유연하고 규칙이 없어 필드 추가,삭제 즉, 변경에 공수가 적다.
- 데이터 모델을 앱의 요구사항에 맞게 데이터를 수용할 수 있다.

단점

- 데이터의 중복이 발생한다.
- 스키마가 자유롭지만 설계를 잘못하면 성능에 큰 영향을 끼칠 수 있다.

스케일링 부분에서 몽고디비는 매우 큰 장점을 가지고 있다.

![스크린샷 2023-01-27 오후 3.43.10.png](MongoDB%206a80cf1ddbe8427db2072183b97296ad/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-01-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.43.10.png)

- 분산에 대한 솔루션(HA, 샤딩)을 자체적으로 지원하고 있어서 스케일 아웃이 매우 간편하다.
- 확장에 대한 부분을 데이터베이스 단에서 알아서 해주기 때문에 애플리케이션에서 신경 쓸 게 거의 없다.

⇒ 대용량 데이터에 대한 처리가 편리하다.

 

## 몽고디비 구조

![스크린샷 2023-01-27 오후 4.20.54.png](MongoDB%206a80cf1ddbe8427db2072183b97296ad/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-01-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.20.54.png)

몽고디비를 처음 설치하면 세개의 데이터베이스가 만들어진다. (몽고디비를 관리하는데 사용된다.)

![스크린샷 2023-01-27 오후 4.24.31.png](MongoDB%206a80cf1ddbe8427db2072183b97296ad/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-01-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.24.31.png)

root 권한만 보인다.

- mysql은 테이블을 만들어야 데이터를 삽입할 수 있는데 몽고디비는 document를 삽입하면 자동으로 collection이 생성된다.
- 같은 컬렉션이더라도 필드가 다를 수 있고 같은 필드에 다른 데이터형을 넣을 수도 있다.
- document는 json 형태로 표현하고 실제로 저장은 bson 형태(binary json = json을 바이트 형태로 표현할 수있는 경량 binary 형식 ⇒ 텍스트 형식인 json보다 binary로 저장하면 더 빠르고 효율적으로 사용될 수 있다.  )로 저장한다.

### 컬렉션 특징

- 동적 스키마를 갖고 있어서 스키마를 수정하려면 필드값을 추가/수정/삭제 하면 된다.
- 자유롭더라도 스키마를 어느정도 정해주긴 해야한다. 왜냐하면 컬렉션 단위로 인덱스를 생성하고, 샤디드 클러스터에서 샤드키를 설정하거나 이런 부분도 컬렉션 안에 도큐먼트 안의 특정 필드를 기준으로 하기 때문에 연관된 데이터를 같은 컬렉션에 담는게 중요하다. (관리나 조회의 편의를 위해)

### document 특징

- 모든 document에는 “_id” 필드가 있고, 없이 생성하면 objectId 타입의 고유한 값을 저장한다. (프라이머리키라고 생각하면 된다.)
- 생성 시, 상위 구조인 데이터베이스나 컬렉션이 없다면 먼저 생성하고 document를 생성한다.
- document의 최대 크기는 16메가로 고정되어 있다.
- 

# 몽고디비 배포 형태

### Standalone

![스크린샷 2023-02-18 오후 5.48.17.png](MongoDB%206a80cf1ddbe8427db2072183b97296ad/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-18_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.48.17.png)

프로세스 하나만 이용해서 띄우는 형식

기본적인 read/write 작업 가능

운영 중 점검이 필요해 서버를 내려야 한다면 서비스 제공에 문제 발생

(스터디,테스트 용도)

### Replica Set

![스크린샷 2023-02-18 오후 5.51.04.png](MongoDB%206a80cf1ddbe8427db2072183b97296ad/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-18_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.51.04.png)

동일한 데이터를 갖고 있는 디비를 세개 설치

한대가 죽더라도 나머지 두개가 살아있으므로 점검이 발생해도 서비스 가능

HA(고가용성==서버 하나가 죽어도 나머지 서버로 운용가능)를 보장

현업에서 가장 많이 사용하는 배포 형태 

### Sharded Cluster

![스크린샷 2023-02-18 오후 5.55.34.png](MongoDB%206a80cf1ddbe8427db2072183b97296ad/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-18_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.55.34.png)

 데이터 분산 

컬렉션의 데이터와 트래픽을 분산

샤드 A, 샤드 B, 샤드 C는 내부적으로 Replica Set으로 되어있다.

각 샤드안에서 서버 하나가 죽더라도 나머지가 서버를 운영할 수 있기 때문에 HA를 보장해주면서도 분산에 대한 솔루션.

mongoDB Atlas

몽고디비에서 제공하는 Daas 플랫폼

AWS 오로라나 documentDB같은 클라우드 형태의 데이터베이스 서비스