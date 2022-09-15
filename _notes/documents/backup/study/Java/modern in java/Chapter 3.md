# 람다 표현식

## 요약
-   개요
-   어디서 어떻게 람다식을 쓰는지
-   실행 패턴
-   함수적 / 타입 인터페이스
-   람다 구성

# 개요
`람다 표현식` 은 매개변수로 보낼 수 있는 익명 메소드의 간략한 표현법이라고 볼 수 있다. 이름이 없지만 파라미터, 내용, 리턴 타입, 예외를 가진다.

-   익명성 : 일반적으로 가지는 메소드 이름이 없기 때문에 익명성을 갖는다.
-   함수 : 람다는 클래스가 아니고, 파라미터, 내용, 리턴 타입, 예외를 갖기 때문에 함수라고 한다.
-   매개변수 : 람다식은 메소드로 매개변수로 전달될 수 있다.
-   간략함 : 보통의 익명 클래스들처럼 복잡하게 작성할 필요가 없다.

`람다식` 의 기원은 계산을 설명하는 람다 계산식이라는 시스템에서 왔다. 왜 람다식을 사용할까? 이전 챕터에서 본 내용들은 자바에서 굉장히 복잡해 보인다. 람다는 이 문제에 대해서 간편하게 메소드에 보낼 수 있다.

람다를 통해서 `Comparator` 객체를 간편하게 작성할 수 있다.

```java
Comparator<Apple> byWeight =
 (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
//   람다 파라미터                        람다 바디
```

-   파라미터 : `compare` 메소드로 전달되는 파라미터이다.
-   화살표 : 파라미터와 람다 바디를 분리한다.
-   람다 바디 : 메소드의 동작부분

이러한 문법은 C#이나 스칼라에서도 사용되고, Javascript에도 유사한 구문이 있다. 람다 스타일은 크게 두 가지로 나뉜다.

1.  expression-style `(parameters) → expression`
2.  block-style `(parameters) → { statements; }`

# 어디서 어떻게 사용하는가

람다는 기능적인 인터페이스가 필요할 때 사용한다.

## 기능적 인터페이스

기능적 인터페이스란 특정한 단 한개의 추상 메소드이다. `Comparator` 이나 `Runnable` 이 예시가 된다.

## 함수 디스크립터

함수형 인터페이스가 가지는 추상 메소드의 시그니처는 람다 표현식을 서술한다. 이것을 `함수 디스크립터` 라고 한다. 예를 들어, `Runnable` 인터페이스는 아무것도 하지 않고, 리턴하지 않는 함수의 시그니처로 볼 수 있다. `run` 이라는 추상메소드가 단 한개의 추상 메소드며, 아무것도 리턴하지 않기 때문이다.

### ex

`() -> void` 는 파라미터가 없고, void를 리턴한다는 의미다.

`(Apple, Apple) -> int` 는 2개의 Apple 타입 파라미터를 갖고, int 타입을 리턴한다는 의미다.

람다를 함수형 인터페이스가 필요한 곳에만 사용하는 이유는 복잡하게 사용하지 않은 그대로도 기능에 잘 맞고, Java 프로그래머들이 이미 단일 추상 메소드에 익숙하기 때문이다. (event handling 등) 가장 중요한 이유는, Java 8 이전에도 쓰였기 때문이다. 이는 람다식 표현으로의 전환에 좋은 기회가 된다.

# 실행 어라운드 패턴

자원 처리에서의 `recurrent pattern` 은 자원의 사용, 처리, 종료 동작을 한다 (File 및 DB 사용). 준비 과정에서는 항상 비슷하고 처리 과정에 있어서 중요한 역할을 한다. 이를 실행 어라운드 패턴이라고 부른다.

```java
public String processFile() throws IOException {
	try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) { 
		return br.readLine();
	}
}
```

현재 코드에는 제한이 있다. 파일에 가장 첫 번째 줄만 읽을 수 있다. 이 다음을 읽으려면 `processFile` 에 다른 동작을 하도록 지정해야 한다. 이는 `BufferedReader` 에 다른 동작을 하도록 하는 것과 같다.

동작 파라미터화는 람다가 하는 일과 일치한다. `Lambda` 로 표현하자면 `BufferedReader` 를 보내서 `String` 을 반환하도록 하는 것과 같다. 람다 표현식은 `인라인` 형식으로 함수적 인터페이스의 추상 메소드를 제공하는 방법이다.

## 람다 전달

다른 람다를 전달해서 기존의 `processFile` 을 재사용할 수 있다.

```java
String oneLine = processFile((BufferedReader br) -> br.readLine());
String twoLines = processFile((BufferedReader br) -> br.readLine() + br.readLine());
```