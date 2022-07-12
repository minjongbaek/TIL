# 함수와 일급 객체

## 일급 객체

다음과 같은 조건을 만족하는 객체를 일급 객체라고 한다. 자바스크립트의 함수는 조건을 모두 만족하므로 일급 객체다.

1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

```javascript

// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
// 런타임(할당 단계)에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increase = function (num) {
    return ++num;
};

const decrease = function (num) {
    return --num;
}

// 2. 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };

// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function maekCounter(aux) {
    let num = 0;

    return function () {
        num = aux(num);
        return num;
    };
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(auxs.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

const decreaser = makeCounter(auxs.decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

## 함수 객체의 프로퍼티

`arguments`, `caller`, `length`, `name`, `prototype` 프로퍼티는 모두 함수 객체의 데이터 프로퍼티다. 이들 프로퍼티는 일반 객체에는 없는 함수 객체 고유의 프로퍼티다.

`__proto__`는 접근자 프로퍼티로 함수 객체 고유의 프로퍼티가 아니라 `Object.prototype` 객체의 프로퍼티를 상속받는다. `__proto__` 프로퍼티는 모든 객체가 상속받아 사용할 수 있다.

### arguments 프로퍼티

함수 객체의 `arguments` 프로퍼티 값은 `arguments` 객체다. `arguments` 객체는 함수 호출 시 전달된 인수들의 정보를 담고 이쓴 순회 가능한 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다. 즉, 함수 외부에서는 참조할 수 없다.

### caller 프로퍼티

`caller` 프로퍼티는 ECMAScript 사양에 포함되지 않은 비표준 프로퍼티다. `caller` 프로퍼티는 함수 자신을 호출한 함수를 가리킨다.

### length 프로퍼티

함수 객체의 `length` 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다. `arguments` ㄹ객체의 `length` 프로퍼티는 인자의 개수를 가리키고, 함수 객체 `length` 프로퍼티는 매개 변수의 개수를 가리킨다.

### name 프로퍼티

함수 객체의 `name` 프로퍼티는 함수 이름을 나타낸다. 익명 함수 표현식의 경우 ES5에서 `name` 프로퍼티는 빈 문자열을 값으로 갖는다. 하지만 ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.

### \_\_proto__ 접근자 프로퍼티

모든 객체는 `[[Prototype]]`이라는 내부 슬롯을 갖는다. `__proto__` 프로퍼티는 `[[Prototype]]` 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티이다.

### prototype 프로퍼티

`prototype` 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor만이 소유하는 프로퍼티다. `prototype` 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.
