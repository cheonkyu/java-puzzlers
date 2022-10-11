# 6. 클래스 퍼즐

## 46번. 애매한 생성자

```java
public class Confusing {
    private Confusing(Object o) {
        System.out.println("Object");
    }

    private Confusing(double[] dArray) {
        System.out.println("double array");
    }

    public static void main(String[] args) {
        new Confusing(null);
    }
}
// double array
```

자바는 오버로딩 메서드를 호출할 때 2가지 과정을 거침
1. 사용할 수 있는 메서드를 고르는 일
2. 1번 과정에서 선택한 메서드 중에서 가장 구체적인 형태의 메서드 선택

Object 보다는 double[]가 구체적인 형태이기에 `double array` 문구 출력

가능한 오버로딩을 하지 말라. (복잡도 증가) 기능이 다른 메서드면 다른 명칭을 이용해라!

오버로딩을 해야 하는 상황이면
1. 서로 호환이 불가능한 파라미터로 자료형 선택
2. 메서드별 일관된 기능

## 47번. 착한 나의 강아지와 고양이!

```java
class Counter {
    private static int count = 0;
    public static final synchronized void increment() {
        count++;
    }
    public static final synchronized int getCount() {
        return count; 
    } 
}

class Dog extends Counter {
    public Dog() { }
    public void woof() { increment(); }
} 

class Cat extends Counter {
    public Cat() { } 
    public void meow() { increment(); }
}

public class Ruckus {
    public static void main(String[] args) { 
        Dog dogs[] = { new Dog(), new Dog() };
        for (int i = 0; i < dogs.length; i++)
            dogs[i].woof();
        Cat cats[] = { new Cat(), new Cat(), new Cat() };
        for (int i = 0; i < cats.length; i++)
            cats[i].meow();
        System.out.print(Dog.getCount() + " woofs and ");
        System.out.println(Cat.getCount() + " meows");
    }
}
```

정적 변수는 선언된 클래스와 상속받은 모든 클래스가 공유합니다.

## 48번. 정적 메서드

```java
class Dog {
    public static void bark() {
        System.out.print("woof ");
    }
}

class Basenji extends Dog {
    public static void bark() { }
}

public class Bark {
    public static void main(String args[]) {
        Dog woofer = new Dog();
        Dog nipper = new Basenji();
        woofer.bark();
        nipper.bark();
    }
}

```

정적 메서드에는 동적 디스패치가 없다.

어떤 정적 메서드를 호출할지는 컴파일 시점에 결정

컴파일 시점에 컴파일러는 정적 메서드 앞의 클래스 또는 인스턴스가 어떤 자료형인지 확인하는데, woofer와 nipper 인스턴스 모두 Dog 클래스이기에 

"woof woof" 출력

## 50번. 우리는 같은 자료형이 아니야

```java
// Type1
public class Type1 {
    public static void main(String[] args) {
        String s = null;
        System.out.println(s instanceof String);
    }
}

```

```java
// Type2
public class Type2 {
    public static void main(String[] args) {
        System.out.println(new Type2() instanceof String);
    }
}
```

```java
// Type3
public class Type3 {
    public static void main(String args[]) {
        Type3 t2 = (Type3) new Object();
    }
}
```

instanceof

1. 왼쪽 피연산자가 null일때 무조건 false
2. 양쪽 피연산자가 클래스 타입이면 둘 중 하나는 다른 클래스에 상속받는 서브 타입이어야 함 (타입 캐스팅도 마찬가지)

## 54번. Null과 Void

```java
public class Null {
    public static void greet() {
        System.out.println("Hello world!");
    }

    public static void main(String[] args) {
        ((Null) null).greet();
    } 
}
```

정적 메서드의 한정자로 표현식을 사용하면 표현식 내부 자체의 값과는 관계없이 메서드를 호출