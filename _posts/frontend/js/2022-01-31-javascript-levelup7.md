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

# JS - Copy

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

이번시간에는 복사(Copy)에 대해 알아보도록 하겠습니다.

1. table of contents
{:toc .large-only}
---

## 복사(Copy)

코드를 보며 이해해봅시다.

```javascript
const user = {
  name: "Changuk",
  age: 26,
  emails: ["dnr8874@naver.com"],
};

const copyUser = user;
console.log(copyUser === user);

user.age = 22;
console.log("user", user);
console.log("copyUser", copyUser);

console.log("------");
console.log("------");

// 출력 결과 :
// true
// user {name: 'Changuk', age: 22, emails: Array(1)}
// copyUser {name: 'Changuk', age: 22, emails: Array(1)}
// ------
// ------
```

`copyUser`에 user 객체를 할당해주었습니다. copyUser는 user와 같은 메모리를 가리키겠군요. 이전 포스트에서 다룬 내용이니 다들 아실거라 생각하고 넘어가겠습니다.

|       1번 메모리       | 2번 메모리 | 3번 메모리 | 4번 메모리 |
| :--------------------: | :--------: | :--------: | :--------: |
| user, copyUser = {~~~} |     {}     |     {}     |     {}     |

그러고 user객체의 age를 22로 할당했습니다. 결과를 한번 볼까요?
copy, copyUser객체 다 같은 값을 나타내고 있네요. 같은 주소를 가리키고 있기 때문입니다. 이를 방지하기 위해 **얕은 복사(shallow copy)**를 사용할 수 있습니다.

```javascript
const user = {
  name: "Changuk",
  age: 26,
  emails: ["dnr8874@naver.com"],
};

const copyUser = Object.assign({}, user);
console.log(copyUser === user);

user.age = 22;
console.log("user", user);
console.log("copyUser", copyUser);

console.log("------");
console.log("------");

// 출력 결과 :
// true
// user {name: 'Changuk', age: 22, emails: Array(1)}
// copyUser {name: 'Changuk', age: 26, emails: Array(1)}
// ------
// ------
```

이제 copyUser는 user의 값 변화에 영향을 받지 않습니다.
개략적인 메모리의 상황도 한번 보도록 합시다. 의도한게 아니라면, 참조형 데이터는 복사개념을 사용하여야 합니다.

|  1번 메모리  |    2번 메모리    | 3번 메모리 | 4번 메모리 |
| :----------: | :--------------: | :--------: | :--------: |
| user = {~~~} | copyUser = {~~~} |     {}     |     {}     |

전개 연산자를 통해서도 간단하게 복사를 할 수 있습니다. 아래 결과를 보시죠.

```javascript
const user = {
  name: "Changuk",
  age: 26,
  emails: ["dnr8874@naver.com"],
};

const copyUser = { ...user };
console.log(copyUser === user);

user.age = 22;
console.log("user", user);
console.log("copyUser", copyUser);

console.log("------");
console.log("------");

// 출력 결과 :
// false
// user {name: 'Changuk', age: 22, emails: Array(1)}
// copyUser {name: 'Changuk', age: 26, emails: Array(1)}
// ------
// ------
```

그럼 얕은 복사와 깊은 복사의 차이가 무엇일까요? 한번 살펴봅시다.

```javascript
user.emails.push("hello@gmail.com");
console.log(user.emails === copyUser.emails);
console.log("user", user.emails);
console.log("copyUser", copyUser.emails);

// 출력 결과
// true
// user (2) ['dnr8874@naver.com', 'hello@gmail.com']
// copyUser (2) ['dnr8874@naver.com', 'hello@gmail.com']
```

push 메서드를 활용해 email 속성에 이메일 데이터를 입력였습니다. 일치연산자를 활용하여 비교를 했는데 **true**가 출력되었군요. 분명 복사를 해서 서로 다른 메모리를 가리키고 있을 텐데, 왜 true가 출력되었을까요.<br>
user의 emails 데이터는 배열 데이터이고, 배열 데이터도 참조형 데이터입니다. 우리는 user의 emails 데이터를 따로 복사처리를 한 적은 없었습니다. 따라서 현재 emails는 같은 메모리 주소를 가리키고 있다고 생각하시면 되겠습니다. 이 때 필요한 개념이 바로 **깊은 복사(deep copy)** 입니다. 하지만 이 깊은 복사는 JavaScript로 구현하기 힘든 부분이 있어, <a href="https://lodash.com/docs/4.17.15#cloneDeep" target="_blank">lodash패키지</a>의 도움을 받아 구현해보도록 하겠습니다.

```javascript
import _ from "lodash";

const user = {
  name: "Changuk",
  age: 26,
  emails: ["dnr8874@naver.com"],
};

const copyUser = _.cloneDeep(user);
console.log(copyUser === user);

user.age = 22;
console.log("user", user);
console.log("copyUser", copyUser);

console.log("------");
console.log("------");

user.emails.push("hello@gmail.com");
console.log(user.emails === copyUser.emails);
console.log("user", user.emails);
console.log("copyUser", copyUser.emails);

// 출력 결과 :
// false
// user {name: 'Changuk', age: 22, emails: Array(1)}
// copyUser {name: 'Changuk', age: 26, emails: Array(1)}
// ------
// ------
// false
// user (2) ['dnr8874@naver.com', 'hello@gmail.com']
// copyUser ['dnr8874@naver.com']
```

이제 email의 주소를 공유하고 있지 않습니다. 깊은 복사가 잘 작동하는군요.
