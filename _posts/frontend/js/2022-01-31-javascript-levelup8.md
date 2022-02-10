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

# JS - Data

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

<a href="https://ko.wikipedia.org/wiki/JSON" target="_blank"></a>JSON(JavaScript Object Notation)데이터와 Storage에 대해 알아보도록 하겠습니다.


1. table of contents
{:toc .large-only}
---

## JSON

JSON은 JavaScript의 객체 표기법입니다. JSON은 "속성-값 쌍" 또는 "키-값 쌍"으로 이루어진 데이터 오브젝트를 전달하기 위해 인간이 읽을 수 있는 텍스트를 사용하는 개방형 표준 포맷입니다. 비동기 브라우저/서버 통신 (AJAX)을 위해, 넓게는 XML(AJAX가 사용)을 대체하는 주요 데이터 포맷이라고 합니다. 특히, 인터넷에서 자료를 주고 받을 때 그 자료를 표현하는 방법으로 알려져 있습니다. 자료의 종류에 큰 제한은 없으며, 특히 컴퓨터 프로그램의 변수값을 표현하는 데 적합하다고 합니다. JSON의 공식 인터넷 미디어 타입은 `application/json`이며, JSON의 파일 확장자는 `.json`입니다..

<br>
JSON에서 사용하는 기본 자료형은 다음과 같습니다.

---

- 수(Number)
- 문자열(String): 0개 이상의 유니코드 문자들의 연속. 문자열은 큰 따옴표(")로 구분하며 역슬래시 이스케이프 문법을 지원.
- 참/거짓(Boolean): true 또는 false 값
- 배열(Array): 0 이상의 임의의 종류의 값으로 이루어진 순서가 있는 리스트. 대괄호로 나타내며 요소는 쉼표로 구분.
- 객체(Object): 순서가 없는 이름/값 쌍의 집합으로, 이름(키)이 문자열.
- null: 빈 값으로, null을 사용

일반적으로 JavaScript에서 객체를 표현할 때 속성에 `""`를 붙이지는 않습니다. 물론 있어도 잘 작동은 합니다. 하지만 JSON으로 표현할 때엔 속성을 `""`로 묶어줘야 합니다. 그럼 임의로 JSON파일을 만들어서 한번 불러와보도록 하겠습니다.

```json
{
  "string": "CHANGUK",
  "number": 123,
  "boolean": true,
  "null": null,
  "object": {},
  "array": []
}
```

```javascript
import myData from "./myData.json";

console.log(myData);

const user = {
  name: "Changuk",
  age: 26,
  emails: ["dnr8874@naver.com", "hello@gmail.com"],
};
console.log("user", user);

const str = JSON.stringify(user);
console.log("str", str);
console.log(typeof str);

const obj = JSON.parse(str);
console.log("obj", obj);

// 출력 결과 :
// {string: 'CHANGUK', number: 123, boolean: true, null: null, object: {…}, …}
// array: []
// boolean: true
// null: null
// number: 123
// object: {}
// string: "CHANGUK"
// [[Prototype]]: Object
// user
// {name: 'Changuk', age: 26, emails: Array(2)}
// str {"name":"Changuk","age":26,"emails":["dnr8874@naver.com","hello@gmail.com"]}
// string
// obj {name: 'Changuk', age: 26, emails: Array(2)}
```

myData.json파일이 잘 출력되는 것을 확인할 수 있습니다. 여기서 하나 알고 가야할 게 있습니다. **JSON파일은 사실 하나의 문자 데이터**입니다. 따라서 JavaScript 파일 내부의 특정 데이터를 JSON의 형태로 문자 데이터화 시켜주는 메서드가 바로 `stringify`라는 메서드 입니다. 예제처럼 꼭 객체 데이터가 아니라도 JavaScript내에서 쓸 수 있는 모든 데이터를 인수로 사용할 수 있습니다. 또 `parse`라는 메서드를 사용하여 JSON형태로 된 데이터를 JavaScript에서 사용할 수 있는 데이터로 재조립할 수 있습니다. <br>
JSON파일은 사용 용도가 명확하기 때문에, 최대한 경량화시켜야 하는 구조이고 단순히 하나의 메모리만 참조할 수 있도록 큰 덩어리의 문자데이터로 관리가 됩니다. 그걸 JavaScript 내에서 사용하려면 `parse`메서드를 사용하면 되고 다시 문자데이터 화(JSON화) 하려면 `stringify`메서드를 사용하면 되는 것이죠.

---

## Storage

<a href="https://developer.mozilla.org/ko/docs/Web/API/Window/localStorage">`MDN문서`</a>를 참조했습니다.

크롬 브라우저의 개발자 도구에서 Application으로 들어가면 Storage 탭이 나옵니다. 그 중 Local Storage를 클릭하면 Key, Value로 어떠한 데이터를 저장할 수 있는 Storage를 제공합니다.
또 Session Storage도 있습니다. 저장한 데이터는 브라우저 세션 간에 공유됩니다. localStorage는 sessionStorage와 비슷하지만, localStorage의 데이터는 만료되지 않고 sessionStorage의 데이터는 페이지 세션이 끝날 때, 즉 페이지를 닫을 때 사라지는 점이 다릅니다. 그럼 예제를 가지고 이해해봅시다.

```javascript
const user = {
  name: "Changuk",
  age: 26,
  emails: ["dnr8874@naver.com", "hello@gmail.com"],
};

localStorage.setItem("user", user);
```

브라우저의 Local Storage를 확인해보면, Key, Value값이 잘 들어가긴 했는데, 뭔가 우리가 원하던 모습은 아닌것 같습니다. Value값에 실제 객체 데이터가 들어가지 않고 [object Object]라는 값이 들어갔거든요. **Storage에 데이터를 저장할 때에는 문자데이터로 변환해서 저장**을 해주어야 합니다. 그러니 이전에 배웠던 메서드를 하나 사용하시죠.

```javascript
const user = {
  name: "Changuk",
  age: 26,
  emails: ["dnr8874@naver.com", "hello@gmail.com"],
};

localStorage.setItem("user", JSON.stringify(user));
```

이제 우리가 원하는 형태의 데이터가 Value값으로 잘 들어간 것을 확인할 수 있습니다. 이제 저장한 데이터를 다시 가져와서 콘솔에 출력해봅시다. Storage의 Value값을 가져오면 JSON형태인 문자데이터이므로, 배웠던 또 다른 메서드를 사용해줍시다.

```javascript
import myData from "./myData.json";

console.log(myData);
const user = {
  name: "Changuk",
  age: 26,
  emails: ["dnr8874@naver.com", "hello@gmail.com"],
};

localStorage.setItem("user", JSON.stringify(user));
console.log(JSON.parse(localStorage.getItem("user")));

// 출력 결과 :
// {name: 'Changuk', age: 26, emails: Array(2)}
```

이제 removeItem으로 Storage안의 데이터를 삭제해주도록 합시다. Application의 Local Storage탭을 확인해보면 아무것도 없는것을 확인할 수 있습니다.

```javascript
import myData from "./myData.json";

console.log(myData);
const user = {
  name: "Changuk",
  age: 26,
  emails: ["dnr8874@naver.com", "hello@gmail.com"],
};

localStorage.setItem("user", JSON.stringify(user));
console.log(JSON.parse(localStorage.getItem("user")));
localStorage.removeItem("user");
// 출력 결과 :
// {name: 'Changuk', age: 26, emails: Array(2)}
```

만약 Storage내의 데이터를 수정하고 싶다면 어떻게 하면 될까요?
일단 데이터를 가져와야 합니다. 그리고 JSON데이터를 사용할 수 있도록 `parse`메서드를 사용해주고, `obj.age`를 사용하여 데이터를 수정해줍니다. 그 후 다시 localStorage에 저장하기 위해 `SetItem` 메서드를 사용하여 저장하면 됩니다.

```javascript
const user = {
  name: "Changuk",
  age: 26,
  emails: ["dnr8874@naver.com", "hello@gmail.com"],
};

const str = localStorage.getItem("user");
const obj = JSON.parse(str);
console.log(obj);
obj.age = 22;
localStorage.setItem("user", JSON.stringify(obj));
```
