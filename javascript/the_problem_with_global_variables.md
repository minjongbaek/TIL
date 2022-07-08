# 전역 변수의 문제점

## 변수의 생명 주기

변수는 선언에 의해 생성되고 할당을 통해 값을 갖는다. 그리고 언젠간 소멸한다. 즉, 변수는 생성되고 소멸되는 생명 주기가 있다.

### 지역 변수의 생명 주기

함수 내부에서 선언된 지역 변수는 함수가 호출되면 생성되고 함수가 종료하면 소멸한다. 즉 지역 변수의 생명 주기는 함수의 생명 주기와 일치한다.

```javascript
var x = 'global';

function foo() {
    console.log(x); // undefined (함수가 실행될 때 지역변수 x는 이미 선언되어 undefined로 초기화 되어 있다.)
    var x = 'local';
}

foo();
console.log(x); // 'global'
```

### 전역 변수의 생명 주기

var 키워드로 선언한 전역 변수는 전역 객체(브라우저 환경이라면 `window` 객체)의 프로퍼티가 된다. 이는 전역 변수의 생명 주기가 전역 객체의 생명 주기와 일치한다.


## 전역 변수의 문제점

* 모든 코드가 전역 변수를 참조하고 변경할 수 있는 암묵적 결합을 허용한다.
* 생명 주기가 길다. 이에 따라 메모리 리소스도 오랜 기간 소비한다.
* 전역 변수는 스코프 체인 상에서 종점에 존재한다. 즉, 전역 변수의 검색 속도가 가장 느리다. 
* 자바스크립트는 파일이 분리되어 있다 해도 하나의 전역 스코프를 공유한다. 따라서 다른 파일 내 동일한 이름의 전역 변수나 전역 함수가 존재한다면 예상치 못한 결과를 가져올 수 있다.


## 전역 변수의 사용을 억제하는 방법

전역 변수를 반드시 사용해야할 이유를 찾지 못한다면 지역 변수를 사용해야 한다. 변수의 스코프는 좁을수록 좋다.

### 즉시 실행 함수

모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.

```javascript
(function () {
    var foo = 10;
}());

console.log(foo); // ReferenceError: foo is not Error
```

### 네임스페이스 객체

전역에 네임스페이스 역할을 담당할 객체를 생성하고 전역 변수처럼 사용하고 싶은 변수를 프로퍼티로 추가하는 방법이다.

```javascript
var MYAPP = {};

MYAPP.person = {
    name: 'minjong',
    address: 'Seoul'
};

console.log(MYAPP.person.name); // minjong
```

### 모듈 패턴

모듈 패턴은 클래스를 모방해서 관련이 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈을 만든다. 모듈 패턴은 자바스크립트의 강력한 기능인 클로저를 기반으로 동작한다. 전역 변수의 억제는 물론 캡슐화까지 구현 가능하다.

```javascript
var Counter = (function () {
    // private 변수
    var num = 0;

    // 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환한다.
    return {
        increase() {
            return ++num;
        },
        decrease() {
            return --num;
        }
    };
}());

console.log(Counter.num); // undefined (private 변수는 외부로 노출되지 않는다.)
console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0
```

### ES6 모듈

ES6 모듈을 사용하면 더는 전역 변수를 사용할 수 없다. ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다. 따라서 모듈 내에서 var 키워드로 선언한 변수는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다.

```html
<script type="module" src="lib.mjs"></script>
<script type="module" src="app.mjs"></script>
```
