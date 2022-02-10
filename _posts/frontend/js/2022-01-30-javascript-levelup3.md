---
layout: post
image: /assets/img/js/javascript.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  안녕하세요?!
invert_sidebar: false
categories:
  - frontend
  - js
---

# JS - Object

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

객체를 다루는 메서드들에 대해 알아보겠습니다. <a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object" target="_blank">`MDN문서`</a>를 참조했습니다.

1. table of contents
{:toc .large-only}
---

prototype이 붙어있지 않은 메서드 들은, **정적(Static) 메서드**라고 불립니다. 따라서 이러한 정적 메서드들은 객체(Object)에 다음과 같이 직접적으로 사용할 수가 없습니다.
그러면 객체의 정적 메서드들은 어떻게 활용해야하는 것인지 알아보도록 합시다.

```javascript
{}.assign()
```

## assign

Object.assign() 메서드는 **출처(Sources) 객체들의 모든 열거 가능한 자체 속성을 복사해 대상(Target) 객체에 붙여넣습니다. 그 후 대상 객체를 반환합니다.** 원본이 변형됩니다. 내용을 이해하기가 좀 난해하네요. 예제를 한번 보겠습니다.

```javascript
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```

assign() 메서드는 하나 이상의 출처 객체들로부터 자체 속성을 복사해 대상 객체에 붙여넣는 메서드입니다. 다음 예제도 추가로 보도록 합시다.

```javascript
const userAge = {
  // key: value
  name: "Changuk",
  age: 26,
};
const userEmail = {
  name: "Changuk",
  email: "dnr8874@naver.com",
};

const target = Object.assign(userAge, userEmail);
console.log(target);
console.log(userAge);
console.log(target === userAge);

const a = { k: 123 };
const b = { k: 123 };
console.log(a === b);

// 출력 결과 :
// {name: 'Changuk', age: 26, email: 'dnr8874@naver.com'}
// {name: 'Changuk', age: 26, email: 'dnr8874@naver.com'}
// true
// false
```

코드의 맨 마지막을 보면 a, b는 서로 같은 객체처럼 보이지만 결과는 거짓이라고 나타나고 있습니다. 이는 JavaScript 데이터의 불변성으로 인한 결과입니다. 이에 대해서는 다른 포스트에서 다루도록 하겠습니다.<br> 간략하게 설명드리자면, 하나의 객체 데이터는 특정 메모리 주소에 값이 들어가 있습니다. 그래서 `userAge`상수는 사실상 메모리에 있는 특정 객체 데이터의 메모리 주소만 가지고 있는 것이라고 할 수 있습니다. 이러한 데이터형을 **참조형 데이터**이라고 합니다. 참조형 데이터로는 **객체, 배열, 함수**가 있습니다.<br>

따라서 a, b는 같은 값을 나타내지만 서로 다른 주소를 가리키고 있기 때문에, 다른 값이라고 출력된다고 할 수 있겠습니다.

---

만약 위 코드에서, userAge, userEmail을 가지고 새로운 객체를 만들고 싶다면 어떻게 해야 할까요?
다음 코드와 같이 실행하면 됩니다.
그럼 **userAge**, **userEmail**은 출처 객체가 되는 것입니다.

```javascript
const target = Object.assign({}, userAge, userEmail);
```

---

## keys

Object.keys() 메소드는 **주어진 객체의 속성 이름들을 일반적인 반복문과 동일한 순서로 순회되는 열거할 수 있는 배열로 반환**합니다.

```javascript
const user = {
  name: "Changuk",
  age: 26,
  email: "dnr8874@naver.com",
};

const keys = Object.keys(user);
console.log(keys);

console.log(user["email"]);

const values = keys.map((key) => user[key]);
console.log(values);

// 출력 결과 :
// (3) ['name', 'age', 'email']
// dnr8874@naver.com
// (3) ['Changuk', 26, 'dnr8874@naver.com']
```
