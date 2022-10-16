```ad-info
title: 평가

코드가 계산되어 값을 만드는 것
```
```ad-info
title: 일급 함수

- 함수를 값으로 다룰 수 있다.
- 조합성과 추상화의 도구
```
```javascript
const add5 = a => a + 5;
connst f1 = () => () => 1; // 함수 출력 또는 값 전달 가능
```

```ad-info
title: 고차 함수
- 함수를 값으로 다루는 함수
- 함수를 인자로 받아서 실행하는 함수
- 함수를 만들어 리턴하는 함수
```
```javascript
const apply1 = f => f(1);
const add2 = a => a + 2;

const addMaker = a => b => a + b;
```

## 기존과 달라진 리스트 순회
- 기존에는 배열 길이를 가져와서 길이만큼 루프 실행
#### 변경 후
```javascript
const list = [1, 2, 3];
for(const a of list) {
	log(a);
}
```

## 이터러블 / 이터레이터 프로토콜
- 이터러블 : 이터레이터를 리턴하는 `[Symbol.iterator]()`를 가진 값
- 이터레이터 : `{value, done}` 객체를 리턴하는 `next()`를 가진 값
- 이터러블 / 이터레이터 프로토콜 : 이터러블을 `for...of`, 전개 연산자 등과 함께 동작하도록 규약

## 제너레이터 / 이터레이터
- 제너레이터 : 이터레이터이며 이터러블을 생성하는 함수
```javascript
function *gen() {
	yield 1;
	yield 2;
	yield 3;
}
let iter = gen(); // 결과는 이터레이터
log(iter.next()); //{value : 1, done : false}
```
### ex) 홀수만 발생하는 iterator
```javascript
function *odds(l) {
	for(let i = 0; i < l; i++) {
		if(i % 2) yield i;
	}
}
let iter2 = odds();
log(iter2.next());

function *infinity(i = 0) {
	while(true) yield i++;
}

function *limit(l, iter) {
	for(const a of iter) {
		yield a;
		if(a == l) return;
	}
}

```