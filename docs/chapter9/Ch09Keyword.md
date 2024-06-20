# Chapter 9. API & Swagger & Annotation


## 1️⃣ java의 Exception 종류들

### ✅ 예외란
- 사용자의 잘못된 조작 또는 개발자의 잘못된 코딩으로 인해 발생하는 프로그램 오류
- **오류**와의 차이점 : 오류는 컴퓨터 하드웨어의 오동작 또는 고장으로 인함

### ✅ 일반 예외(Exception)
- 컴파일 체크 예외
- 자바 소스를 컴파일하는 과정에서 예외 처리 코드가 필요한지 검사
- Exception 상속

### ✅ 실행 예외(Runtime Exception)
- 컴파일 과정에서 예외 코드를 검사하지 않는 예외
- RuntimeException 상속

### ✅ 다양한 예외 종류들

**1. NullPointerException**
- null 값을 가지고 있는 참조 변수로 객체 접근 연산자인 도트(.)를 사용할 때 발생

**2. ArrayIndexOutOfBoundsException**
- 배열에서 할당된 배열의 인덱스 범위를 초과해서 사용할 경우 발생

**3. NumberFormatException**
- 숫자로 변환할 될 수 없는 문자열 데이터를 숫자로 변경할 경우 발생

**4. ClassCastException**
- 억지로 타입 변환을 시도할 경우 발생
```
public class ClassCastExceptionExample {
	static class Animal {}
	static class Dog extends Animal{}
	static class Cat extends Animal{}

	
	public static void main(String[] args) {
		// Dog 객체 생성
    	Dog dog = new Dog();
    	changeDog(dog);
    	
    	// Cat 객체 생성
    	Cat cat = new Cat();
    	changeDog(cat);         // Cat 객체를 Dog 객체로서 억지로 변환시도
    }
	public static void changeDog(Animal animal) {
		Dog dog = (Dog) animal; // ClassCastException 발생 가능
	}
}
```



## 2️⃣ @Valid

### ✅ 정의
1. 객체의 제약 조건을 검증하돌록 지시하는 어노테이션
2. 기본적으로 컨트롤러에서 동작(컨트롤러가 아닌 다른 계층에서 유효성 검사하고 싶을 때는 **@Validated**를 사용)
3. 유효성 검증에 실패할 경우, MethodArgumentNotValidException 발생

### ✅ 사용 방법

1. 의존성 주입
```
implementation 'org.springframework.boot:spring-boot-starter-validation'
```

2. 유효성 검사를 할 객체를 만들고 각 필드에 검사할 유효성 어노테이션 붙여주기
```
public class Person {
    @NotNull
    @Size(max = 64)
    private String name;
        ...
}
```

3. Controller의 메소드에 @Vaild 붙이기(@Valid는 주로 @RequestBody에 붙는다)
```
@PostMapping("/user/add") 
public ResponseEntity<Void> addUser(@Valid @RequestBody AddUserRequest addUserRequest) {
     ,,,
}
```


## 3️⃣ Stream

### ✅ 정의
- 자바 8 이후에 람다를 활용해 배열과 컬렉션을 함수형으로 간단하게 처리하는 기술
- 데이터의 흐름, 즉 배열 또는 컬렉션 인스턴스에 함수 여러 개를 조합해서 원하는 결과를 필터링하고 가공된 결과를 얻음

### ✅ 구성요소
1. 생성하기 : 스트림 인스턴스 생성
2. 가공하기 : 필터링 및 맵핑 등 원하는 결과를 만들어가는 중간 작업
3. 결과 만들기 : 최종적으로 결과를 만들어냄

- 간단한 순서 예시 : 전체 -> 맵핑 -> 필터링 1 -> 결과 만들기 -> 결과물

### ✅ 생성하기

**1. 배열 스트림**

```
String[] arr = new String[]{"a", "b", "c"};
Stream<String> stream = Arrays.stream(arr);
```

**2. 컬렉션 스트림**

```
List<String> list = Arrays.asList("a","b","c");
Stream<String> stream = list.stream();
```

**3. Stream.builder()**
- 빌더를 사용하여 스트림에 직접적으로 원하는 값을 넣는다.

```
Stream<String> builderStream = Stream.<String>builder()
                                .add("a").add("b").add("c")
                                .build(); // [a, b, c]
```

### ✅ 가공 하기(중간 연산)

**1. Filtering**
- 스트림 내 요소들을 하나씩 평가해서 걸러내는 작업, if문 역할

```
List<String> list = Arrays.asList("a","b","c");
Stream<String> stream = list.stream()
	                    .filter(list -> list.contains("a"));
                        // 'a'가 들어간 요소만 선택  [a]
```

**2. Mapping**
- 스트림 내 요소들을 하나씩 특정 값으로 변환하는 작업

```
Stream<String> stream = list.stream()
	.map(String::toUpperCase);
	//[A,B,C]
    
    .map(Integers::parseInt);
    // 문자열 -> 정수로 변환
```

**3. Sorting**
- 스트림 내 요소들을 정렬하는 작업
```
Stream<String> stream = list.stream()
	.sorted() // [a,b,c] 오름차순 정렬
    .sorted(Comparator.reverseOrder()) // [c,b,a] (내림차순)
```

### ✅ 결과 만들기(최종 연산)

**1. Calculating**
- 기본형 타입 사용할 경우, 요소들로 연산 수행 가능
```
IntStream stream = list.stream()
	.count()   //스트림 요소 개수 반환
    .sum()     //스트림 요소의 합 반환
    .min()     //스트림의 최소값 반환
    .max()     //스트림의 최대값 반환
    .average() //스트림의 평균값 반환
```

**2. Collecting**
- 스트림의 요소를 원하는 자료형으로 변환

**3. Matching**
- 특정 조건을 만족하는 요소가 있는지 체크한 결과를 반환: anyMatch(하나라도 만족하는 요소가 있는지), allMatch(모두 만족하는지), noneMatch(모두 만족하지 않는지)

---
---

## 🔍 참고 레퍼런스

### Stream
- https://velog.io/@yun8565/Java-%EC%8A%A4%ED%8A%B8%EB%A6%BCStream-%EC%A0%95%EB%A6%AC 
- https://futurecreator.github.io/2018/08/26/java-8-streams/

### Valid
- https://mangkyu.tistory.com/174









