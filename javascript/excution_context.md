# 실행 컨텍스트

## 소스코드의 타입

ECMAScript 사양은 소스코드를 4가지 타입으로 구분한다. 4가지 타입의 소스코드는 실행 컨텍스트를 생성한다.

![소스코드의 타입](<./../.gitbook/assets/Screen Shot 2022-07-21 at 2.31.47 PM.png>)

## 소스코드의 평가와 실행

모든 소스코드는 평과와 실행 과정으로 나누어 처리된다.

평과 과정에서는 변수나 함수 등의 선언문을 먼저 실행하고 식별자는 실행 컨텍스트가 관리하는 스코프에 등록되고 `undefined`로 초기화 된다.

실행 과정에서는 선언문을 제외한 소스코드가 순차적으로 실행된다. 이때 변수나 함수의 참조를 실행 컨텍스트가 관리하는 스코프에서 검색하여 취득한다. 실행 결과는 다시 실행 컨텍스트가 관리하는 스코프에 등록된다.

## 실행 컨텍스트의 역할

식별자 (변수, 함수, 클래스 등의 이름)를 등록하고 스코프와 코드 실행 순서를 관리한다. 모든 코드는 실행 컨텍스트를 통해 실행되고 관리된다.

식별자와 스코프는 렉시컬 환경으로 관리하고 코드 실행 순서는 실행 컨텍스트 스택으로 관리한다.

### 실행 컨텍스트 스택

실행 컨텍스트 스택은 코드의 실행 순서를 관리한다. 실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트는 언제나 현재 실행 중인 코드의 실행 컨텍스트다.

### 렉시컬 환경

렉시컬 환경은 스코프와 식별자를 관리한다. 스코프에 포함된 식별자를 등록하고 식별자에 바인딩된 값을 관리하는 환경 레코드, 상위 스코프를 가리키는 외부 레시컬 환경에 대한 참조로 두 개의 컴포넌트로 구성된다.

## 실행 컨텍스트와 블록 레벨 스코프

`var` 키워드로 선언한 변수는 오로지 함수의 코드 블록만 지역 스코프로 인정한다. 하지만 `let`, `const` 키워드로 선언한 변수는 모든 코드 블록(함수, if, for, while, try/catch 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```javascript
let x = 1;

if (true) {
    let x = 10;
    console.log(x); // 10 (블록 레벨 스코프를 따른다.)
}

console.log(x); // 1
```