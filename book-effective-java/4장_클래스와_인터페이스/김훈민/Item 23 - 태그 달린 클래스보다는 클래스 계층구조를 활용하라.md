# Item 23 - 태그 달린 클래스보다는 클래스 계층구조를 활용하라

## 태그 달린 클래스는 단점이 많다

```java
package effectivejava.chapter4.item23.taggedclass;

// 코드 23-1 태그 달린 클래스 - 클래스 계층구조보다 훨씬 나쁘다! (142-143쪽)
class Figure {
    enum Shape { RECTANGLE, CIRCLE };

    // 태그 필드 - 현재 모양을 나타낸다.
    final Shape shape;

    // 다음 필드들은 모양이 사각형(RECTANGLE)일 때만 쓰인다.
    double length;
    double width;

    // 다음 필드는 모양이 원(CIRCLE)일 때만 쓰인다.
    double radius;

    // 원용 생성자
    Figure(double radius) {
        shape = Shape.CIRCLE;
        this.radius = radius;
    }

    // 사각형용 생성자
    Figure(double length, double width) {
        shape = Shape.RECTANGLE;
        this.length = length;
        this.width = width;
    }

    double area() {
        switch(shape) {
            case RECTANGLE:
                return length * width;
            case CIRCLE:
                return Math.PI * (radius * radius);
            default:
                throw new AssertionError(shape);
        }
    }
}
```

- 열거 타입 선언, 태그 필드, switch 등 쓸데없는 코드가 많다.
- 장황하고, 오류를 내기 쉽고, 비효율적이다.
- 만약 필드를 final로 선언하고 싶으면 굳이 사용하지 않는 값도 생성자에서 초기화해야 한다.
- 또 다른 도형을 추가하려면 쓸데없는 코드가 늘어난다.
- 태그 달린 클래스 ⇒ 클래스 계층구조를 어설프게 흉내낸 아류이다.

## 그래서 계층 구조를 사용하는 것이 맞다!

```java
package effectivejava.chapter4.item23.hierarchy;

// 코드 23-2 태그 달린 클래스를 클래스 계층구조로 변환 (144쪽)
abstract class Figure {
    abstract double area();
}
```

```java
package effectivejava.chapter4.item23.hierarchy;

// 코드 23-2 태그 달린 클래스를 클래스 계층구조로 변환 (144쪽)
class Circle extends Figure {
    final double radius;

    Circle(double radius) { this.radius = radius; }

    @Override double area() { return Math.PI * (radius * radius); }
}
```

```java
package effectivejava.chapter4.item23.hierarchy;

// 코드 23-2 태그 달린 클래스를 클래스 계층구조로 변환 (144쪽)
class Rectangle extends Figure {
    final double length;
    final double width;

    Rectangle(double length, double width) {
        this.length = length;
        this.width  = width;
    }
    @Override double area() { return length * width; }
}
```

```java
package effectivejava.chapter4.item23.hierarchy;

// 태그 달린 클래스를 클래스 계층구조로 변환 (145쪽)
class Square extends Rectangle {
    Square(double side) {
        super(side, side);
    }
}
```

- 위에서 이야기한 쓸데없는 코드가 모두 사라짐.
- 관련 없던 데이터 필드를 모두 제거해 남은 필드들은 모두 final 이다
- 만약 삼각형(`Triangle`) 태그를 추가할 예정이었다면, `Shape`과 `Switch`를 사용하지 않고 `Figure` 구현 클래스를 만들면서 쉽게 구현 가능하다.
- 컴파일러가 필드를 초기화하고 추상 메서드 구현이 되었는지 확인해준다.
