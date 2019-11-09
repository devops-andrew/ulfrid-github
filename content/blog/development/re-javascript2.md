---
title: '다시 자바스크립트 정리2'
date: '2019-06-05T19:39:21.169Z'
category: 'development'
---

![](./images/js-2.png)

# 자바스크립트 정리하기 2

## 배열(Array)

배열(Array)은 객체와 더불어 실제로 웹 애플리케이션을 구현할 때 가장 많이 쓰이는 데이터 유형이다. 배열을 선언하는 방식은 객체와 유사하다. Python은 list가 해당한다.

```bash
var arr = [];
```

'[]'를 배열 리터럴이라고 하며 배열을 정의할 때 사용한다.

## 배열의 요소

객체는 속성 / 값의 조합으로 데이터를 저장하지만 배열은 인덱스 / 값의 조합으로 데이터를 저장한다.

```js
// arr 변수에 빈 배열을 선언
var arr = []

// 배열의 0번째 인덱스에 10을 대입
arr[0] = 10

console.log(arr) // [10]
```

배열의 인덱스는 0부터 시작한다. 빈 배열에 최초로 값을 추가하면 0번째 인덱스에 값이 추가된다.

## 배열 조작하기

배열을 조작하는 방법은 아래와 같이 직접 인덱스에 접근해서 조작하는 방법이 있다.

```js
// 인덱스를 직접 접근해서 사용하는 경우
var arr = []
arr[0] = 100
arr[1] = 20
arr[0] = 10
console.log(arr) // [10, 20]
```

자바스크립트 내장 API를 사용하여 배열을 조작하는 방법은 다음과 같다.

```js
// 자바스크립트 내장 API를 사용하는 경우
var arr = []
arr.push(100)
arr.push(20)
arr.splice(0, 1, 10)
console.log(arr) // [10, 20]
```

## 자주 사용하는 배열 API

배열을 조작할 때 주로 사용하는 API는 다음과 같다.

push() : 배열에 데이터 추가 (맨 끝 인덱스부터 추가됨)
slice() : 배열의 특정 인덱스에 있는 값을 반환 (배열의 내용이 변환되지 않음)
splice() : 배열의 특정 인덱스에 있는 값을 변경 (배열의 내용이 변경됨)
pop() : 배열의 마지막 인덱스의 값을 꺼냄 (배열의 내용이 변경됨)
shift() : 배열의 첫번째 인덱스의 값을 꺼냄 (배열의 내용이 변경됨)

## Object

자바스크립트는 객체 기반 언어다. 객체는 키(key) - 값(value) 형태로 이루어져 있으며 아래와 같다.

```js
var obj = {
  //key:value,
}
```

위 코드는 obj라는 변수에 객체를 새로 할당한 코드다. 여기서 {}라는 기호가 객체를 의미하며 이를 객체 리터럴이라고 표현한다. 일반적으로 객체를 생성할 때는 객체 리터럴을 사용하여 위와 같은 방식으로 선언한다.

## 속성 추가

객체를 생성하고 나면 두가지 방식으로 속성(property)를 추가할 수 있다. 하나는 '.'노테이션, 또다른 하나는
'[]'브라켓 노테이션이다. 브라켓 노테이션의 경우 인자값을 문자열로 제공하야한다.

```js
// 객체 정의
var obj = {}

// num 속성을 추가하고 숫자 1을 할당.
obj.num = 1
```

위와 같은 방법 이외에도 아래와 같이 속성을 추가할 수 있습니다.

```js
// 객체 정의
var obj = {}

// num 속성을 추가하고 숫자 20을 할당.
obj['num'] = 20
```

## 속성 값 변경

이미 정의한 속성을 변경하는 방법은 해당 속성을 다시 접근하여 값을 할당하는 것이다.
이때도 마찬가지로 닷 노테이션이나 브라켓 노테이션을 이용한다.

```js
// 객체 정의
var obj = {}

// num 속성을 추가하고 숫자 10을 할당
obj.num = 10

// num 속성의 값에 숫자 20을 다시 할당
obj['num'] = 20
```

# this

this는 함수의 실행 컨텍스트를 가리키는 예약어이다. 여기서 실행 컨텍스트는 사전적인 정의로 '함수가 실행되는 환경'이며 좀 더 쉽게 접근하기 위해서는 '함수가 실행될 때의 컨텍스트'로 이해할 수 있다.

다른 언어와 다르게 자바스크립트의 this는 상황에 따라 다른 값들을 가르킨다.

## 첫 번째 this

```js
console.log(this) // window
```

this의 가장 기본적인 컨텍스트는 글로벌(전역) 컨텍스트이다. 여기서 출력된 window는 자바스크립트의 최상위 객체를 가리킨다.

## 두 번째 this

아래와 같은 객체가 있다고 가정하자.

```js
var obj = {
  num: 10,
  printNum: function() {
    console.log(this.num)
  },
}

obj.printNum() // 10
```

객체 속성 함수 안에서의 this는 기본적으로 해당 객체를 가리킨다.

## 세 번째 this

일반 함수(함수 선언문)에서의 this

```js
function showComment() {
  console.log(this)
}
```

위 함수를 아래와 같이 실행시키면 window 객체를 가리키고 있다는 것을 알 수 있다. 결론적으로 일반 함수의 this는 전역 컨텍스트 이다.

```js
showComment() // window
```

```js
function Developer() {
  console.log(this)
}
var dev = new Developer()
```

위 코드는 실행하자마자 바로 아래와 같은 결과를 콘솔에 출력한다.

```js
Developer {}
```

그 이유는 new로 인스턴스를 생성하는 순간 함수가 실행되기 때문이다. 그리고 여기서 알 수 있는 사실은 생성자 함수의 this는 함수의 내부를 가리킨다는 것이다.

## 네 번째 this

네 번째로 살펴볼 this는 실제로 웹 개발을 할 때 가장 많이 마주하게 되는 this이다. 바로 데이터를 받아올 때 사용하는 HTTP 요청과 같은 비동기 처리 코드이다.

```js
function fetchData() {
  axios.get('domain.com/products').then(function() {
    console.log(this)
  })
}
```

위 함수를 실행하면 결과는 window다.

```js
fetchData() // window
```

기본적으로 HTTP 요청과 같은 비동기 처리 코드는 전역 컨텍스트를 갖는다. 정리해서 비동기 처리 코드의 콜백 함수는 전역 컨텍스트를 가리킨다.

## 함수 선언문

함수 선언문은 아래와 같은 함수 정의 방식을 의미한다.

```js
// 함수 선언문
function functionName() {
  // ...
}
```

## 생성자 함수

그리고 생성자 함수는 아래와 같이 함수를 이용해 새 인스턴스를 선언하는 함수를 의미한다.

```js
function Developer() {
  // ...
}
var dev = new Developer()
```

자바스크립트는 프로토타입 기반 언어이다. 클래스 기반 언어가 아니기 때문에 위와 같이 함수를 이용하여 인스턴스를 생성할 수 있다.

## 클로져(Closure)

클로져는 함수의 실행이 끝난 뒤에도 함수에 선언된 변수의 값을 접근할 수 있는 자바스크립트의 성질이다. 자바스크립트를 다른 언어와 비교했을 때 차별화되는 유일한 특징이면서 처음 접하면 정말 애매하게 다가오기도 한다.

```js
function addNumber() {
  var number = 0

  return function() {
    return number++
  }
}
```

위 코드는 addNumber()라는 함수를 하나 생성하고 number 변수를 하나 선언한다. number라는 변수는 현재 함수 안에 선언되어 있기 때문에 함수 안에서만 유효한 유효 범위를 갖게 된다.

```js
function addNumber() {
  var number = 0
}

addNumber()
console.log(number) // Uncaught ReferenceError: counter is not defined
```

함수 밖에서 number 변수를 참조하려고 하면 오류가 발생된다. 그 이유는 함수 밖에서 number라는 변수가 선언된 적이 없기 때문이다.

```js
function addNumber() {
  var number = 0

  return function() {
    return number++
  }
}
```

number 변수 다음으로 주목할 부분은 함수를 반환하는 부분이다. 여기서 이렇게 함수를 반환할 수 있는 이유는 '함수를 변수나 인자로 넘길 수 있는 자바스크립트의 성질(일급 객체)' 때문이다. addNumber() 함수를 실행해보자.

```bash
addNumber();
```

결과확인~

```bash
console.log(addNumber());
```

출력된 결과는 아래와 같다.

```js
ƒ () {
    return number++;
  }
```

그럼 이제 반환된 함수를 살펴보면 counter++라는 코드가 보인다. 그리고 그 변수를 아래와 같이 접근하면 당연히 또 오류가 난다.

```js
function addNumber() {
  var number = 0

  return function() {
    return number++
  }
}

addNumber()
console.log(number) // Uncaught ReferenceError: counter is not defined
```

코드를 찬찬히 살펴보면 addNumber()함수의 실행이 끝난 시점에서는 number라는 변수는 더이상 접근할 수 없는 상태가 된다. 함수 안에 선언한 변수는 함수 안에서만 유효 범위를 갖기 때문이다.

```js
function addNumber() {
  var number = 0

  return function() {
    return number++
  }
}

var add = addNumber()
add() // 0
add() // 1
add() // 2
```

위와 같이 코드를 실행했을 때 동작하는 이유는 addNumber()라는 함수가 반환한 함수를 add라는 변수에 담아놨기 때문에 add 변수 자체가 함수처럼 동작하는 것이다.

이처럼 함수의 실행이 끝나고 나서도 함수 안의 변수를 참조할 수 있는게 바로 클로져이다.

## 함수형 프로그래밍

함수형 프로그래밍은 현재 듣고 있는 유인동님의 강의가 정리되면 다시 정리해 보도록 하겠다.
