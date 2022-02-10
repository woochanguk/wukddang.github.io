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

# JS - RegExp

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

JavaScript 정규표현식에 대해 알아보도록 하겠습니다. <a href="https://heropy.blog/2018/10/28/regexp/" target="_blank">블로그</a> 내용을 참조했습니다.

1. table of contents
{:toc .large-only}
---

## JavaScript 정규표현식(RegExp)

정규표현식이란 문자열을 검색하고 대체하는 데 사용 가능한 일종의 형식 언어(패턴)입니다. 간단한 문자 검색부터 이메일, 패스워드 검사 등의 복잡한 문자 일치 기능 등을 정규식 패턴으로 빠르게 수행할 수 있습니다.

#### 역할

- 문자 검색 (Search)
- 문자 대체 (Replace)
- 문자 추출 (Extract)

#### 테스트 사이트

- [https://regex101.com/](https://regex101.com/)
- [https://regexr.com/](https://regexr.com/)
- [https://regexper.com/](https://regexper.com/)

#### 정규식 생성 방법

생성 방법에는 두가지가 있습니다.

`RegExp` 생성자 함수를 호출하여 사용할 수 있습니다.

```javascript
const regexp1 = new RegExp("표현");
// new RegExg(표현식)

const regexp2 = new RegExp("[a-z]", "gi");
// new RegExg(표현식, 플래그)
```

`[a-z]`는 a부터 z까지의 영어 소문자를 검색하는 패턴이고, `gi`는 일치하는 모든 내용을 검색하겠다는 g와 대소문자를 구분하지 않겠다는 i를 사용하였습니다.

정규표현식은 **/** 로 감싸진 패턴을 리터럴로 사용합니다.

```javascript
const regexp1 = /표현/;
// /표현식/

const regexp2 = /[a-z]/gi;
// /표현식/플래그
```

그럼 먼저 상수를 만들어서, 정규표현식으로 검색할 문자 데이터를 생성해줍시다.

```javascript
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;
```

이제 정규표현식을 생성합시다. 먼저 **the**라는 단어를 한번 찾아보죠. RegExp의 두 번째 인수는 비워두도록 하겠습니다. 결과를 한번 확인해 볼까요?

```javascript
const regexp = new RegExp("the", "");
console.log(str.match(regexp));

// 출력 결과 :
// ['the', index: 15, input: '\n010-1234-5678\nthesecon@gmail.com\nhttps://www.omdb…ck brown fox jumps over the lazy dog.\nabbcccdddd\n', groups: undefined]
// 0: "the"
// groups: undefined
// index: 15
// input: "\n010-1234-5678\nthesecon@gmail.com\nhttps://www.omdbapi.com/?apikey=7035c60c&s=frozen\nThe quick brown fox jumps over the lazy dog.\nabbcccdddd\n"
// length: 1
// [[Prototype]]: Array(0)
```

출력된 데이터는 배열 데이터 입니다. 인덱스가 0번인 데이터만 확인하면 됩니다. 보면, 정규표현식에 패턴으로 입력한 "the"가 잘 출력되고 있는걸 볼 수 있습니다. 결국 **str**이라는 input에 입력한 문장에서 the를 찾아 배열 데이터로 만들어주는데, 기본적으로는 제일 처음에 찾은 단어만 배열로 만들어줍니다. 그렇다면 문장 안의 모든 the를 찾아서 배열로 만들려면 어떻게 해야 할까요? 2 번째 인자에 플래그라고도 불리는 `g`값을 넣어주면 됩니다.

```javascript
const regexp = new RegExp("the", "g");
console.log(str.match(regexp));

// 출력 결과 :
// (2) ['the', 'the']
```

하지만 문장을 찾아보면 the 는 3개인데 2개밖에 찾질 못했네요. 이유는 4번째 문장의 The가 대문자이기 때문에 찾지 못한 것인데, 만약 대소문자를 가리지 않고 찾고 싶다면 `i` 플래그를 추가해주면 됩니다.

```javascript
const regexp = new RegExp("the", "gi");
console.log(str.match(regexp));

// 출력 결과 :
// (3) ['the', 'The', 'the']
```

리터럴 방식으로 만든 정규표현식도 한번 확인해봅시다.

```javascript
const regexp = /the/gi;
console.log(str.match(regexp));

// 출력 결과 :
// (3) ['the', 'The', 'the']
```

---

## JavaScript 메서드 (정규 표현식)

정규표현식을 다루는 다양한 메서드들이 있습니다. 한번 확인해봅시다.

| 메소드  | 문법                                 | 설명                                                  |
| ------- | ------------------------------------ | ----------------------------------------------------- |
| test    | **정규식.test(문자열)**              | 일치 여부(Boolean) 확인                               |
| match   | **문자열.match(정규식)**             | 일치하는 문자열의 배열(Array) 반환                    |
| replace | **문자열.replace(정규식, 대체문자)** | 일치하는 문자열을 대체하고 대체된 문자열(String) 반환 |

#### test

```javascript
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;

const regexp = /fox/gi;
console.log(regexp.test(str));

// 출력 결과 :
// true
```

#### match

```js
const regexp = /the/gi;
console.log(str.match(regexp));

// 출력 결과 :
// (3) ['the', 'The', 'the']
```

#### replace

replace() 메서드에 대해 알아보겠습니다. 원본을 손상시키지는 않습니다.

```javascript
const regexp = /fox/gi;
console.log(str.replace(regexp, "AAA"));
console.log(str);
// 출력 결과 :
// 010-1234-5678
// thesecon@gmail.com
// https://www.omdbapi.com/?apikey=7035c60c&s=frozen
// The quick brown AAA jumps over the lazy dog.
// abbcccdddd

// 010-1234-5678
// thesecon@gmail.com
// https://www.omdbapi.com/?apikey=7035c60c&s=frozen
// The quick brown fox jumps over the lazy dog.
// abbcccdddd
```

## 플래그(옵션)

| 플래그 | 설명                                         |
| ------ | -------------------------------------------- |
| g      | 모든 문자 일치(global)                       |
| i      | 영어 대소문자를 구분 않고 일치 (ignore case) |
| m      | 여러 줄 일치 (multi line)                    |

만약 표현에 **.** 값을 입력하고 싶다면 어떻게 해줘야 할까요? 정규표현식에서 **.**은 특정한 문자를 검색하는 일종의 패턴입니다. 따라서 하나의 명령으로 동작할 수 있다는 것이죠. 명령이 아닌, 일반적인 문자로 해석되게 하려면 어떻게 해줘야 할까요? `\`**(이스케이프 문자)**를 사용하면 됩니다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;
console.log(str.match(/\./gi));
```

이번엔 `.`뒤에 `$`도 한번 붙여보겠습니다. $는 꼭 . 뒤에 붙여야 합니다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;

console.log(str.match(/\.$/gi));

// 출력 결과 :
// null
```

위 코드의 의미는, $ 앞의 하나의 단어로 해당하는 줄이 끝나는 부분을 찾아서 끝나는 부분을 일치시킨다는 뜻입니다. 그럼 str 상수 내에서 끝나는 부분은 어디일까요? 일단 해당 상수 내에서는 없군요. 그런데 만약 여기에 여려줄에 해당하는 플래그인 `m`을 넣어준다면, 어떻게 될까요? 다음과 같이 찾아서 배열을 생성한 것을 확인할 수 있습니다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;

console.log(str.match(/\.$/gim));

// 출력 결과 :
// ['.']
```

플래그 **g**와 **m**이 헷갈릴 수 있습니다. 전체 영역에서 검색한다는 것은 g
를 사용하면 되는 것이고, m은 기본적인 문자데이터의 시작, 끝 부분은 하나만 존재하기 때문에 줄마다의 시작 끝 부분을 찾아서 해석하겠다는 의미로 생각해주시면 될 것 같습니다.

## 패턴

정규표현식 패턴에는 여러가지가 있습니다. 반복해서 살펴보면서 익숙해집시다.

| 패턴    | 설명                                                 |
| ------- | ---------------------------------------------------- |
| ^ab     | 줄(Line) 시작에 있는 ab와 일치                       |
| ab$     | 줄(Line) 끝에 있는 ab와 일치                         |
| .       | 임의의 한 문자와 일치                                |
| a\|b    | a 또는 b와 일치                                      |
| ab?     | b가 없거나 b와 일치                                  |
| {3}     | 3개 연속 일치                                        |
| {3, }   | 3개 이상 연속 일치                                   |
| {3, 5}  | 3개 이상 5개 이하 (3~5개) 연속 일치                  |
| [abc]   | a 또는 b 또는 c                                      |
| [a-z]   | a부터 z 사이의 문자 구간에 일치 (영어 소문자)        |
| [A-Z]   | A부터 Z 사이의 문자 구간에 일치 (영어 대문자)        |
| [0-9]   | 0부터 9 사이의 문자 구간에 일치 (숫자)               |
| [가-힣] | 가부터 힣 사이의 문자 구간에 일치 (한글)             |
| \w      | 63개 문자(Word: 대소영문52개 + 숫자10개 + \_)에 일치 |
| \b      | 63개 문자에 일치하지 않는 문자 경계(Boundary)        |
| \d      | 숫자(Digit)에 일치                                   |
| \s      | 공백(Space, Tab 등)에 일치                           |
| (?=)    | 앞쪽 일치(Lookahead)                                 |
| (?<=)   | 뒤쪽 일치(Lookbehind)                                |

#### $

코드를 보며 이해해보도록 합시다. 먼저 $를 확인해볼까요?

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;

console.log(str.match(/d$/g));
// 출력 결과 :
// null
```

끝 자리가 d와 일치하는 문장을 찾고자 했습니다. 마지막 줄의 d를 인식할 것이라 생각했지만 결과는 null이 나왔네요. 이전의 예제와 같은 맥락으로 이해하면 될 것 같습니다. `m` 플래그를 추가해주죠

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;

console.log(str.match(/d$/gm));
// 출력 결과 :
// ['d']
```

#### ^

이번에는 `^`기호를 한번 확인해봅시다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;

console.log(str.match(/^t/gm));

// 출력 결과 :
// ['t']
```

확인되는 문장 맨 앞의 문자 t는 두개나 있는데, 출력은 하나만 되었군요. 대소문자구분 때문에 그런것 같습니다. `i`플래그를 사용해줍시다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;

console.log(str.match(/^t/gim));
// 출력 결과 :
// (2) ['t', 'T']
```

#### .

`.`패턴도 한번 알아봅시다.
`console.log(str.match(/./g))`라는 패턴을 사용하면 str의 모든 문자가 다 출력되는 것을 확인할 수 있습니다.

만약 http라는 단어를 찾는다고 할 때, .을 포함하여 패턴을 작성하려면 어떻게 해야할까요? 다음과 같이 작성해주면 됩니다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;

console.log(str.match(/h..p/g));

// 출력 결과 :
// ['http']
```

물론 다음과 같은 패턴을 작성하면 http 말고 h로 시작하고 p로 끝나는 모든 문자들이 다 출력이 되겠군요.<br>

#### |

`|`패턴은 찾으려 하는 특정 문자 또는 다른 문자를 찾는 표현입니다. 코드를 보며 알아봅시다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;

console.log(str.match(/fox|dog/g));

// 출력 결과 :
// (2) ['fox', 'dog']
```

#### ?

이번에는 `?`패턴에 대해 알아봅시다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
`;

console.log(str.match(/https/g));

// 출력 결과 :
// ['https']
```

문자 데이터 마지막 줄을 보면 http...로 시작하는 줄이 있군요. 하지만 정규표현식을 https로 작성해서 출력 결과는 https만 나타나는 것을 볼 수 있습니다. 이때 `?`패턴을 사용하면 효과적입니다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
`;

console.log(str.match(/https?/g));

// 출력 결과 :
// (2) ['https', 'http']
```

#### {}

`{}`패턴에 대해 알아봅시다. 예제를 살펴보죠.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
`;

console.log(str.match(/d{1}/));

// 출력 결과:
// ['d', index: 48, input: '\n010-1234-5678\nthesecon@gmail.com\nhttps://www.omdb…r the lazy dog.\nabbcccdddd\nhttp://localhost:1234\n', groups: undefined]
```

{1}을 사용하면 d와 1개가 일치하는 문자 하나가 발견되어 출력된 것을 확인할 수 있습니다. 그럼 2개가 연속하여 일치하는 것을 문자 데이터 전체에서 찾으면 어떻게 될까요? 다음 코드와 같은 결과를 보여줍니다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
`;

console.log(str.match(/d{2}/g));

// 출력 결과:
// (2) ['dd', 'dd']
```

2개 이상 연속된 결과를 찾는다고 한다면 어떻게 될까요?

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
`;

console.log(str.match(/d{2,}/g));

// 출력 결과:
// ['dddd']
```

조금 유용한 예제를 한번 살펴봅시다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
`;

console.log(str.match(/\w{2,3}/g));

// 출력 결과 :
// (42) ['010', '123', '567', 'the', 'sec', 'on', 'gma', 'il', 'com', 'htt', 'ps', 'www', 'omd', 'bap', 'com', 'api', 'key', '703', '5c6', '0c', 'fro', 'zen', 'The', 'qui', 'ck', 'bro', 'wn', 'fox', 'jum', 'ps', 'ove', 'the', 'laz', 'dog', 'abb', 'ccc', 'ddd', 'htt', 'loc', 'alh', 'ost', '123']
```

`\w`패턴은 숫자를 포함한 영어 알파벳을 의미합니다. 그러니 그 데이터가 2번 이상, 3번 이하 연속되는 데이터를 전부 다 출력해준다는 의미가 되겠군요.<br>
표현식을 좀 더 변형해 보겠습니다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
`;

console.log(str.match(/\b\w{2,3}\b/g));

// ['010', 'com', 'www', 'com', 'The', 'fox', 'the', 'dog']
```

`\b`는 숫자를 포함한 영어 알파벳이 아닌 특수문자들로 경계를 만들어준다는 의미 입니다.

#### []

`[]`표현은 어떻게 사용할까요?

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
`;

console.log(str.match(/[fox]/g));
// 출력 결과 :
// (14) ['o', 'o', 'o', 'o', 'f', 'o', 'o', 'f', 'o', 'x', 'o', 'o', 'o', 'o']
```

**f, o, x**를 모두 찾아 출력된 것을 확인할 수 있습니다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
`;

console.log(str.match(/[fox]/g));
// 출력 결과 :
// (14) ['o', 'o', 'o', 'o', 'f', 'o', 'o', 'f', 'o', 'x', 'o', 'o', 'o', 'o']
```

[]에 0-9를 입력하면 숫자가 모두 출력되는 것을 확인할 수 있습니다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
`;

console.log(str.match(/[0-9]/g));
// 출력 결과 :
// (21) ['0', '1', '0', '1', '2', '3', '4', '5', '6', '7', '8', '7', '0', '3', '5', '6', '0', '1', '2', '3', '4']
```

이전에 배운 `{}`표현과 혼합해서 패턴을 작성해보도록 합시다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
`;

console.log(str.match(/[0-9]{1,}/g));
// 출력 결과 :
// (6) ['010', '1234', '5678', '7035', '60', '1234']
```

문자 데이터 내에 한글도 입력해봅시다. 한글을 찾으려면 []를 다음과 같이 작성해주면 됩니다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
동해물과 백두산이 마르고 닳도록
`;

console.log(str.match(/[가-힣]{1,}/g));
// 출력 결과 :
// ['동해물과', '백두산이', '마르고', '닳도록']
```

#### \w

63개의 문자를 찾고자 할 때 사용하는 패턴입니다.

#### \b

63개 문자가 아닌 경계를 설정하고 할 때 사용하는 패턴입니다. 예제를 보면서 익숙해집시다. `\b`로 문자가 아닌 경계를 설정하고 f로 시작하면서 `\w`를 사용하여 63개의 문자로 연속되는 단어를 찾아주는 패턴을 작성하였습니다. 출력 결과를 보죠

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
동해물과_백두산이 마르고 닳도록
`;

console.log(str.match(/\bf\w{1,}\b/g));
// 출력 결과 :
// (2) ['frozen', 'fox']
```

이번에는 모든 숫자에 일치시키는 패턴을 사용해봅시다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
동해물과_백두산이 마르고 닳도록
`;

console.log(str.match(/\d/g));
// 출력 결과 :
// (21) ['0', '1', '0', '1', '2', '3', '4', '5', '6', '7', '8', '7', '0', '3', '5', '6', '0', '1', '2', '3', '4']
```

숫자가 연속해서 일치하는 패턴도 사용해 봅시다. 숫자 덩어리들만 출력되는군요.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
동해물과_백두산이 마르고 닳도록
`;

console.log(str.match(/\d{1,}/g));
// 출력 결과 :
// (6) ['010', '1234', '5678', '7035', '60', '1234']
```

#### \s

모든 공백문자를 출력해주는 패턴 입니다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
http://localhost:1234
동해물과_백두산이 마르고 닳도록
`;

console.log(str.match(/\s/g));
// 출력 결과 :
// (18) ['\n', '\n', '\n', '\n', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', '\n', '\n', '\n', ' ', ' ', '\n']
```

새롭게 문자열을 만들어서 정규표현식을 사용해보고, replace() 메서드를 사용하여 모든 공백문자를 없애주도록 하겠습니다.

```js
const h = `  the hello  world       !

`;

console.log(h.match(/\s/g));
// 출력 결과 :
// thehelloworld!
```

#### 앞쪽 일치(Lookahead)

앞쪽 일치에 대해 알아보겠습니다. 이메일 주소의 **@**앞쪽 사용자 ID를 일치시키고자 하는데요, 예제를 한번 살펴보죠.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;

console.log(str.match(/.{1,}(?=@)/g));
// 출력 결과 :
// ['thesecon']
```

#### 뒤쪽 일치(Lookbehind)

뒤쪽 일치에 대해 알아보겠습니다. 뒤쪽 일치이기 떄문에, 패턴도 뒤쪽에 작성해야 합니다. 예제를 봅시다.

```js
const str = `
010-1234-5678
thesecon@gmail.com
https://www.omdbapi.com/?apikey=7035c60c&s=frozen
The quick brown fox jumps over the lazy dog.
abbcccdddd
`;

console.log(str.match(/(?<=@).{1,}/g));
// 출력 결과 :
// ['gmail.com']
```
