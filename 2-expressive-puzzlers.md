# 2. 표현식 퍼즐

## 1번. 홀수 확인

```java
public class Oddity {
    public static boolean isOdd(int i) {
        return i % 2 == 1;
    }

    public static void main(String[] args) {
    }
}
```

나머지 연산의 결과는 0이 아닌 값을 반환할 때는 항상 왼쪽 피연산자의 부호와 같음

```java
public class Oddity {
    public static boolean isOdd(int i) {
        return (i & 2) != 0;
    }

    public static void main(String[] args) {
    }
}
```

## 8번. Dos Equis

```java
public class DosEquis {
    public static void main(String[] args) {
        char x = 'X';
        int i = 0;
        System.out.print(true  ? x : 0); // X
        System.out.print(false ? i : x); // 88
    }
}
```

`여러 자료형을 혼합해서 계산하면 복잡해짐`

조건 연산자의 최종 자료형을 결정하는 규칙

1. 두 번째와 세 번째 피연산자의 자료형이 같다면 해당 자료형으로 결과를 냄
2. byte, short, char 자료형을 T 자료형이라고 하면, 
피연산자 중 하나가 T 자료형이고 다른 하나가 T 자료형으로 변환 가능한 int 상수라면 T 자료형으로 결과를 냄
3. 만약 2번이 아니라면 '이항 숫자 확산'을 적용. 두 번째와 세 번째 피연산자 중 큰 자료형으로 결과를 냄

## 9번. 같은 것 같으면서도 다른 것(1)

```java
public class Tweedledum {
    public static void main(String[] args) {
        // Put your declarations for x and i here

        x += i;     // Must be LEGAL
        x = x + i;  // Must be ILLEGAL
    }
}
```

복합 할당 연산자에는 +=, -= 등등

자바는 T가 E1의 자료형일 때 복합 할당 E1 op = E2 가 단순 할당 E1 = (T)((E1)op(E2))와 같다고 정의

```java
short x = 0;
int i = 123456;

x += i; // 자동 자료형 변환이 일어남
x = x + i; // 'possible loss of precision' 오류가 발생하고 컴파일이 안됨
```

## 10번. 같은 것 같으면서도 다른 것(2)

```java
public class Tweedledee {
    public static void main(String[] args) {
        // Put your declarations for x and i here

        x = x + i;  // Must be LEGAL
        x += i;     // Must be ILLEGAL
    }
}
```

```java
Object x = "Buy";
String i = "Effective Java!";
x = x + i; // 통과
x += i; // 좌측 피연산자가 String 자료형이 아닌 참조 자료형이므로 오류가 발생 
```
