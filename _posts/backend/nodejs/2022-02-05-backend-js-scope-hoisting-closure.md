---
layout: post
image: /assets/img/nodejs/NodeJS.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  JavaScript의 Scope, Hoisting, Closure에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - backend
  - nodejs
---

# NodeJS - JavaScript Basic 2

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `backend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. table of contents
{:toc .large-only}
---

이번시간에는 자주 들어봤지만, 매번 헷갈리는 개념들인 `Scope, Hoisting, Closure` 등에 대해서 짚고 넘어가고자 합니다.
JavaScript의 경우 변수간의 참조관계가 굉장히 복잡해지기 쉬우므로, 위의 개념들을 정확하게 숙지하는 것이 필요합니다.

---

## Code tester 
코드 예제는 여기서 확인해주세요.
<iframe src="https://codesandbox.io/embed/jovial-hermann-yk8xfq?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="blog-js"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

## Hoisting - var

먼저 간단한 문제를 풀면서 시작해보도록 합시다.

```javascript
var x = 1;
console.log(x);
```

이러한 코드가 있을 때, 출력 결과는 어떻게 될까요? 당연히 **1**이 됩니다. 그럼 다음 코드는 출력 결과가 어떻게 될까요? 여기서는 **undefined**가 출력됩니다.

```javascript
console.log(x);
var x = 1;
```

이런 결과는 **var라는 키워드의 특성** 때문입니다. var 키워드는 변수를 선언할 때, 그 선언이 이 scope의 맨 위로 **Hoisting**이 됩니다. 간단하게 **변수의 선언만 지금 있는 위치에서 맨 위로 끌어올린다**고 생각하시면 됩니다. <br>
즉, 아래 코드와 동일하겠지요.

```javascript
var x;
console.log(x);
x = 1;
```

이것이 **Hoisting**입니다. 이 경우는 var 키워드를 사용하지 않았기 때문에 변수의 선언 자체가 이루어지지 않으므로 **ReferenceError**가 발생합니다.

```javascript
console.log(x);
x = 1;
```

---

## Hoisting - function

아래의 코드는 각각의 실행결과가 어떻게 될까요?

```javascript
function foo() {
  return "foo";
}
console.log(foo());
```

```javascript
console.log(foo());
function foo() {
  return "foo";
}
```

두 코드의 실행결과는 동일합니다. 이유는, **function도 Hoisting 대상이기 때문**입니다. 함수의 선언이 아래에 있던, 위에 있던 해당 scope의 맨 위로 올라가기 때문입니다. <br>
또한 var 키워드는 선언 후 값의 초기화가 이루어지기 때문에 값의 초기화 이전에 값을 확인해보면 **undefined**가 출력되지만, 함수의 선언은 `function foo() {}` 자체가 하나의 덩어리이기 때문에 위의 두 코드는 출력 결과가 동일하다고 설명드릴 수 있습니다.

---

## function, lexical scope

scope가 무엇인지 확인해보도록 하죠.<br>

<img src="/assets/img/nodejs/scopebinding.png" width="450" height="350"  >

위의 코드 안의 `function bar() {}` 안에서 x라는 변수를 사용하였을 때, 이 변수는 어떤 값을 가리키는지를 어떻게 판단할 수 있을까요? 코드의 어떤 식별자가 실제로 어떤 값을 가리키는지 결정하는 것을 **binding** 이라고 하고 **JavaScript**에서의 **binding**은 **lexical scope**이라는 방법을 사용하게 됩니다. <br>
**lexical scope**이란 간단히 말하자면 안쪽(inner scope)에서는 바깥쪽(outer scope) 변수에 접근할 수 있다는 뜻입니다.<br> 다음 코드를 예로 들 수 있겠습니다.

```javascript
function foo() {
  var x = "Hello";
  console.log(x); // 'Hello'
}

console.log(x); // ReferenceError
```

위의 코드에서 **함수 바깥의 console.log(x)는 함수 안의 x를 참조할 수 없습니다**. 아래의 코드는 잘 작동하겠죠?

```javascript
var x = "Hello";

function foo() {
  console.log(x); // "Hello"
}

console.log(x); // "Hello"
```

## var, block scoping

아래 코드를 확인해보죠. x는 어떤 값을 출력할까요?

```javascript
var x = 1;
if (true) {
  var x = 2;
}
console.log(x); // 2
```

분명 선언을 2번 나누어서 했을 텐데, 왜 x는 2가 나올까요? 이유는 **var가 block을 무시하기 때문**입니다. 따라서 선언이 합쳐지게 되고, 두 개의 x는 같은 값이 됩니다. <br>(var는 위와 같은 규칙을 따르지만, let과 const는 block scoping이 됩니다.)

---

## Closure

**JavaScript**에서 아주 많이 듣게 될 **Closure**에 대해 알아보겠습니다. **Closure**는 **funcion**과 **function**을 가리키는 **pointer**, **function**이 참조하는 **여러가지 변수들의 합** **(=environment)**으로 표현됩니다. 즉 `Closure = function + environment` 인 것이죠. 차근차근 알아봅시다.<br>
**Closure는 function이 하나 생길때 마다 하나씩 생깁니다.** 정확히 말씀드리자면 **enviroment는 함수 자신을 둘러싼, 접근할 수 있는 모든 스코프**를 뜻합니다.
<br>

먼저 **Closure**의 예시를 살펴보며 이해해봅시다.
### Ex.1
```javascript
function add(x) {
  return function print(y) {
    return x + " and " + y;
  };
}

const saltAnd = and("salt");
console.log(saltAnd("pepper")); // salt and pepper
console.log(saltAnd("sugar")); // salt and sugar

const waterAnd = and("water");
console.log(waterAnd('juice'))
```

`and()` 함수로 만들어진 **saltAnd**의 **Closure**는 다음과 같다고 할 수 있습니다.
- **함수**: `print()`
- **환경**: `x => "salt"` (x는 salt로 연결)

`add()`와 같은 함수들을 보통 **higher-order function**이라고 합니다. **higher-order function**이란 **다른 함수를 반환**해주는 **고차원 함수**라는 의미인데, **Closure**는 **higher-order function**을 만드는 데 유용합니다. <br> 

함수는 위와 같이 변수처럼 할당될 수 있고 **Callback**으로 입력되는 둥 여러가지 작업을 할 수 있기 때문에 **Closure**는 **JavaScript**에서 매우 흔하게 볼 수 있는 현상이라고 할 수 있습니다. 추가적으로 아래의 코드를 살펴보겠습니다.

### Ex.2
```javascript
function add(x) {
  return function print(y) {
    return x + " and " + y;
  };
}

const saltAnd = and("salt");
console.log(saltAnd("pepper")); // salt and pepper
console.log(saltAnd("sugar")); // salt and sugar

const waterAnd = and("water"); 
console.log(waterAnd('juice')); // water and juice
```
- **saltAnd** : `x => "salt"`
- **waterAnd** : `x => "water"`
로 바인딩 되어 있습니다. 즉, **saltAnd**, **waterAnd**는 서로 다른 **Closure**를 형성하고 있다고 할 수 있습니다. 이유는 **saltAnd**, **waterAnd** 모두 함수는 같은 **print**이지만, 주어진 변수가 다르기 때문입니다.

추가적으로 예시를 살펴보죠
### Ex.3
아래 코드의 실행과정에서 **Closure**는 총 몇 개가 생성되었을까요?
```js
function foo() {
  function bar() {

  }
  function baz() {

  }
}
foo()
```
정답은 3개입니다. (foo: 1개, bar: 1개, baz: 1개)

### Ex.4
아래 코드의 실행과정에서 **Closure**는 총 몇 개가 생성되었을까요?
```js
function foo() {
  function bar() {

  }
  function baz() {

  }
}
foo()
foo()
```
`bar()`, `baz()`는 이미 선언된 함수처럼 보이지만 `foo()`가 실행되는 시점에서 선언된 함수이기 때문에 다른 함수로 볼 수 있습니다. 따라서 **Closure**는 총 5개라고 할 수 있습니다. (foo: 1개, bar: 2개, baz: 2개)

### Ex.5
아래 코드가 실행되면 어떤 결과가 출력될까요?
```js
function getCounter() {
  var result = {
    count: count,
    total: 0
  }
  function count() {
    result.total += 1
  }
  return result
}
var counter = getCounter()
counter.count()
counter.count()

console.log(counter.total)
```
`getCounter()` 함수가 실행되면, **result** 객체가 생성되는데 `getCounter()` 내부의 `count()` 함수가 생성됨과 동시에 **Closure**가 생성됩니다. 그리고 `count()` 함수를 두번 실행해주면 **result** 객체의 **total** 값이 1씩 증가하여 출력결과는 2가 되겠죠

### Ex.6
아래 코드의 결과는 쉽게 예측할 수 있겠지만, **Closure**의 관점에서 생각해봅시다.
```js
function getCounter() {
  var result = { count: count, total }
  function count() {
    result.total += 1
  }
  return result
}
var counterA = getCounter()
counterA.count()
counterA.count()

var counterB = getCounter() 
counterB.count()

console.log(counterA.total, counterB.total)
```
변수 **counterA**의 **result** 객체가 있을 것이고, 변수 **counterB**에 해당하는 **result** 객체가 있을 것입니다. 두 객체는 별개의 객체이고, 내부에 `count()` 함수가 존재합니다. `count()`가 실행될 때 마다 객체에 속해있는 **result**값이 변경되겠지요. 즉 정리하면 아래와 같을 것입니다.

- **counterA** : 첫 **getCounter** 실행 때 만들어진 **total**과 **count**로 이루어진 객체
- **counterB** : 두 번째 **getCounter** 실행 때 만들어진 **total**과 **count**로 이루어진 객체

**getCounter**의 **Closure**가 서로 다르기 때문에 생기는 현상이라고 할 수 있겠습니다.

### Ex.7
다음 코드의 결과는 어떻게 될까요?
```js
var numCounters = 0

function getCounter() {
  numCounters += 1
  
  var result = {
    count: count,
    total: 0
  }
  function count() {
    result.total += 1
  }
  return result
}
var counterA = getCounter()
counterA.count()
counterA.count()

var counterB = getCounter()
counterB.count()

console.log(counterA.total, counterB.total, numCounters)
```
`getCounter()` 함수가 선언될 때 마다 **Closure**가 하나씩 생성되는데, 이 **Closure**는 **numCounters**와 바인딩 되어 1을 증가시키면서 생성됩니다.

## Vscode debug

**Vscode**에서도 한번 확인해보겠습니다. 실행하기에 앞서 **.vscode**폴더 내부에 **launch.json**파일을 생성해주겠습니다. 다음과 같이 작성해주면, **Debug**를 실행해줄 수 있습니다.

```json
{
  "configurations": [
    {
      "name": "Launch via NPM",
      "request": "launch",
      "runtimeArgs": ["run-script", "debug"],
      "runtimeExecutable": "npm",
      "skipFiles": ["<node_internals>/**"],
      "type": "pwa-node"
    }
  ]
}
```
그리고 package.json파일을 다음과 같이 수정하였습니다.
```json
{
  ...
  "scripts": {
    "debug": "node src/main.js"
  },
  ...
}
```
그 후에 **debug** 버튼을 클릭하고 초록색 삼각형을 클릭해주면 **debug**모드가 실행됩니다. 어떻게 동작하는지 궁금한 지점에 **breakpoint**를 걸어주면 이해하는데 큰 도움이 될 것입니다. 한번 실행해보면, **Closure**가 어떻게 생성되는지를 잘 이해할 수 있을 것입니다. 또 앞서 배웠던 Call Stack에 대해서도 깊게 이해할 수 있을 것입니다. 시도해보시죠.