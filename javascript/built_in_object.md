# 빌트인 객체

## 자바스크립트 객체의 분류

자바스크립트 객체는 다음과 같이 크게 3개의 객체로 분류할 수 있다.

* 표준 빌트인 객체 (ECMAScript 사양에 정의된 객체)
* 호스트 객체 (자바스크립트 실행 환경에서 제공하는 객체)
* 사용자 정의 객체 (사용자가 직접 정의한 객체)

## 표준 빌트인 객체

ECMAScript 사양에 정의된 객체를 말하며 전역 객체의 프로퍼티로서 제공된다. 따라서 별도의 선언 없이 전역 변수처럼 언제나 참조할 수 있다.

`Math`, `Reflect`, `JSON`을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다. 생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공하고 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.

```javascript
const numObj = new Number(1.5);

console.log(typeof numObj); // object (생성자 함수로 호출한 인스턴스)

console.log(numObj.toFixed()); // 2 (Number.prototype 메서드 호출)

console.log(Number.isInteger(0.5)) // false(Number 정적 메서드 호출)
```

## 원시값과 래퍼 객체

원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없지만 원시값에 대해 객체처럼 마침표 표기법으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해준다. 암묵적으로 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.

이처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체라고 한다.

```javascript
// 식별자 str은 문자열을 값으로 갖는다.
const str = 'hello';

// 식별자 str은 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.
str.name = 'Lee';

// 식별자 str은 새롭게 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 새롭게 생성된 래퍼 객체에는 name 프로퍼티가 존재하지 않는다.
console.log(str.name);
```

## 전역 객체

전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체다.

전역 객체는 표준 빌트인 객체와 환경에 다른 호스트 객체, 그리고 `var` 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.

### 빌트인 전역 프로퍼티

빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미한다.

#### Infinity

`Infinity` 프로퍼티는 무한대를 나타내는 숫자값을 갖는다.

```javascript
console.log(3/0); // Infinity
console.log(-3/0);// Infinity

console.log(typeof Infinity) // number
```

#### NaN

`NaN` 프로퍼티는 숫자가 아님을 나타내는 숫자값 NaN을 갖는다.

```javascript
console.log(Number('xyz')); // NaN
console.log(1 * 'string'); // NaN

console.log(typeof NaN); // number
```

#### undefined

`undefined` 프로퍼티는 원시 타입 undefined를 값으로 갖는다.

```javascript
var foo;
console.log(foo); // undefined
console.log(typeof undefined); // undefined
```

### 빌트인 전역 함수

빌트인 전역 함수는 애플리케이션 전역에서 호출할 수 잇는 빌트인 함수로서 전역 객체의 메서드다.

#### eval

`eval` 함수는 자바스크립트 코드를 나타내는 문자열을 인수로 전달받는다. 전달받은 문자열 코드가 표현식이라면 `eval` 함수는 문자열 코드를 런타임에 평가하여 값을 생성하고, 표현식이 아닌 문이라면 런타임에 실행한다.

```javascript
eval('1 + 2;');
eval('var x = 5;');

console.log(x); // 5

const f = eval('(function() { return 1; })');
console.log(f()); // 1
```

#### isFinite

전달받은 인수가 정상적인 유한수인지 검사하여 불리언 값을 반환한다. 인수의 타입이 숫자가 아닌 경우, 숫자로 타입을 변환한 후에 검사를 수행한다.

```javascript
isFinite(0);// true
isFinite('10'); // true
isFinite(null); // true

isFinite(Infinity); // false
isFinie(NaN); // false
isFinie('Hello'); // false
```

#### isNaN

전달받은 인수가 `NaN`인지 검사하여 그 결과를 불리언 타입으로 반환한다. 인수의 타입이 숫자가 아닌 경우, 숫자로 타입을 변환한 후에 검사를 수행한다.

```javascript
isNaN(NaN); // true
isNaN(undefined) // true

isNaN(10); // false
isNaN('10'); // false
isNaN(true); // false
```

#### parseFloat

전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환한다.

```javascript
parseFloat('3.14'); // 3.14
parseFloat('10.00'); // 10
```

#### parseInt

전달받은 인수가 문자열이 아니면 문자열로 변환한 다음, 정수로 해석하여 반환한다.

```javascript
parseInt('10'); // 10
parseInt('10.123'); // 10

parseInt('10.123'); // 10
```

두 번째 인수로 진법을 나타내는 기수를 전달하게 되면 해당 기수의 숫자로 해석하여 반환한다.

```javascript
parseInt('10'); // 10
parseInt('10', 2); // 2
parseInt('10', 8); // 8
parseInt('10', 16); // 16
```

#### encodeURI / decodeURI

완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩 및 디코딩 한다.

```javascript
const uri = 'http://book.minjongdev.com?name=백민종';
const enc = encodeURI(uri);
console.log(enc); // http://book.minjongdev.com?name=%EB%B0%B1%EB%AF%BC%EC%A2%85
console.log(decodeURI('http://book.minjongdev.com?name=%EB%B0%B1%EB%AF%BC%EC%A2%85')); // http://book.minjongdev.com?name=백민종
```

#### encodeURIComponent / decodeURIComponent

`encodeURIComponent` 함수는 URI 구성 요소를 인수로 전달받아 인코딩 및 디코딩한다. 쿼리 스트링 구분자로 사용되는 `=`, `?`, `&`까지 인코딩한다.

```javascript
const uriComp = 'name=벡민종&job=programmer';

let enc = encodeURIComponent(uriComp);
console.log(enc); // name%3D%EB%B2%A1%EB%AF%BC%EC%A2%85%26job%3Dprogrammer

let dec = decodeURIComponent(uriComp);
console.log(dec); // name=벡민종&job=programmer

// encodeURI 함수는 인수로 전달받은 문자열을 완전한 URI로 간주하기 때문에 =, ?, &를 인코딩하지 않는다.
enc = encodeURI(uriComp);
console.log(enc); // name=%EB%B2%A1%EB%AF%BC%EC%A2%85&job=programmer

dec = decodeURI(uriComp);
console.log(dec); // name=벡민종&job=programmer
```

### 암묵적 전역

선언하지 않은 식별자에 값을 할당했을 때 자바스크립트 엔진은 전역 객체에 프로퍼티를 동적으로 생성한다. 이처럼 선언하지 않은 식별자가 전역 변수처럼 동작하는 현상을 암묵적 전역이라한다.

```javascript
var x = 10;

function foo() {
    y = 20; // window.y = 20;
}
foo();

console.log(x + y); // 30;
```
