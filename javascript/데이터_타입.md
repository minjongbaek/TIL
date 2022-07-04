# 데이터 타입

## 데이터 타입

자바스크립트(ES6)는 7개의 데이터 타입을 제공한다. 7개의 데이터 타입은 크게 원시 타입과 객체 타입으로 분류할 수 있다.

![데이터 타입](<../.gitbook/assets/Screen Shot 2022-07-04 at 8.51.57 PM.png>)

### 숫자(Number) 타입

자바스크립트는 모든 숫자를 실수로 처리하며, 정수만 표현하기 위한 데이터 타입이 별도로 존재하지 않는다.

```javascript
console.log(1 === 1.0); // true
console.log(4 / 2); // 2
console.log(3 / 2); // 1.5
```

### undefined 타입

자바스크립트 엔진이 변수를 초기화 할 때 사용하는 값으로, 선언 이후 초기화 된 적이 없는 변수를 참조하면 `undefined`가 반한된다.

변수에 값이 없다는 것을 명시하고 싶을때는 `null`을 할당한다.

### null 타입

`null`은 변수에 값이 없다는 것을 의도적으로 명시할 때 사용한다. 이는 이전에 할당되어 있던 값에 대한 참조를 명시적으로 제거하는 것을 의미한다. `null`을 할당하게 되면 자바스크립트 엔진은 누구도 참조하지 않는 메모리 공간에 가비지 콜렉션을 수행한다.

함수가 유효한 값을 반환할 수 없는 경우 명시적으로 `null`을 반환하기도 한다.

### 심벌(Symbol) 타입

ES6에서 추가된 7번째 타입으로, 다른 값과 중복되지 않는 유일무이한 값이다. 생성된 값은 외부에 노출되지 않는다.

## 데이터 타입의 필요성

값은 메모리에 저장하고 참조할 수 있어야 하기 때문에 값을 저장할 때 확보해야 하는 메모리 공간의 크기를 결정해야한다.

`0100 0001`은 숫자로 해석하면 65지만 문자열로 해석하면 'A'가 된다. 즉, 메모리로 부터 읽어온 2진수를 어떻게 해석할지 결정해야한다.

## 동적 타이핑

자바스크립트는 변수를 선언할 때 타입을 선언하지 않는다. 어떠한 데이터 타입의 값이라도 자유롭게 할당할 수 있다.

```javascript
var foo;
console.log(typeof foo); // undefined

foo = 3;
console.log(typeof foo) // number

foo = 'Hello';
console.log(typeof foo) // string

foo = true;
console.log(typeof foo) // boolean

foo = null;
console.log(typeof foo) // object

foo = function () {};
console.log(typeof foo) // function
```

자바스크립트의 변수는 선언이 아닌 할당에 의해 타입이 결정된다. 그리고 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있다.
