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

# JS - Spread

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

전개 연산자(Spread)에 대해 알아보도록 하겠습니다.

1. table of contents
{:toc .large-only}
---

## 전개 연산자 (Spread)

예제를 한번 보겠습니다.

```javascript
const fruits = ["Apple", "Banana", "Cherry"];
console.log(fruits);
console.log(...fruits);
// console.log("Apple", "Banana", "Cherry");

function toObject(a, b, c) {
  return {
    a: a,
    b: b,
    c: c,
  };
}

console.log(toObject(...fruits));

// 출력 결과 :
// (3) ['Apple', 'Banana', 'Cherry']
// Apple Banana Cherry
// {a: 'Apple', b: 'Banana', c: 'Cherry'}
```

위 코드에서 볼 수 있듯, fruits라는 배열 앞에 ...인 전개 연산자를 사용해주면, 배열의 원소 하나하나를 문자열로 출력해줍니다.<br>
toObject 함수는, a, b, c 인수에 값을 입력받으면 각각 key 값인 a, b, c에 할당되도록 객체 데이터를 반환해주고 있습니다.이전의 예제와 동일하게 전개 연산자를 사용해주면 객체 데이터가 전개되어 출력됩니다.<br>

하나 더 살펴볼까요?
전개 연산자는 다음과 같이 입력 인수에도 사용이 가능합니다.

fruits 배열은 Orange가 포함되어 4개가 되었지만 배열을 받을 toObject함수는 3개의 인수밖에 받을 수가 없네요. 이 때 전개 연산자를 사용해주면 됩니다. "Apple"은 a로, "Banana"는 b로 들어가게 되고 나머지 "Cherry"와 "Orange"는 c로 들어가게 됩니다.
이때, c가 나머지 모든 인수들을 다 받기 때문에 c를 **나머지 매개변수(rest parameter)**라고 부릅니다. 출력 결과를 한번 보죠

```javascript
const fruits = ["Apple", "Banana", "Cherry", "Orange"];
console.log(fruits);
console.log(...fruits);
// console.log("Apple", "Banana", "Cherry");

function toObject(a, b, ...c) {
  return {
    a: a,
    b: b,
    c: c,
  };
}

console.log(toObject(...fruits));

// 출력 결과 :
// {a: 'Apple', b: 'Banana', c: Array(2)}
// a: "Apple"
// b: "Banana"
// c: (2) ['Cherry', 'Orange']
// [[Prototype]]: Object
```

나머지 매개변수는 남은 인수들을 모두 받아 배열이 된 것을 확인할 수 있습니다. 또 한가지 알고 넘어가면 좋은게 있습니다. 객체 속성의 이름과 변수의 이름이 같다면, 축약해서 사용할 수 있습니다.

```javascript
function toObject(a, b, ...c) {
  return {
    a,
    b,
    c,
  };
}
```

또 화살표 함수로도 표현하여 축약한 코드로 만들어보겠습니다.

```javascript
const fruits = ["Apple", "Banana", "Cherry", "Orange"];
console.log(fruits);
console.log(...fruits);
// console.log("Apple", "Banana", "Cherry");

const toObject = (a, b, ...c) => ({ a, b, c });

console.log(toObject(...fruits));
```
