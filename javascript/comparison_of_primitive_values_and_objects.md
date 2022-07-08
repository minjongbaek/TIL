# 원시 값과 객체의 비교

## 원시 값

### 변경 불가능한 값

원시 값은 변경 불가능한 값이다. 이는 원시 값 자체를 변경할 수 없다는 것이지 변수 값을 변경할 수 없다는 것이 아니다. 변수는 언제든지 재할당을 통해 변수 값을 변경할 수 있다.

변수 값을 변경하기 위해 원시 값을 재할당 하게되면 새로운 메모리 공간을 확보하고 재할당한 값을 저장한 후, 변수가 참조하던 메모리 공간의 주소를 변경한다. 값의 이러한 특성을 불변성이라고 한다.

### 문자열과 불변성

자바스크립트는 원시 타입인 문자열 타입을 제공한다. 문자열은 유사 배열 객체이면서 이터러블이므로 배열과 유사하게 각 문자에 접근 가능하다.

```javascript
var str = 'word';

console.log(str); // word

str[0] = 'W'; // 문자열은 변경 불가능하기 때문에 작동하지 않는다.

console.log(str[0]); // w

str = 'Word'; // 'word'과 'Word' 모두 존재한다. 식별자 str 은 'string'을 가리키고 있다가 'word'를 가리키도록 변경된 것이다.

console.log(str) // Word
```

### 값에 의한 전달

변수에 원시 값을 갖는 변수를 할당하면 변수에는 할당되는 변수의 원시값이 복사되어 전달된다. 두 변수의 원시 값은 어느 한쪽에서 재할당을 통해 값을 변경하더라도 서로에게 영향을 끼치지 않는다.

```javascript
var score = 80;
var copy = score; // score 변수의 값인 80이 복사되어 할당된다.

console.log(score, copy); // 80 80
console.log(score === copy) // true (숫자 값 80을 갖는다는 점은 동일하지만, 두 80은 다른 메모리 공간에 저장된 별개의 값이다.)

score = 100;

console.log(score, copy) // 100 80
console.log(score === copy) // false
```

## 객체

객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티의 수는 정해져있지 않고 동적으로 추가되고 삭제할 수 있다. 따라서 원시 값과 같이 확보해야할 메모리 공간의 크기를 사전에 정해둘 수 없다.

### 변경 가능한 값

참조 타입의 값인 객체는 변경 가능한 값이다. 객체를 할당한 변수에는 생성된 객체가 실제로 저장된 메모리 공간의 주소가 저장되어 있다. 이 값을 참조 값이라고 한다.

객체는 변경 가능한 값이므로 메모리에 저장된 객체를 직접 수정할 수 있다. 이때는 변수에 재할당을 하지 않았으므로 변수의 참조 값은 변경되지 않는다.

```javascript
var person = {
    name: 'minjong'
};

console.log(person); // {name: 'minjong}

person.name = 'minjong baek';
person.address = 'Seoul';

console.log(person); // {name: 'minjong baek', address: 'Seoul'}
```

### 참조에 의한 전달

하나의 객체는 여러 개의 식별자가 공유할 수 있다. 객체를 가리키는 변수를 다른 변수에 할당하면 원본의 참조 값이 복사되어 전달된다. 이로 인해 부작용이 발생하게 된다.

```javascript
var person = {
    name: 'minjong'
};

var copy = person;

console.log(person === copy); // true

copy.name = 'minjong baek';
person.address = 'Seoul';

console.log(person); // {name: 'minjong baek', address: 'Seoul'}
console.log(copy); // {name: 'minjong baek', address: 'Seoul'}
```

![참조에 의한 전달](<../.gitbook/assets/Screen%20Shot%202022-07-06%20at%203.31.36%20PM.png>)
