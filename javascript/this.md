# this

메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경하기 위해 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다. 자신이 속한 객체 혹은 자신이 생성할 인스턴스를 가리키는 자기 참조 변수를 `this`라고 부른다.

`this`가 가리키는 값은 함수 호출 방식에 의해 동적으로 결정된다.

```javascript
console.log(this); // window

function square(number) {
    // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
    console.log(this); // window;
    return number * number;
}

square(2); // window;

const person = {
    name: 'minjong',
    getName() {
        // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
        console.log(this); // {name: 'minjong', getName: ƒ}
        return this.name;
    }
}

console.log(person.getName()); // minjong

function Person(name) {
    this.name = name;
    // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    console.log(this);
}
```

## 함수 호출방식과 this 바인딩

`this` 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되엇는지에 따라 동적으로 결정된다.

![this 바인딩은 함수 호출 방식에 따라 결정된다.](<./../.gitbook/assets/Screen Shot 2022-07-21 at 11.15.53 AM.png>)

### 일반 함수 호출

일반 함수로 호출하면 함수 내부의 `this`에는 전역 객체가 바인딩 된다. 메서드 내 중첩 함수나 콜백 함수도 마찬가지다.

```javascript
function square(number) {
    // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
    console.log(this); // window;
    return number * number;
}

var value = 1;

const obj = {
    value: 100,
    foo() {
        console.log("foo's this: ", this); // {value:100, foo: ƒ}
        console.log("foo's this.value: ", this.value); // 100
        
        // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 내부의 this에는 전역 객체가 바인딩 된다.
        function bar() {
            console.log("bar's this: ", this); // window
            console.log("bar's this.value: ", this.value) // 1
        }
    }
}
```

### 메서드 호출

매서드는 프로퍼티에 바인딩된 함수로 객체에 포함된 것이 아닌 독립적으로 존재하는 별도의 객체이며 객체의 프로퍼티가 함수 객체를 가리키고 있을 뿐이다.

![메서드는 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체다.](<./../.gitbook/assets/Screen Shot 2022-07-21 at 10.51.43 AM.png>)

이는 메서드가 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수 있고 일반 변수에 할당하여 일반 함수로 호출할 수 있음을 의미한다. 따라서 메서드 내부의 `this`는 메서드를 호출한 객체에 바인딩 된다.

```javascript
function person(name) {
    this.name = name;
}

Person.prototype.getName = function () {
    return this.name;
}

const me = new Person('minjong');

console.log(me.getName()); // minjong

Person.prototype.name = '민종';

console.log(Person.prototype.getName()); // 민종
```

### 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩된다.

```javascript
function Circle(radius) {
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20

const circle3 = Circle(15);
console.log(circle3); // undefined (일반 함수로 호출된 Circle에는 반환문이 없으므로 undefined)
console.log(radius); // 일반 함수로 호출된 Circle 내부 this는 전역 객체를 가리킨다.ㄴ
```

### Function.prototype.apply / call / bind 메서드에 의한 간접 호출

`apply`, `call`, `bind` 메서드는 `Function.prototype`의 메서드다.

`Function.prototype.apply`, `Function.prototype.call` 메서드는 `this`로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다.

```javascript
function getThisBinding() {
    return this;
}

console.log(getThisBinding()); // window

// this로 사용할 객체
const thisArg = { a: 1 };

// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1, 2, 3])); // {a: 1}
// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg, 1, 2, 3)); // {a: 1}
```

`Function.prototype.bind` 메서드는 함수를 호출하지 않는다. 첫 번째 인수로 전달한 값으로 `this` 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

`bind` 메서드는 `this`와 메서드 내부의 중첩 함수 또는 콜백 함수의 `this`가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

```javascript
const person = {
    name: 'minjong',
    foo(callback) {
        // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
        setTimeout(callback.bind(this), 100);
    }
};

person.foo(function () {
    console.log(`Hi! My name is ${this.name}.`); // Hi! My name is minjong.
})
```
