# ECMAScript 6 <sup>[git.io/es6features](http://git.io/es6features)</sup>

## 소개
ECMAScript 2015 라고도 알려진 ECMAScript 6은 ECMAScript의 표준 중 가장 최신 버전이다. ES6는 해당 언어에 대한 중요한 업데이트이며 2009년에 ES5가 표준화 된 이후의 첫번째 업데이트다. 주요 자바스크립트 엔진의 기능 구현은 [진행 중](http://kangax.github.io/es5-compat-table/es6/)이다.

ECMAScript 6 언어의 전체 사양은 [ES6 표준](http://www.ecma-international.org/ecma-262/6.0/)을 참조하자.

ES6는 아래의 새로운 기능을 포함하고 있다.
- [arrows](#arrows)
- [classes](#classes)
- [enhanced object literals](#enhanced-object-literals)
- [template strings](#template-strings)
- [destructuring](#destructuring)
- [default + rest + spread](#default--rest--spread)
- [let + const](#let--const)
- [iterators + for..of](#iterators--forof)
- [generators](#generators)
- [unicode](#unicode)
- [modules](#modules)
- [module loaders](#module-loaders)
- [map + set + weakmap + weakset](#map--set--weakmap--weakset)
- [proxies](#proxies)
- [symbols](#symbols)
- [subclassable built-ins](#subclassable-built-ins)
- [promises](#promises)
- [math + number + string + array + object APIs](#math--number--string--array--object-apis)
- [binary and octal literals](#binary-and-octal-literals)
- [reflect api](#reflect-api)
- [tail calls](#tail-calls)

## ECMAScript 6 Features

### Arrows
화살표 함수는 `=>` 문법을 이용해서 함수를 짧게 표현한다. C#, 자바8, 커피스크립트와 비슷한 문법을 사용한다. 함수 표현식 본문 뿐만아니라 명령문 블록 본문도 지원한다. 일반 함수와 달리 화살표 함수는 자신을 둘러싼 어휘적 this를 사용한다.

```JavaScript
// 함수 표현식 본문
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({even: v, odd: v + 1}));

// 명령문 본문
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

// 어휘적 this
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```

더보기: [MDN Arrow Functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

### Classes
ES6 클래스는 프로토타입-기반 OO 패턴이다. 편리한 선언적 형식을 사용하면 클래스 패턴을 쉽게 만들 수 있으며, 이전 버젼과 상호 운영성이 있습니다. 클래스는 프로토타입 기반 상속, 슈퍼 호출, 인스턴스와 정적 메소드 그리고 생성자를 제공한다.

```JavaScript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

더보기: [MDN Classes](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

### Enhanced Object Literals
객체 리터럴은 객체 생성시 프로토타입을 설정할 수 있다. `foo: foo` 형식으로 할당할 때는 `foo`만 적어도 된다. 메소드를 정의할 수 있고 슈퍼를 호출할 수 있다. 함수식으로 프로퍼티 이름 계산이 가능하다. 또한 객체 리터럴과 클래스 정의는 비슷하기 때문에 객체기반 설계의 장점을 얻을 수 있다.

```JavaScript
var obj = {
    // __proto__
    __proto__: theProtoObj,
    // ‘handler: handler’의 간단한 표현
    handler,
    // 메쏘드
    toString() {
      // 슈퍼 호출
      return "d " + super.toString();
    },
    // (동적으로) 계산된 프로퍼티 이름
    [ 'prop_' + (() => 42)() ]: 42
};
```

더보기: [MDN Grammar and types: Object literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Object_literals)

### Template Strings
템플릿 문자열을 이용하면 손쉽게 문자열을 만들수 있다. 펄, 파이썬의 문자열 인터폴레이션 기능과 비슷하다. 인젝션 공격을 차단하거나 고수준 데이터 구조를 유지하는 문자열을 만들기 위해 태그를 추가하여 만들 수 있다.

```JavaScript
// 기본적인 리터럴 문자열 생성
`In JavaScript '\n' is a line-feed.`

// 여러 줄 문자열
`In JavaScript this is
 not legal.`

// 문자열 인터폴레이션
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// Construct an HTTP request prefix is used to interpret the replacements and construction
POST`http://foo.org/bar?a=${a}&b=${b}
     Content-Type: application/json
     X-Credentials: ${credentials}
     { "foo": ${foo},
       "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

더보기: [MDN Template Strings](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/template_strings)

### Destructuring
Destructuring은 패턴 매칭을 사용하여 바인딩 시키며, 배열과 객체를 매칭하는데 사용된다. Destructuring은 표준 객체인 `foo["bar"]`를 찾는 것과 비슷하며, 값을 찾을 수 없다면, `undefined`로 할당된다.

```JavaScript
// 배열 매칭
var [a, , b] = [1,2,3];

// 객체 매칭
var { op: a, lhs: { op: b }, rhs: c }
       = getASTNode()

// 짧은 표현으로 객체를 매칭
// `op`, `lhs`, `rhs` 로 바인딩 된다.
var {op, lhs, rhs} = getASTNode()

// 매개변수에서도 사용 가능
function g({name: x}) {
  console.log(x);
}
g({name: 5})

// destructuring 실패(매칭값이 없을 경우)
var [a] = [];
a === undefined;

// destructuring 실패(매칭값이 없지만, 기본값이 설정된 경우)
var [a = 1] = [];
a === 1;
```

더보기: [MDN Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

### Default + Rest + Spread
함수를 정의할 때 파라매터의 기본값을 지정할 수 있다. 나머지 파라매터는 배열로 전달된다. 그리고 arguments 대신 사용한다. 함수를 호출할 때 펼침 연산자를 이용하면 배열의 각 배열 요소를 파라매터로 전달할 수 있다.

```JavaScript
function f(x, y=12) {
  // y 값이 전달되지 않았거나 undefined로 넘어올 경우 y값은 12다.
  return x + y;
}
f(3) == 15
```
```JavaScript
function f(x, ...y) {
  // y는 배열이다.
  return x * y.length;
}
f(3, "hello", true) == 6
```
```JavaScript
function f(x, y, z) {
  return x + y + z;
}
// 배열의 각 요소를 매개변수로 전달 f(1,2,3)
f(...[1,2,3]) == 6
```

더보기: [Default parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters), [Rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters), [Spread Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)

### Let + Const
`let, const`는 블록 단위 스코프 바인딩이다. `let`은 새로운 `var` 이며, `const`는 단일 할당입니다. 정적 제한은 할당전에 사용하지 못하게 제한합니다.

```JavaScript
function f() {
  {
    let x;
    {
      // okay, 블록 범위안에 선언
      const x = "sneaky";
      // const 재할당 에러
      x = "foo";
    }
    // 이미 블록 안에 선언 되있어서 에러
    let x = "inner";
  }
}
```

더보기: [let statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), [const statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

### 이터레이터 + For..Of
이터레이터 객체는 공통 언어 런타임(Common Language Runtime, CLR)에서의 IEnumerable, 자바에서의 Iterable과 같은 사용자 지정 반복을 가능하게 해준다. 기존 `for..in`으로 작성했던 코드를 `for..of`를 통해 커스텀 이터레이터 기반의 이터레이션으로 일반화 할 수 있다. 반복자를 위한 배열을 구현할 필요가 없고, LINQ와 같은 게으른 디자인 패턴을 사용할 수 있다.

```JavaScript
let fibonacci = {
  [Symbol.iterator]() {
    let pre = 0, cur = 1;
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        return { done: false, value: cur }
      }
    }
  }
}

for (var n of fibonacci) {
  // 1000 초과 시 중단
  if (n > 1000)
    break;
  console.log(n);
}
```

([타입스크립트](http://typescriptlang.org)에서만 사용되는 표현식 타입 구문) 이터레이션은 덕 타입 인터페이스 기반이다.

```TypeScript
interface IteratorResult {
  done: boolean;
  value: any;
}
interface Iterator {
  next(): IteratorResult;
}
interface Iterable {
  [Symbol.iterator](): Iterator
}
```

더보기: [MDN for...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)

### 제너레이터
제너레이터는 `function*` 와 `yield`를 사용하여 이터레이터 작성을 단순화 한다. function*로 정의된 함수는 제너레이터 인스턴스를 반환한다. 제너레이터는 `next` 와 `throw`를 포함하고 있는 이터레이터의 하위 유형이다.
이것들은 값을 제너레이터로 다시 되돌릴 수 있게 해준다. 그래서 `yield`는 값을 반환하는 표현식이다.

참고: ‘await’와 같은 비동기 프로그래밍을 활성화하는 데 사용할 수 있다. ES7의 `await` 제안을 참조하자.

```JavaScript
var fibonacci = {
  [Symbol.iterator]: function*() {
    var pre = 0, cur = 1;
    for (;;) {
      var temp = pre;
      pre = cur;
      cur += temp;
      yield cur;
    }
  }
}

for (var n of fibonacci) {
  // 1000 초과 시 중단
  if (n > 1000)
    break;
  console.log(n);
}
```

([타입스크립트](http://typescriptlang.org)에서만 사용되는 표현식 타입 구문) 제너레이터 인터페이스는 다음과 같다.


```TypeScript
interface Generator extends Iterator {
    next(value?: any): IteratorResult;
    throw(exception: any);
}
```

더보기: [MDN Iteration protocols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)

### Unicode
완전한 유니코드를 지원한다. 코드 포인트를 처리할 수 있는 유니코드 리터럴과 정규표현식의 `u` 옵션이 있다. 21비트 코드 포인트 수준에서 문자열을 처리하는 새로운 API도 있다. 이러한 추가 기능으로 글로벌 어플리케이션 개발을 지원한다.

```JavaScript
// ES5.1과 같다
"𠮷".length == 2

// 정규 표현식에서 'u' 옵션을 사용한다
"𠮷".match(/./u)[0].length == 2

// 새로운 형식
"\u{20BB7}"=="𠮷"=="\uD842\uDFB7"

// 새로운 문자열 옵션
"𠮷".codePointAt(0) == 0x20BB7

// for-of 반복자
for(var c of "𠮷") {
  console.log(c);
}
```

더보기: [MDN RegExp.prototype.unicode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/unicode)

### Modules
언어레벨에서 컴포넌트 정의를 위한 모듈을 제공한다. 유명한 자바스크립트 모듈 로더(AMD, CommonJS)에서 가져온 패턴이다. 호스트에 정의된 기본 로더로 런타임에 실행되는 것을 정의한다. 암시적 비동기 모델 - 요청된 모듈이 사용할 수 있고, 처리 될때까지 코드는 실행되지 않는다.

```JavaScript
// lib/math.js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
```
```JavaScript
// app.js
import * as math from "lib/math";
alert("2π = " + math.sum(math.pi, math.pi));
```
```JavaScript
// otherApp.js
import {sum, pi} from "lib/math";
alert("2π = " + sum(pi, pi));
```

추가 기능으로 `export default` 와 `export *` 가 있다.

```JavaScript
// lib/mathplusplus.js
export * from "lib/math";
export var e = 2.71828182846;
export default function(x) {
    return Math.log(x);
}
```
```JavaScript
// app.js
import ln, {pi, e} from "lib/mathplusplus";
alert("2π = " + ln(e)*pi*2);
```

더보기: [import statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import), [export statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

### Module Loaders
모듈 로더 지원:
- 동적 로딩
- 상태 분리
- 전역 네임스페이스 분리
- Compilation hooks
- Nested virtualization

기본 모듈 로더를 설정 할 수 있으며, 새로운 로더를 제한되거나, 격리된 컨텍스트에서 코드를 실행하거나, 평가 할 수 있도록 구성할 수 있습니다.

```JavaScript
// 동적 로딩 – ‘System’ 은 기본 로더이다.
System.import('lib/math').then(function(m) {
  alert("2π = " + m.sum(m.pi, m.pi));
});

// 실행 시키는 샌드 박스 생성 – 새로운 로더
var loader = new Loader({
  global: fixup(window) // ‘console.log’ 로 교체
});
loader.eval("console.log('hello world!');");

// 직접 모듈 캐시 조작
System.get('jquery');
System.set('jquery', Module({$: $})); // 경고 : 아직 완료되지 않았다.
```

### Map + Set + WeakMap + WeakSet
공통 알고리즘을 위한 효율적인 데이터 구조. WeakMaps은 열은 키 객체 참조를 제공해준다. 그래서 가비지 컬랙션을 통해 메모리 누수를 막을 수 있다.


```JavaScript
// Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

// Maps
var m = new Map();
m.set("hello", 42);
m.set(s, 34);
m.get(s) == 34;

// Weak Maps
var wm = new WeakMap();
wm.set(s, { extra: 42 });
wm.size === undefined

// Weak Sets
var ws = new WeakSet();
ws.add({ data: 42 });
// 추가된 객체에는 다른 참조가 없기 때문에 set에 포함되지 않는다.
```

더보기: [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map), [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set), [WeakMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap), [WeakSet](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)

### 프록시
프록시는 호스트 객체에 있는  모든 것을 사용할 수 있는 객체를 만들 수 있게 해준다. 프록시는 가로채기, 객체 가상화, 로깅/프로파일링 등에 사용된다.

```JavaScript
// 정상적인 객체 프록시
var target = {};
var handler = {
  get: function (receiver, name) {
    return `Hello, ${name}!`;
  }
};

var p = new Proxy(target, handler);
p.world === 'Hello, world!';
```

```JavaScript
// 정상적인 함수 프록시
var target = function () { return 'I am the target'; };
var handler = {
  apply: function (receiver, ...args) {
    return 'I am the proxy';
  }
};

var p = new Proxy(target, handler);
p() === 'I am the proxy';
```

모든 런타임 단계의 메타 설정에 사용할 수 있는 트랩들이 있다.

```JavaScript
var handler =
{
  get:...,
  set:...,
  has:...,
  deleteProperty:...,
  apply:...,
  construct:...,
  getOwnPropertyDescriptor:...,
  defineProperty:...,
  getPrototypeOf:...,
  setPrototypeOf:...,
  enumerate:...,
  ownKeys:...,
  preventExtensions:...,
  isExtensible:...
}
```

더보기: [MDN Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

### 심볼
심볼은 객체 상태의 접근 제어를 가능하게 해준다. 심볼을 사용하면 객체의 속성으로 ES5에서의 `string` 혹은 `symbol`로 입력할 수 있다  심볼은 새로운 원시 타입이다. 추가 `description` 매개변수는 심볼의 디버깅에 사용될 뿐, 심볼에 대한 접근이나 기능이 아니다. 심볼은 gensym와 같이 유일한 값 하지만, `Object.getOwnPropertySymbols`와 같은 리플렉션 기능을 통해 외부에 노출되어 있기 때문에 private 하지는 않다.


```JavaScript
var MyClass = (function() {

  // 모듈 안에서만 접근 가능한 심볼
  var key = Symbol("key");

  function MyClass(privateData) {
    this[key] = privateData;
  }

  MyClass.prototype = {
    doStuff: function() {
      ... this[key] ...
    }
  };

  return MyClass;
})();

var c = new MyClass("hello")
c["key"] === undefined
```

더보기: [MDN Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)

### 내장 객체의 서브클래스화
ES6에서는 `Array`, `Date`, 돔 `Element`와 같은 내장 객체를 서브클래스화 할 수 있다. 

`Ctor`라는 이름의 함수 객체 생성은 이제 2단계를 사용한다. (모두 가상으로 실행)
- 객체 할당을 위해 `Ctor[@@create]`를 호출하고, 해당 객체에 특별 속성을 설치한다.
- 새 인스턴스에서 생성자를 실행해서 초기화한다.

`@@create` 심볼은 `Symbol.create`을 통해 사용할 수 있다. 이제 내장 객체는 `@@create`를 명시적으로 노출하게 된다.

```JavaScript
// Array 클래스의 슈도 코드
class Array {
    constructor(...args) { /* ... */ }
    static [Symbol.create]() {
        // 'length' 값을 업데이트하기 위해 
        // 특별한 [[DefineOwnProperty]]를 설치
    }
}

// Array 서브클래스의 사용자 코드
class MyArray extends Array {
    constructor(...args) { super(...args); }
}

// 'new'의 2단계:
// 1) 객체 할당을 위한 @@create 호출
// 2) 새로운 인스턴스에서의 생성자 실행
var arr = new MyArray();
arr[1] = 12;
arr.length == 2
```

### Math + Number + String + Array + Object APIs		
수학 라이브러리, 배열과 문자열 헬퍼 함수, 객체 복사를 위한 Object.assign() 함수가 있다.

```JavaScript
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2

"abcde".includes("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll('*')) // 진짜 배열 반환
Array.of(1, 2, 3) // new Array(..)와 비슷하지만 인자가 여러개다. (new Array()는 크기 인자를 1개만 받음)
[0, 0, 0].fill(7, 1) // [0,7,7]
[1, 2, 3].find(x => x == 3) // 3
[1, 2, 3].findIndex(x => x == 2) // 1
[1, 2, 3, 4, 5].copyWithin(3, 0) // [1, 2, 3, 1, 2]
["a", "b", "c"].entries() // [0, "a"], [1,"b"], [2,"c"] 반복자
["a", "b", "c"].keys() // 0, 1, 2 반복자
["a", "b", "c"].values() // "a", "b", "c" 반복자

Object.assign(Point, { origin: new Point(0,0) })
```

더보기: [Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number), [Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math), [Array.from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from), [Array.of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/of), [Array.prototype.copyWithin](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin), [Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

### Binary and Octal Literals		
이진수(`b`)와 8진수(`o`)를 위해 숫자 리터럴 두 개 추가되었다.

```JavaScript
0b111110111 === 503 // true
0o767 === 503 // true
```

### 프로미스
프로미스는 비동기 프로그래밍을 위한 라이브러리다. 프로미스는 미래에 사용할 수 있는 값의 일급 표현이다. 프로미스는 현존하는 많은 자바스크립트 라이브러리에 사용되고 있다.

```JavaScript
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
```

더보기: [MDN Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

### Reflect API
완전한 reflection API는 런타임 단계에서 객체의 메타 작업을 보여준다. 실제로 Proxy API의 반대이며, 프록시 트랩과 동일한 메타작업을 하는 메서드 호출을 허용한다. 특히 프록시를 구현할 때 유용하다.

```JavaScript
// 아직 예제가 준비되지 않았다.
```

더보기: [MDN Reflect](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

### Tail Calls
꼬리 호출이 스택을 무한대로 생성되지 않게 보장해준다. 무제한 입력에도 재귀 알고리즘을 안전하게 해준다.

```JavaScript
function factorial(n, acc = 1) {
    'use strict';
    if (n <= 1) return acc;
    return factorial(n - 1, n * acc);
}

// 대부분 구현에서는 스택오버플로우가 발생한다.,
// 그러나 임의 입력에도 ES6에서는 안전하다.
factorial(100000)
```
