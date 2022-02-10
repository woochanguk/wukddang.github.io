---
layout: post
image: /assets/img/js/javascript.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: "#ccc"
theme_color: "#ccc"
description: >
  JavaScript를 Setting해 봅시다.
invert_sidebar: false
categories:
  - frontend
  - js
---

# JS - NodeJS Setting & Function

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

이번시간에는 NodeJS의 기본적인 세팅 및 JavaScript 함수에 대해 알아보겠습니다.

1. table of contents
{:toc .large-only}

---

## 개발 서버 실행과 빌드

일반적인 패키지들은 다음과 같이 표현되어 있습니다.
`Major.Minor.Patch Eg. 14.18.3`
설명드리자면 다음과 같습니다.
Major: 기존 버전과 호환되지 않는 새로운 버전<br>
Minor: 기존 버전과 호환되는 새로운 기능이 추가된 버전<br>
Patch: 기존 버전과 호환되는 버그 및 오타 등이 수정된 버전<br>
^: Major 버전 안에서 가장 최신 버전으로 업데이트 가능 이 기호를 사용하여 npm 명령어를 통한 update를 이용할 수 있습니다. 없으면 update가 실행되지 않고 바로 종료가 됩니다. npm install로 설치했을 때 ^ 기호가 붙어있을 수 있기 때문에 package.json 파일에서 제거해도 상관 없습니다.

---

## 화살표 함수

축약형으로 사용할 수 있습니다. 중괄호 안에 코드가 들어가면 `return`을 사용해 주어야 합니다. 객체를 반환하고 싶다면 소괄호()로 한번 감싸주어야 합니다.

```javascript
const x = 7;
const hello = (x) => x * 2;
// 출력 결과 : 14
```

```javascript
const x = 7;
const hello = (x) => {
  return x * 2;
};

// 출력 결과 : 14
```

```javascript
const hello = (x) => ({
  name: "changuk",
});

// 출력 결과 : {name: "changuk"}
```

## 즉시실행함수 (IIFE: Immediately-Invoked Function Expression)

함수를 만들 때 따로 이름을 짓지 않고 바로 실행할 수 있도록 작성하는 함수입니다. 아래 코드처럼 사용하시면 되겠습니다.

```javascript
const a = 7;
(function () {
  console.log(a * 2);
})();

//출력 결과 : 14
```

---

## 호이스팅(Hoisting)

호이스팅이란 함수 선언부가 유효범위 최상단으로 끌어올려지는 현상이라고 정의할 수 있습니다. 코드를 보며 알아보도록 하죠.

```javascript
const a = 7;

const double = function () {
  console.log(a * 2);
};

double();

// 출력 결과 : TypeError: double is not a function
```

다음과 같이 작성하면 잘 작동하게 됩니다. 그런데 만약 `double()`을 함수 선언보다 먼저 호출하게 되면 어떻게 될까요?? 결론부터 말씀드리면 `TypeError`가 발생하게 됩니다. 아직 함수가 만들어진 상태가 아니기 때문에 함수가 아니라는 에러가 발생하는 것이지요. 기본적으로 `JavaScript`는 위에서부터 순차적으로 코드를 읽어가기 떄문입니다. 하지만 `함수를 선언`하는 코드를 `함수를 표현`하는 코드로 바꾸게 되면 정상적으로 작동하게 됩니다. 아래 코드를 보시죠.

```javascript
const a = 7;

function double() {
  console.log(a * 2);
}

double();

// 출력 결과 : 14
```

호이스팅은 그럼 어떤 상황에 유용한 것일까요?? 함수 안에는 복잡한 내용들이 많이 들어가게 되는것이 일반적이기 때문에 특정 함수를 실행하기 위해서 함수를 항상 위쪽에 선언하는 것은 개발자가 코드를 해석하는 입장에서 불편하게 합니다. 따라서 자바스크립트에서는 호이스팅이란 현상을 사용하여 개발자는 함수의 이름을 보고 개략적인 기능을 유추해볼 수 있고, 자바스크립트는 함수를 실행할 수 있기 때문에 함수를 정의한 부분은 전체 코드의 최하단에 두는 경우도 종종 있습니다.

---

함수 선언을 작성할 때 되도록 호이스팅을 사용하여 함수를 하단에 정의하라는 의미는 아닙니다. 대신 특정 코드들이 먼저 호출되어 있고 선언이 뒤에 되어있는 경우, 코드 해석에 혼동만 하지 않으면 될 것 같습니다. 결국 `함수의 선언`이 호출 부분보다 밑에 작성되어 있어도 호이스팅에 의해 문제없이 작동되게 됩니다.

---

## 타이머 함수

타이머 함수에 대해 간략하게 알아보겠습니다. <br>
`setTimeout(함수, 시간)` : 일정 시간 후 함수 실행<br>
`setInterval(함수, 시간)` : 시간 간격마다 함수 실행<br>
`clearTimeout()` : 설정된 Timeout 함수를 종료<br>
`clearInterval()` : 설정된 Interval 함수를 종료<br>

다음과 같은 함수들이 있습니다. 구글링을 하여 사용해도 상관은 없지만, 한글로 정리해두고자 글을 작성하였습니다.

```javascript
setTimeout(function () {
  console.log("changuk!");
}, 3000);
```

`setTimeout`의 시간 간격은 ms입니다. 따라서 위 코드의 시간 간격은 3초가 되겠네요. 코드실행 후 3초뒤에 입력한 문자열이 출력됩니다. 화살표 함수를 사용해서 작성해줄 수도 있겠습니다.

```javascript
const timer = setTimeout(() => {
  console.log("changuk!");
}, 3000);
```

`clearTimeout`을 사용하여 `setTimeout`으로 정의된 내용을 종료시킬 수 있습니다. `addEventListener`를 사용하여 html의 특정 태그를 클릭하면 `cleartimeout`이 실행되도록 만들어줄 수 있습니다.<br>
`setInterval`함수는, 특정 시간마다 계속해서 함수가 실행될 수 있도록 합니다. 위와 동일하게 `clearIntterval`함수를 사용하면 종료해줄 수 있습니다.

## 콜백(CallBack)

콜백에 대해 알아보도록 하겠습니다. 콜백은, 함수의 인수로 사용되는 함수라고 할 수 있습니다. 이전에 타이머 함수에서 알아보았던 여러 함수들이 있습니다. 그 함수들에 입력되는 함수를 `callback`이라고 부릅니다. 그럼 이 인수로 입력되는 함수가 왜 `callback`이라고 불리게 될까요? 아래 예제를 보며 알아보도록 하겠습니다.

```javascript
function timeout() {
  setTimteout(() => {
    console.log("changuk!");
  }, 3000);
}

timeout();
console.log("Done");

// 출력 결과 :
// Done
// changuk!
```

위에서 볼 수 있듯, `timeout()`함수를 먼저 입력하였는데 뒤에 작성한 `console.log("Done")`이 먼저 출력이 되었습니다. 3초 뒤에 특정 로직이 수행되도록 함수를 작성하였기 때문에 다음 코드가 먼저 실행되었습니다. 그럼 `timeout()`함수가 먼저 출력된 후 그 다음 코드가 실행되도록 하려면 어떻게 해야 할까요? 이때 `callback`이라는 개념이 사용됩니다.

```javascript
function timeout(callback) {
  setTimteout(() => {
    console.log("changuk!");
    callback();
  }, 3000);
}

timeout(() => {
  console.log("Done");
});

// 출력 결과 :
// changuk!
// Done
```

우리는 `callback`이라는 인수를 사용하여 `timeout()`을 실행하는 코드를 작성하였습니다. 아래 익명함수로 작성되어 있는 부분이 함수 선언부의 인수 callback 부분에 들어가서 `changuk!`이 출력된 후에 `Done`이 출력되게 됩니다. 처리에 오래 걸리는 함수가 있다면 callback을 사용하여 그 실행 후에 특정 함수가 작성되도록 만들 수 있습니다.
