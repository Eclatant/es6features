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

### 화살표 함수
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
ES6 classes are a simple sugar over the prototype-based OO pattern.  Having a single convenient declarative form makes class patterns easier to use, and encourages interoperability.  Classes support prototype-based inheritance, super calls, instance and static methods and constructors.

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

More info: [MDN Classes](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

### 개선된 객체 리터럴
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

### 템플릿 문자열
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
Destructuring allows binding using pattern matching, with support for matching arrays and objects.  Destructuring is fail-soft, similar to standard object lookup `foo["bar"]`, producing `undefined` values when not found.

```JavaScript
// list matching
var [a, , b] = [1,2,3];

// object matching
var { op: a, lhs: { op: b }, rhs: c }
       = getASTNode()

// object matching shorthand
// binds `op`, `lhs` and `rhs` in scope
var {op, lhs, rhs} = getASTNode()

// Can be used in parameter position
function g({name: x}) {
  console.log(x);
}
g({name: 5})

// Fail-soft destructuring
var [a] = [];
a === undefined;

// Fail-soft destructuring with defaults
var [a = 1] = [];
a === 1;
```

More info: [MDN Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

### 파라메터 기본값, 나머지 파라매터, 펼침 연산자
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
Block-scoped binding constructs.  `let` is the new `var`.  `const` is single-assignment.  Static restrictions prevent use before assignment.


```JavaScript
function f() {
  {
    let x;
    {
      // okay, block scoped name
      const x = "sneaky";
      // error, const
      x = "foo";
    }
    // error, already declared in block
    let x = "inner";
  }
}
```

More MDN info: [let statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), [const statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

### Iterators + For..Of
Iterator objects enable custom iteration like CLR IEnumerable or Java Iterable.  Generalize `for..in` to custom iterator-based iteration with `for..of`.  Don’t require realizing an array, enabling lazy design patterns like LINQ.

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
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  console.log(n);
}
```

Iteration is based on these duck-typed interfaces (using [TypeScript](http://typescriptlang.org) type syntax for exposition only):
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

More info: [MDN for...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)

### Generators
Generators simplify iterator-authoring using `function*` and `yield`.  A function declared as function* returns a Generator instance.  Generators are subtypes of iterators which include additional  `next` and `throw`.  These enable values to flow back into the generator, so `yield` is an expression form which returns a value (or throws).

Note: Can also be used to enable ‘await’-like async programming, see also ES7 `await` proposal.

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
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  console.log(n);
}
```

The generator interface is (using [TypeScript](http://typescriptlang.org) type syntax for exposition only):

```TypeScript
interface Generator extends Iterator {
    next(value?: any): IteratorResult;
    throw(exception: any);
}
```

More info: [MDN Iteration protocols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)

### 유니코드
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
Language-level support for modules for component definition.  Codifies patterns from popular JavaScript module loaders (AMD, CommonJS). Runtime behaviour defined by a host-defined default loader.  Implicitly async model – no code executes until requested modules are available and processed.

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

Some additional features include `export default` and `export *`:

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

More MDN info: [import statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import), [export statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

### Module Loaders
Module loaders support:
- Dynamic loading
- State isolation
- Global namespace isolation
- Compilation hooks
- Nested virtualization

The default module loader can be configured, and new loaders can be constructed to evaluate and load code in isolated or constrained contexts.

```JavaScript
// Dynamic loading – ‘System’ is default loader
System.import('lib/math').then(function(m) {
  alert("2π = " + m.sum(m.pi, m.pi));
});

// Create execution sandboxes – new Loaders
var loader = new Loader({
  global: fixup(window) // replace ‘console.log’
});
loader.eval("console.log('hello world!');");

// Directly manipulate module cache
System.get('jquery');
System.set('jquery', Module({$: $})); // WARNING: not yet finalized
```

### Map + Set + WeakMap + WeakSet
Efficient data structures for common algorithms.  WeakMaps provides leak-free object-key’d side tables.

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
// Because the added object has no other references, it will not be held in the set
```

More MDN info: [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map), [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set), [WeakMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap), [WeakSet](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)

### Proxies
Proxies enable creation of objects with the full range of behaviors available to host objects.  Can be used for interception, object virtualization, logging/profiling, etc.

```JavaScript
// Proxying a normal object
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
// Proxying a function object
var target = function () { return 'I am the target'; };
var handler = {
  apply: function (receiver, ...args) {
    return 'I am the proxy';
  }
};

var p = new Proxy(target, handler);
p() === 'I am the proxy';
```

There are traps available for all of the runtime-level meta-operations:

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

More info: [MDN Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

### Symbols
Symbols enable access control for object state.  Symbols allow properties to be keyed by either `string` (as in ES5) or `symbol`.  Symbols are a new primitive type. Optional `description` parameter used in debugging - but is not part of identity.  Symbols are unique (like gensym), but not private since they are exposed via reflection features like `Object.getOwnPropertySymbols`.


```JavaScript
var MyClass = (function() {

  // module scoped symbol
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

More info: [MDN Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)

### Subclassable Built-ins
In ES6, built-ins like `Array`, `Date` and DOM `Element`s can be subclassed.

Object construction for a function named `Ctor` now uses two-phases (both virtually dispatched):
- Call `Ctor[@@create]` to allocate the object, installing any special behavior
- Invoke constructor on new instance to initialize

The known `@@create` symbol is available via `Symbol.create`.  Built-ins now expose their `@@create` explicitly.

```JavaScript
// Pseudo-code of Array
class Array {
    constructor(...args) { /* ... */ }
    static [Symbol.create]() {
        // Install special [[DefineOwnProperty]]
        // to magically update 'length'
    }
}

// User code of Array subclass
class MyArray extends Array {
    constructor(...args) { super(...args); }
}

// Two-phase 'new':
// 1) Call @@create to allocate object
// 2) Invoke constructor on new instance
var arr = new MyArray();
arr[1] = 12;
arr.length == 2
```

### 수학, 숫자, 문자열, 배열, 객체 API
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

### 2진수와 8진수
이진수(`b`)와 8진수(`o`)를 위해 숫자 리터럴 두 개 추가되었다.

```JavaScript
0b111110111 === 503 // true
0o767 === 503 // true
```

### Promises
Promises are a library for asynchronous programming.  Promises are a first class representation of a value that may be made available in the future.  Promises are used in many existing JavaScript libraries.

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

More info: [MDN Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

### Reflect API
Full reflection API exposing the runtime-level meta-operations on objects.  This is effectively the inverse of the Proxy API, and allows making calls corresponding to the same meta-operations as the proxy traps.  Especially useful for implementing proxies.

```JavaScript
// No sample yet
```

More info: [MDN Reflect](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

### Tail Calls
Calls in tail-position are guaranteed to not grow the stack unboundedly.  Makes recursive algorithms safe in the face of unbounded inputs.

```JavaScript
function factorial(n, acc = 1) {
    'use strict';
    if (n <= 1) return acc;
    return factorial(n - 1, n * acc);
}

// Stack overflow in most implementations today,
// but safe on arbitrary inputs in ES6
factorial(100000)
```
