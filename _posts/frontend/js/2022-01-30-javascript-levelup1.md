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

# JS - String

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. table of contents
{:toc .large-only}

---

## 문자(String)

문자열 데이터를 처리하는 메서드(Method)와 속성(Property)들을 알아보겠습니다. <a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String" target="_blank">`MDN문서`</a>를 참조하였습니다.

---

#### indexOf

indexOf() 메서드는 호출한 String 객체에서 주어진 값과 일치하는 첫 번째 인덱스를 반환합니다. 일치하지 않으면 -1을 반환합니다. 아래 예제를 보면서 알아봅시다.

```javascript
const result1 = "Hello world!".indexOf("world");
const result2 = "Hello world!".indexOf("changuk");
console.log(result1);
console.log(result2);
// 출력 결과 :
// 6
// -1
```

---

#### length

length 속성은 UTF-16코드 유닛을 기준으로 문자열의 길이를 나타냅니다.

```javascript
const str = "0123";
console.log(str.length);

// 출력 결과 :
// 4
```

---

#### slice

slice() 메소드는 문자열의 일부를 추출하면서 새로운 문자열을 반환합니다. 두 개의 인수를 입력받는데, 추출 시작점부터 시작하여, 추출 종료점의 직전까지 추출됩니다. 예제를 보며 이해해봅시다.

```javascript
const str = "Hello world!";

console.log(str.slice(0, 3));
console.log(str.slice(6, 11));

// 출력 결과 :
// Hel
// world
```

---

#### replace

replace() 메서드는 어떤 패턴에 일치하는 일부 또는 모든 부분이 교체된 새로운 문자열을 반환합니다.

```javascript
const str = "Hello world!";
console.log(str.replace("world!", "Changuk!"));
console.log(str.replace(" world!", ""));

// 출력 결과 :
// Hello Changuk!
// Hello
```

---

#### match

match() 메서드는 문자열이 정규식과 매치되는 부분을 검색합니다. 아래 정규식 표현 부분은 추후 다루도록 하겠습니다.

```javascript
const str = "thesecond@gmail.com"

console.log(str.match(/.+(?=
@)/)[0]);

// 출력 결과 : 0
// thesecond
```

---

#### trim

trim() 메서드는 문자열 양 끝의 공백을 제거합니다. 공백이란 모든 공백문자(space, tab, NBSP 등)와 모든 개행문자(LF, CR 등)를 의미합니다.

```javascript
const str = "     Hello world  ";

console.log(str.trim());

// 출력 결과 :
// Hello world
```
