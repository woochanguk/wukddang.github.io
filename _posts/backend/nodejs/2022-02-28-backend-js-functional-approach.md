---
layout: post
image: /assets/img/nodejs/NodeJS.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  Functioal Approach로 JavaScript를 살펴보겠습니다.
invert_sidebar: false
categories:
  - backend
  - nodejs
---

# JavaScript Basic - Functional Approach

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `backend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. table of contents
{:toc .large-only}
---

이번시간에는 **JavaScript**에 대한 **Functional Apporach**를 알아보겠습니다.

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

## Functional Apporach
이번시간에는 **ES2015**이후로 등장한 **JavaScript Style**에 대해 알아보도록 하겠습니다. <br>

보통 **JavaScript**에서 함수는 객체로 취급될 수 있어서 함수 자체를 인자로 넣거나 함수가 잘 활용될 수 있는 언어이기 때문에 **JavaScript**를 일종의 함수형 언어로 보는 시각이 많습니다. 문제를 풀면서 이해를 해봅시다.

### Ex.1
다음과 같은 데이터가 있다고 가정하겠습니다.

```js
// @ts-check
/* eslint-disable no-restricted-syntax */

/**
 * typeof Person
 *
 * @property {number} age
 * @property {string} city
 * @property {string | string[]} [pet]
 *
 *
 *
 * */

/** @type {Person[]} */
const people = [
  {
    age: 20,
    city: "서울",
    pet: ["cat", "dog"],
  },
  {
    age: 40,
    city: "부산",
  },
  {
    age: 31,
    city: "대구",
    pet: ["cat", "dog"],
  },
  {
    age: 36,
    city: "서울",
  },
  {
    age: 27,
    city: "부산",
    pet: "cat",
  },
  {
    age: 24,
    city: "서울",
    pet: "dog",
  },
];
```
이 중, <br>

1. **30대 미만이 한 명이라도 사는 모든 도시**
2. **각 도시별로 개와 고양이를 키우는 사람의 수
를 구해보세요.**<br>

각 문제를 첫 번째는 일반적인 방식, 두 번째는 함수적 접근방식으로 풀어보도록 하겠습니다.
#### Solve 1-1.
```js
function solveA() {
  /** @type {string[] */

  const cities = [];

  for (const person of people) {
    if (person.age < 30) {
      if (!cities.find((city) => person.city === city)) {
        cities.push(person.city);
      }
    }
  }

  return cities;
}

console.log("solveA", solveA());
```

#### Solve 1-2.
```js
function solveAModern() {
  const allCities = people
    .filter((person) => person.age < 30)
    .map((person) => person.city);

  const set = new Set(allCities);
  return Array.from(set);
}

console.log("solveAModern", solveAModern());
```
- `filter()`
  - 30세 미만의 **person**만 가지도록 **filter**로 추려냈습니다.
- `map()`
  - 추려낸 **person**의 **city**만을 새로운 배열로 반환했습니다.
- `Set()`
  - 중복을 제거하기 위해 **allCities**를 집합으로 정의해주었습니다
- `Array.from()`
  - 집합으로 정의된 **set**을 다시 배열로 반환해주었습니다.

---
위와 같은 단순한 문제에서는 두 방법이 크게 차이가 나지 않는다고 생각할 수 있지만, 첫 번째 작성한 방법은 **문제가 복잡해지면 복잡해질수록 데이터의 변이를 예측하기가 힘들지만** 함수적 접근방법은 **데이터의 변이에 더 강하다**고 할 수 있습니다. <br>

**if**, **for**와 같은 구문을 사용하지 않고 **method**만을 가지고 기능을 잘 구현해줄 수 있습니다. 그리고 **if**, **for**구문보단 명료하게 의미를 이해할 수 있다는 장점도 있습니다. 두 번째 방법은 다음과 같이 객체 구조분할을 적용해서 사용할 수도 있습니다.

#### Solve 1-2(1).
```js
function solveAModern() {
  const allCities = people
    .filter(({ age }) => age < 30)
    .map(({ city }) => city);

  const set = new Set(allCities);
  return Array.from(set);
}

console.log("solveAModern", solveAModern());
```