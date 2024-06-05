# Chapter 8. API 응답 통일 & 에러 핸들러  


## 1️⃣ Spring의 의존성 주입

### ✅ 의존성 주입(DI : Dependency Injection)이란?
- 스프링 컨테이너에서 객체 Bean을 먼저 생성해두고 생성한 객체를 지정한 객체에 주입하는 방식
- ✏️ **Bean이란?** 
    - 스프링 컨테이너에 의해 관리되는 재사용 가능한 소프트웨어 컴포넌트
    - 즉, 스프링 컨테이너가 관리하는 객체 

- 의존관계 주입은 애플리케이션 런타이에 외부에서 실제 구현 객체를 생성하고 Client에 전달해 Client와 Server의 실제 의존관계가 연결되는 것을 말한다.

### ✅ 의존성 주입 방식
**1. 필드 주입(Field Injection)**
- 클래스에 선언된 필드에 생성된 객체에 주입해주는 방식
- @Autowired 어노테이션을 활용하여 명시
- Example
```
@Controller
public class PetController{
	@Autowired
}

```

**2. 수정자 주입(Setter Based Injection)**
- 클래스의 수정자를 통해서 의존성을 주입해주는 방식
- Example
```
@Controller
public class PetController{

    private PetService petService;

	@Autowired
    public void setPetService(PetService petService){
    	this.petService = petService;
    }
}

```

**3. 생성자 주입(Constructor Based Injection)**
- 클래스의 생성자를 통해서 의존성을 주입해주는 방식
- 생성자 주입은 인스턴스가 생성될 때 1회 호출을 보장
- 생성자 주입 시 필드에 final 키워드를 사용
- Example
```
@Controller
public class PetController{

    private final PetService petService;

	@Autowired
    public PetController(PetService petService){
    	this.petService = petService;
    }
}

```


## 2️⃣ IoC 컨테이너

### ✅ 제어의 역전(Inversion of Control, IoC)

- 기존의 프로그래머는 객체의 생성, 설정, 초기화, 메소드 호출 - 소멸 등을 직접 관리
- 하지만 프레임워크를 통해 객체의 생명 주기를 프레임워크에 위임이 가능
- 즉, **외부 라이브러리**를 통해 작성하는 코드를 호출하고 흐름을 제어한다.
- 예시 -> 직장에 갈 때 내가 운전하는 것이 아닌 운전기사가 운전. 이를 통해 운전 외에 다른 일에 집중 가능!


### ✅ IoC 컨테이너

- 스프링 프레임워크에서 객체를 생성, 관리, 책임, 의존성 관리를 해주느 컨테이너
- 🎁**컨테이너란?** - 객체의 생명주기를 관리, 생성된 인스턴스들에게 추가적인 기능을 제공하도록 하는 것


### ✅ IoC 컨테이너의 유형
**1. BeanFactory**
- 단순히 컨테이너에서 객체를 생성하고 DI를 처리하는 기능만 제공
- Bean을 등록, 생성, 조회, 반환 관리


**2. ApplicationContext**
- Bean을 등록, 생성, 조회, 반환 관리 기능
- 추가 기능
    - 국제화 지원되는 텍스트 메시지 관리
    - 이미지 파일 자원 로드
    - 리스너로 동록된 빈에게 이벤트 발생 알려줌


## 3️⃣ RestControllerAdvice

### ✅ RestControllerAdvice이란?
- 스프링에서 전역적으로 예외를 처리할 수 있는 어노테이션
- 스프링 예외를 미리 처리해둔 ResponseEntityExceptionHandler를 추상 클래스로 제공 


### ✅ RestControllerAdvice vs ControllerAdvice
- 공식 문서에서 -> RestController = ControllerAdvice + ResponseBody, 즉 컨트롤러에서 리턴하는 값이 응답 값의 body로 세팅되어 클라이언트에게 전달

- RestControllerAdvice : 리턴값을 바로 클라이언트에게 전달 / ControllerAdvie : 리턴값을 기준으로 동일한 이름 view를 찾음 


## 4️⃣ lombok

### ✅ lombok이란?
- 어노테이션 기반으로 코드를 자동완성 해주는 라이브러리
- Getter, Setter, Equlas, ToString 등 다양한 코드를 자동완성 시켜준다.

### ✅ lombok의 주요 기능들(워크북에 나온 내용 위주)
1. @Getter, @Setter
- Getter : 내부의 멤버변수에 저장된 값을 외부로 리턴
- Setter : 외부로부터 데이터를 전달받아 멤버변수에 저장

2. @AllArgsConstructor
- 모든 변수를 사용하는 생성자를 자동완성 시켜주는 어노테이션

3. @NoArgsConstructor
- 어떤 변수도 사용하지 않는 기본 생성자를 자동완성 시켜주는 어노테이션

4. @RequiredArgsConstructor
- 특정 변수만을 활용하는 생성자를 자동완성 시켜주는 어노테이션

5. @Builder
- 점증적 생성자 패턴 -> 빈즈 패턴 -> 빌드 패턴
- 자바빈즈 패턴처럼 받되, 데이터 일관성을 위해 정보들을 다 받은 후에 객체 생성

