## 정적 팩토리 메서드(Static Factory Method)
: 객체 생성의 역할을 하는 클래스 메서드 (생성자 대신 사용)

예시 1 : LocalTime 클래스의 `of` 메서드

![image](https://github.com/KimJyun/Book-Effective-Java-3-E/assets/31675698/7654338e-e75a-47e1-86b0-7b7be7b4e32f)

예시 2 : enum 의 요소를 조회할 때 사용하는 `valueof` 메서드
```java
public enum Color {
	BLUE,
	BLACK;
}

Color color_blue = Color.valueOf("BLUE");
Color color_black = Color.valueOf("BLACK");
```

### 장점
1. 이름을 가질 수 있음
	- 생성자가 여러 개 필요할 것 같다면 `&rarr;` 여러 개의 정적 팩토리 메서드를 만들고 각 차이를 나타내는 이름 짓기
2. 호출할 때마다 인스턴스를 새로 생성할 필요 없음
	- 반복되는 요청에 같은 객체 반환
	- 불변 클래스(immutable class) : 인스턴스를 미리 만들어 놓거나, 새로 생성한 인스턴스를 캐싱할 필요 없음
	- `Boolean.valueOf(boolean)` : 객체를 아에 생성 안함
	- 플라이웨이트 패턴(Flyweight pattern)도 비슷한 기법
3. 반환 타입의 하위 타입 객체를 반환 가능
	- 상속을 사용할 때 확인 가능. 
		- 생성자의 역할을 하는 정적 팩토리 메서드가 반환값을 가지고 있기 때문에 가능한 특징
		```java
		public class Level {
			...
			public static Level of(int score){
				if(score < 50) return new Basic();
				else if (score < 80) return new Intermediate();
				else return new Advanced();
			}
		}
```

4. 객체 생성을 캡슐화 할 수 있음
	- 자유롭게 형 변환 가능
		- DTO `&leftarrow;` Entity
		- Entity `&leftarrow;`DTO
		
			```java
			public class MemberDto {
				private String name;
				private int age;
				public static MemberDto from(Member member){
					return new MemberDto(member.getName(), member.getAge());
				}
			}
```

### 정적 팩토리 메서드 네이밍 컨벤션
- `from` : 하나의 매개 변수를 받아서 객체를 생성
- `of` : 여러 개의 매개 변수를 받아서 객체를 생성
- `getInstance | instance` : 인스턴스를 생성, 이전에 반환했던 것과 같을 수 있음
- `newInstance | create` : 새로운 인스턴스를 생성
- `get[OtherType]` : 다른 타입의 인스턴스를 생성. 이전에 반환했던 것과 같을 수 있음
- `new[OtherType]` : 다른 타입의 새로운 인스턴스를 생성