# Chapter 7. JPA를 통한 엔티티 설계, 매핑 & 프로젝트 파일 구조 이해


## 1️⃣ 영속성 컨텍스트

### ✅ 개념
- 엔티티를 영구 저장하는 환경
- 애플리케이션과 DB 사이에서 객체를 보관하는 가상의 DB 역할
- 엔티티 매니저를 통해서 영속성 컨텍스트에 접근하고 관리할 수 있다.

### ✅ 엔티티(Entity)란?
- JPA에서 엔티티란 DB 테이블에 대응하는 하나의 클래스
- @Entity 어노테이션 활용하여, 특정 DB 테이블과 연동

### ✅ 엔티티 생명주기
1. 비영속(new/trasient) : 영속성 컨텍스트와 관계가 없는 상태
```
// 객체를 생성한 상태(비영속)
Member member = new memeber();
member.setId("member1");
member.setUsername("회원1");
```

2. 영속(managed) : 영속성 컨텍스트에 저장된 상태
```
// 객체를 생성한 상태(비영속)
Member member = new Member();
member.setId("member1");
member.setUsername(“회원1”);
EntityManager em = emf.createEntityManager();
em.getTransaction().begin();

// 객체를 저장한 상태(영속)
em.persist(member);     // 엔티티 매니저를 사용해 엔티티를 영속성 컨텍스트에 저장
```

3. 준영속(detached) : 영속성 컨텍스트에 저장되었다가 분리된 상태
```
// 준영속 상태
em.detach(member);
```

4. 삭제(removed) : 삭제된 상태
```
// 객체를 삭제한 상태(삭제)
em.remove(member);
```

### ✅ 영속성 컨텍스트의 이점
- **1차 캐시** : 영속 상태의 엔티티를 이곳에 저장하기 때문에 만약 엔티티를 조회했을 때 1차 캐시에 엔티티가 존재하면 DB에서 안찾아봐도 됨
    
    - 🔍 DB 데이터 조회 흐름
    1. 1차 캐시에서 엔티티를 찾는다.
    2. 있으면 메모리에 있는 1차 캐시에서 엔티티를 조회하고 반납한다.
    3. 없으면 DB에서 조회
    4. 조회한 데이터로 엔티티를 생성해 1차 캐시에 저장(엔티티를 영속상태로)
    5. 조회한 엔티티 반환

<br>

- **영속 엔티티의 동일성 보장**
    - 예시
    ```
    Member a = em.find(Member.class, "member1");
    Member b = em.find(Member.class, "member1");
    System.out.print(a==b) // true
    ```
    - 동일성과 동등성 비교
        - 동일성 : 실제 인스턴스가 같다. '=='를 사용해 비교
        - 동등성 : 실제 인스턴스가 다를 수 있지만 인스턴스가 가지고 있는 값이 같다.

<br>

- **트랜잭션 지원**
    - 객체를 영속성 컨텍스트에 저장해도 DB에 바로 Insert 쿼리를 날리지 않음
    - SQL 쿼리를 모았다가 flush(영속성 컨텍스트의 변경내용을 DB에 반영할 때) 될 때 모아둔 쿼리를 날림 -> 쓰기 지연
    - 예시
    ```
    EntityManager em = emf.createEntityManager();
    EntityTransaction transaction = em.getTransaction();

    //엔티티 매니저는 데이터 변경시 트랜잭션을 시작해야 한다.
    transaction.begin(); // 트랜잭션 시작

    em.persist(memberA);
    em.persist(memberB);
    //여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.

    //커밋하는 순간 데이터베이스에 INSERT SQL을 보낸다.
    transaction.commit(); // 트랜잭션 커밋
    ``` 

<br>

- **변경 감지**
    - JPA로 엔티티를 수정할 때는 단순히 엔티티를 조회해서 데이터를 변경
    - 예시
    ```
    EntityManager em = emf.createEntityManager();
    EntityTransaction transaction = em.getTransaction();
    transaction.begin(); // 트랜잭션 시작

    // 영속 엔티티 조회
    Member memberA = em.find(Member.class, "memberA");

    // 영속 엔티티 데이터 수정
    memberA.setUsername("hi");
    memberA.setAge(10);

    //em.update(member) 이런 코드가 있어야 하지 않을까?

    transaction.commit(); // 트랜잭션 커밋
    ``` 


## 2️⃣ 양방향 매핑

### ✅ 양방향 매핑 규칙
- 객체의 두 관계 중 하나를 연관관계의 주인으로 지정
- 연관관계의 주인만이 외래 키를 관리(등록, 수정)
- 주인이 아닌 쪽은 읽기만 가능
- 주인은 mappedBy 속성 사용 X
- 주인이 아니면 mappedBy 속성으로 주인 지정(외래키가 있는 곳을 주인으로 지정) 



## 3️⃣ N + 1 문제

### ✅ N + 1 문제란?
- 연관 관계에서 발생하는 이슈로 연관 관계가 설정된 엔티티를 조회할 경우 데이터 갯수(n)만큼 연관관계의 조회 쿼리가 추가로 발생하여 데이터를 읽어오게 된다. 즉, 1번의 쿼리를 날렸을 때 의도하지 않는 N번의 쿼리가 추가적으로 실행되는 것.

### ✅ When, Who, How, Why 발생?
1. 언제 발생하는가?
- JPA Repository를 활용해 인터페이스 메소드를 호출할 때

2. 누가 발생시키는가?
- 1:N 또는 N:1 관계를 가진 엔티티를 조회할 때 발생

3. 어떤 상황에 발생되는가?
- JPA Fetch 전략이 EAGER 전략으로 데이터를 조회하는 경우
- JPA Fetch 전략이 LAZY 전략으로 데이터를 가져온 이후에 연관 관계인 하위 엔티티를 다시 조회하는 경우
- fetch란? -> 특정 엔티티를 조회할 때 연관관계에 있는 다른 엔티티를 언제 불러올까에 대한 옵션

4. 왜 발생하는가?
- JPA Repository로 find 시 실행하는 첫 쿼리에서 하위 엔티티까지 한 번에 가져오지 않고, 하위 엔티티를 사용할 때 추가로 조회하기 때문에.

### ✅ fetch 전략에 따른 발생 경우
- EAGER(즉시 로딩인 경우)
1. JPQL에서 만든 SQL을 통해 데이터를 조회
2. 이후 JPA에서 Fetch 전략을 가지고 해당 데이터의 연관 관계인 하위 엔티티들을 추가 조회
3. 2번 과정으로 N + 1 문제 발생

<br>

- LAZY(지연 로딩인 경우)
1. JPQL에서 만든 SQL을 통해 데이터를 조회
2. JPA에서 Fetch 전략을 가지지만, 지연 로딩이기 때문에 추가 조회는 하지 않음
3. 하지만, 하위 엔티티를 가지고 작업하게 되면 추가 조회가 발생하기 때문에 결국 N + 1 문제 발생

    - 🔍 JPQL이란? 
    -> JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어를 제공한다. 따라서 테이블을 대상으로 쿼리 하는 것이 아닌 엔티티 객체를 대상으로 쿼리한다. JPQL은 SQL을 추상화했기 때문에 특정 데이터베이스 SQL에 의존하지 않는 장점이 있다.
