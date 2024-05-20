## 싱글톤(Singleton)

- 정의 : 인스턴스를 오직 하나만 생성할 수 있는 클래스
- 예시 : 무상태(stateless) 객체, 설계상 유일해야 하는 시스템 컴포넌트
- 단점 : 테스트하기 어려움
- 만드는 방식
    1. 생성자 → `private`, 인스턴스 접근 → `public static 멤버`
        
        ```java
        public class A {
        	public static final A INSTANCE = new A();
        	private A(){...}	
        }
        ```
        
        - private 생성자 : A.INSTANCE 초기화할 때, 딱 한 번 호출됨
        - 장점
            - 싱글톤임이 API에 명백하게 드러남
            - 간결함
    2. 생성자 → `private`, 인스턴스 접근 → `정적 팩토리 메서드`
        
        ```java
        public class A {
        	private static final A INSTANCE = new A();
        	private A(){...}
        	public static A getInstance(){ return INSTANCE; }
        }
        ```
        
        - `getInstance()`는 항상 같은 객체의 참조를 반환
        - 장점
            - API를 바꾸지 않고도 싱글톤이 아니게 변경할 수 있음
            - 정적 팩토리 메서드를 제네릭 싱글턴 팩토리로 만들 수 있음
            - 정적 팩토리의 메서드 참조를공급자(supplier)로 사용할 수 있음 (`ex)Supplier<A>`)
    - 직렬화
        - 단순히 `Serializable` 을 구현한다고 선언하는 것만으로는 부족함
        - 모든 인스턴스 필드를 일시적(transient)로 선언 + `readResolve` 메서드를 제공해야 함
            - 그렇지 않으면 직렬화된 인스턴스를 역직렬화할 때마다 새로운 인스턴스 생성됨
                
                ```java
                private Object readResolve() {
                	return INSTANCE;
                }
                ```
                
    3. 열거 타입 방식의 싱글톤 (바람직한 방법)
        
        ```java
        public enum A {
        	INSTANCE;
        }
        ```
        
        - 간결, 직렬화 쉽게 가능
        - 리플렉션 공격에도 싱글톤 보장
        - 단, 싱글톤이 Enum 외의 클래스를 상속해야 하면, 이 방법은 사용할 수 없음
	        (열거 타입이 다른 인터페이스를 구현하도록 선언할 수는 있음)

