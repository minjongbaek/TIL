# strict mode

## strict mode

strict mode는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 잇는 코드에 대해 명시적인 에러를 발생시킨다.

## strict mode 적용에 의한 변화

### 일반 함수의 this

strict mode에서 함수를 일반 함수로서 호출하면 `this`에 `undefined`가 바인딩된다. 생성자 함수가 아닌 일반 함수 내부에서는 `this`를 사용할 필요가 없기 때문이다.

```javascript
(function() {
    'use strict';

    function foo () {
        console.log(this); // undefined
    };
    foo();

    function Foo() {
        console.log(this); // Foo
    }
    new Foo();
}());
```

### arguments 객체

strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```javascript
(function (a) {
    'use strict';

    a = 2;

    console.log(arguments); // { 0: 1, length: 1 }
}(1));
```
