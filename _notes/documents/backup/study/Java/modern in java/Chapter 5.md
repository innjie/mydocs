# Chapter 5

# 개요

-   필터링, 자르기, 매핑
-   찾기, 매칭, 줄이기
-   수 관련 스트림
-   다중 데이터 원본에서 스트림 만들기
-   무한 스트림

# Filtering

## 필터링과 예측

스트림 인터페이스는 `filter` 메소드를 포함한다. 이 메소드는 `predicate` 라는 `boolean`을 반환하는 매개함수를 포함하며, 이 매개 함수에 일치하는 요소 스트림을 반환한다.

```java
// Example
List<Dish> vegetarianMenu = menu.stream()
 .filter(Dish::isVegetarian)
 .collect(toList());
```

### 특수 요소 필터링

스트림은 `distinct` 라는 메소드를 포함한다. 이 메소드는 특수한 요소를 가진 stream을 반환한다. (스트림으로 인해 생성된 hashCode나 equals 객체)

```java
List<Integer> numbers = Arrays.asList(1, 2, 1, 3, 3, 2, 4);
numbers.stream()
 .filter(i -> i % 2 == 0) // 2, 2, 4
 .distinct() // 2, 4
 .forEach(System.out::println); // 2, 4 출력
```

# Slicing

## `predict` 를 사용한 slicing

`Java 9` 에서는 스트림을 효율적으로 사용하기 위한 `takeWhile` 과 `dropWhile` 을 지원한다.

## takeWhile

```java
List<Dish> specialMenu = Arrays.asList(
 new Dish("seasonal fruit", true, 120, Dish.Type.OTHER),
 new Dish("prawns", false, 300, Dish.Type.FISH),
 new Dish("rice", true, 350, Dish.Type.OTHER),
 new Dish("chicken", false, 400, Dish.Type.MEAT),
 new Dish("french fries", true, 530, Dish.Type.OTHER));
```

예를 들어, 320 이상의 객체를 가져오려면 `filter` 를 사용해서 쿼리를 실행했을 것이다.

하지만, 처음부터 선언은 정렬이 되어 있다. 이런 상황에서 filter를 사용하는 것은 스트림 전체를 순회하는 것과 같다. 대신에, 320보다 큰 객체를 만나면 멈추는 방법이 있다. 이러한 방법은 리스트의 크기가 크다면 훨씬 효과적이다. `takeWhile` 은 조건에 실패하면 즉시 멈춘다. 따라서 다음과 같이 코드를 작성할 수 있다.

```java
List<Dish> slicedMenu1
 = specialMenu.stream()
 .takeWhile(dish -> dish.getCalories() < 320)
 .collect(toList());
```

## dropWhile

`dropWhile` 은 `takeWhile` 을 보완하는 연산자다. `predicate` 가 `false` 인 경우에는 다 제거하고 `true` 일때 중단하며, 남은 요소들을 모두 반환한다. 그리고 유한한 개수일 때만 동작한다.

## truncate

스트림은 주어진 길이보다 짧은 스트림을 반환하는 `limit(n)` 을 지원한다. 요구되는 사이즈 n이 limit으로 전달되고, 정렬되었다면 첫번째 요소는 최댓값이다. 물론, `Set` 과 같은 정렬되지 않은 스트림에서도 동작한다. 이 때는 결과값에 대한 정렬 상태를 알 수 없다.

## Skipping

스트림은 몇개의 요소를 건너뛸 수 있는 `skip(n)` 연산을 제공한다. 만일 n개보다 스트림이 작다면, 빈 스트림을 반환한다. `limit` 과는 상호보완적이다.

```java
List<Dish> dishes = menu.stream()
 .filter(d -> d.getCalories() > 300)
 .skip(2)
 .collect(toList());
```

# Mapping

SQL에서는 테이블에서 특정 열을 조회할 수 있다. 이와 비슷하게 `map` 과 `flatMap` 을 제공한다.

## 스트림 각 요소에 접근하는 방법

스트림은 매개변수를 함수로 갖는 `map` 을 지원한다. 각 요소가 새 요소에 매치된다.

```java
List<String> dishNames = menu.stream()
 .map(Dish::getName)
 .collect(toList());
```

`getName` 이 `String` 타입을 리턴하기 때문에, 결과적으로 `Stream<String>` 이 반환된다.

다음과 같이 사용도 가능하다.

```java
List<String> words = Arrays.asList("Modern", "Java", "In", "Action");
List<Integer> wordLengths = words.stream()
 .map(String::length)
 .collect(toList());
```

## Arrays.Stream()과의 사용

```java
words.stream()
 .map(word -> word.split("")) // 각 단어를 글자 단위로 분리
 .map(Arrays::stream) // 각 배열을 분리된 스트림으로 생성
 .distinct()
 .collect(toList());
```

위 코드를 `flatMap` 을 활용하여 변경할 수 있다.

```java
List<String> uniqueCharacters =
 words.stream()
 .map(word -> word.split(""))
 .flatMap(Arrays::stream) // 각 스트림을 한개의 스트림으로 반환
 .distinct()
 .collect(toList());
```

`flatMap` 은 스트림이 아닌 배열을 매핑하는데 효과적이다. 모든 분리된 스트림은 한개의 스트림으로 들어온다. 다시 말해, 스트림의 각 값을 다른 스트림의 값으로 대체해서 하나의 스트림으로 만드는 것이다.

# 검색과 매칭

## 최소 한개가 해당하는지 검사

### anymatch

`anyMatch` 는 최소 한 개가 해당하는지 검사하는 연산이다. 또한, `boolean` 을 반환하므로 `terminal operation` 이다.

## 모든 요소가 해당하는지 검사

### allMatch

`anyMatch` 와 비슷하지만 스트림의 모든 요소가 조건에 부합하는 지 체크한다.

### NoneMatch

`allMatch` 의 반대이다. 주어진 조건에 해당하는 요소가 하나도 없음을 보장한다.

세 연산 모두 `short-circuiting` 이다. (&&와 ||의 자바 스트림 버전)

## Short-circuiting evaluation

어떤 연산들은 결과를 만들기 위해 스트림 전체를 확인할 필요가 없다. `and` 연산과 거대한 `boolean` 표현을 확인하면 된다. 단지 한개의 `false` 만 찾으면 되기 때문이다.

스트림에서 `allMatch`, `noneMatch`, `findFirst`, `findAny` 와 같은 연산자들은 결과를 도출하기 위해 모든 요소를 탐색할 필요가 없다. 조건에 맞지 않는 한개의 요소가 나오면, 결과는 도출된다. 비슷하게, `limit` 연산자도 해당한다. 이 연산은 주어진 크기에 맞는 스트림을 만드는 작업이 필요하다.

## 요소 검색

```java
Optional<Dish> dish =
 menu.stream()
 .filter(Dish::isVegetarian)
 .findAny();
```

`findAny` 는 임의의 요소를 반환한다. 이는 다른 스트림과의 결합에 사용될 수 있다. 스트림 파이프라인은 한개로 수행하며 `short-circuiting` 을 활용하여 최대한 빠르게 종료된다.

p 143

### `Optional`

`Optional<T>` 클래스는 컨테이너 클래스이다. 또한 값의 존재나 부재를 나타낸다.

대표 메소드는 다음과 같다.

-   `isPresent()` Optional이 `값을 포함` 하면 true, 아니면 false를 반환한다.
-   `ifPresent(Consumer<T> block)` 값이 존재하면 파라미터 블록을 실행한다.
-   `T get()` 값이 존재하면 값을 반환하고, 없다면 `NoSuchElementException` 을 반환한다.
-   `T orElse(T other)` 값이 존재하면 반환하고, 없다면 default 값을 반환한다.

```java
menu.stream()
 .filter(Dish::isVegetarian)
 .findAny() // Optional<Dish> 반환
 .ifPresent(dish -> System.out.println(dish.getName()); // 값이 존재하면 출력
```

### 첫번째 값 찾기

어떤 스트림은 논리적으로 나타낼 순서를 가진다. 이런 경우 첫 번째 요소를 찾을 때, `findFirst` 메소드를 사용할 수 있다. 이는 `findAny` 와 비슷하다.

```java
List<Integer> someNumbers = Arrays.asList(1, 2, 3, 4, 5);
Optional<Integer> firstSquareDivisibleByThree = someNumbers.stream()
																								.map(n -> n * n) // 1, 4, 9, 16, 25
																							  .filter(n -> n % 3 == 0)
																							  .findFirst(); // 9
```

# Reducing

`reduce` 연산을 사용해서 복잡한 쿼리들을 실행한 결과를 얻을 수 있다. Integer와 같은 단일 결과 값을 얻기 위해 반복적으로 수행해야 하는 쿼리들이 그렇다.

## 요소 더하기

```java
//for-each의 요소 합 구하기
int sum = 0;
for (int x : numbers) {
 sum += x;
}
```

`for-each` 루프 코드 속에는 두개의 파라미터가 존재한다.

-   `sum` 변수의 초기값 (0)
-   리스트 요소를 결합하는 `연산자` (+)

위의 요소 합을 stream에서는 다음과 같이 작성할 수 있다.

```java
// stream의 요소 합 구하기
int sum = numbers.stream().reduce(0, (a, b) -> a + b);
// stream의 요소 곱 구하기
int product = numbers.stream().reduce(1, (a, b) -> a * b);
```

-   초기 값 (0)
-   이전 값과 새 값을 결합하는 `BinaryOperator<T>` , 람다를 사용하여 `((a, b) -> a + b)`

`map` 과  `reduce`를 사용하는 것은 `map-reduce` 패턴으로 더 잘 알려져있다. 이는 구글의 웹 검색으로도 유명한데, 쉽게 병렬 구조로 작성할 수 있기 때문이다. 

### reduce 메소드와 병렬
`reduce`를 사용하는 것은 하나씩 반복하는 것은 추상적인 내부 반복을 통해 병렬적으로 나타낼 수 있다. 병렬은 입력과 처리, 결과를 각각 분리함을 의미한다.
```java
int sum = numbers.parallelStream().reduce(0, Integer :: sum);
```
<hr>

### stateless VS stateful
stream 대신에 `parallelStream`을 사용해서 병렬적으로 콜렉션을 만들 수 있다.

#### stateless
`map`과 `filter`와 같은 메소드는 입력 스트림을 가지며 0 또는 1개의 결과 스트림을 만든다. 이 연산들은 내부적인 상태(state)가 존재하지 않는다. 내부적으로 람다식이나 메소드가 들어오지 않는다는 의미이다.
`reduce`, `sum`, `max`와 같은 메소드는 결과를 도출하기 위한 내부적인 상태가 존재한다. 내부 상태는 얼마나 큰 스트림이 들어오던간에 정해진 크기가 주어진다.

#### stateful
filter나 map처럼 동작하는 `sorted`나 `distinct`는 스트림을 받고, 다른 스트림을 생성한다. (중간 연산자)
이 두 연산자들은 앞으로의 연산을 위해 이전 연산의 결과들이 필요하다. 따라서 처리하기 위한 데이터 버퍼 크기가 유동적이고, 처리하려는 데이터가 크거나 무한한 경우에는 문제가 생길 수 있다.

<hr>

150p 실습

<hr>

# Numeric streams
```java
int calories = menu.stream()
					.map(Dish :: getCalories)
					.reduce(0, Integer :: sum);
```
위 코드의 문제는 `boxing cost`가 증가한다는 것이다. `Integer`는 메소드 수행 전에 기본형으로 언박싱이 되어야 한다.  Java 8에서는  `IntStream`, `DoubleStream`, `LongStream` 등을 제공해서 박싱하는데 드는 비용을 감소시킨다. 위의 코드를 다음과 같이 변경할 수 있다.

```java
int calories = menu.stream()
					.mapToInt(Dish :: getCalories)
					.sum();
```

위와 같이 `sum()` 메소드는 기본적으로 0을 가지기 때문에 편리하다. (연산에 문제가 생기면 확인 가능)

## range & rangeClosed
- 범위를 나타내는 메소드
- `range` : 종료 제한 없음
- `rangeClosed` : 종료 값 존재

```java
IntStream evenNumbers = IntStream.rangeClosed(1, 100)
								.filter(n -> n % 2 == 0);

System.out.println(evenNumbers.count());
```
- **`count()`** : terminal operation

### flatMap

#### Code Example
```java
	List<String> results = 
			animals.stream().map(animal -> animal.split(""))
							.flatMap(Arrays::stream) 
							.collect(Collectors.toList());
```

```ad-tip
title: flatMap

스트림의 형태가 배열과 같을 때, 모든 원소를 `단일 원소 스트림`으로 변환하여 사용할 수 있음
이렇게 사용하면 `filter` 등 다른 일반 스트림 메소드를 활용할 수 있다.

예제 코드에서, `flatMap(Arrays::stream)`이 배열을 스트림으로 변환해주는 코드
위 코드를 사용해서 전체 스트림을 평면화한 스트림을 얻을 수 있다.

```

<hr>
# Stream 만들기
## 1. Stream.of
- 명시적 값 스트림 생성 (파라미터의 숫자 등)
```java
Stream<String> stream = Stream.of("Modern", "Java", "In", "Action");
// 대문자로 변경 후 출력
stream.map(String::toUpperCase).forEach(System.out.::println);
```
- 빈 스트림 생성
```java
Stream <String> emptyStream = Stream.empty();
```

## 2. Stream nullable
- Java 9에서 추가됨
- ex) map에서 키에 할당된 값이 없는 경우에 사용하면 명시적으로 체크할 필요가 없다
```java
String homeValue = System.getProperty("home");
// nullable 사용 X
Stream<String> homeValueStream = 
					homeValue == null ? Stream.empty() : Stream.of(value)

// nullable 사용
Stream<String> homeValueStream = Stream.ofNullable(System.getProperty("home"));

// flatMap과 사용
Stream<String> values = Stream.of("config", "home", "user")
						.flatMap(key -> Stream.ofNullable(System.getProperty(key)));
```

## 3. Stream & Array
- `Arrays.stream` 사용
```java
int [] numbers = {2, 3, 5, 7, 11, 13};
int sum = Arrays.stream(numbers).sum();
```

