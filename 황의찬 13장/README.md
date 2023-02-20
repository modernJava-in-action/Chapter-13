# Chapter 13 - 디폴트 메서드

자바 8에서는 기본 구현을 포함하는 인터페이스를 정의하는 두 가지 방법을 제공한다. 첫 번째는 인터페이스 내부에 정적 메서드를 사용하는 것이다. 두 번째는 인터페이스의 기본 구현을 제공할 수 있도록 디폴트 메서드 기능을 사용하는 것이다.

결과적으로 기존 인터페이스를 구현하는 클래스는 자동으로 인터페이스에 추가된 새로운 메서드의 디폴트 메서드를 상속받게 된다. 이렇게 하면 기존의 코드 구현을 바꾸도록 강요하지 않으면서도 인터페이스를 바꿀 수 있다.

### 호환성 💡

- 바이너리 호환성 : 뭔가를 바꾼 이후에도 에러 없이 기존 바이너리가 실행될 수 있는 상황.
- 소스 호환성 : 코드를 고쳐도 기존 프로그램을 성공적으로 재컴파일할 수 있는 상황.
- 동작 호환성 : 코드를 바꾼 다음에도 같은 입력값이 주어지면 프로그램이 같은 동작을 실행하는 상황.

## 디폴트 메서드

자바 8에서는 호환성을 유지하면서 API를 바꿀 수 있도록 새로운 기능인 <strong>디폴트 메서드(default method)</strong>를 제공한다. 이제 인터페이스는 자신을 구현하는 클래스에서 메서드를 구현하지 않을 수 있는 새로운 메서드 시그니처를 제공한다. 인터페이스를 구현하는 클래스에서 구현하지 않은 메서드는 디폴트 메서드를 통해 인터페이스 자체에서 기본으로 제공한다.

디폴트 메서드는 default라는 키워드로 시작하며 다른 클래스에 선언된 메서드처럼 메서드 바디를 포함한다. 그렇기에 소스 호환성이 유지된다.

이렇게 되면 이미 존재하는 추상 클래스와 자바 8의 인터페이스가 무엇이 다르냐고 물어볼 수 있다.

1. 클래스는 하나의 추상 클래스만 상속받을 수 있지만 인터페이스를 여러 개 구현할 수 있다.
2. 추상 클래스는 인스턴스 변수(필드)로 공통 상태를 가질 수 있다. 하지만 인터페이스는 인스턴스 변수를 가질 수 없다.

## 디폴트 메서드 활용 패턴

디폴트 메서드를 이용하는 두 가지 방식은 선택형 메서드(optional method)와 동작 다중 상속(multiple inheritance of behavoir)이다.

### 선택형 메서드

자바 8 등장이전 인터페이스를 구현하는 클래스는 사용하지 않는 메서드에 대해 비어있는 메서드까지 필수적으로 구현해주어야 했다. 하지만 디폴트 메서드를 이용하면 메서드의 기본 구현을 인터페이스로부터 제공받기 때문에 빈 구현을 제공할 필요가 없다. 이를 통해 불필요한 코드의 양을 줄일 수 있다.

### 동작 다중 상속

자바에서 클래스는 한 개의 다른 클래스만 상속할 수 있지만 인터페이스는 여러 개 구현할 수 있다. 메서드가 중복되지 않는 최소한의 인터페이스를 유지한다면 코드에서 동작을 쉽게 재사용하고 조합할 수 있다.

## 해석 규칙 💡

클래스는 여러 인터페이스를 구현할 수 있다. 그렇기에 같은 시그니처를 갖는 디폴트 메서드를 상속받는 상황이 생길 수 있다. 이런 문제는 C++의 다이아몬드 문제와도 유사하며, 자바에서 문제 해결을 위해 반드시 알아야할 세 가지 규칙이 존재한다.

1. <strong>클래스가 항상 이긴다.</strong> 클래스나 슈퍼클래스에서 정의한 메서드가 디폴트 메서드보다 우선권을 갖는다.
2. <strong>1번 규칙 이외의 상황에서는 서브인터페이스가 이긴다</strong>. 상속관계를 갖는 인터페이스에서 같은 시그니처를 갖는 메서드를 정의할 때는 서브인터페이스가 이긴다.
3. 여전히 디폴트 메서드의 우선순위가 결정되지 않았다면 여러 인터페이스를 상속받는 클래스가 <strong>명시적으로 디폴트 메서드를 오버라이드하고 호출</strong>해야 한다.

### 디폴트 메서드를 제공하는 서브 인터페이스가 이긴다.

> 상황 1)

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F1z5JC%2FbtqQXB3A8PY%2FL4ughtdnj0mQK6cEZDe240%2Fimg.png)

인터페이스 A, B가 존재하며 클래스 C가 B와 A를 구현한다고 할때 C에서 호출하는 hello는 2번 규칙에 따라 B의 hello가 실행된다.

> 상황 2)

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdVG4z7%2FbtqQ0PUzVya%2F8SvHKlXQdgTzfQUaGGsFkK%2Fimg.png)

```java
public class D implements A { }
public class C extends D implements B, A {
	public static void main(String... args) {
		new C().hello();
	}
}
```

클래스 D는 hello를 오버라이드하지 않았고 단순히 인터페이스 A를 구현했다. 따라서 D는 인터페이스 A의 디폴트 메서드 구현을 상속받는다. 2번 규칙에 따라 클래스나 슈퍼클래스에 메서드 정의가 없을 때는 디폴트 메서드를 정의하는 서브인터페이스가 선택된다. 따라서 컴파일러는 인터페이스 A와 B의 hello 중 하나를 선택하는데 B가 A를 상속받는 관계이므로 B의 hello가 실행된다.

만약 D가 hello를 오버라이드 했다면 클래스인 D가 슈퍼 클래스의 메서드를 정의된 것이기 때문에 D의 hello가 실행된다.

### 충돌 그리고 명시적인 문제해결

> 상황 3)

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fba1FmG%2FbtqQPAj8xfY%2F1RCcgGLzlwUsfYbh8ZNc11%2Fimg.png)

```java
public interface A {
	default void hello() { ... }
}

public interface B {
	default void hello() { ... }
}

public class C implements B, A { }
```

A와 B 인터페이스 간의 상속관계도 없어 디폴트 메서드의 우선순위가 결정되지 않았다. 따라서 자바 컴파일러는 어떤 메서드를 호출해야 할지 알수 없으므로 에러를 발생시킨다.

충돌해결을 위해서는 개발자가 직접 클래스 C에서 사용하려는 메서드를 명시적으로 선택해야 한다.

```java
public class C implements B, A {
	void hello() {
		B.super.hello();
	}
}
```

### 다이아몬드 문제

> 상황 4)

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlEVFV%2FbtqQLmfBpFx%2Fb1eOUVBkD7gFr682pUSLw1%2Fimg.png)

```java
public interface A {
	default void hello() { ... }
}

public interface B extends A { }
public interface C extends A { }

public class D implements B, C { 
	public static void main(String... args) {
		new D().hello();
	}
}
```

다이어그램의 모양이 다이아몬드를 닮아 다이아몬드 문제라 부른다. D가 구현하는 B와 C 중 선택할 수 있는 메서드는 오직 A의 디폴트 메서드 뿐이다. D는 A의 hello를 호출한다.

만약 B에 같은 디폴트 메서드 hello가 있었다면 가장 하위의 인터페이스인 B의 hello가 호출될 것이다. B와 C가 모두 디폴트 메서드를 정의했다면 디폴트 메서드 우선순위로 인해 에러가 발생하고 명시적인 호출이 필요하게된다.

만약 C에서 디폴트 메서드가 아닌 추상메서드 hello를 추가하면 어떻게 될까?

```java
public interface C extends A {
	void hello();
}
```

C는 A를 상속받으므로 C의 추상 메서드 hello가 A의 디폴트 메서드 hello보다 우선권을 갖는다. 따라서 B와 C중 선택하지 못하며 컴파일에러가 발생한다.