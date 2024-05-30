
# 6주차 미션

## API Endpoint, Request Body, Request Header, query String, Pathvariable이 포함된 API 명세서 만들기!

### 1️⃣ 홈 화면

- API Endpoint

```
GET /api/home
```


### 2️⃣ 마이 페이지 리뷰 작성

- API Endpoint
```
POST api/review/{store-id}/upload
```
-> store-id 말고 review-id 를 통해 해결하는 것도 괜찮을지도?

- Request Body
```
{
    "store_id" : "1",
    "stars" : "4.5",
    "content" : "리뷰 내용 작성",
    "image" : "음식사진.png"
}
```

-> 이미지는 나중에 공부하겠찌만, 따로 빼서 정리한다?

- Request Header

```
Authorization : accessToken (String)
```

### 3️⃣ 미션 목록 조회


- API Endpoint

```
모든 게시글 조회 : GET api/missions

단일 게시글 조회 : GET api/missions/{mission-id}
```

- Request Header

```
Authorization : accessToken (String)
```

### 4️⃣ 미션 성공 누르기


- API Endpoint
```
PATCH api/missions/{missoin-id}/success
```

- Request Body
```
{
    "isSuccess" : "complete"
}
```

- Request Header

```
Authorization : accessToken (String)
```
### 5️⃣ 회원 가입 하기(소셜 로그인 고려 X)


- API Endpoint

```
# 서비스 이용 동의
POST /api/members/signup/service

# 개인정보 입력
POST /api/members/signup/info

# 선호 음식 선택
POST /api/members/signup/prefer-food

```

- Request Body

```
# 서비스 이용 동의
{
    "total_agree" : "true",
    "age_agree" : "true",
    "service_agree" : "true",
    "privacy_agree" : "true",
    "location_agree" : "true",
    "marketing_agree" : "true"

}


# 개인정보 입력
{
    "member_id" : "1",
    "name" : "류장원",
    "gender" : "male",
    "birth" : "2001-03-19",
    "address" : "서울특별시 동작구 상도동"

}



# 선호 음식 선택
{
    "prefer_food" : "한식"

}
```

- Request Header

```
Authorization : accessToken (String) -> 로그인할 때 사용!
```

-> 서블렛, 웹플럭스에 관해 좀 더 공부해보기!
