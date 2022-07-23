# 클래스

## 클래스 호이스팅

클래스는 값처럼 사용할 수 있는 일급 객체이며, 함수다. 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 평과 과정에 함수 객체를 생성한다. 이때 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수, 즉 `constructor`이다.

클래스 선언문도 변수 선언, 함수 정의와 마찬가지로 호이스팅이 발생한다. 단, 클래스는 `let`, `const` 키워드로 선언한 변수처럼 호이스팅 된다.

```javascript
const Person = '';
{
    console.log(Person); // ReferenceError: Cannot access 'Person' before initialization (호이스팅이 발생하지 않았다면 ''이 출력되어야 한다.)
    
    class Person {}
}
```

## 인스턴스 생성

클래스는 생성자 함수이며 `new` 연산자와 함께 호출되어 인스턴스를 생성한다.

```javascript
const Person = class MyClass {};

const me = new Person();

console.log(me); // MyClass {}
// MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자다.
console.log(MyClass); // ReferenceError: MyClass is not defined
```

## 메서드

클래스 몸체에서 정의할 수 있는 메서드는 `constructor`, 프로토타입 메서드, 정적 메서드의 세 가지가 있다.

### constructor

`constructor`는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다. 

함수 객체가 가지고 있는 `prototype` 프로퍼티가 가리키는 프로토타입 객체의 `constructor` 프로퍼티는 클래스 자신을 가리킨다.

### 프로토타입 메서드

클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 객체 생성 방식과는 다르게 클래스의 `prototype` 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.

```javascript
class Person {
    constructor(name) {
        this.name = name;
    }

    sayHi() {
        console.log(`Hi! My name is ${this.name}`);
    }
}

const me = new Person('Lee')l
me.sayHi(); // Hi My name is Lee
```

### 정적 메서드

`static` 키워드를 붙여 정적 메서드를 정의할 수 있다. 정적 메서드가 바인딩된 클래스는 인스턴스의 프로토타입 체인 상에 존재하지 않기 때문에 인스턴스에서 호출할 수 없다.

```javascript
class Person {
    constructor(name) {
        this.name = name;
    }

    static sayHi() {
        console.log('Hi');
    }
}
console.log(Person.sayHi()); // Hi

const person = new Person('minjong');
console.log(person.sayHi()); // TypeError: person.sayHi is not a function
```

### 정적 메서드와 프로토타입 메서드의 차이

1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

## 클래스의 인스턴스 생성 과정

1. `new`연산자와 함께 클래스를 호출하면 인스턴스가 먼저 생성되고 `this`에 바인딩된다.
2. `constructor`의 내부코드가 실행되어 `this`에 바인딩되어 있는 인스턴스를 초기화한다.
3. 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.

```javascript
class Person {
    constructor(name) {
        // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩 된다.
        console.log(this);
        console.log(Object.getPrototypeOf(this) === Person.prototype);

        // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
        this.name = name;

        // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    }
}
```

## 프로퍼티

### 인스턴스 프로퍼티

인스턴스 프로퍼티는 `constructor` 내부에서 정의해야 한다. `constructor` 내부에서 `this`에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티가 된다.

### 접근자 프로퍼티

접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수, 즉 `getter` 함수와 `setter`함수로 구성되어 있다.

### 클래스 필드 정의

클래스 필드란 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어다. ECMAScript 2022부터 정식 표준이 되었다.

```javascript
class Person {
    // 클래스 필드에 문자열을 할당
    name = 'Lee';

    //  클래스 필드에 함수를 할당
    getName = function() {
        return this.name;
    }
}

const me = new Person();
console.log(me); // Person {name: 'Lee', getName: ƒ}
console.log(me.getName()); // Lee
```

### private 필드 정의 제안

자바스크립트의 인스턴스 프로퍼티는 인스턴스를 통해 클래스 외부에서 언제나 참조할 수 있다. `private` 필드를 정의하면 클래스 내부에서만 참조 가능하다. 마찬가지로 ECMAScript 2022부터 정식 표준이 되었다.

```javascript
class Person {
    #name = ''; // # 키워드로 private 필드를 정의한다.

    constructor(name) {
        this.#name = name;
    }
}

const me = new Person('minjong');

// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name); // SyntaxError: Private field '#name' must be declared in an enclosing class
```

### static 필드 정의

static public 필드, static private 필드, static private 메서드를 정의할 수 있다. 마찬가지로 ECMAScript 2022부터 정식 표준이 되었다.

```javascript
class MyMath {
    // static public
    static PI = 22 / 7;
    // static private
    static #num = 10;

    static increment() {
        return ++MyMath.#num;
    }
}

console.log(MyMath.PI); // 3.14285...
console.log(MyMath.increment()); // 11
```

## 상속에 의한 클래스 확장

상속에 의한 클래스 확장은 프로토타입 기반 상속과는 다른 개념이다. 프로토타입 기반 상속은 프로토타입 체인을 통해 상속 받는 개념이지만 상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것이다.


```javascript
class Animal {
    constructor(age, weight) {
        this.age = age;
        this.weight = weight;
    }

    eat() { return 'eat'; }

    move() { return 'move'; }
}

// Animal 클래스를 상속받은 Bird 클래스
class Bird extends Animal {
    fly() { return 'fly'; }
}

const bird = new Bird(1, 5);

console.log(bird); // Bird {age: 1, weight: 5}
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true

console.log(bird.eat()); // eat
console.log(bird.move()); // move
console.log(bird.fly()); // fly
```

![상속에 의해 확장된 클래스 Bird에 의해 생성된 인스턴스의 프로토타입 체인](<../.gitbook/assets/Screen Shot 2022-07-23 at 4.10.03 PM.png>)


### extends 키워드

`extends`키워드를 사용하여 상속받을 클래스를 정의한다. 수퍼클래스와 서브클래스는 인스턴스의 프로토타입 체인뿐 아니라 클래스 간의 프로토타입 체인도 생성한다. 이를 통해 프로토타입 메서드, 정적 메서드 모두 상속이 가능하다.

표준 빌트인 객체도 `extends` 키워드를 사용하여 확장할 수 있다.

### 동적 상속

`extends` 키워드 다으멩는 클래스뿐만이 아니라 `Construct` 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다.

```javascript
function Base1() {}

class Base2 {}

let condition = true;

class Derived extends (condition ? Base1 : Base2) {}
```

### 서브클래스의 consturctor

서브클래스에서 `constructor`를 생략하면 클래스에 다음과 같은 `constructor`가 암묵적으로 정의된다.

```javascript
// args는 new 연산자와 함께 클래스를 호출할 때 전달한 인수의 리스트다.
// super()는 슈퍼클래스의 consturctor를 호출하여 인스턴스를 생성한다.
constructor(...args) { super(...args) }
```

### super 키워드

`super`키워드는 함수처럼 호출할 수 있고 `this`와 같이 식별자처럼 참조할 수 있는 특수한 키워드다. `super`는 다음과 같이 동작한다.

* `super`를 호출하면 슈퍼클래스의 `constructor`를 호출한다.
* `super`를 참조하면 슈퍼클래스의 메서드를 호출할 수 있다.

### 상속 클래스의 인스턴스 생성 과정

1. 서브클래스는 자신이 인스턴스를 직접 생성하지 않고 `super`메서드를 호출하여 슈퍼클래스에게 인스턴스 생성을 위임한다.
2. 슈퍼클래스는 인스턴스를 생성하고 `this`에 인스턴스를 바인딩한다.
3. 슈퍼클래스는 `this`에 바인딩되어 있는 인스턴스를 초기화한다.
4. `super`가 반환한 인스턴스가 서브클래스 `this`에 바인딩 된다.
5. 서브클래스의 인스턴스를 초기화한다. (`this`에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 `constructor`가 인수로 전닯다은 초기값으로 인스턴트 프로퍼티를 초기화한다.)
6. 모든 처리가 끝난 인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.
