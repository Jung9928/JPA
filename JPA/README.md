
# ORM 이란?
- Object-relational mapping의 약자로 객체와 DB를 매핑해주는 프레임워크이다.
- 객체는 객체대로 설계하고 관계형 DB는 관계형 DB대로 각각 설계하여 생성한 객체와 DB를 매핑해주는 역할을 수행한다.




# JPA 등장배경
- 서비스를 개발하고 확장하는 과정에서 로직에서 사용되는 Object의 수와 RDB의 테이블 간 관계는 점점 복잡해진다.
- 최근에는 NoSQL이 많이 사용되고는 있지만 큰 규모의 서비스에서 적용되는 테이블 간 관계 표현하는 것은 NoSQL로는 벅차다.
- 그래서 현실적인 대안은 RDB인데.... 그러나 규모가 큰 서비스를 개발/확장해나가면서 생성되는 많은 객체들을 SQL로 변환해서 
  RDB에 저장하는 작업이 쉽지 않다. (일일히 객체와 DB를 Mapping 해줘야 함)




# JPA란?
- Java Persistence API의 약자로, 자바 진영의 ORM 기술 표준이다.
- 마치 객체를 Java의 Collection에 저장하듯이 간단하게 DB에 저장할 수 없을까? 라는 고민 끝에 등장하게되었다. **(ex : list.get(memberId); )**
- 개발자가 객체와 DB 사이에서 Mapping하는 작업을 JPA가 대신 해줌으로써 생산성 향상!

![image](https://user-images.githubusercontent.com/45419456/130347285-0aaf2559-0ec0-42d9-b720-1c2b54ca46d8.png)
- 위 그림은 JPA 2.1 표준 명세이며, JPA는 인터페이스의 모음이다. 
- JPA 2.1 표준 명세를 구현한 3가지 구현체가 Hibernate, EclipseLink, DataNucleus이며, JPA 인터페이스를 구현한 대표적인 오픈소스가 Hibernate이다.

- 버전은 2006년 1.0버전부터 시작해서 2013년 2.1버전이 출시되며 "스토어드 프로시저 접근", "컨버터", "엔티티 그래프" 기능이 추가됨으로써 2.1 버전 이후 부터 많이 사용되고 있다.


# JPA 동작 방식
![image](https://user-images.githubusercontent.com/45419456/130348386-b2071801-f6f8-4d5c-9daf-e67bbe474a50.png)
- JPA는 어플리케이션과 JDBC 사이에서 Mapping 과정을 자동으로 수행해준다.
즉, 개발자가 JPA를 사용하면 JPA 내부에서 JDBC API를 사용하여 SQL를 생성하고 호출하여 DB와 통신을 수행한다.
- 개발자는 그냥 JPA 문법만 사용하면 되므로 편리하다.

### 1) JPA 저장
![image](https://user-images.githubusercontent.com/45419456/130348612-1f5fa9fb-c992-4003-9262-eecb33f31787.png)
- 예를 들어, MemberDAO에서 Member 객체에 대한 데이터를 DB에 저장하고 싶다면 개발자는 아래처럼 코드를 작성하면 된다.
```Java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("persistence-unit name 속성명");
EntityManager em = emf.createEntityManager();

Member member = new Member();
....
em.persist(member);   // ---> 이 문장이 핵심이다. 단 1줄로 DB에 객체를 저장할 수 있다.
```
