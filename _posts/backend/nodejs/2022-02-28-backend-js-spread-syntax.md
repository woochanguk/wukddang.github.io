---
layout: post
image: /assets/img/nodejs/NodeJS.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  JavaScript의 Spread Syntax에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - backend
  - nodejs
---

# JavaScript Basic - Spread Syntax

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `backend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. table of contents
{:toc .large-only}
---

이번시간에는 **JavaScript**의 **Spread Syntax** 문법에 대해 알아보겠습니다.

---
## Code tester 
코드 예제는 여기서 확인해주세요.
<iframe src="https://codesandbox.io/embed/jovial-hermann-yk8xfq?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="blog-js"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>
---

## Spread syntax(...)
**Spread syntax**는 **ES2015**에서 새로 추가된 문법입니다. 병합, 구조 분해 할당(**destructuring**) 등에 다양하게 활용할 수 있습니다. **spread**의 뜻이 흩뿌린다라는 의미인 것을 생각하면 이해가 쉬워질 것입니다. 아래 예제를 살펴보죠. 결과가 어떻게 될까요?

### Ex.1
```js
const personalData = {
  nickname: 'Changuk',
  email: 'dnr8874@gmail.com',
}

const publicData = {
  age: 25,
}

const user = {
  ...personalData,
  ...publicData,
}
```
**user**는 **nickname**, **email**, **age**를 가지는 새로운 객체가 될 것입니다. 흩뿌려서 객체가 전달이 된다는 의미로 이해하면 될 것 같습니다.

### Ex.2
다른 예제를 한번 더 살펴보죠.

```js
const overrides = {
  DATABASE_HOST: 'myhost.com',
  DATABASE_PASSWORD: 'mypassword',
}

const config = {
  DATABASE_HOST: 'default.host.com',
  DATABASE_PASSWORD: '****',
  DATABASE_USERNAME: 'myuser',
  ...overrides,
}
/*
{
  DATABASE_HOST: 'myhost.com',
  DATABASE_PASSWORD: 'mypassword',
  DATABASE_USERNAME: 'myuser',
}
*/ 
```
위와 같이 코드를 작성하면, 기존에 작성된 데이터를 덮어써줄 수 있습니다.

### Ex.3
```js
const user = {
  nickname: 'Changuk',
  age: 25,
  email: 'dnr8874@gmail.com',
}

const { nickname, ...personalData } = user
console.log(personalData) 
// { age: 22, email: 'dnr8874@gmail.com'}
```
**nickname** 이외의 데이터를 묶어서 사용하고 싶으면 다음과 같이 객체 구조분할과 **Spread Syntax**를 사용해주면 됩니다.

### Ex.4 
객체뿐 아니라, 배열에도 적용이 가능합니다. 그리고 배열을 분할해서 나눠줄 수도 있습니다.

```js
const pets = ['dog', 'cat']
const predators = ['wolf', 'cougar']
const animals = [...pets, ...predators]
console.log(animals) 
// ['dog', 'cat', 'wolf', 'cougar']
```

```js
const [head, ...rest] = [1, 2, 3]
console.log(head) // 1
console.log(rest) // [2, 3]
```

실전에선 다음과 같이 사용될 수 있을 것입니다.

```js
const shouldOverride = false

const user = {
  ...{
    email: 'abc@def.com',
    password: '****",
  },
  ...{
    nickname: 'foo',
  },
  ...(shouldOverride
    ? {
      email: 'fff@fff.com'
      }
    : null),
}
console.log(user)
```
**shouldOverride**가 **true**일 경우에만 작동하는 코드를 작성해줄 수 있습니다.<br>

함수에도 동일하게 작성해줄 수 있습니다.

```js
function foo(head, ...rest) {
  console.log(head)
  console.log(rest)
}

foo(1, 2, 3, 4)
```
1만 **head**에 할당되고, 나머지는 배열로 **rest**에 전달되는 것을 볼 수 있습니다.