# 5. 예외 처리 퍼즐

## 36번. 결정하기 힘든 프로그램

```java
public class Indecisive { 
    public static void main(String[] args) {
        System.out.println(decision());
    }

    static boolean decision() {
        try {
            return true;
        } finally {
            return false;
        }
    } 
}
```

try-finally 구문은 try 블록을 벗어날 때 finally 블록을 반드시 실행
=> return, break, continue, throw 등으로 finally 블록을 벗어나면 안됨
=> 예외를 확실하게 처리해서 finally 블록에서 예외가 발생하는 일이 없게 해야함

## 37번. 이상한 예외

```java
// 컴파일 안됨
// 자바 문서를 보면 try 구문에서 catch 구문에 적혀 있는 예외를 발생시키지 않으면 catch 구문에서 해당 예외를 처리하는 것은
// 컴파일 오류로 취급
import java.io.IOException;

public class Arcane1 {
    public static void main(String[] args) {
        try {
            System.out.println("Hello world");
        } catch(IOException e) {
            System.out.println("I've never seen println fail!");
        }
    }
}
```

```java
// 컴파일 됨
// catch에서 Exception 또는 Throwable을 처리할 때는 try 구문 내용에 관계 없이 컴파일 됨
public class Arcane2 {
    public static void main(String[] args) {
        try {
            // If you have nothing nice to say, say nothing
        } catch(Exception e) {
            System.out.println("This can't happen");
        }
    }
}
```

```java
// 컴파일 됨
// 인터페이스 메서드에서 throws 키워드를 사용하는 것은 예외를 제한하는 기능
interface Type1 {
    void f() throws CloneNotSupportedException;
}

interface Type2 {
    void f() throws InterruptedException;
}

interface Type3 extends Type1, Type2 {
}

public class Arcane3 implements Type3 {
    public void f() {
        System.out.println("Hello world");
    }

    public static void main(String[] args) {
        Type3 t3 = new Arcane3();
        t3.f();
    }
}
```

## 38번. 초대받지 않는 손님

```java
public class UnwelcomeGuest {
    public static final long GUEST_USER_ID = -1;

    private static final long USER_ID;
    static {
        try {
            USER_ID = getUserIdFromEnvironment();
        } catch (IdUnavailableException e) {
            USER_ID = GUEST_USER_ID;
            System.out.println("Logging in as guest");
        }
    }

    private static long getUserIdFromEnvironment() 
            throws IdUnavailableException { 
        throw new IdUnavailableException(); // Simulate an error
    }

    public static void main(String[] args) {
        System.out.println("User ID: " + USER_ID);
    }
}

class IdUnavailableException extends Exception { 
}
```

공백 final 필드에은 확실하게 할당되지 않았다고 판단된 시점에만 넣을 수 있게 만든 것
명시적인 할당 규칙 때문에 컴파일 오류가 발생하면 헬퍼 메서드를 사용해라!

## 39번. 안녕, 그리고 잘 가

```java
public class HelloGoodbye {
    public static void main(String[] args) {
        try {
            System.out.println("Hello world");
            System.exit(0);
        } finally {
            System.out.println("Goodbye world");
        }
    } 
}
```

System.exit() 메서드는 현재 스레드뿐 아니라 현재 스레드와 관련된 모든 스레드도 종료시키기에
try 블록이 끝나기 전에 프로그램이 종료됨
