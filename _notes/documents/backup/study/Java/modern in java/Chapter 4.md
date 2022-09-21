# 개요

-   `Stream` 이란?
-   `Collections` VS `Stream`
-   `Internal` VS `External` 반복자
-   `Intermediate` VS `Terminal` 연산

# Stream

## 스트림이란?

`Stream` 은 컬렉션을 조작하는데 있어서 선언하는 방법의 Java API 이다. Stream은 멀티스레드 코드 없이도 병렬로 동작할 수 있다.

-   코드는 선언적으로 작성되어 있어 코드의 목적과 사용되는 함수를 알기 쉽다. 동작 파라미터화와 같이 동적 요구사항에도 쉽게 새로운 코드를 작성할 수 있다.
-   블록 기반의 코드 작성은 데이터 처리 파이프라인에 대해 복잡하게 만든다. 예를 들어, 조건에 따라 정렬해서 객체들을 리턴하는 과정만 해도 `filter`, `map`, `sorted` 와 연결해서 코드를 깔끔하게 작성할 수 있다.

위에서 사용한 연산들이 특별히 스레딩 모델에 의존하지 않기 때문에, 내부적으로도 단일 스레드거나 잠재적으로는 멀티코어 구조에 최대화된다.

### 그 외 라이브러리 : Guava, Apache, lambdaj

-   Guava `multimaps` 와 `multisets` 와 같은 추가 컨테이너 클래스 제공
-   Apache 작은 단위를 제공
-   lambdaj 컬렉션을 조작하는 유틸리티 제공

## 스트림의 이해

`Stream` 이란 무엇인가? 간단한 정의로는 `데이터 처리 연산을 돕는 일련의 자원 요소` 이다.

### 특성

-   일련의 요소 콜렉션과 같이, 스트림은 값의 집합 인터페이스를 제공한다. 콜렉션은 데이터 구조이므로, 특정 시간 / 공간 복잡성을 가지는 정렬과 포함 연산이 이루어진다 (ArrayList, LinkedList). 그에 비해 Stream은 filter, sorted, map 등을 사용하여 컴퓨팅 연산을 표현한다. 콜렉션은 데이터에 관한 연산이고, Stream은 컴퓨터 계산이다.
-   자원 스트림은 콜렉션, 배열, 입출력 자원으로부터의 데이터를 사용한다. 정렬된 컬렉션에서 스트림을 생성하면 순서가 유지된다.
-   데이터 처리 연산 스트림은 데이터베이스와 절차 지향 프로그래밍의 연산과 비슷한 것들을 지원한다. 스트림 연산은 순서대로 또는 병렬로 실행될 수 있다.

이 외에도 2개의 중요한 특성이 있다.

-   파이프라인 많은 스트림 연산은 스트림 자체를 리턴한다. 리턴되는 스트림은 거대 파이프라인의 형태를 한다. 이것은 `laziness` 와 `short-circuiting` 을 가능하게 하고, 데이터베이스 쿼리 형태로 볼 수 있게 해준다.
-   내부 반복 `iteratior` 를 사용하는 콜렉션과는 반대로, 내부적으로 반복이 이루어진다.

## Code Example

```java
import static java.util.stream.Collectors.toList;
List<String> threeHighCaloricDishNames = menu.stream() 
																				.filter(dish -> dish.getCalories() > 300) 
																				.map(Dish::getName)
																				.limit(3)
																				.collect(toList());
System.out.println(threeHighCaloricDishNames);
```

-   데이터 소스 : `List<dish>` → 일련의 요소를 제공
-   데이터 처리 연산 : `filter`, `map`, `limit`, `collect`
-   파이프라인 : `collect` 가 반환하는 스트림의 형태, 원본의 쿼리 결과

# Stream VS Collections

## Collection

-   모든 데이터 구조의 값을 가지는 내부 저장형 데이터 구조
-   저장되기 전에 연산됨

## Stream

-   요구사항에 따라 데이터가 연산된 개념적으로 고정된 데이터 구조
-   필요한 값만을 추출해 연산하며 `생산자 - 소비자` 개념 적용
-   값들이 연산되기 때문에 `느슨한 구조 콜렉션`

반복자와 비슷하게, `Stream` 또한 한번만 사용이 가능하다.

```java
List<String> title = Arrays.asList("Modern", "Java", "In", "Action");
Stream<String> s = title.stream();
s.forEach(System.out::println); // 반복문처럼 출력
s.forEach(System.out::println); // 위에서 수행했으므로 IllegalStateExeption 발생
```

## 외부 반복 VS 내부 반복

### 외부 반복

-   `Collection` 인터페이스 사용 시 필요한 사용자에 의한 반복자 (foreach 등)
-   데이터를 가져오는 것과 처리를 각각 해야 함

### 내부 반복

-   `Stream` 라이브러리에서 사용하며 결과만을 사용자에게 반환
-   하드웨어에 맞는 데이터를 자동적으로 선별