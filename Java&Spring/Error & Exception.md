# Error 와 Exception
## 런타임 에러
- 자바에서는 실행 시 발생할 수 있는 오류를 **Error 와 Exception** 두가지로 구분한다.
### Error
- 프로그램 코드에 의해서 수습될 수 없는 **심각한** 오류
	-  ex )_메모리 부족 , 스택오버플로우
- 대처할 방법이 없다.
### Exception
- 프로그램 **코드에 의해서 수습**될 수 있는 **다소 미약한 오류**
- 예외가 발생하더라도 이에 대한 대응 코드를 미리 작성해 놓음으로써 어느 정도 비정상적인 종료 혹은 동작을 막을 수 있다.
#### 계층 구조
![[Pasted image 20230920044831.png]]

##### Checked Exception
- 반드시 예외를 처리하도록 강제함
- 컴파일 단계에서 확인
	- _IOEception
	- _FIleNotFoundExcpetion
	- _SQLException
##### Unchecked Exception
- 명시적인 처리를 하지 않아도 된다.
- 런타임 단계
	- _NullPointerException
	- _IllegalArgumentException
	- _IndexOutOfBoundException
	- _SystemException\

#### Exception 처리의 3가지 방법
1. try-catch - 다른 작업으로 흐름 유도
2. throws 호출한 쪽에게 예외 처리 위임
3. throw 명확한 의미의 예외로 처리

##### try-with-resource란?
- 입출력 관련된 resource들에 접근해서 사용하게 되면 닫는 것이 굉장히 중요하다. 
- 닫는 과정에서도 예외가 발생할 수 있기 때문에 예외 처리를 해줘야 하는 코드가 추가적으로 발생
```java
try (파일을 열거나 자원을 할당하는 명령문) {
... 
}
```
- 이를 사용하면 코드가 간단해지고 자동으로 이를 닫아준다.
- 하지만 이를 사용하기 위해선 **해당 클래스가 AutoCloseable 인터페이스를 구현하고 있어야 한다.**