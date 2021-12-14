# Set과 Map

## Set객체

- 중복되지 않는 유일한 값들의 집합이다.
- 수학적 집합의 특성과 일치한다. (수학적 집합을 구현하기 위한 자료구조)
- 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.
- 배열과 유사하다.

### 배열과의 차이점

- 동일한 값을 중복하여 포함할 수 없다.
- 요소 순서에 의미가 없다.
- 인덱스로 요소에 접근할 수 없다.

### Set객체 생성

`const set = new Set(); // 빈 Set객체 생성`

Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다. (중복 제거됨)

```jsx
const set1 = new Set([1, 2, 3, 3]); // Set(3) {1, 2, 3}
const set2 = new Set('hello']); // Set(3) {"h", "e", "l", "o"}

// Set을 사용해 배열의 중복 요소 제거
const setArr = array => [...new Set(array)];
setArr([2, 1, 2, 3, 4, 3, 4]); // [2, 1, 3, 4];
```

### 요소 개수 확인

Set.prototype.size 프로퍼티 사용

`const set = new Set([1, 2, 3]);`
`console.log(set.size); // 3`

### 요소 추가

Set.prototype.add 메서드 사용

```jsx
const set = new Set();

set.add(1);
set.add(2).add(3).add(3); // 메서드 체이닝(연속적 호출)가능, 중복 값 무시
```

- Set객체는 NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
- 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.

```jsx
const set = new Set();

set
	.add(NaN)
	.add(NaN)
  .add(1)
  .add('a')
  .add(true)
  .add(undefined)
  .add(null)
  .add({})
  .add([]);

console.log(set); 
// Set(7) {NaN, 1, "a", true, undefined, null, {}, []}
```

### 존재 여부 확인

Set.prototype.has 메서드를 사용

```jsx
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

### 요소 삭제

Set.prototype.delete 메서드 사용

- delete메서드는 삭제 여부를 나타내는 불리언 값을 반환한다.
- 존재하지 않는 요소를 삭제하면 무시된다.
- 메서드 체이닝(연속 호출)을 할 수 없다.

```jsx
const set = new Set([1, 2, 3]);

set.delete(2);
set.delete(1);
console.log(set); // Set(2) {3}
```

요소 일괄 삭제

Set.prototype.clear 메서드 사용

`set.clear();`

### 요소 순회

Set.prototype.forEach 메서드 사용

세 개의 인수를 전달받는다.

- 첫 번째 인수: 현재 순회 중인 요소값
- 두 번째 인수: 현재 순회 중인 요소값
- 세 번째 인수: 현재 순회 중인 Set 객체 자체

(첫 번째 인수와 두 번째 인수는 같은 값이다.)

```jsx
const set = new Set([1, 3, 5]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) {1, 3, 5}
3 3 Set(3) {1, 3, 5}
5 5 Set(3) {1, 3, 5}
*/
```

Set 객체는 이터러블이기 때문에 for ... of 문으로 순회할 수 있고, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있다.

```jsx
const set = new Set([1, 2, 3]);

for (const value of set) {
  console.log(value); // 1 2 3 - for...of
}

console.log([...set]); // [1, 2, 3] - 스프레드

const [a, ...rest] = [...set];
console.log(a, rest); // 1, [2, 3] - 디스트럭처링
```

### 집합 연산

Set 객체는 수학적 집합을 구현하기 위한 자료구조다.

따라서 Set 객체를 통해 교집합, 합집합, 차집합 등을 구현할 수 있다.

- 교집합- `setA.intersection(setB)`
- 합집합 - `setA.union(setB)`
- 차집합 - `setA.difference(setB)`
- 상위 집합(부분 집합) - `setA.isSuperset(setB)`

---

## Map 객체

- 키와 값의 쌍으로 이루어진 컬렉션이다.
- 객체와 유사하다.

객체와 차이점

1. 키로 사용할 수 있는 값: 객체를 포함한 모든 값 (객체: 문자열 또는 심벌 값)
2. 이터러블이다.
3. 요소 개수 확인: map.size (객체: Object.keys(obj).length)

### Map 객체 생성

- Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다.
- 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.
- 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다.

```jsx
const map = new Map(); // 빈 Map 객체 생성, Map(0) {}

const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
// Map(2) {'key1' => 'value1', 'key2' => 'value2'}

const map2 = new Map([['key1', 'value1'], ['key1', 'value2']]);
// Map(1) {"key1" => "value2"}
```

### 요소 개수 확인

Map.prototype.size 프로퍼티 사용

`const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);`

`console.log(map.size); // 2`

### 요소 추가

Map.prototype.set 메서드 사용

```jsx
const map = new Map();

map.set('key1', 'value1');
map
	.set('key2', 'value2')
	.set('key3', 'value3') // 메서드 체이닝(연속적 호출) 가능
	.set('key3', 'value4'); // 중복된 키를 갖는 요소를 추가하면 값이 덮어써짐

// Map(3) {'key1' => 'value1', 'key2' => 'value2', 'key3' => 'value4'}
```

Map 객체는 Set과 같이 NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
(같은 키로 사용한 것처럼 덮어 써진다.)

`map.set(NaN, 'value1').set(NaN, 'value2'); // Map(1) { NaN => 'value2' }`

객체는 문자열 또는 심벌 값만 키로 사용할 수 있지만 Map 객체는 키 타입에 제한이 없다.

객체를 포함한 모든 값을 키로 사용할 수 있다. (객체와의 가장 큰 차이점)

### 요소 취득

Map.prototype.get 메서드 사용

```jsx
const map = new Map();
map.set('key1', 'value1');

map.get('key1'); // 'value1'
map.get('nokey'); // 존재하지 않으면 undefined
```

### 요소 존재 여부 확인

Map.prototype.has 메서드 사용

has 메서드는 요소의 존재 여부를 나타내는 불리언 값을 반환한다.

```jsx
const map = new Map();
map.set('key1', 'value1');

map.has('key1'); // true
map.has('nokey'); // false
```

### 요소 삭제

Map.prototype.delete 메서드 사용

delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.

```jsx
const map = new Map();
map.set('key1', 'value1');

map.delete('key1');
map.delete('nokey'); // 존재하지 않는 키를 사용하면 무시됨
```

요소 일괄 삭제

Map.prototype.clear 메서드 사용 (항상 undefined 반환)

`map.clear();`

### 요소 순회

Map.prototype.forEach 메서드 사용

세 개의 인수를 전달받는다.

- 첫 번째 인수: 현재 순회 중인 요소값
- 두 번째 인수: 현재 순회 중인 요소키
- 세 번째 인수: 현재 순회 중인 Map 객체 자체

```jsx
const map = new Map([['key1', 'value1'], ['key2', 'value2']]);

map.forEach((v, k, map) => console.log(v, k, map));

/*
value1 key1 Map(2) {'key1' => 'value1', 'key2' => 'value2'}
value2 key2 Map(2) {'key1' => 'value1', 'key2' => 'value2'}
*/
```

Map 객체는 이터러블이기 때문에 for ... of 문으로 순회할 수 있고, 스프레드 문법과 배열 디스트럭처링 할당의 대상이 될 수 있다.

```jsx
const map = new Map([['key1', 'value1'], ['key2', 'value2']]);

for (const entry of map) {
  console.log(entry); // ['key1', 'value1'], ['key2', 'value2'] - for...of
}

console.log([...map]);
// [['key1', 'value1'], ['key2', 'value2']] - 스프레드

const [a, b] = map;
console.log(a, b); // ['key1', 'value1'], ['key2', 'value2'] - 디스트럭처링
```

Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.

- Map.prototype.keys - 요소키를 값으로 갖는 이터러블/이터레이터인 객체 반환
- Map.prototype.values - 요소값을 값으로 갖는 이터러블/이터레이터인 객체 반환
- Map.prototype.entries - 요소키와 요소값을 값으로 갖는 이터러블/이터레이터인 객체 반환

```jsx
const map = new Map([['key1', 'value1'], ['key2', 'value2']]);

for (const key of map.keys()) {
  console.log(key); // key1 key2
}

for (const value of map.values()) {
  console.log(value); // value1 valye2
}

for (const entry of map.entries()) {
  console.log(entry); // ['key1', 'value1'] ['key2', 'value2']
}
```

--- 
<br>

> ref) 모던 자바스크립트 딥다이브