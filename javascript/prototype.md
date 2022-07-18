# 프로토타입

## 프로토타입

프로토타입은 어떤 객체의 상위(부모) 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티(메서드 포함)를 제공한다. 프로토타입을 상속받은 하위(자식) 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.

자바스크립트는 프로토타입 기반의 객체지향 프로그래밍 언어다. 

## 객체지향 프로그래밍

객체지향 프로그래밍은 객체의 상태를 나타내는 데이터(프로퍼티)와 상태 데이터를 조작할 수 있는 동작(메서드)을 하나의 논리적인 단위로 묶은 복합적인 자료구조이다.

## 상속과 프로토타입

어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다. 자바스크립트는 프로토타입을 기반으로 상속을 구현한다.

```javascript
function Circle(radius) {
    this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를 공유할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
    return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

![상속에 의한 메서드 공유](<./../.gitbook/assets/Screen%20Shot%202022-07-18%20at%209.06.51%20PM.png>)

## 프로토타입 객체

모든 객체는 하나의 프로토타입을 갖는다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있다.

![객체와 프로토타입과 생성자 함수는 서로 연결되어 있다.](<./../.gitbook/assets/Screen%20Shot%202022-07-18%20at%209.13.20%20PM.png>)

### \_\_proto__ 접근자 프로퍼티

모든 객체는 `[[Prototype]]` 내부 슬롯에 직접 접근할 수 없지만 `__proto__`접근자 프로퍼티를 통해 자신의 프로토타입에 간접적으로 접근할 수 있다.

객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용한다.

#### \_\_proto__ 접근자 프로퍼티는 상속을 통해 사용된다.

`__proto__` 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 `Object.prototype`의 프로퍼티다. 모든 객체는 상속을 통해 `Object.prototype.__proto__` 접근자 프로퍼티를 사용할 수 있다.

### 함수 객체의 prototype 프로퍼티

함수 객체만이 소유하는 `prototype` 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다. 따라서 생성자 함수로서 호출할 수 없는 함수는 `prototype` 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.

생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용한다.

### 프로토타입의 consturctor 프로퍼티와 생성자 함수

모든 프로토타입은 `consturctor` 프로퍼티를 갖는다. 이 `constructor` 프로퍼티는 `prototype` 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.

## 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

`Object` 생성자 함수에 인수를 전달하지 않거나 `undefined` 또는 `null`을 인수로 전달하면서 호출하면 내부적으로는 추상 연산 `OrdinaryObjectCreate`를 호출하여 `Object.prototype`을 프로토타입으로 갖는 빈 객체를 생성한다.

객체 리터럴이 평가될 때는 추상 연산 `OrdinaryObjectCreate`를 호출하여 빈 객체를 생성하고 프로퍼티를 추가하도록 정의되어 있다.

`Object` 생성자 함수 호출과 객체 리터럴의 평가는 추상 연산 `OrdinaryObjectCreate`를 호출하여 빈 객체를 생성하는 점에서 동일하나 `new.target`확인이나 프로퍼티를 추가하는 세부 내용은 다르기 때문에 객체 리터럴에 의해 생성된 객체는 `Object` 생성자 함수가 생성한 객체가 아니다.

## 프로토타입의 생성 시점

프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.

### 사용자 정의 생성자 함수와 프로토타입 생성 시점

생성자 함수로서 호출할 수 있는 함수, 즉 `constructor`는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

```javascript
console.log(Person1.prototype); // {consturctor: ƒ} (함수 정의가 평가되어 객체를 생성하는 시점에 프로토타입도 생성된다.)

function Person1(name) {
    this.name = name;
};

const Person2 = name => {
    this.name = name;
};

console.log(Person2.prototype); // undefined (non-consturctor는 프로토타입이 생성되지 않는다.)
```

### 빌트인 생성자 함수와 프로토타입 생성 시점

`Object`, `String`, `Number`, `Function`, `Array` 등과 같은 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다. 생성된 프로토타입은 빌트인 생성자 함수의 `prototype` 프로퍼티에 바인딩된다.

## 프로토타입 체인

자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없으면 `[[Prototype]]` 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 이를 프로토타입 체인이라 한다.

프로토타입 체인의 최상위에 위치하는 객체는 언제나 `Object.prototype`이다. `Object.prototype`의 프로토타입은 `null`이다.

## 오버라이딩과 프로퍼티 섀도잉

프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰지 않고 인스턴스 프로퍼티로 추가한다. 이처럼 상속 관계에 의해 프로퍼티가 가려지는 현상을 프로퍼티 섀도잉이라 한다.

```javascript
const Person = (function () {
    function Person(name) {
        this.name = name;
    }

    Person.prototype.sayHello = function () {
        console.log(`Hi! My name is ${this.name}`);
    };

    return Person;
}());

const me = new Person('minjong');

me.sayHello = function () {
    console.log(`Hey! My name is ${this.name}`)
}

me.sayHello(); // Hey! My name is minjong
```

## 프로토타입의 교체

### 생성자 함수에 의한 프로토타입의 교체

```javascript
const Person = (function () {
    function Person(name) {
        this.name = name;
    }

    // 생성자 함수의 `prototype`프로퍼티를 통해 교체
    Person.prototype = {
        sayHello() {
            console.log(`Hi! My name is ${this.name}`);
        }
    };

    return Person;
}());

const me = new Person('minjong');

console.log(me.constructor === Person); // false (constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.)
console.log(me.constructor === Object); // true (프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.)
```

### 인스턴스에 의한 프로토타입의 교체

프로토타입은 생성자 함수의 `prototype` 프로퍼티뿐 아니라 인스턴스의 `__proto__` 접근자 프로퍼티를 통해 접근할 수 있다. `__proto__` 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 이미 생성된 객체의 프로토타입을 교체하는 것이다.

```javascript
function Person(name) {
    this.name = name;
}

const me = new Person('minjong');

const parent = {
    sayHello() {
        console.log(`Hi! My name is ${this.name}`);
    }
}

Object.setPrototypeOf(me, parent); // me 객체의 프로토타입을 parent 객체로 교체한다.

me.sayHello(); // Hi! My name is minjong

console.log(me.constructor === Person); // false (constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.)
console.log(me.constructor === Object); // true (프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.)
```

## instanceof 연산자

`instanceof` 연산자는 생성자 함수의 `prototype`에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.

```javascript
const Person = (function () {
    function Person(name) {
        this.name = name;
    }

    Person.prototype.sayHello = function () {
        console.log(`Hi! My name is ${this.name}`);
    };

    return Person;
}());

const me = new Person('minjong');

me.sayHello = function () {
    console.log(`Hey! My name is ${this.name}`)
}

me.sayHello(); // Hey! My name is minjong

console.log(me.constructor === Person); // false

console.log(me instanceof Person); // true (me 객체의 프로토타입 체인 상에 존재하므로 true)
console.log(me instanceof Object); // true (me 객체의 프로토타입 체인 상에 존재하므로 true)
```

