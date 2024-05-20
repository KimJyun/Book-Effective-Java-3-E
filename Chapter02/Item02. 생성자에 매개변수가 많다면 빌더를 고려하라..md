### 점층적 생성자 패턴

```java
public class AClass {
	private final int a; // 필수
	private final int b; // 필수
	private final int c; // 선택
	private final int d; // 선택
	
	public AClass(int a, int b){
		this(a, b, 0);
	}
	
	public AClass(int a, int b, int c){
		this(a, b, c, 0);
	}
```

```java
AClass ac = new AClass(1,2,3,4);
```

- 매개변수 개수가 많아지면 클라이언트 코드를 작성하거나 **읽기 어려움**

### 자바 빈즈 패턴(JavaBeans pattern)

```java
public class AClass {
	private final int a; // 필수
	private final int b; // 필수
	private final int c; // 선택
	private final int d; // 선택
	
	public AClass(){}
	
	public void setA(int v){a=v;}
	public void setB(int v){b=v;}
	public void setC(int v){c=v;}
	public void setD(int v){d=v;}
```

```java
AClass ac = new AClass();
ac.setA(1);
ac.setB(2);
ac.setC(3);
ac.setD(4);
```

- 객체 하나를 만들려면 메서드를 **여러 개 호출**해야 함
- 객체가 완전히 생성되기 전까지는 **일관성이 무너진 상태**에 놓임
- 클래스를 불변으로 만들 수 없음

### 빌더 패턴(Builder pattern)

```java
public class AClass {
	private final int a; // 필수
	private final int b; // 필수
	private final int c; // 선택
	private final int d; // 선택
	
	public static class Builder {
		// 필수 매개변수
		private final int a;
		private final int b;
		
		// 선택 매개변수 - 기본값으로 초기화
		private int c = 0;
		private int d = 0;
		
		public Builder(int a, int b){
			this.a = a;
			this.b = b;
		}
		public Builder c(int val){ c = val; return this; }
		public Builder d(int val){ d = val; return this; }
		
		public AClass build(){ return new AClass(this); }
	}
	
	public AClass(Builder builder){
		a = builder.a;
		b = builder.b;
		c = builder.c;
		d = builder.d;
	}
}
```

```java
Aclass ac = new AC.Builder(1, 2).c(3).d(4).build();
```

- 장점
    - 쓰기 쉽고, 읽기 쉬움
    - 계층적으로 설계된 클래스와 함께 쓰기 좋음
- 단점
    - 객체를 만들려면, 그에 앞서 빌더부터 만들어야 함 → 성능에 민감한 상황에서는 문제가 될 수 있음
    - 점층적 생성자 패턴보다 코드가 장황해 → **매개변수 4개 이상**은 될 때 사용해라