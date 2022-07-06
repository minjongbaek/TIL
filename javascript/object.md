# 객체

## 객체

자바스크립트는 객체 기반의 프로그래밍 언어이며, 자바스크립트를 구성하는 거의 모든 것이 객체다. 원시 값을 제외한 나머지 값(함수, 배열 정규 표현식 등)은 모두 객체다.

원시 값은 변경이 불가능한 값이지만, 객체는 변경 가능한 값이다.

### 구성 요소

객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키와 값 (key - value)으로 구성된다.

자바스크립트의 함수는 일급객체이므로 값으로 취급되어 프로퍼티 값으로 사용할 수 있다. 프로퍼티 값이 함수인 경우 일반 함수와 구분하기 위해 메서드라 부른다.

![객체는 프로퍼티의 집합](<../.gitbook/assets/Screen Shot 2022-07-06 at 1.34.47 PM.png>)

![객체의 프로퍼티와 메서드](<../.gitbook/assets/Screen Shot 2022-07-06 at 1.34.58 PM.png>)


## ES6에서 추가된 객체 리터럴의 확장 기능

### 프로퍼티 축약 표현

ES6에서는 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있다. 이때 프로퍼티 키는 변수 이름으로 자동 생성된다.

```javascript
var x = 1, y = 2;
var objA = {
    x: x,
    y: y
};

console.log(objA); // {x: 1, y: 2}

var objB = { x, y };

console.log(objB); // {x: 1, y: 2}
```

### 계산된 프로퍼티 이름

키를 동적으로 생성하는 경우 프로퍼티 키로 사용할 표현식을 대괄호([...])로 묶어서 사용할 수 있다.

```javascript
var prefix = 'prop';
var i = 0;
var j = 0;

var objA = {};
objA[prefix + '-' + ++i] = i;
objA[prefix + '-' + ++i] = i;
objA[prefix + '-' + ++i] = i;

console.log(objA); // {prop-1: 1, prop-2: 2, prop-3: 3}

const objB = {
    [`${prefix}-${++j}`]: j,
    [`${prefix}-${++j}`]: j,
    [`${prefix}-${++j}`]: j
}

console.log(objB); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

### 메서드 축약 표현

ES6에서는 메서드를 정의할 때 function 키워드를 생략한 축약 표현을 사용할 수 있다.

```javascript
var objA = {
    name: 'minjong',
    sayHi: function() {
        console.log('Hi! ' + this.name); // Hi! minjong
    }
};

objA.sayHi();

const objB = {
    name: 'minjong',
    sayHi() {
        console.log('Hi! ' + this.name); // Hi! minjong
    }
};

objB.sayHi();
```