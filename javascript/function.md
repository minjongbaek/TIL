# 함수

## 함수

함수는 일련의 과정을 문(statement)으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것이다.

![함수의 구성 요소](<../.gitbook/assets/Screen Shot 2022-07-05 at 3.44.32 PM.png>)

## 함수 정의

![함수 정의 방식](<../.gitbook/assets/Screen%20Shot%202022-07-05%20at%203.53.41%20PM.png>)

모든 함수 정의 방식은 함수를 정의한다는 면에서는 동일하다. 단, 미묘하지만 중요한 차이가 있다.

### 함수 선언문

함수 선언문은 표현식이 아닌 문이기 때문에 함수 이름을 생략할 수 없다.

```javascript
function (x, y) {
    return x + y;
} // SyntaxError: Function statements require a function name
```

함수 이름은 함수 몸체 내부에서만 유효한 식별자로 외부에서 함수를 호출할 수 없어야 한다. 그렇다면 어떻게 호출할 수 있을까?

```javascript
function add(x, y) {
    return x + yl
}

console.log(add(2, 3)) // 5;
```

위 코드를 보면 `add`라는 이름으로 함수를 호출한다. `add`는 자바스크립트 엔진이 생성된 함수를 호출하기 위해 암묵적으로 생성한 함수 이름과 동일한 이름의 식별자이다. 즉, 함수는 함수 이름이나 변수로 호출하는 것 처럼 보이지만, 실제로는 객체를 가리키는 식별자로 호출한다.

![함수는 함수 객체를 가리키는 식별자로 호출한다.](<../.gitbook/assets/Screen%20Shot%202022-07-05%20at%204.28.34%20PM.png>)

### 함수 표현식

자바스크립트의 함수는 일급 객체다. 함수가 일급 객체라는 것은 함수를 값처럼 자유롭게 사용할 수 있다는 의미다.

```javascript
var add = function foo (x, y) {
    return x + y;
};

console.log(add(3, 7)); // 10
console.log(foo(3, 7)); // ReferenceError: foo is not defined (함수 이름은 함수 몸체 내부에서만 유효한 식별자로 참조 헤러가 발생한다.)
```

### 함수 생성 시점과 함수 호이스팅

함수 선언문이 함수 이름으로 식별자를 생성하고 생성된 함수 객체를 할당하므로 함수 표현식과 유사하게 동작하는 것 처럼 보인다. 하지만 이 둘은 동일하게 동작하지 않는다.

```javascript
console.dir(add); // f add(x, y)
console.dir(sub); // undefined

console.log(add(2, 5));
console.log(sub(2, 5)); // TypeError: sub is not a function

function add(x, y) {
    return x + y;
}

var sub = function (x, y) {
    return x - y;
}
```

모든 선언문이 그렇듯 함수 선언문도 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행된다. 따라서 런타임 단계에서는 이미 함수 객체가 생성되어 있고 함수 이름과 동일한 식별자에 할당까지 완료된 상태가 된다. 이처럼 함수 선언문이 코드의 최상단으로 끌어 올려진 것 처럼 동작하는 자바스크립트 고유의 특징을 함수 호이스팅이라고 한다.

함수 표현식의 함수 리터럴은 런타임 시점에 평가되어 함수 객체가 된다. 따라서 함수 표현식으로 함수를 정의하면 함수 호이스팅이 아니라 변수 호이스팅이 발생된다.

### 화살표 함수

ES6에서 도입된 화살표 함수는 function 키워드 대신 화살표`=>`를 사용해 좀 더 간략한 방법으로 함수를 선언할 수 있다.

```javascript
const add = (x, y) => x + y; // 화살표 함수는 익명함수로 정의한다.
console.log(add(1, 1)); // 2
```

기존 함수와 this 바인딩 방식이 다르고, prototype 프로퍼티가 없으며 arguments 객체를 생성하지 않는다.


## 함수 호출

함수를 가리키는 식별자와 한 쌍의 소괄호인 함수 호출 연산자로 호출한다. 함수를 호출하면 현재의 실행 흐름을 중단하고 호출된 함수로 실행을 옮긴다.

### 인수 확인

자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않는다. 그리고, 자바스크립트는 동적 타입 언어기 때문에 매개변수의 타입을 사전에 지정할 수도 없다.

```javascript
function add(x, y) {
    return x + y;
}

console.log(add(1, 2)); // 3
console.log(add(2)); // NaN
console.log(add('minjong' + ' baek')); // minjong baek
```

## 참조에 의한 전달과 외부 상태의 변경

함수의 매개변수는 함수 몸체 내부에서 변수와 동일하게 취급되므로 매개변수 또한 타입에 따라 값에 의한 전달, 참조에 의한 전달 방식을 그대로 따른다.

```javascript
funcction changeValue(num, obj) {
    num += 100;
    obj.name = 'minjong'
}

var number = 10;
var person = { name: 'min' };

console.log(number, person); // 10 {name: 'min}

changeValue(number, person);

console.log(number, person); // 10 {name: 'minjong'} (참조 값이 전달된 객체의 경우 부수효과로 인해 원본값도 함께 변경된다.)
```

![값에 의한 호출과 참조에 의한 호출](<../.gitbook/assets/Screen%20Shot%202022-07-05%20at%206.03.36%20PM.png>)

## 다양한 함수의 형태

### 즉시 실행 함수

즉시 실행 함수는 익명 함수를 사용하는 것이 일반적이지만, 이름이 있는 함수도 사용할 수 있다. 하지만 괄호 내의 기명 함수는 함수 리터럴로 취급되기 때문에 함수 몸체에서만 참조할 수 있는 식별자로 다시 호출할 수 없다.

```javascript
(function () {
    var a = 3;
    var b = 5;
    return a * b;
}());

(function foo() {
    var a = 3;
    var b = 5;
    return a * b;
}());

foo(); // ReferenceError: foo is not defined
```

### 콜백 함수

```javascript
function repeat(n, f) {
    for (var i = 0; i < n; i++) {
        f(i);
    }
}

var logAll = function (i) {
    console.log(i);
}

repeat(5, logAll); // 0 1 2 3 4

var logOdds = function (i) {
    if (i % 2 === 1) console.log(i);
}

repeat(5, logOdds) // 1, 3
```

위 코드에서 `logAll`함수와 `logodds` 함수는 `repeat`함수의 매개변수를 통해 내부로 전달된다. 이처럼 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백 함수라고 한다.

`repeat`함수처럼 매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수를 고차 함수라고 한다. 콜백 함수는 고차 함수에 의해 호출되며 이때 고차 함수는 필요에 따라 콜백 함수에 인수를 전달할 수 있다.
