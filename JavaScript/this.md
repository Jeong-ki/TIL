## this

객체지향 프로그래밍 - 객체는 상태(state)를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조이다.

동작을 나타내는 메서드는 자신이 속한 객체의 상태(프로퍼티)를 참조하고 변경할 수 있어야 한다.  


> this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 **자기 참조 변수**다.
this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.  
>  

this를 지역 변수처럼 사용할 수 있다.
this가 가리키는 값(this바인딩)은 함수 호출 방식에 의해 동적으로 결정된다.

<aside>
💡 바인딩이란 식별자와 값을 연결하는 과정을 의미한다.
변수 선언은 변수 이름(식별자)와 메모리 공간의 주소를 바인딩 하는 것이다.
this 바인딩은 this와 this가 가리킬 객체를 바인딩하는 것이다.
</aside>  <br>
this 참조 예시

```javascript
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  }
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person('Lee');
```

2.함수 호출 방식과 this 바인딩

this 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

기본적으로 this에는 전역 객체가 바인딩된다.

함수 호출 방식에 따른 this 바인딩

- 일반 함수 호출
    - 전역 객체 바인딩
    - use strict가 적용된 일반 함수에는 undefined가 바인딩
    - 일반 함수로 호출된 모든 함수(중첩, 콜백함수 포함) → 전역 객체가 바인딩

일반 함수로 호출된 모든 함수(중첩, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩 된다.

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); // {value: 100, foo: ƒ}
    // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
    setTimeout(function () {
      console.log("callback's this: ", this); // window
      console.log("callback's this.value: ", this.value); // 1
    }, 100);
  }
};

obj.foo();
```

this를 명시적으로 바인딩할 수 있는 메서드

- Function.prototype.apply / Function.prototype.call / Function.prototype.bind

(+this <- that / 화살표 함수)

중첩/콜백함수 바인딩을 메서드의 this바인딩과 일치시키기

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    // this 바인딩(obj)을 변수 that에 할당한다.
    const that = this;

    // 콜백 함수 내부에서 this 대신 that을 참조한다.
    setTimeout(function () {
      console.log(that.value); // 100
    }, 100);
  }
};

obj.foo();
```

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 콜백 함수에 명시적으로 this를 바인딩한다.
    setTimeout(function () {
      console.log(this.value); // 100
    }.bind(this), 100);
  }
};

obj.foo();
```

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100); // 100
  }
};

obj.foo();
```

### 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체(마침표' . '연산자 앞에 기술한 객체)가 바인딩 된다.

this에 바인딩될 객체는 호출 시점에 결정된다.

### 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 (미래에)생성할 인스턴스가 바인딩된다.

```jsx
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

```jsx
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다. 즉, 일반적인 함수의 호출이다.
const circle3 = Circle(15);

// 일반 함수로 호출된 Circle에는 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로 호출된 Circle 내부의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

Function.prototype.apply/call/bind 메서드에 의한 간접 호출

- apply와 call 메서드 - this로 사용할 객체를 전달하면서 함수를 호출

- apply - 호출할 함수의 인수를 배열로 묶어 전달

- call - 쉼표로 구분한 리스트 형식으로 전달

<br>  

### apply, call의 대표적인 용도

- argument 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우
- argument 객체는 배열이 아니기 때문에 Array.prototype.slice같은 배열 메서드를 사용할 수 없지만 apply와 call 메서드를 이용하면 가능!

```jsx
function convertArgsToArray() {
  console.log(arguments);

  // arguments 객체를 배열로 변환
  // Array.prototype.slice를 인수없이 호출하면 배열의 복사본을 생성한다.
  const arr = Array.prototype.slice.call(arguments);
  // const arr = Array.prototype.slice.apply(arguments);
  console.log(arr);

  return arr;
}

convertArgsToArray(1, 2, 3); // [1, 2, 3]
```

bind

- apply, call와 달리 함수를 호출하지 않고 this로 사용할 객체만 전달
- 메서드의 this와 메서드 내부의 중첩, 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

bind 사용 전

```jsx
const person = {
  name: 'Lee',
  foo(callback) {
    setTimeout(callback, 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // window.name
});
```

bind 사용

```jsx
const person = {
  name: 'Lee',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```

ref)
자바스크립트 딥다이브