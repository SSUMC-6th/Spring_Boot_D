# Chapter 5. 실전 SQL - 어떤 Query를 작성해야 할까?


## ✅ JOIN 연산자
- JOIN은 데이터베이스 내의 여러 테이블에서 가져온 레코드를 조합하여 하나의 테이블이나 결과 집합으로 표현해 주는 방법이다.

### 1. INNER JOIN
- 조인하는 테이블의 ON 절의 조건이 일치하는 결과만 출력(만약 값이 존재하지 않으면 출력 x)
- Exmaple

```
select u.userid, name 
from usertbl as u inner join buytbl as b 
on u.userid=b.userid 
where u.userid="111" -- join을 완료하고 그다음 조건을 따진다.

```

### 2. OUTER JOIN
- 내부 조인은 두 테이블에 모두 데이터가 있어야만 결과가 나오지만, 외부 조인은 한쪽에만 데이터가 있어도 결과가 나온다.

- 1️⃣ LEFT OUTER JOIN: 왼쪽 테이블의 모든 값이 출력되는 조인, 첫 번째 테이블을 기준으로 한다.

```
Ex.

SELECT STUDENT.NAME, PROFESSOR.NAME 
FROM STUDENT LEFT OUTER JOIN PROFESSOR -- STUDENT를 기준으로 왼쪽 조인
ON STUDENT.PID = PROFESSOR.ID 
WHERE GRADE = 1

```
- 2️⃣ RIGHT OUTER JOIN: 오른쪽 테이블의 모든 값이 출력되는 조인
- 3️⃣ FULL OUTER JOIN: 왼쪽 또는 오른쪽 테이블의 모든 값이 출력되는 조인



