# Spring과 RestAPI

#### ApplicationContext = Spring Container의 객체

- 개발자들의 노력을 줄여줘서 편리하다
- Spring Container가 모두 관리해주니까 개발자는 비즈니스 로직만 짜면 된다 ! 
- 의존성 주입(DI)을 해준다 !



- bean 객체를 spring Container에 주입하는 두가지 방법

  1. @Component

  ```java
  @Component
  public class Student{
  
  }
  ```

  ​	→ 내가 개발한 것을 등록할 때 사용한다.

  

  ​	2 . @Configuration + @Bean

  ```java
  @Configuration
  public class WebConfig{
  
  	@Bean
  	Student student(){
  		return new Student();
      }
  }
  ```

  ​	→ 이미 개발된 것을 등록할 때 사용한다.

  

  ex )

  ```java
  @Configuration
  public class BoardConfig {
  	
  	@Bean	//bean으로 등록됨 
  	ModelMapper modelMapper() {	//이미 내부에 구현되어있는 기능 (DTO, DAO 매핑 시 사용)
  		return new ModelMapper();
  	}
  }
  ```





#### @Autowired

- bean으로 등록 후 (@Component, @Configuration + @Bean으로 등록) 다른 클래스에서 언제든지 꺼내쓸 수 있도록 하는 어노테이션
- @Resource, @Inject 를 써도됨, 하지만 이게 일반적



- 주로 멤버필드, **생성자**, Setter 위에서 사용한다.

  * 하지만 멤버필드에 사용하는 것은 지양한다.
  * 그 이유는 ? 
    1. 불변성
    2. 순환의존성
    3. 단일 책임 위배	

  - 그래서 주로 생성자 위에 하고 멤버필드는 다음과 같이 final private 로 사용

    ``` java
    final private BoardRepository boardRepository;
    final private ModelMapper modelMapper;
    ```

    



#### RestAPI

- @Service, @Contoller, @Repository 

  → @Component 를 내장하고 있어서 di를 주입함

  

- @RestController			

  → @Component 를 내장하고 있어서 di를 주입함

  

```java
@RestController
public class BoardRestController {		

@GetMapping(“/index”)	→ http를 get으로 사용하겠다. 
String index(){		
	return “index”;		// json 형태로 반환해줌	
	}
}
```

