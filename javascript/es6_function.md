# ES6 함수의 추가 기능

## 함수의 구분

ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다. 다시 말해, ES6 이전의 모든 함수는 `callable`이면서 `constructor`이다.

```javascript
var foo = function () {
    return 1;
};

var obj = { foo };

console.log(foo()); // 1 (일반적인 함수 호출)
new foo(); // foo {} (생성자 함수로서 호출)
console.log(obj.foo()); // 1 (메서드로서 호출)
```

ES6 이전의 함수는 사용 목적에 따라 명확한 구분이 없으므로 호출 방식에 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성한다. 이런 혼란스러운 문제를 해결하기 위해 ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 명확히 구분했다.

![ES6 함수의 구분](<./../.gitbook/assets/Screen Shot 2022-07-26 at 4.53.01 PM.png>)

## 메서드

ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다. ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 `[[HomeObject]]`를 갖기 때문에 `super`키워드를 사용할 수 있다.

```javascript
const obj = {
    x: 1,
    foo() { return this.x }, // 메서드
    bar: function() { return this.x } // 일반 함수
};

new obj.foo(); // obj.foo is not a constructor
new obj.bar(); // bar {}

obj.foo.hasOwnProperty('prototype') // false (ES6 메서드이므로 prototype 프로퍼티가 없다.)

obj.bar.hasOwnProperty('prototype') // true (일반 함수이므로 prototype 프로퍼티가 있다.)
```

## 화살표 함수

화살표 함수는 `function` 키워드 대신 화살표 `=>`를 사용하여 기존의 함수 정의 방식보다 더 간략하게 함수를 정의할 수 있다. 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.

```javascript
const multiply = (x, y) => x * y; // 함수 표현식으로 정의해야 한다.
console.log(multiply(2, 3)) // 6

const arrow1 = x => { ... }; // 매개변수가 한 개인 경우 소괄호를 생략할 수 있다.
const arrow2 = () => { ... }; // 매개변수가 없는 경우 소괄호를 생략할 수 없다.

const arrow3 = () => const x = 1; // SyntaxError: Unexpected token 'const' (중괄호를 생략하는 경우 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다.)


// 아래 두 코드는 결과 값이 같다.
[1, 2, 3].map(function(v) {
    return v * 2;
}); // (3) [2, 4, 6]

[1, 2, 3].map(v => v * 2); // (3) [2, 4, 6] (화살표 함수도 일급 객체이므로 고차 함수에 인수로 전달할 수 있다.)
```

### 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 `non-constructor`이다.
2. 중복된 매개변수 이름을 선언할 수 없다.
3. 화살표 함수는 함수 자체의 `this`, `arguments`, `super`, `new.target` 바인딩을 갖지 않는다.

### this

화살표 함수는 함수 자체의 `this` 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 `this`를 참조하면 상위 스코포의 `this`를 그대로 참조한다.

화살표 함수가 `Function.prototype.call`, `Function.prototype.apply`, `Function.prototype.bind` 메서드를 호출할 수 없다는 의미는 아니다. 다만, 함수 자체의 `this` 바인딩을 갖지 않기 때문에 `this`를 교체할 수 없고 언제나 상위 스코프의 `this` 바인딩을 참조한다.

### arguments

화살표 함수는 `arguments` 바인딩을 갖지 않는다. 따라서 `this`와 마찬가지로 상위 스코프의 `arguments`를 참조한다. 화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 `Rest` 파라미터를 사용해야 한다.

## Rest 파라미터

Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.

```javascript
function foo(param, ...rest) { // 일반 매개변수와 함께 사용할 수 있다.
    console.log(param, rest); // 1 [2, 3, 4, 5]
    console.log(foo.length); // 1 (매개변수 개수를 타나내는 length 프로퍼티에 영향을 주지 않는다.)
}

foo(1, 2, 3, 4, 5);
```

## 매개변수 기본값

ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다. Rest 파라미터에는 기본값을 지정할 수 없다.

```javascript
function sum(x = 0, y = 0) {
    console.log(arguments, sum.length);
    return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
```