# 8. 클래스 심화 퍼즐

하이딩, 섀도윙, 모호화를 지양해라!

## `용어정리` 이름 재사용과 관련된 기술

### 오버라이딩

자식 클래스에 있는 인스턴스 메서드가 부모 클래스의 접근 가능한 메서드와 동일한 이름과 매개변수를 가지면 오버라이딩

```java
class Base {
    public void f() {}
}

class Derived extends Base {
    public void f() {} // Base.f() 오버라이딩
}
```

하이딩

부모 클래스의 필드, 정적 메서드, 클래스, 또는 인터페이스와 동일한 이름으로 자식 클래스의 필드, 정적 메서드, 클래스, 또는 인터페이스의 이름을 만들면, 자식에서 만든 요소가 부모에 있는 요소를 하이딩합니다.

```java
class Base {
    public static void f() {}
}

class Derived extends Base {
    public static void f() { } // Base.f() 숨김
}
```

오버로딩

클래스의 메서드끼리 이름은 같은데 매개변수가 다르면 메서드 오버로딩

```java
class CircuitBreaker {
    public void f(int i) { } // int 오버로딩
    public void f(String s) { } // String 오버로딩
}
```

섀도윙

변수, 메서드, 클래스, 인터페이스의 이름을 같은 영역에서 동일하게 사용하면 이름이 겹쳐 섀도윙이 발생

```java
class WhoKnows {
    static String sentence = "I don't know.";

    public static void main(String[] args) {
        String sentence = "I know!"; // static 필드 sentence 섀도윙
        System.out.println(sentence); // 지역변수 호출
    }
}
```


```java
class Belt {
    private final int size;
    public Belt(int size) { // 매개변수 size가 필드 size를 섀도윙
        this.size = size;
    }
}
```

모호화

변수와 클래스 또는 인터페이스의 이름이 같은 영역에서 사용되면, 변수가 다른 타입의 이름을 모호화한다고 표현

```java
public class Obscure {
    static String System;

    public static void main(String args[]) {
        // 다음 줄은 System 식별자가 모호화되어 컴파일되지 않습니다.
        System.out.println("hello, obscure world!");
    }
}
```

## 66번. private 접근 제한자 문제

```java
class Base {
    public String className = "Base";
}

class Derived extends Base {
    private String className = "Derived";
}

public class PrivateMatter {
    public static void main(String[] args) {
        System.out.println(new Derived().className);
    }
}
```

하이딩은 필드, 정적 메서드, 또는 중첩 자료형의 이름이 부모의 접근 가능한 필드, 메서드, 자료형의 이름이 동일한 경우에 발생

하이딩하는 필드가 하이딩 되는 필드와 완전히 다른 자료형을 가져도 컴파일은 됨

하이딩은 리스코프 치환 원칙을 위반하는 것

* 부모 클래스에서 할 수 있는 일은 자식 클래스에서도 모두 할 수 있어야 한다.

SOLID
SRP : 단일 책임 원칙
OCP : 개방 패쇄 원칙
LSP : 리스코프 치환 원칙
ISP : 인터페이스 분리 원칙
DIP : 의존 관계 역전 원칙

## 68번. 회색의 그림자

```java
public class ShadesOfGray {
    public static void main(String[] args){
        System.out.println(X.Y.Z);
    }
}

class X {
    static class Y {
        static String Z = "Black";
    } 

    static C Y = new C();
}

class C {
    String Z = "White";
}

```

변수 이름과 클래스 이름이 같으면서, 같은 공간에 있는 경우에는 변수 이름을 우선적으로 사용 => 변수 이름이 클래스 이름을 '모호화(obsure)' 한다고 표현

## 70번. 패키지 문제

```java
package click;
public class CodeTalk {
    public void doIt() {
        printMessage();
    }

    void printMessage() {
        System.out.println("Click");
    }
}

package hack;
import click.CodeTalk;

public class TypeIt {
    private static class ClickIt extends CodeTalk {
        void printMessage() {
            System.out.println("Hack");
        }
    }

    public static void main(String[] args) {
        ClickIt clickit = new ClickIt();
        clickit.doIt();
    }
}
```

package-private 메서드는 다른 패키지에 있는 메서드를 직접 오버라이딩할 수 없다.

package-private 메서드는 해당 패키지 외부에서 직접적으로 오버라이딩할 수 없습니다.

## 72번. final 키워드의 위기

```java
class Jeopardy {
    public static final String PRIZE = "$64,000";
}

public class DoubleJeopardy extends Jeopardy {
    public static final String PRIZE = "2 cents";

    public static void main(String[] args) {
        System.out.println(DoubleJeopardy.PRIZE);
    }
}
```

final 키워드는 필드에 적용하는 경우와 메서드에 적용하는 경우 완전히 다른 결과를 낸다.
메서드에 final 키워드를 적용할 때, 인스턴스 메서드는 오버라이딩 할 수 없으며, 정적 메서드는 하이딩할 수 없다.
필드에 final 키워드를 적용할 때는 값을 한 번 이상 할당할 수 없을 뿐

## 74번. 정체성 위기

```java
public class Conundrum {
    public static void main(String[] args) {
        Enigma e = new Enigma();
        System.out.println(e.equals(e));
    }
}

final class Enigma {
    // Provide a class body that makes Conundrum print false.
    // Do *not* override equals.
}
```
