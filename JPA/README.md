
# ORM
- Object-relational mapping의 약자로 객체와 DB를 매핑해주는 프레임워크이다.
- 객체는 객체대로 설계하고 관계형 DB는 관계형 DB대로 각각 설계하여 생성한 객체와 DB를 매핑해주는 역할을 수행한다. <br/><br/><br/><br/>





# JPA 등장배경
- 서비스를 개발하고 확장하는 과정에서 로직에서 사용되는 Object의 수와 RDB의 테이블 간 관계는 점점 복잡해진다.
- 최근에는 NoSQL이 많이 사용되고는 있지만 큰 규모의 서비스에서 적용되는 테이블 간 관계 표현하는 것은 NoSQL로는 벅차다.
- 그래서 현실적인 대안은 RDB인데.... 그러나 규모가 큰 서비스를 개발/확장해나가면서 생성되는 많은 객체들을 SQL로 변환해서 
  RDB에 저장하는 작업이 쉽지 않다. (일일히 객체와 DB를 Mapping 해줘야 함)  <br/><br/><br/><br/>

  
  
  
  
# JPA란?
- Java Persistence API의 약자로, 자바 진영의 ORM 기술 표준이다.
- 마치 객체를 Java의 Collection에 저장하듯이 간단하게 DB에 저장할 수 없을까? 라는 고민 끝에 등장하게되었다. **(ex : list.get(memberId); )**
- 개발자가 객체와 DB 사이에서 Mapping하는 작업을 JPA가 대신 해줌으로써 생산성 향상!

![image](https://user-images.githubusercontent.com/45419456/130347285-0aaf2559-0ec0-42d9-b720-1c2b54ca46d8.png)
- 위 그림은 JPA 2.1 표준 명세이며, JPA는 인터페이스의 모음이다. 
- JPA 2.1 표준 명세를 구현한 3가지 구현체가 Hibernate, EclipseLink, DataNucleus이며, JPA 인터페이스를 구현한 대표적인 오픈소스가 Hibernate이다.

- 버전은 2006년 1.0버전부터 시작해서 2013년 2.1버전이 출시되며 "스토어드 프로시저 접근", "컨버터", "엔티티 그래프" 기능이 추가됨으로써 2.1 버전 부터 많이 사용되고 있다.  <br/><br/><br/><br/>


# JPA 동작 방식
![image](https://user-images.githubusercontent.com/45419456/130348386-b2071801-f6f8-4d5c-9daf-e67bbe474a50.png)
- JPA는 어플리케이션과 JDBC 사이에서 Mapping 과정을 자동으로 수행해준다.
즉, 개발자가 JPA를 사용하면 JPA 내부에서 JDBC API를 사용하여 SQL를 생성하고 호출하여 DB와 통신을 수행한다.
- 개발자는 그냥 JPA 문법만 사용하면 되므로 편리하다.  <br/><br/><br/><br/>

### 1) JPA 저장 과정
![image](https://user-images.githubusercontent.com/45419456/130348612-1f5fa9fb-c992-4003-9262-eecb33f31787.png)
- 예를 들어, MemberDAO에서 Member 객체에 대한 데이터를 DB에 저장하고 싶다면 개발자는 아래처럼 코드를 작성하면 된다.
```Java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("persistence-unit name 속성명");
EntityManager em = emf.createEntityManager();

Member member = new Member();
....
em.persist(member);   // ---> 이 문장이 핵심이다. 단 1줄로 DB에 객체를 저장할 수 있다.
```
- 이렇게 JPA의 persist 메소드를 사용하면 인자로 전달한 member객체(JPA에서는 엔티티라 칭함)를 분석한 후, INSERT SQL을 생성하게 되고 JDBC API를 사용하여 SQL을 DB에 전송하게 된다.  <br/><br/><br/><br/>


### 2) JPA 조회 
![image](https://user-images.githubusercontent.com/45419456/130349253-0a4800d1-d96e-498e-b511-01ead1092e84.png)
- 예를 들어, Member 객체를 조회하고 싶을 경우 개발자는 member의 pk값과 객체의 class명을 JPA에 전달한다. 
```Java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("persistence-unit name 속성명");
EntityManager em = emf.createEntityManager();

Member findMember = em.find(Member.class, db의 PK);  // member 객체 조회
```
- find 메소드를 실행하게 되면 JPA는 엔티티(객체)의 정보를 토대로 적절한 SELECT SQL을 생성하게 된다.
- 그 후, JDBC API를 사용하여 SQL을 DB에 전달하고 DB로부터 결과(Result Set)을 받아오게 되며 
  가져온 ResultSet을 member객체에 매핑하게 된다.

- 이와 같이 JPA가 개발자를 대신해서 쿼리를 생성해주기 때문에 Object와 RDB 간의 패러다임 불일치를 해결할 수 있는게 장점이다.  <br/><br/><br/><br/>


# JPA를 왜 사용해야 할까?
#### 1) SQL 중심의 개발에서 객체 중심으로의 개발이 가능해진다.
#### 2) Java Collection에 데이터를 삽입/삭제하는 것처럼 간단하게 사용가능 함으로써 개발 생산성이 향상.
```Java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("persistence-unit name 속성명");
EntityManager em = emf.createEntityManager();

....

em.persist(member);                                 // 저장
Member member = em.find(Member.class, db의 PK);     // 조회
em.setName("변경할 이름");                           // 수정
em.remove(member);                                  // 삭제
```
(수정의 경우, JPA가 자동으로 UPDATE SQL Query를 생성하여 DB에 전달하게 되므로 사용이 편하다.)

#### 3) 유지보수가 편리하다.
* 기존의 방식 : 필드 변경 시, 관련된 SQL을 모두 수정해야 함.
* JPA : 필드만 추가하면 끝 --> JPA가 SQL 처리를 전담함. 

#### 4) Object와 RDB간의 패러다임 불일치 해결

#### 5) JPA의 성능 최적화 기능

#### 6) 지연로딩(Lazy Loading)

#### 7) 데이터 접근 추상화와 벤더 독립성  
#### 8) 표준


