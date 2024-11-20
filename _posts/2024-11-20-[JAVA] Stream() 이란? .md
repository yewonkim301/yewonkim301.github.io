---
title: "[JAVA] Stream() 이란?"
date: 2024-11-20
categories: [JAVA]
tags: [TIL, JAVA]
---

## 📍 Stream(스트림) 이란?

- 스트림은 사전적으로 '흐르다'는 의미를 가지며, 프로그래밍에서는 '데이터의 흐름'을 의미한다.
- 스트림은 데이터 소스를 추상화하여 데이터 소스가 무엇인던지 같은 방식으로 다룰 수 있게 되어, 코드의 재사용성이 높다.

<br />

### # Stream의 특징

1. 스트림은 데이터 소스를 변경하지 않는다.

2. 스트림은 일회용이다.
   : Iterator로 컬렉션의 요소를 모두 읽고 나면 사용할 수 없는 것처럼, 스트림도 한 번 사용하면 다시 사용할 수 없기 때문에 다시 사용하고 싶다면 다시 생성해야 한다.

<br />

---

### # Stream의 연산과 메서드

1. **중간 연산** : 연산 결과가 스트림이므로, 스트림에 연속해서 연산할 수 있다.

| 중간 연산                              | 반환 타입         | 설명                              |
|----------------------------------------|-------------------|-----------------------------------|
| `distinct()`                           | `Stream<T>`       | 중복을 제거                       |
| `filter(Predicate<T> predicate)`       | `Stream<T>`       | 조건에 맞지 않는 요소 제외        |
| `limit(long maxSize)`                  | `Stream<T>`       | 스트림의 일부를 잘라낸다.         |
| `skip(long n)`                         | `Stream<T>`       | 스트림의 일부를 건너뛴다.         |
| `peek(Consumer<T> action)`             | `Stream<T>`       | 스트림의 요소에 작업 수행         |
| `sorted()`                             | `Stream<T>`       | 스트림의 요소를 정렬              |
| `sorted(Comparator<T> comparator)`     | `Stream<T>`       | 구분자를 이용하여 스트림의 요소를 정렬 |
| `map(Function<T, R> mapper)`           | `Stream<R>`       | 스트림의 요소를 변환              |
| `flatMap(Function<T, Stream<R>> mapper)` | `Stream<R>`       | 스트림을 평탄화                   |
| `mapToInt(ToIntFunction mapper)`       | `IntStream`       | 스트림의 요소를 `int`로 변환      |
| `flatMapToInt(Function<T, IntStream> mapper)` | `IntStream`       | 스트림을 평탄화 후 `IntStream` 반환 |
| `mapToDouble(ToDoubleFunction<T> mapper)` | `DoubleStream`   | 스트림의 요소를 `double`로 변환   |
| `flatMapToDouble(Function<T, DoubleStream> mapper)` | `DoubleStream` | 스트림을 평탄화 후 `DoubleStream` 반환 |
| `mapToLong(ToLongFunction mapper)`     | `LongStream`      | 스트림의 요소를 `long`으로 변환   |
| `flatMapToLong(Function<T, LongStream> mapper)` | `LongStream`    | 스트림을 평탄화 후 `LongStream` 반환 |


<br />

2. **최종 연산** : 연산 결과가 스트림이 아니라 연산이므로, 단 한번만 사용할 수 있다.

| 종료 연산                              | 반환 타입         | 설명                              |
|----------------------------------------|-------------------|-----------------------------------|
| `forEach(Consumer<? super T> action)`  | `void`            | 각 요소에 지정된 작업 수행        |
| `forEachOrdered(Consumer action)`      | `void`            | 순서대로 각 요소에 작업 수행      |
| `count()`                              | `long`            | 스트림의 요소 개수 반환           |
| `max(Comparator<? super T> comparator)` | `Optional<T>`    | 스트림의 최대값 반환              |
| `min(Comparator<? super T> comparator)` | `Optional<T>`    | 스트림의 최소값 반환              |
| `findAny()`                            | `Optional<T>`     | 스트림 요소 중 아무거나 하나 반환 |
| `findFirst()`                          | `Optional<T>`     | 스트림의 첫 번째 요소 반환        |
| `allMatch(Predicate<T> p)`             | `boolean`         | 주어진 조건을 모두 만족시키는지 확인 |
| `anyMatch(Predicate<T> p)`             | `boolean`         | 주어진 조건을 하나라도 만족시키는지 확인 |
| `noneMatch(Predicate<T> p)`            | `boolean`         | 주어진 조건을 모두 만족하지 않는지 확인 |
| `toArray()`                            | `Object[]`        | 스트림의 모든 요소를 배열로 반환   |
| `toArray(IntFunction<A[]> generator)`  | `A[]`             | 스트림의 모든 요소를 배열로 반환   |
| `reduce(BinaryOperator<T> accumulator)` | `Optional<T>`    | 스트림의 요소를 하나씩 줄여가며 결과 계산 |
| `reduce(T identity, BinaryOperator<T> accumulator)` | `T` | 초기값과 함께 요소를 줄여 결과 반환 |
| `reduce(U identity, BinaryOperator<U> accumulator, BinaryOperator<U> combiner)` | `U` | 병렬 스트림을 줄이는 연산         |
| `collect(Collector<T, A, R> collector)` | `R`              | 스트림의 요소를 수집              |
| `collect(Supplier<R> supplier, BiConsumer<R, T> accumulator, BiConsumer<R, R> combiner)` | `R` | 요소를 그룹화하거나 분할하여 결과 반환 |

<br />

---

### # For과 Stream의 차이

| | 원시 for문 | 향상된 for문 | stream |
|---------------|---------------|---------------|---------------|
| 형식 | for(초기화 ; 조건 ; 후처리) | for( Integer i : list) | stream 생성 -> 중간 연산 -> 최종 연산 |
| 등장 시기 | java1부터 | java5 부터 등장 | java8부터 등장 |
| 특징 | 대체로 가장 좋은 성능을 보임 | 가독성 & 안정성 증가 | 가독성 증가 |

<br />

#### # 향상된 for문

전통 방식으로 반복문을 돌리게 되면, 내부 인덱스를 벗어남으로 인해 발생하는 `java.lang.IndexOutOfBoundsException` 에러를 간혹 마주허게 된다.
**향상된 for each문**은 배열 혹은 리스트의 인덱스로 반복을 진행하는 것이 아니라, 내부 원소들을 모두 반복하기 때문에 이러한 에러를 방지할 수 있으며 이에 따라 **안정성이 증가**된다.

<br />

#### # Stream

![img](/assets/img/til/stream.png)

- 스트림은 함수객체로 표현되면, 람다식이나 메서드 참조가 가능하다. 이는 표현을 간결하게 해준다는 장점이 있지만, 람다식이기 때문에 외부변수(지역 변수)를 사용하여 수정할 수 없다는 단점이 있다.
- 스트림에서는 `continue`, `break`를 이용한 로직은 사용할 수 없다.
- 또한 스트림은 내부 메소드들을 참조하는 형식이기 때문에 에러 스택 트레이싱이 다소 어렵다.

<br />

#### # 정리

원시타입의 데이터만을 반복하고 싶을 때는 for문을 사용하는 것이 성능상 좋다. 하지만 실무적인 부분에서 가독성과 유지보수 측면을 고려한다면 스트림을 사용하는 것도 좋다. 따라서 한 가지만을 사용한다기보다 사용 목적과 사용 환경을 고려하여 사용하는 것이 중요하다.

<br /><br />


참고 : 
- [[Java/자바] Stream(스트림)이란? 사용법 총정리](https://hstory0208.tistory.com/entry/Java%EC%9E%90%EB%B0%94-Stream%EC%8A%A4%ED%8A%B8%EB%A6%BC%EC%9D%B4%EB%9E%80)
- [👀[JAVA] For과 Stream은 어떤 차이가 있는걸까?](https://velog.io/@foureaf/JAVA-For%EA%B3%BC-Stream%EC%9D%80-%EC%96%B4%EB%96%A4-%EC%B0%A8%EC%9D%B4%EA%B0%80-%EC%9E%88%EB%8A%94%EA%B1%B8%EA%B9%8C)
- [우아한 테크 / [10분 테코톡] 크리스, 로마의 stream vs for](https://www.youtube.com/watch?v=by8hb75i9X4)