---
layout: post
image: /assets/img/nodejs/NodeJS.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  NodeJS의 호이스팅, 스코프, 클로져, 함수에 대해 알아봅시다.
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

이번시간에는 자주 들어봤지만, 매번 헷갈리는 개념들인 `Scope, Hoisting, Closure, Function` 등에 대해서 짚고 넘어가고자 합니다.
JavaScript의 경우 변수간의 참조관계가 굉장히 복잡해지기 쉬우므로, 위의 개념들을 정확하게 숙지하는 것이 필요합니다.

---

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

JavaScript에서 아주 많이 듣게 될 **Closure**에 대해 알아보겠습니다. Closure는 **funcion** **+** function을 가리키는 **pointer**와 function이 참조하는 **여러가지 변수들의 합** **(=environment)**으로 표현됩니다.차근차근 알아봅시다.<br>
**Closure는 function이 하나 생길때 마다 하나씩 생깁니다.** 정확히 말씀드리자면 **enviroment는 함수 자신을 둘러싼, 접근할 수 있는 모든 스코프**를 뜻합니다.
<br>

---

**Closure**의 예시부터 살펴봅시다.

```javascript
function add(x) {
  return function print(y) {
    return x + " and " + y;
  };
}

const saltAnd = and("salt");
console.log(saltAnd("pepper"));
console.log(saltAnd("sugar"));
```
