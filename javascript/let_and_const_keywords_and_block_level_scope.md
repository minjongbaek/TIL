# let, const 키워드와 블록 레벨 스코프

## var 키워드로 선언한 변수의 문제점

`var` 키워드는 다음과 같은 단점이 있다.

* `var` 키워드로 선언한 변수는 중복 선언이 가능하다.
* `var` 키워드로 선언한 변수는 함수의 코드 블록만을 지역 스코프로 인정하기 때문에 `for`, `if` 등의 코드 블록 내에서 선언해도 모두 전역 변수가 된다.
* 변수 호이스팅에 의해 `var` 키워드로 선언한 변수는 변수 선언문 이전에 참조가 가능하다.

이러한 단점들이 가독성을 떨어뜨리고 오류를 발생시킬 여지를 남긴다. ES6에서는 새로운 변수 선언 키워드인 `let`과 `const`를 도입했다.

## let 키워드

### 변수 중복 선언 금지

`let` 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러가 발생한다.

```javascript
var foo = 123;
var foo = 456;

let bar = 123; // let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

### 블록 레벨 스코프

`let` 키워드로 선언한 변수는 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

![블록 레벨 스코프의 중첩](<../.gitbook/assets/Screen Shot 2022-07-05 at 3.42.08 PM.png>)

### 변수 호이스팅

`var` 키워드로 선언한 변수는 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 '선언 단계'와 '초기화 단계'가 한 번에 진행된다. 그러나, `let` 키워드로 선언한 변수는 '선언 단계'와 '초기화 단계'가 분리되어 진행된다.

런타임 이전에 자바스크립트 엔진에 의해 선언 단계가 실행되지만 초기화는 변수 선언문에 도달했을 때 실행된다.

초기화 단계 이전에 변수에 접근하려고 하면 참조 에러가 발생하게 된다. 스코프의 시작 지점부터 초기화 지점까지 변수를 참조할 수 없는 구간을 일시적 사각지대라고 부른다.

![let 키워드로 선언한 변수의 생명 주기](<../.gitbook/assets/Screen Shot 2022-07-08 at 5.22.51 PM.png>)

```javascript
let foo = 1;

{
    console.log(foo); // ReferenceError: Cannot access 'foo' before initialization (호이스팅이 발생하기 때문에 전역 변수 foo의 값을 출력하지 않고 참조 에러가 발생한다.)
    let foo = 2;
}
```

자바스크립트는 `let`, `const`를 포함한 모든 선언 (`function`, `class` 등)을 호이스팅한다. 단 ES6에서 도입된 `let`, `const`, `class를` 사용한 선언문은 호이스팅이 발생하지 않는 것 처럼 동작한다.


## const 키워드

### 선언과 초기화

`const` 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.

`const` 키워드로 선언한 변수는 `let` 과 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것 처럼 동작한다.

```javascript
{
    console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
    const foo = 1;
    console.log(foo); // 1
}

console.log(foo); // ReferenceError: foo is not defined (블록 레벨 스코프를 갖는다.)
```

### 재할당 금지

`var`이나 `let` 키워드로 선언한 벼눗와 다르게 `const` 키워드로 선언한 변수는 재할당이 금지된다.

```javascript
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```

### 상수

변수의 상대 개념인 상수는 재할당이 금지된 변수를 말한다. `const` 키워드로 선언된 변수에 원시 값을 할당한 경우 할당된 값을 변경할 수 있는 방법은 없다.

### const 키워드와 객체

`const` 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다. 원시 값은 재할당 없이 변경할 수 있는 방법이 없지만, 객체는 직접 변경이 가능하기 때문이다.

`const` 키워드는 재할당을 금지할 뿐 '불변'을 의미하지는 않는다.

```javascript
const person = {
    name: 'minjong'
};

person.name = 'Baek';

console.log(person) // {name: "Baek"}
```
