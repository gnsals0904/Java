# Item 25 - 톱레벨 클래스는 한 파일에 하나만 담으라

소스 파일 하나에 TopLevel 클래스를 여러 개 선언하더라도 자바 컴파일러는 불평하지 않는다.

하지만.. 아무런 득도 없고 심각한 위험을 감수해야하는 행위이다.

만약 아래와 같이 코드가 짜여져 있다면

```java
// Main.java
public class Main {

    public static void main(String[] args) {
        System.out.println(Utensil.NAME + Dessert.NAME);
    }
}
```

```java
// Utensil.java
class Utensil {
    static final String NAME = "pan";
}

class Dessert {
    static final String NAME = "cake";
}
```

문제없이 실행은 잘 된다.

그러나 만약 아래의 클래스가 추가된다면

```java
// Dessert.java
class Utensil {
    static final String NAME = "pot";
}

class Dessert {
    static final String NAME = "pie";
}
```

운좋게 컴파일러의 동작 순서에 따라 에러가 출력될수도 아닐수도 있다.

### 해결 방법

해결방법은 그냥 간단하다.

그냥 클래스당 하나의 파일을 만들어주면 된다.

⇒ 톱레벨 클래스들을 서로 다른 소스 파일로 분리하면 된다.

굳이 같이 한 파일에 쓰고 싶다면

```java
public class Main {

    public static void main(String[] args) {
        System.out.println(Utensil.NAME + Dessert.NAME);
    }
    
    private static class Utensil{
        static final String NAME = "pan";
    }
    
    private static class Dessert{
        static final String NAME = "caks";
    }
}
```

이렇게 static으로 선언하면 된다.
