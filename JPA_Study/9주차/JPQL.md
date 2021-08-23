# JPQL

- 객체지향 쿼리언어- 엔테테 객체를 대상으로 쿼리한다.
- 특정 데이터베이스 SQL에 의존하지 않는다.
- JPQL은 결국 SQL로 변환된다.



## JPQL 문법

- 엔티티와 속성은 대소문자 구분한다.
    - select m from Member as m where m.age>18 (멤버 테이블 대문자)
- JQPL키워드는 대소문자 구분 X
- 엔티티 이름 사용, 테이블 이름이 아님
- 별칭은 필수 , as는 생략가능

-  TypeQuery,Query
    - TypeQuery: 반환 타입이 명확할 때 사용
        ```java
        TypeQuery<Member> query = 
            em.createQuery("Select m from Member m " , Member.Class);
        ```
    - Query; 반환 타입이 명확하지 않을 때 사용
        ```java
        Query query = 
        em.createQuery("Select m.username(String),m.age(int) from Member m");
        ```

- 결과 조회 API
    - query.getResultList(): 결과가 하나 이상일 때, 리스트 반환
        - 결과가 없으면 빈 리스트 반호나
    
    - query.getSingleresult(): 결과가 정확히 하나, 단일 객체 반환
        - 결과가 없으면 NoResultException
        - 둘이상이면: NonUniqueResultException

- 파라미터 바인딩 
    - Select m From Member m where m.username = :username
        - query.setParameter("username",usernameParam);

## 프로젝션
- Select 절에 조회할 대상을 지정하는 것

- 프로젝션 대상: 엔티티,임베디드타입,스칼라 타입(숫자 문자등 기본 데이터 타입)
    
    - Select m from Member m : 엔티티 프로젝션 (영속성 컨텍스트에서 관리됨)
    - Select m.team From Member m :엔티티 프로젝션

    - Select m.address From Member m : 임베디드 타입 프로젝션

    - Select m.username,m.age from Member m : 스칼라 타입 프로젝션 
    - DISTINCT로 중복 제거


- 프로젝션 -여러 값 조회
    - Query타입으로 조회
    - Object[] 타입으로 조회
    - new 명령어로 조회
        - 단순 값을 DTO로 바로 조회
        - Select new jpabook.jpql.UserDto(m.username,m.age)From Member m 
        - 패키지 명을 포함한 전체 클래스 명 입력
        - 순서와 타입이 일치하는 생성자 필요

## 페이징 API

- JPA는 페이징을 다음 두 API로 춫상화
    - setFirstResult(int startPosition): 조회 시작 위치(0부터 시작)
    - setMaxResult(int maxResult): 조회할 데이터 수

- 예시
```java
String jpql = "select m from Member m order by m.name desc";

List<Member> resultList = em.createQuery(jpql,Member.class)
    .setFristResult(10)
    .setMaxResult(20)
    .getResultList();

```

