---
title: '다시 자바스크립트 정리'
date: '2019-06-05T17:18:01.169Z'
category: 'development'
---

![](./images/js-1.png)

## **Scope(유효범위)**

스코프란 변수의 유효 범위를 의미 한다. 다른 프로그래밍 언어와는 다르게 자바스크립트의 변수는 유효 범위가 전역으로 시작한다. 예를 들어 아래와 같은 변수 선언은 자바스크립트로 접근할 수 있는 모든 영역에서 같은 값을 갖는다.

`var a = 10;`

만약 함수를 만들어 아래와 같이 접근하더라도 동일한 값을 출력하게 된다.

```js
var a = 10
function getA() {
  console.log(a)
}
getA() // 10
```

## **함수 단위로 구분되는 변수 범위**

기본적으로 변수의 유효 범위는 전역 범위를 갖는다고 하지만, 함수 안에서 새로 선언하는 경우 함수 단위의 새로운 유효 범위를 갖는다.

```js
var a = 10
function getA() {
  var a = 20
  console.log(a)
}
getA() // 20
console.log(a) // 10
```

위 코드는 함수 바깥에서 변수 `a`를 선언하고 10을 대입한 뒤, `getA()`라는 함수를 선언하면서 함수 안에 변수 `a`를 새로 선언하고 20을 대입한 코드이다. `getA()` 함수를 실행하면 함수 안의 변수인 `a`가 20의 값으로 콘솔에 출력된다. 함수의 실행이 끝나고 나서 `console.log(a);`로 다시 `a`의 값을 출력하면 10이 출력된다.

여기서 변수의 유효 범위는 함수 단위(function)로 한정된다는 것을 알 수 있다.

## const & let

const와 let 예약어는 ES6에서 사용하는 변수 선언 방식이다.

# let

let 예약어는 한번 선언하면 다시 선언할 수 없다.

```js
// 똑같은 변수를 재선언할 때 오류
let a = 1
let a = 2 // Uncaught SyntaxError: Identifier 'a' has already been declared
```

# const

const 예약어는 한번 할당한 값을 변경할 수 없다.

```js
// 값을 다시 할당했을 때 오류
const a = 10
a = 20 // Uncaught TypeError: Assignment to constant variable.
```

- 단, 객체 {}또는 배열 []로 선언했을 때는 객체의 속성(property)과 배열의 요소(element)를 변경할 수 있다.

```js
// 객체로 선언하고 속성 값을 변경
const b = {
  num: 10,
  text: 'hi',
}
console.log(b.num) // 10

b.num = 20
console.log(b.num) // 20
// 배열로 선언하고 배열 요소를 추가
const c = []
console.log(c) // []

c.push(10)
console.log(c) // [10]
```

# 블록 유효범위

ES5의 var를 이용한 변수 선언 방식과 let & const를 이용한 변수 선언 방식의 가장 큰 차이점은 블록 유효범위이다.

# var의 유효 범위

var의 유효 범위는 함수의 블록 단위로 제한된다. 흔히 함수 스코프(function scope)라고 표현된다.

```js
var a = 10
function print() {
  var a = 10
  console.log(a)
}
print() // 10
```

print 함수 앞에 선언한 a와 print 함수 안에 선언한 a는 각자 다른 유효 범위를 갖는다. var a = 10; 은 자바스크립트 전역 객체인 window에 추가가 되고 var a = 10;는 print() 함수 안에서만 유효한 범위를 갖는다.

# for 반복문에서의 var 유효 범위

var의 유효 범위가 {}에 제한되는지 확인해 보는 예제.

```js
var a = 10
for (var a = 0; a < 5; a++) {
  console.log(a) // 0 1 2 3 4 5
}
console.log(a) // 6
```

var a = 10;로 변수 a를 선언한 상태에서 for 반복문에 동일한 변수 이름 a를 사용했다. 이렇게 되면 {} 으로 변수의 유효 범위가 제한되지 않기 때문에 for 반복문이 끝나고 나서 console.log(a); 를 출력하면 for 반복문의 마지막 결과 값이 출력된다.

# const와 let의 블록 유효범위

이번엔 위 반복문 코드에 var 대신 let을 적용해보겠다.

```js
var a = 10
for (let a = 0; a < 5; a++) {
  console.log(a) // 0 1 2 3 4 5
}
console.log(a) // 10
```

반복문의 조건 변수 a를 let으로 선언하니 변수의 유효 범위가 for 반복문의 {} 블록 안으로 제한되었다.
