# Chapter 6. API URL의 설계 & 프로젝트 세팅


## 1️⃣ REST API

### ✅ API(Application Programming Interface)란
- 어플리케이션을 프로그래밍할 때, 보다 쉽게 할 수 있도록 해주는 도구들을 의미
- 응용 프로그램에서 사용할 수 있또록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다. 주로 파일 제어, 창 제어, 화상 처리, 문자 제어 등을 위한 인터페이스를 제공한다. - [출처 : 위키백과]

### ✅ REST(Representational State Transfer)란
1. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,
2. HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해
3. 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미합니다.

- ✏️ CRUD Operation
    - Create : 데이터 생성(POST)
    - Read : 데이터 조회(GET)
    - Update : 데이터 수정(PUT, PATCH)
    - Delete : 데이터 삭제(DELETE)

### ✅ REST API란
- HTTP를 기반으로 하는 웹 서비스 아키텍처를 의미하며 HTTP 메소드와 자원을 이용해 서로 간의 통신을 주고받는 방법

### ✅ REST Endpoint란
- 서비스를 사용 가능하도록 하는 서비스에서 제공하는 커뮤니케이션 채널의 한쪽 끝. 즉 요청을 받아 응답을 제공하는 서비스를 사용할 수 있는 지점을 의미

### ✅ HTTP Method
1. GET : 리소스 조회
2. POST:  요청 데이터 처리, 주로 등록에 사용
3. PUT : 리소스를 대체(덮어쓰기), 해당 리소스가 없으면 생성
4. PATCH : 리소스 부분 변경 (PUT이 전체 변경, PATCH는 일부 변경)
5. DELETE : 리소스 삭제


## 2️⃣ 세부적인 API 설계

### ✅ Path Variable(@PathVariable)
- 경로 변수를 표시하기 위해 매개변수에 사용
- 경로 변수는 중괄호 {id}로 둘러싸인 값을 나타낸다
- URL 경로에서 변수 값을 추출하여 매개변수에 할당

### ✅ Query String
- 사용자가 입력 데이터를 전달하는 방법중의 하나로, url 주소에 미리 협의된 데이터를 파라미터를 통해 넘기는 것을 말함
- query parameters( 물음표 뒤에 = 로 연결된 key value pair 부분)을 url 뒤에 덧붙여서 추가적인 정보를 서버 측에 전달하는 것

### ✅ Requset Body
- 서버로 전달되는 실제 데이터를 담고 있음(JSON 형태, form-data 형태)
- 예시 코드

```
{
	“name” : “류장원”,
	“phoneNum” : “010-1111-2222”,
	"nickName" : "randy",
}
```

### ✅ Request Header
- 서버와 전송 시 메타데이터. 즉, **전송에 관련된 기타 정보들이 담기는 부분**



---
---
### 🔍 참고 레퍼런스
1. REST Endpoint 내용 - https://blog.naver.com/ghdalswl77/222401162545

