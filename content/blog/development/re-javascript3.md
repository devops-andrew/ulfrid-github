---
title: '다시 자바스크립트 정리3'
date: '2020-09-10T19:39:21.169Z'
category: 'development'
---

#Javascript

let's do it javascript

모든 자바스크립트를 정리하지는 않습니다. 리액트를 사용할때 필요한 자바스크립트 포인트를 정리합니다!

No more `var` !!!

왜? 지난 버전의 자바스크립트는 var로 모든 것을 선언했고 그로 인해 스코프 오염을 걱정해야 했습니다.(물론 관리 잘하면 상관 없지만)

그래서 최신 버전의 자바스크립트는 명확한 스코핑을 위해 상수와 변수 선언을 위한 두가지를 준비했습니다.

물론 이 두가지를 써도 잘 못쓰면 결국 스코프는 오염됩니다. 결국 잘 사용하려면 전역변수를 남발하지 않고 각 블록에서 잘 사용하는게 좋습니다.

상수는  `const` , 변수는 `let`

`const` : 상수이므로 재할당 불가. 내부 속성 값은 수정 가능합니다.

`let` : 지역 변수를 지원하는 변수 선언  방법입니다.

### Template Literals 템플릿 리터럴

```jsx
` ` 백틱이라고 부릅니다.

let gwanghwamoon  `넌 어땠는지, 아직 여름이 남아
왠지 난 조금 지쳤던 하루
광화문 가로수 은행잎 물들 때
그제야 고갤 들었었나 봐`
```

### Arrow function 화살표 함수

```jsx
function welcome(name){
  return "안녕하세요" + name
}

===>

const welcome = () => "안녕하세요" + name
```

### Destructuring 구조분해 할당 및 비구조화 할당 (리액트에서 많이 쓰임)

```jsx
//배열
let [name] = ["Tom", 10, "Seoul"];
console.log(name) // "Tom"

let x = [1,2,3,4,5]
let[y,z] = x
console.log(y) // 1
console.log(z) // 2

//오브젝트
let o = {p:42, q:true}
let {p,q}  = o;

console.log(p) // 42
console.log(q) // true

//선언없는 할당 
let a, b;

({a,b} = {a:1, b:2});
console.log(a) // 1
console.log(b) // 2

//새로운 변수명 주기
let o = {p:42, q:true}
let {p:foo ,q:bar}  = o;

console.log(foo) // 42
console.log(bar) // true

//default 파라미터 설정 및 새로운 변수명에 할당하기
let {a: aa = 10, b: bb = 5} = {a : 3);

console.log(aa); // 3
console.log(bb); // 5

//함수에서 할당
const draw = ({size='big', cords = {x:0, y:0}, radius =25}) => console.log(size, cords, radius); 
draw({cords:{x:18, y:30},radius: 30}); // big { x: 18, y: 30 } 30

//위와 같이 할당할수 있으나, 위의 경우는 매개변수 없이는 함수 호출이 불가능함. 만약 매개변수 없이 호출하려면 
const draw = ({size='big', cords = {x:0, y:0}, radius =25} = {}) => console.log(size, cords, radius); 
//위와 같이 빈객체를 할당하면 매개변수 없이 함수 호출이 가능함

//중첩 객체 (Nested Object) 및 배열의 구조분해

const metadata = {
    title: "django",
    translations: [
       {
        locale: "kr",
        localization_tags: [ ],
        last_edit: "2014-04-14T08:43:37",
        url: "/kr/django",
        title: "장고"
       }
    ],
    url: "/mycode/ulfrid"
};

const { title: englishTitle, translations: [{ title: localeTitle }] } = metadata;

console.log(englishTitle); // "django"
console.log(localeTitle);  // "장고"

```

### spread 전개 연산자 및 rest 파라미터

```jsx
// 먼저 전개연산자(spread)에 대해서 이해해야 한다.
const arr = [1, 2, 3, 4, 5];

console.log(arr); // [ 1, 2, 3, 4, 5 ]
console.log(...arr); // 1, 2, 3, 4, 5
console.log(1, 2, 3, 4, 5); // 1, 2, 3, 4, 5

//이런식의 활용도 가능하다.
const aArr = [1, 2, 3];
const bArr = [4, 5, 6];

// array.concat
console.log(aArr.concat(bArr)); // [ 1, 2, 3, 4, 5, 6 ]

// spread
console.log([...aArr, ...bArr]); // [ 1, 2, 3, 4, 5, 6 ]

// rest 파라미터에 대해 이해해보자 기본적으로 동작은 spread의 반대와 같다.
// spread는 배열에 들어있는 데이터를 개별적으로 전개해서 꺼내주지만, 
// rest 파라미터는 인자로 받은 데이터중, 레스트 선언된 부분의 데이터를 배열로 변환한다.

let [name, ...rest] = ["Tom", 10, "Seoul"];
console.log(name) // Tom
console.log(rest) // [10, seoul]

function func(...param) {
  console.log(param);
}

func(1, 2, 3); // [ 1, 2, 3 ]
```

### 콜백을 벗어나기 위한 노력 Promise와 async/await

```jsx
const fs = require('fs');
fs.readdir('.', function (err, files) {
	if (err) {
		console.log('Error finding files: ' + err) }
	else { console.log(files);
	}
});
// 위 fs.readdir이 끝나기 전에 수행 
console.log("ENDED");

// 이를 promise로 개선하면

fs = require('fs');
const fsPromises = fs.promises;
fsPromises.readdir('.')
	.then(files => {
		console.log(files);
	})
	.catch(err => console.error(err));

fsPromises
// 위 fsPromises.readdir이 끝나기 전에 수행 
console.log("ENDED");

// async/await로 적용하면 

const fs = require('fs');
const fsPromises = fs.promises;
async function fn() {
    try {
			let files = await fsPromises.readdir('.');
			console.log(files); }
		catch(err) { 
			console.error(err);
		} 
}
fn(); 
// async 함수 이기에, 완료 전에 다음 로직이 동작 
console.log("ENDED");
```

### 클래스와 상속

```jsx
//ES6 부터 클래스 생성 방식
class Person{ 
	constructor(name, age){
		this.name = name;
		this.age  = age;
	}
	print(){
		console.log(this.name + ", " + this.age);
	}
}

const tom = new Person("Tom", 10);
tom.print() // Tom, 10

class Developer extends Person {
	constructor(name, age, field) {
			super(name, age);
			this.field = field;
	}
	print() {
			super.print();
			console.log(`field: ${this.field}`);
	}
}
```

클래스를 `class`로 선언하는것과 상속 시 `extends`를 쓴다는게 포인트!

### Array method 다양한 어레이 메소드들

```jsx
arr.push() // 배열의 맨뒤에 값을 삽입

arr.pop() // 배열의 마지막 값을 삭제

arr.unshift() // 배열의 첫번째에 값을 삽입

arr.shift() // 배열의 첫번째 값을 삭제

arr.splice() // 배열의 특정위치에 요소를 추가하거나 삭제

arr.slice(startIndex, endIndex) // 시작인덱스 부터 끝인덱스(불포함)전까지 얕은복사로 새로운 배열을 반환

arr.concat() // 다수의 배열을 합치고 병합된 배열의 사본을 반환

arr.every() // 모든 요소가 조건을 통과하는지 테스트 모든요소가 true면 true 아니며 false (and)

arr.some() // 함수의 결과가 조건에 true일때 까지 반복  하나라도 true면 true (or)

arr.forEach(function(arg)) // 배열의 각원소별로 지정된 함수를 실행

arr.map() // 배열의 각 원소별로 지정된 함수를 실행한 결과로 구성된 새로운 배열을 반환

arr.filter() // 지정된 함수의 결과 값에 해당하는 원소들로만 구성된 새로운 배열을 반환

arr.reduce() // 누산기 및 배열의 각 값에 대해 연산할수 있도록 함수를 적용할 수 있음

arr.indexOf() // 원하는 요소의 위치 찾기

arr.lastIndexOf() // 뒤에서부터 순회해서 요소 찾기

arr.includes() // 요소의 포함 여부 true || false

arr.find() // 주어진 판별함수를 만족하는 첫번째 요소를 반환 없으면 undefined 

arr.flat() // 중첩 배열 뎁스 제거 flatten

이밖에 reverse(), sort(), toString(), valueOf(), join() 등이 있음
```

### 고차 함수(high order function)

```jsx
//함수를 인자로 받거나 반환이 가능하고, 다른 함수를 조작하는 함수 (함수/클래스 모두 객체)

function base_10(fn) {
	function wrap(x,y) {
		return fn(x, y) +10;
	}
	return wrap;
}

function mysum(x,y) {
	return x + y;
}

mysum = base_10(mysum);

console.log(mysum(1, 2)); // 13

// 이를 애로우펑션으로 바꾸면

const base_10 = fn => (x, y) => fn(x, y) +10;

let mysum = (x, y) => x + y;
mysum = base_10(mysum);

console.log(mysum(1,2)); // 13

```

### 커링(currying)

리액트는 함수형 프로그래밍을 지향!
컴포넌트의 많은 루틴을 순수함수로서 작성하기를 요구
상탯값/ 속성값이 같으면, 항상 같은 값을 반환해야 함!
사이드 이펙트를 발생시키지 않아야 함!
컴포넌트의 상태값을 불변 객체(immutable object)로 관리해야하는데 이는 얕은 복사(shallow copy)를 하면 안된다는 말.
상태값을 수정시에는 기존값을 변경하는게 아니라, 같은 이름의 새로운 객체를 생성해야 함 
순수 함수는 무엇인가? 
하나이상의 인자를 받고, 인자를 변경하지 않고, 참조하여 새로운 값을 반환 Side Effects가 없도록 구성

순수함수가 아닌 예

순수함수의 예

```jsx

let tom = {
	name: "Tom",
	canRun: false
};

function not_pure_fn(){
	tom.canRun = true; // 오브젝트를 직접 수정
}

```

```jsx
const pure_fn1 = (person) => (
{
	...person,  
	canRun: true 
} // 새로운 오브젝트를 생성해서 변경
);
```

### 순수함수에서 데이터변환에 자주 쓰이는 메소드

```jsx
reduce, filter, map, join
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]; 
const number = numbers.reduce((acc, n) => acc + n, 0); 
console.log(number);
const even_numbers = numbers.filter(i => i % 2 == 0); 
console.log(even_numbers);
```

### 커링

일부의 인자를 고정한 새로운 함수를 반환하는 함수를 만드는 기법(like Python Decorators)

```jsx
function userLogs(username) {
	function wrap(message) {
	console.log(`${username} - ${message}`);
	} 
	return wrap;
}
const log = userLogs('ulfrid'); 
log('Hello World');

const userLogs = username => message => {
	 console.log(`${username} - ${message}`);
};

const log = userLogs('ulfrid');
log('Hello World');
```=
