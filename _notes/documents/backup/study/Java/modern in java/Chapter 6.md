# 개요
- collectors 클래스 사용
- 스트림 데이터 -> 단일 값 변환
- 분할된 데이터 그룹화
- 커스텀 콜렉터

# Collector
- function -> element
- 스트림을 간소화하여 단일 값 도출
- 요소 그룹화
- 요소 나누기

## 스트림 요약
- 스트림 내의 값들을 특정 기준으로 단일 값을 도출한다.
```java
long howManyDishes = menu.stream().collect(Collectors.counting());

// collectors
long howManyDishes = menu.stream().count();
```

### 최대 / 최솟값 구하기
- `maxBy`, `minBy` 이용
- 비어있을 경우에는 반환하지 않음
```java
Comparator<Dish> dishCaloriesComparator =
    Comparator.comparingInt(Dish::getCalories);

Optional<Dish> mostCalorieDish =
    menu.stream()
        .collect(maxBy(dishCaloriesComparator));
```
### 요약
- 합계와 관련한 메소드 제공 : `summingInt`
```java
int totalCalories = menu.stream().collect(summingInt(Dish::getCalories));
```
- Long, Double type에 대응하는 `averagingLong`, `averagingDouble` 제공
```java
double avgCalories = menu.stream().collect(averagingInt(Dish::getCalories));
```
