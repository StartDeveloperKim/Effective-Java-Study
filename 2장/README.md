
# 아이템1

---

# 생성자 대신 정적 팩터리 메서드를 고려

## 장점

---

### 1. 이름을 가질 수 있다.

기본적으로 인스턴스를 생성하려면 new 키워드를 통해 생성한다.

정적 팩터리 메서드를 사용하면 어떤 객체가 생성되는 구체적으로 알 수 있다.

```java
public classCar {

privateString engine;
privateString handle;

private Car() {}

public Car newInstance(String engine, String handle) {
        Car car =newCar();
        car.engine = engine;
        car.handle = handle;
				return car;
    }
}
```

- 한 클래스에 시그니처가 같은 생성자가 여러 개 필요할 것 같으면, 생성자를 정적 팩터리 메서드로 바꾸고 각각의 차이를 잘 드러내는 이름을 지어주자.

### 2. 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.

불변 클래스에 인스턴스를 미리 만들어 놓거나하여 미리 생성해 놓은 인스턴스를 가져다 쓰는 방식을 사용할 수 있다.

```java
public staticBoolean valueOf(String s) {
		return parseBoolean(s) ? TRUE:FALSE;
}
```

Boolean 클래스의 valueOf 메서드다. 파라미터로 전달되는 문자열에 따라 TRUE나 FALSE가 반환되는데 이 때 TRUE와 FALSE는 Boolean 클래스에 필드로 정의되어있고 미리 인스턴스가 할당되어 있는 객체이다.

```java
public static finalBooleanTRUE=newBoolean(true);
public static finalBooleanFALSE=newBoolean(false);
```

- 생성 비용이 큰 객체일 때 사용할 수 있다.

### 3. 반환 타입의 하위 타입 객체를 반환할 수 있다.

```java
public class KoreaPerson implements Person{
    private final String country = "KOREA";
    private String name;

    private KoreaPerson() {}

    public static Person newInstance(String name) {
        KoreaPerson koreaPerson = new KoreaPerson();
        koreaPerson.name = name;
        return koreaPerson;
    }

    @Override
    public void hello() {
        System.out.println("hello my name is " + this.name);
    }
}
```

인터페이스인 Person을 상속받았다. 이렇게 반환 타입은 Person으로 상위 타입인데 실제로 사용하는 것은 하위타입의 객체이다. 

### 4. 입력 메개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

반환 타입의 하위 타입이기만 하면 어떤 클래스의 객체를 반환하든 상관없다.

```java
public static Score of(int score) {
        if (score >= 90) {
            return new High();
        } else if (70 <= score) {
            return new Medium();
        } else {
            return new Low();
        } 
    }
```

### 5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.

인터페이스나 클래스가 만들어지는 시점에서 하위 타입 또는 클래스가 존재하지 않아도 언제든지 기존의 인터페이스나 클래스를 상속받으면 사용할 수 있다. 

즉, 반환값이 인터페이스가 된다면 정적 팩터리 메서드의 변경없이 구현체를 변경할 수 있다.

## 단점

---

### 1. 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.

### 2. 정적 팩터리 메서드는 프로그래머가 찾기 없다.

- API 문서를 잘 작성하는 방법
- 메서드 이름도 널리 알려진 규약을 따라 짓는 식으로 문제를 완화해줘야 한다.
