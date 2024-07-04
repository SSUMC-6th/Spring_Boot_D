## s3
- 어디서나 데이터를 저정하고 검색할 수 있도록 구축된 객체 스토리지
- AWS에서 제공하는 클라우드 스토리지 서비스로 다양한 유형의 미디어를 저장하고 관리하는 데 사용되는 웹 기반 스토리지 시스템
    ### s3 버킷 특징
    
    - 이름이 유일해야 함(전세계에서 유일한 ID만 가능!)
    - 버킷을 만들기 위해서는 리전을 선택해야 함
    - 버전 관리 가능
    ### s3 객체 특징

    - 객체 하나의 크기는 1Byte ~ 5TB
    - 저장 가능한 객체 갯수는 무제한
    - 객체마다 각각의 접근 권한 설정 가능

    ### 버킷/객체 개념

    - 버킷 = 마트
    - 객체 = 물건
    - 버킷 안에 다양한 데이터를 넣을 수 있고, 그 각각의 데이터에는 이름, 크기 등의 정보가 담겨있음. 이렇게 정보가 담긴 데이터 하나하나가 S3 안의 객체!
    - 즉, s3 버킷에 저장되는 데이터는 모두 객체!

## MIME Type
MIME(Multipurpose Internet Mail Extensions)
- 오직 텍스트만 보낼 수 있었던 SMTP의 단점을 보완하여, 메세지 내부에 다른 파일을 전송할 수 있도록 하는 전자메일 프로토콜
- MIME으로 인코딩한 파일은 Content-type 필드를 헤더에 담게 되며, 이를 통해 전송된 자원의 형식을 명시

### MIME TYPE
인터넷에 전달되는 파일 포맷 및 포맷 컨텐츠를 위한 식별자
### MIME Type (Media Type) 의 구조
기본적으로 type/subtype 구조
type은 파일 종류, subtype은 파일 포맷
이때 타입은 discrete(개별)타입과 multipart 타입으로 나뉨
- discrete(개별)타입: text, image, audio, video, model, font, application(모든 종류의 바이너리 데이터)
- multipart 타입: form-data, mixed, alternative

## MultipartFile
- Spring에서 업로드된 파일을 다룰 때 사용되는 인터페이스로 HTTP 요청을 통해 업로드된 파일의 메타데이터 및 내용을 담고 있음
