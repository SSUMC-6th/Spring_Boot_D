# Chapter 10. API & Paging


## 1️⃣ Spring Data JPA의 Paging

### ✅ 페이징(Paging)이란??
- 전체 데이터 중의 일부를 보여주는 방식

### ✅ Pageable과 Page
1. Pageable
- 조회하고 싶은 페이지의 정보를 담는 객체(페이지 번호, 페이지 크기, 정렬 방식 등)
- 쿼리 파라미터 형식으로 페이지 정보를 보내서 원하는 데이터를 얻음
- PageRequest 클래스를 통해 인스턴스화 가능

2. Page
- 데이터와 함께 페이징 정보(전체 페이지 수, 전체 데이터, 현재 페이지 번호 등)을 제공하는 인터페이스
- 일반적으로 Repository의 조회 메서드에서 Pageable 객체를 파라미터로 받아 결과로 반환될 때 사용

### ✅ PageRequest 생성
```
PageRequest page = PageRequest.of(0, 10);
```
- 첫 번째 파라미터 : 페이지 순서 / 두 번째 파라미터 : 단일페이지 크기
- 페이지 순서는 0부터 시작!!

### ✅ Page 내부 코드
```
public interface Page<T> extends Slice<T> {

	static <T> Page<T> empty() {
		return empty(Pageable.unpaged());
	} // 빈 페이지를 생성해 반환

	static <T> Page<T> empty(Pageable pageable) {
		return new PageImpl<>(Collections.emptyList(), pageable, 0);
	} // Pageable을 받아 빈 페이지를 생성하고 반환

	int getTotalPages(); // 전체 페이지 개수를 반환

	long getTotalElements(); // 전체 엔티티 개수를 반환

	<U> Page<U> map(Function<? super T, ? extends U> converter);
}
```




## 2️⃣ 객체 그래프 탐색
- 객체에서 참조를 사용해 연관된 객체를 찾는 것


### ✅ SQL과 JPA의 차이
1. SQL
- SQL를 직접 다루면 이에 따라 탐색할 수 잇는 범위가 정해지는 제약이 생김
- 예를 들어 SQL이 B,C를 조회하는 SQL이면 이 둘만 참조 가능

2. JPA
- JPA는 연관된 객체를 사용하는 시점에 적절한 SELECT SQL을 실행
- 지연 로딩 : 실제 객체를 사용하는 시점까지 데이터베이스 조회를 미룸
- 즉, 연관된 객체를 함께 조회할지 아니면 실제 사용되는 시점에 지연해서 조회할 를 설정을 통해 정의 가능


## 3️⃣ JPQL(Java Persistence Query Language)
- JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어를 제공
- 테이블을 대상으로 쿼리 하는 것이 아니라 엔티티 객체를 대상으로 쿼리

### ✅ JPQL 문법
```
1. select _ from _ [where] _ [groupby] _ [having] _ [orderby] _
2. update _ [where] _
3. delete _ [where] _
```

### ✅ TypedQuery, Query
1. TypeQuery
- 반환 타입이 명확할 때 사용
```
TypedQuery<Member> query1 = em.createQuery("select m from Member m", Member.class);
TypedQuery<String> query2 = em.createQuery("select m.username from Member m", String.class);
```
- 두 번째 파라미터에 타입 정보 작성

2. Query
- 반환 타입이 명확하지 않을 때 사용
```
Query query3 = em.createQuery("select m.username, m.age from Member m");
```
- String 과 int 타입을 함께 조회할 때는 반환 타입 명확 X


**🔍 특징**
- 대소문자 구분 -> 엔티티와 속성은 대소문자 구분 / 반면에 키워드는 구분 X
- 엔티티 이름 -> @Entity(name = "abcd")로 설정
- 별칭 -> JPQL에서 엔티티 별칭은 필수적 명시








