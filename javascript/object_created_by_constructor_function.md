# 생성자 함수에 의한 객체 생성

생성자 함수란 `new` 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다. 생성자 함수에 의해 생성된 객체를 인스턴스라 한다.

## Object 생성자 함수

객체 리터럴을 사용하는 방법 이외에도 `new` 연산자와 함께 `Object` 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다. 

## 생성자 함수

자바스크립트는 Object 생성자 함수 이외에도 `String`, `Number`, `Boolean`, `Function` `Array`, `Date`, `RegExp`, `Promise` 등의 빌트인 생성자 함수를 제공한다.

### 객체 리터럴에 의한 객체 생성 방식의 문제점

객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편하다. 하지만 객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성하므로 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 비효율적이다.

```javascript
const circle1 = {
    radius: 5,
    getDiameter() {
        return 2 * this.radius;
    }
};

console.log(circle1.getDiameter()); // 10

// circle1 프로퍼티 구조가 동일함에도 같은 프로퍼티와 메서드를 기술해야한다.
const circle2 = {
    radius: 10,
    getDiameter() {
        return 2 * this.radius;
    }
};

console.log(circle2.getDiameter()); // 10
```

### 생성자 함수에 의한 객체 생성 방식의 장점

생성자 함수에 의한 객체 생성 방식은 프로퍼티 구조가 동일한 객체 여러개를 간편하게 생성할 수 있다. 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 `new` 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.

```javascript
function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// 인스턴스의 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

### 생성자 함수의 인스턴스 생성 과정

생성자 함수의 역할은 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿(클래스)으로서 동작하여 인스턴스를 생성하고 생성된 인스턴스를 초기화하는 것이다.

생성자 함수 내부에 인스턴스를 생성하고 반환하는 코드를 작성하지 않아도 `new` 연산자와 함께 생성자 함수를 호출하면 자바스크립트 엔진은 다음과 같은 과정을 거쳐 암묵적으로 인스턴스를 생성하고 반환한다.

#### 1. 인스턴스 생성과 this 바인딩

암묵적으로 빈 객체(인스턴스)가 생성되고 `this에` 바인딩된다. 생성자 함수 내부의 `this`가 생성자 함수가 생성할 인스턴스를 가리키는 이유가 바로 이때문이다.

#### 2. 인스턴스 초기화

생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 `this`에 바인딩되어 있는 인스턴스를 초기화한다.

#### 3. 인스턴스 반환

생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.

```javascript
function Circle(radius) {
    // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.
    console.log(this); // Circle {}

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }
    
    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}

const circle = new Circle(1); // Circle {}
console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
```

만약 `this`가 아닌 다른 객체를 명시적으로 반환하면 `this`가 반한되지 못하고 `return`문에 명시한 객체가 반환된다.

```javascript
function Circle(radius) {
    // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }
    
    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
    return {};
}

const circle = new Circle(1);
console.log(circle); // {}
```

명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 `this`가 반환된다.


```javascript
function Circle(radius) {
    // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }
    
    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    // 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.
    return 100;
}

const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
```

## 내부 메서드 [[Call]]과 [[Construct]]

함수는 객체이므로 일반 객체처럼 프로퍼티와 메서드를 소유할 수 있다. 다만 일반 객체는 호출할 수 없고 함수는 호출할 수 있다.

함수는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론, 함수로서 동작하기 위한 `[[Enviroment]]`, `[[FormalParameters]]` 등의 내부 슬롯과 `[[Call]]`, `[[Construct]]`같은 내부 메서드를 추가로 가지고 있다.

모든 함수 객체는 내부 메서드 `[[Call]]`을 갖지만, 모든 함수 객체가 `[[Construct]]`를 갖는 것은 아니다. 즉, 모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니다.

![모든 함수 객체는 callable이지만 모든 함수 객체가 consturctor인 것은 아니다.](<../.gitbook/assets/Screen Shot 2022-07-12 at 5.06.26 PM.png>)


### constructor와 non-constructor의 구분

자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성 할 때 방식에 따라 함수를 `constructor`와 `non-constructor`로 구분한다.

* `constructor`: 함수 선언문, 함수 표현식, 클래스
* `non-constructor`: 메서드(ES6 메서드 축약 표현), 화살표 함수

```javascript
// 함수 선언문과 표현식을 통한 함수 정의
function foo() {};
const bar = function() {};
// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다.
const baz = {
    x: function () {}
};

new foo(); // -> foo {}
new bar(); // -> bar {}
new baz.x(); // -> x {}

// 화살표 함수 정의
const arrow = () => {};
// 메서드 정의: ES6의 메서드 축약 표현만 메서드로 인정한다.
const ob = {
    x() {}
};

new arrow(); // TypeError: arrow is not a constructor
new obj.x(); // TypeError: obj.x is not a constructor
```

## new.target

생성자 함수가 `new` 연산자 없이 호출되는 것을 방지하기 위해 ES6에서는 `new.target`을 지원한다.

함수 내부에서 `new.target`을 사용하면 `new`연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있다. `new`연산자와 함께 생성자 함수로서 호출되면 함수 내부의 `new.target`은 함수 자신을 가리키고, 일반 함수로서 호출되면 `undefined`이다.

```javascript
function Circle(radius) {
    if (!new.target) {
        // new 연산자와 함께 생성자 함수를 재귀 호출하여 인스턴스를 반환한다.
        return new Circle(radius);
    }
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

const circle = Circle(5);
console.log(circle); // Circle {radius: 5, getDiameter: ƒ}
```
