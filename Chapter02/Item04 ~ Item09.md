### Item04. 인스턴스화를 막으려거든 private 생성자를 사용하라

- 추상클래스로 만드는 것으로는 인스턴스화를 막을 수 없음 (하위 클래스를 만들어 인스턴스화하면 됨)

→ `private 생성자`를 만들면 됨

```java
public class A{
	private A(){ throw new AssertionError(); }
}
```

- 상속을 불가능하게 할 수 있음

### Item05. 자원을 직접 명시하지 말고, 의존 객체 주입을 사용하라

- 사용하는 자원에 따라 동작이 달라지는 클래스에서는 정적 유틸리티 클래스나 싱글톤 방식이 부적합함

→ 의존 객체 주입 : 클래스가 여러 자원 인스턴스 지원 + 클라이언트가 원하는 자원을 사용

```java
public class A {
	private final B b;
	
	public A(B b){
		this.b = Objects.requireNonNull(b);
	}
}
```

- 장점
	- 클래스의 유연성, 재사용성, 테스트 용이성 개선
    
    - **팩토리 메서드 패턴(Factory Method Pattern)**
        - 생성자에 자원 팩토리를 넘겨주는 방식
        - 예시 : Supplier<T> 인터페이스

### Item06. 불필요한 객체 생성을 피하라

- 똑같은 기능의 객체를 매번 생성하지 말고, **객체 하나를 재사용**하자
    - `String s = new String("a")`; 사용X, `String s = “a”;` 사용
    - `Boolean(String)` 사용X, `Boolean.valueOf(String)` 사용
    - `박싱된 기본 타입(Long)` 사용X, `기본 타입(long)` 사용

### Item07. 다 쓴 객체 참조를 해제하라

- 참조를 다 썼을 때, **null 처리(참조 해제)** 하라
- 메모리 누수 주의
    - 자기 메모리를 직접 관리하는 클래스
    - 캐시
    - 리스너(listener) 혹은 콜백(callback)

### Item08. finalizer와 cleaner 사용을 피하라

- finalizer와 cleaner 사용 하지 말자
    - 성능, 보안 문제 등 문제가 많음

### Item09. try-finally 보다는 try-with-resources 를 사용하라

- 정확하고 쉽게 **자원 회수** 가능
- 자원이 둘 이상이면 try-finally 방식은 너무 지저분함
- 예시 (catch절도 가능)
    
    ```java
    static String firstLineOfFile(String path, String defaultVal){
    	try(BufferedReader br = new BufferedReader(new FileReader(path))){
    		return br.readLine();
    	} catch (IOException e) {
    		return defaultVal;
    	}
    }
    ```