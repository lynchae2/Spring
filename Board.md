

#### Board.java

- entity(DB) 클래스 생성하기

  ```java
  @Entity
  @Table(name = "테이블명")
  public class 클래스명{
  
  }
  ```

  

- PK가 되는 속성이 무조건 하나 이상 있어야 한다

  ```java
  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)	//자동으로 증가하는 키 
  private int id;
  
  @NotNull	//validation constraint
  @NotBlank	//validation constraint
  @Size(min=5,max=20)	//값 사이즈 제한 
  private String title;
  
  private String password;
  
  //getter, setter 추가하기
  ```

  





#### BoardDto.java

- DTO란 Data Transfer Object 이다.
- 사용자에게 노출시킬 데이터만 처리하기 위한 클래스 







#### BoardRepository.java

- 데이터에 접근하기 위한 인터페이스
- 이 인터페이스를 통해 Board 데이터에 접근할 수 있다.
- JpaRepository  인터페이스를 상속한다.  내부에 @Bean 이 되어있기 때문에 이 클래스에는 따로 어노테이션을 지정해주지 않아도 됨

- BoardRestController로 가져가서 사용할 것임 

```java
final private BoardRepository boardRepository;
final private ModelMapper modelMapper;

@Autowired
public BoardRestController(BoardRepository boardRepository, ModelMapper modelMapper) {
	this.boardRepository = boardRepository;
	this.modelMapper = modelMapper;
}
```

 





#### BoardRestController.java

- Rest API 로 설계
- 클래스명 위에 @RestController 추가



- CRUD
  1. GET -- SELECT
  2. POST -- INSERT 
  3. PUT -- UPDATE
  4. DELETE - DELETE



- Rest API 를 만들 때 리턴타입을 무조건 ResponseEntity로 해야한다.

```java
// rest api를 만들땐 무조건 ResponseEntity 사용 !!!
@GetMapping("/board")
ResponseEntity board(@Param("id") int id) {
	Optional<Board> optionalBoard = boardRepository.findById(id); 
    // optional 타입 --> java8 , null를 쉽게 처리하기 위해 사용한다.

	if (optionalBoard.isPresent()) {
	// board 객체를 boardDto 객체로 매핑
	// ModelMapper modelMapper = new ModelMapper();
		return new ResponseEntity<>(modelMapper.map(optionalBoard.get(), BoardDto.class), HttpStatus.OK);
		} 
    else {
		ErrorResponse errorResponse = new ErrorResponse("잘못된 요청", "데이터가 없습니다.");
		return new ResponseEntity<>(errorResponse, HttpStatus.BAD_REQUEST);
	}
}
```





- Contoller에서 ModelMapper modelMapper = new ModelMapper(); 이렇게 매번 생성해 줄 수 없으니 BoardConfig 라는 클래스 하나 생성해서 사용한다.

```
@Configuration
public class BoardConfig {
	
	@Bean	//bean으로 등록됨 
	ModelMapper modelMapper() {
		return new ModelMapper();
	}
}
```

