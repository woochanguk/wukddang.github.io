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

# JS - Destructuring Assignment

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

구조 분해 할당(Destructuring Assignment)에 대해 알아보겠습니다.

1. table of contents
{:toc .large-only}
---

## 구조 분해 할당(비구조화 할당, Destructuring Assignment)

바로 예제를 보면서 확인하도록 하겠습니다.

```javascript
const user = {
  name: "Changuk",
  age: 26,
  email: "dnr8874@naver.com",
};

const { name, age, email, address } = user;

console.log(`사용자의 이름은 ${name}입니다.`);
console.log(`${name}의 나이는 ${age}세입니다.`);
console.log(`${name}의 이메일 주소는 ${email}입니다.`);
console.log(address);

// 출력 결과 :
// 사용자의 이름은 Changuk입니다.
// Changuk의 나이는 26세입니다.
// Changuk의 이메일 주소는 dnr8874@naver.com입니다.
// undefined
```

다음과 같이, 구조 분해 할당이라는 것은, **user 객체 데이터의 내용을 구조 분해해서 원하는 내용만 꺼내서 사용하겠다**는 의미입니다. 또 구조 분해 할당에서는 추가적인 기능을 사용할 수 있습니다. user데이터에 address속성이 있다면 그 값을 사용하고, 값이 undefined라면 address값에 기본값을 지정해줄 수 있습니다.

```javascript
const { name, age, email, address = "Korea" } = user;
```

또 구조 분해 할당한 변수를 그대로 사용하고 싶지 않다면 다음과 같이 변수를 지정해줄 수도 있습니다.

```javascript
const user = {
  name: "Changuk",
  age: 26,
  email: "dnr8874@naver.com",
  address: "USA",
};

const { name: changuk, age, email, address } = user;

console.log(`사용자의 이름은 ${changuk}입니다.`);
console.log(`${changuk}의 나이는 ${age}세입니다.`);
console.log(`${changuk}의 이메일 주소는 ${email}입니다.`);
console.log(address);

// 출력 결과 :
// 사용자의 이름은 Changuk입니다.
// Changuk의 나이는 26세입니다.
// Changuk의 이메일 주소는 dnr8874@naver.com입니다.
// USA
```

객체 뿐만 아니라 배열도 구조 분해 할당을 사용해줄 수 있습니다. 아래 예제를 봅시다. 배열이기 때문에, `[]`를 사용해주어야 합니다.

```javascript
const fruits = ["Apple", "Banana", "Cherry"];
const [a, b, c, d] = fruits;
console.log(a, b, c, d);

// 출력 결과 :
// Apple Banana Cherry undefined
```

만약 구조 분해 할당을 통해 **"Banana"**만 추출하고 싶다면?? 다음과 같이 해주면 됩니다.

```javascript
const fruits = ["Apple", "Banana", "Cherry"];
const [, b] = fruits;
console.log(b);

// 출력 결과 :
// Banana
```

```javascript
const fruits = ["Apple", "Banana", "Cherry"];
const [, , c] = fruits;
console.log(c);

// 출력 결과 :
// Cherry
```
