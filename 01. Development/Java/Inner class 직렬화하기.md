---
tags:
  - 학습정리
  - 책_Kotlin_In_Action
  - Java
---
- 아래 코드에서는 `View` 의 상태를 직렬화하려 한다.
- `View` 를 직렬화하는 일은 쉽지 않지만 필요한 모든 데이터를 다른 도우미 클래스로 복사할 수 는 있다.
- 그래서 `View` 의 상태를 저장하는 `State` 인터페이스를 선언하고 `Serializable` 을 구현하였다.

```java
interface View {
	State getCurrentState()
	void restoreState(state: State)
}
```

```java
interface State implements Serializable
```

- 이제 `View` 를 구현한 `Button` 클래스의 상태를 저장하기 위해 `State` 를 구현한 `ButtonState` 클래스를 만들었다.
- 다만 `ButtonState` 클래스의 위치를 `Button` 클래스 안으로 하였다.

```java
public class Button implements View {
	@Override
	public State getCurrentState() {
		return new ButtonState();
	}
	@Override
	public void restoreState(State state) { /*...*/ }

	// Inner Class
	public class ButtonState implements State { /*...*/ }
}
```


- 이 코드를 수행하여 `ButtonState` 를 직렬화하면 `java.io.NotSerializableException: Button` 이라는 오류가 발생한다.
- `ButtonState` 를 직렬화 하려 한건데 왜 `Button` 을 직렬화할 수 없다고 예외가 발생할까?

- 자바에서 다른 클래스 안에 정의한 클래스는 자동으로 내부 클래스(inner class)가 된다는 사실을 기억하면 어디가 잘못된 건지 명확히 알 수 있다.

- 이 예제의 `ButtonState` 클래스는 바깥쪽 `Button` 클래스에 대한 참조를 묵시적으로 포함한다.
- 그 참조로 인해 `ButtonState` 를 직렬화할 수 없다.
- `Button` 을 직렬화할 수 업으므로 버튼에 대한 참조가 `ButtonState` 의 직렬화를 방해한다.

- 이 문제를 해결하려면 `ButtonState`를 `static` 클래스로 선언해야 한다.
- 자바에서 중첩 클래스를 `static` 으로 선언하면 그 클래스를 둘러싼 바깥쪽 클래스에 대한 묵시적인 참조가 사라진다.

