# 클로저

클로저는 자바스크립트 고유의 개념이 아니다. 함수를 일급 객체로 취급하는 함수형 프로그래밍 언어에서 사용되는 중요한 특성이다.

외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 클로저라고 부른다.

> 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.
> -MDN

## 함수 객체의 내부 슬롯 [[Environment]]

함수는 자신의 내부 슬롯`[Environment]]`에 자신이 저장된 환경, 즉 상위 스코프의 참조를 저장한다.

## 클로저와 렉시컬 환경

MDN에서는 클로저를 다음과 같이 정의한다.

> 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다. -MDN

위 정의에서 "그 함수가 선언된 렉시컬 환경" 이란 함수가 정의된 위치의 스코프, 즉 상위 스코프를 의미하는 실행 컨텍스트의 렉시컬 환경을 말한다.

```javascript
const x = 1;

function outer() {
    const x = 10;
    const inner = function () { console.log(x) };
    return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
// 그러나 outer 함수의 렉시컬 환경은 소멸하지 않기 때문에 지역 변수 x에 접근이 가능하다.
const innerFunc = outer();
innerFunc(); // 10
```

자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저라 할 수 있지만, 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.

## 클로저의 활용

클로저는 상태를 안젆라게 변경하고 유지하기 위해 사용한다. 상태가 의도치 않게 변경되지 않도록 상태를 안전한게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위함이다.

```javascript
// 카운트 상태 변경 함수
// 즉시 실행 함수는 호출된 이후 소멸하지만 반환된 클로저는 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하고 있다.
const increase = (function () {
    // 카운트 상태 변수
    let num = 0;

    // 클로저
    return function () {
        return ++num;        
    }
}());

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

## 캡슐화와 정보 은닉

캡슐화는 객체의 프로퍼티와 메서드를 하나로 묶는 것을 말한다. 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라고 한다.

정보 은닉은 외부에 공개할 필요가 없는 일부 데이터를 외부에 공개되지 않도록 감추어 적절치 못한 접근을 금지하고, 객체 간의 상호 의존성, 즉 결합도를 낮추는 효과가 있다.

그동안 자바스크립트는 정보 은닉을 완전하게 지원하지 않았지만, ECMAScript 2019 이후로 private한 클래스 필드를 만들 수 있게 되었다.

```javascript
class ClassWithPrivateField {
  #privateField
}

class ClassWithPrivateMethod {
  #privateMethod() {
    return 'hello world'
  }
}

class ClassWithPrivateStaticField {
  static #PRIVATE_STATIC_FIELD
}
```
