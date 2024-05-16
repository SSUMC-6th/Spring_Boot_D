-- 1. 내가 진행중, 진행 완료한 미션 모아서 보는 쿼리(페이징 포함)

```
SELECT * FROM member_mission as mm
WHERE member_id = 1  // 사용자 아이디는 1이라 가정
AND mission_status in ("in_progress", "completed")
ORDER BY mm.updated_at DESC
LIMIT 10 offset (n-1)*10;
```

-- 2. 리뷰 작성하는 쿼리, 사진의 경우는 일단 배제
```
SELECT member.name, review.score, review.body, review.created_at
FROM review
JOIN member ON review.member_id = member.id
```
--3. 홈 화면 쿼리 (현재 선택 된 지역에서 도전이 가능한 미션 목록, 페이징 포함)

```
SELECT region.name AS region_name, 
       (SELECT COUNT(*) FROM member_mission WHERE member_id = 1 AND status = 'complete') AS complete_count,
       store.name AS store_name,
       mission.deadline, mission.mission_spec, mission.reward, 
       member_mission.status
FROM member_mission
JOIN mission ON member_mission.mission_id = mission.id
JOIN store ON mission.store_id = store.id
JOIN region ON store.region_id = region.id
JOIN member ON member_mission.member_id = member.id
WHERE member.id = 1
AND mission.created_at < (SELECT created_at FROM mission where id = 1)
ORDER BY mission.created_at DESC
LIMIT 10;
```

-- 4. 마이 페이지 화면 쿼리
```
SELECT member.name, member.email, member.point
FROM member
WHERE member.id = 1;
```