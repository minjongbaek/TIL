# 프로퍼티 어트리뷰트

## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다. 프로퍼티 어트리뷰트는 내부 슬롯으로 직접 접근할 수 없지만, `Object.getOwnPropertyDescriptor` 메서드를 사용하여 반환된 프로퍼티 디스크립터 객체를 통해 간적적으로 확인할 수 있다.

> 프로퍼티 상태란?
> 
> * 프로퍼티의 값 [[Value]]
> * 값의 갱신 가능 여부 [[Writable]]
> * 열거 가능 여부 [[Enumerable]]
> * 재정의 가능 여부 [[Configurable]]

```javascript
const person = {
    name: 'minjong'
};

person.age = 20;

console.log(Object.getOwnPropertyDescriptor(person, 'name')); // {value: 'minjong', writable: true, enumerable: true, configurable: true}
console.log(Object.getOwnPropertyDescriptors(person)); // getOwnPropertyDescriptors 함수는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 확인할 수 있다.
/* {
    age: {value: 20, writable: true, enumerable: true, configurable: true},
    name: {value: 'minjong', writable: true, enumerable: true, configurable: true}
    }
*/
```

## 데이터 프로퍼티와 접근자 프로퍼티

프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분할 수 있다.

### 데이터 프로퍼티

데이터 프로퍼티는 아래 4개의 프로퍼티 어트리뷰트를 갖는다. 이 프로퍼티 어트리뷰트는 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.

#### [[Value]]

프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값이다. 프로퍼티 키를 통해 값을 변경하면 `[[Value]]`에 값을 재할당한다. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 `[[Value]]`에 값을 저장한다.

#### [[Writable]]

프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값을 갖는다.

#### [[Enumerable]]

열거 가능 여부를 나타내며 불리언 값을 갖는다.

#### [[Configurable]]

재정의 가능 여부를 나타내며 불리언 값을 갖는다.

`[[Configurable]]`의 값이 `false`인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다. 단, `[[Writable]]`이 `true`인 경우 `[[Value]]`의 변경과 `[[Writable]]`을 `false`로 변경하는 것은 허용된다.

### 접근자 프로퍼티

접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다. 접근자 프로퍼티는 다음과 같은 프로퍼티 어트리뷰트를 갖는다.

#### [[Set]]

접근자 프로퍼티를 통해 데이터 프로퍼티 값을 저장할 때 호출되는 접근자 함수다. 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰티 `[[Set]]`의 값, 즉 `setter` 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다.

#### [[Enumerable]]

데이터 프로퍼티의 `[[Enumerable]]`과 같다.

#### [[Configurable]]

데이터 프로퍼티의 `[[Configurable]]`과 같다.

```javascript
const person = {
    firstName: 'Minjong',
    lastName: 'Baek',
    
    // getter 함수
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },

    // setter 함수
    set fullName(name) {
        [this.firstName, this.lastName] = name.split(' ');
    }
};

console.log(person.firstName + ' ' + person.lastName); // Minjong Baek

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장. setter 함수가 호출된다.
person.fullName = 'Heeji Kim';
console.log(person); // {firstName: 'Heeji', lastName: 'Kim'}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조. getter 함수가 호출된다.
console.log(person.fullName); // Heeji Kim

console.log(Object.getOwnPropertyDescriptor(person, 'fullName')); // {enumerable: true, configurable: true, get: ƒ, set: ƒ}
```

## 프로퍼티 정의

`Object.defineProperty`, `Object.defineProperties` 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다. 인수로는 객체의 참조와 데이터 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체를 전달한다.

## 객체 변경 방지

객체는 변경 가능한 값이므로 재할당 없이 직접 변경이 가능하다. 자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다. 객체 변경 방지 메서드들은 객체의 변경을 급지하는 강도가 다르다.

![객체의 변경을 방지하는 다양한 메서드](<../.gitbook/assets/Screen Shot 2022-07-11 at 12.34.29 PM.png>)

### 객체 동결

위에 언급된 변경 받지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지 못한다. 객체의 중첩 객체까지 동결하여 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 `Object.freeze` 메서드를 호출해야 한다.

```javascript
function deepFreeze(target) {
    if (target && typeof target === 'object' && !Object.isFrozen(target)) {
        Object.freeze(target);
        Object.keys(target).forEach((key) => deepFreeze(target[key]));
    }
    return target;
}

const person = {
    name: 'minjong',
    address: { city: 'Seoul' }
};

deepFreeze(person);
person.address.city = 'Busan';
console.log(person); // {name: "minjong", address: {city: "Seoul"}}
```
