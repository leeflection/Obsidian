# IoC & DI
## IoC(Inversion of Control)
> 제어의 역전
> 코드 흐름이 제 3자에게 위임되는 것

```java
public class A {
    
    @Autowired
    private B b; // 필드 주입방식
}
```
- B라는 객체가 Spring Container에서 관리되고 있는 _bean 이라면
  @Autowired_ 를 통해 객체를 주입받을 수 있다.
- 개발자가 직접 객체를 관리하지 않고, Spring Container에서 객체를 생성하며 해당 객체에 주입시켜 준 것이다.
	- 이것이 바로 제어의 역전 
### 템플릿 메서드 패턴
> 상위 클래스의 견본 메서드에서 하위 클래스가 오버라이딩한 메서드를 호출하는 패턴
- IoC를 구현하기 위한 패턴 중 하나이다.
## DI(Dependency Injection)
> IoC를 구현하기 위해 사용하는 디자인 패턴 중 하나
> 객체의 의존관계를 외부에서 주입시키는 패턴
### DI의 3가지 조건
1. 클래스 모델이나 코드에는 런타임 시점의 의존관계가 드러나지 않는다. 그러기 위해서는 인터페이스에만 의존하고 있어야 한다.
2. 런타임 시점의 의존관계는 컨테이너나 팩토리 같은 제3의 존재가 결정한다.
3. 의존관계는 사용할 오브젝트에 대한 레퍼런스를 외부에서 주입해줌으로써 만들어진다.
### IoC와 DI는 무엇이 다른가?
- **IoC**는 객체의 흐름, 생명주기관리 등 독립적인 제 2자에게 역할과 책임을 취임하는 개념, 즉 **원칙** 중 하나이고
- **DI**는 IoC를 달성하는 **디자인 패턴** 중 하나로 좀 더 구체적인 행위라고 할 수 있다.
## Spring IoC 컨테이너
- DI를 사용하기 위해선 객체 생성이 우선되어야 한다.
- 어디서 객체 생성을 해야 할까?
	- 바로 Spring Framework가 필요한 객체를 생성하고 관리하는 역할을 대신해 준다.
- [I] Bean : 스프링이 관리하는 객체
- [I] 스프링 IoC 컨테이너 : 'Bean'을 담고있는 통
### Spring Bean 등록 방법
#### 1. @Component
-  클래스 선언 부 위에 해당 어노테이션을 작성
#### 2. @Bean
-  클래스 선언부에 @Configuration 어노테이션이 붙은 클래스의 메서드에 @Bean 을 붙이면 리턴되는 객체가 Bean으로 관리된다.
### Spring Bean 사용 방법
#### 1. Autowired
##### 필드 주입
```java
@Component
public class ProductService {
		
    @Autowired
    private ProductRepository productRepository;
		
		// ...
}
```
- 필드 위에 @Autowired 붙이기
- 프레임워크의 의존성이 강해지는 단점
- NPE 발생 가능
##### setter 주입
```java
@Component
public class ProductService {
 
    private final ProductRepository productRepository;
 
    @Autowired
    public void setProductRepository(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }
		
		// ...
}
```
- setter 메서드 위에 @Autowired 를 붙이면 Spring이 setter를 사용해서 자동으로 의존성을 주입해준다.
- 변경이 가능해진다. 보안적 약점
##### 생성자 주입
```java
@Component
public class ProductService {
 
    private final ProductRepository productRepository;
 
    @Autowired
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }
}
```
- 객체의 최초 생성 시점에 Spring이 의존성을 주입해 준다.
- 스프링에서 권장하는 방식
###### 생성자 주입을 권장하는 이유
1. 순환 참조를 방지할 수 있다.
	- 필드 주입, 수정자 주입은 빈이 생성된 후에 참조를 하기 때문에 경고 없이 구동된다.
2. 불변성
	- 생성자로 의존성을 주입할 때 final로 선언할 수 있고, 변할 가능성이 사라지게 된다.
#### 2. ApplicationContext
- Spring IoC 컨테이너에서 빈을 수동으로 가져올 수도 있다.