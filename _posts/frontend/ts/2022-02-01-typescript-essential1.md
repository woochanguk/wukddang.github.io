---
layout: post
image: /assets/img/ts/typescript.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  TypeScript
invert_sidebar: false
categories:
  - frontend
  - ts
---

# TS - Beginning

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

이번에는 `TypeScript`에 대해 알아보겠습니다.

1. table of contents
{:toc .large-only}
---
## Contents

- [**TypeScript?**](#typescript)
  - [**Typed JavaScript at Any Scale**](#typed-javascript-at-any-scale)
- [**설치 및 사용**](#설치-및-사용)
  - [**NodeJS**](#nodejs)
- [**간단한 컴파일러 사용 예제**](#간단한-컴파일러-사용-예제)
  - [**TypeScript 전역 설치**](#typescript-전역-설치)

---

## TypeScript?

<a href="https://typescript-v2-497-ortam.vercel.app/" target="_blank">`TypeScript`</a>란 뭘까요? 한번 알아보죠. 링크에서 나오는 TypeScript의 특징들을 정리해 보겠습니다.

#### Typed JavaScript at Any Scale

- TypeScript extends JavaScript by adding **types.**
- By understanding JavaScript, TypeScript **saves you time catching errors and providing fixes before you run code.**
- Any browser, any OS, anywhere JavaScript runs. Entirely Open Source

TypeScript는 JavaScript와 같은 **프로그래밍 언어**입니다. 일반적으로 TypeScript는 JavaScript의 **Typed Superset**이라고 부릅니다.

> **TypeScript는 JavaScript의 Typed Superset**

TypeScript는 JavaScript의 기능을 강화시키고, Type이란 개념까지 추가시킨 언어라고 생각할 수 있습니다.
한 가지 신기한 점은, TypeScript는 Transpile하는 과정을 통해 plain JavaScript로 변환됩니다. 여기서 **plain**이란 의미는 브라우저, NodeJS등에서 이해할 수 있는 JavaScript 파일이라는 의미로 해석됩니다. 그러니, 런타임(runtime)에서는 실행될 수 없는 TypeScript파일을 런타임에서 이해할 수 있는 JavaScript파일로 Transpile해준다고 이해하면 되겠습니다.
<br>
요약하면 다음과 같습니다.

- TypeScript는 **프로그래밍 언어**입니다.
- TypeScript는 **Transpiled 언어**입니다.
- JavaScript는 **Interpreted 언어**입니다.

JavaScript는 Interpreted 언어이기 때문에, 컴파일 시점이 존재하지 않습니다. 따라서 코드에 문제점이 있다면, **코드를 실행하는 시점에 도달해야 문제를 파악**할 수 있는 반면 TypeScript는 Transpiled 언어입니다. 따라서 컴파일 시점과 일맥상통하게, 코드를 실행하기 전에 Trasnpile하는 시점이 존재하므로 **JavaScript가 런타임 환경에서만 코드 오류를 발견할 수 있다는 단점을 보완**해줄 수 있는 프로그래밍 언어라고 할 수 있겠습니다.
<br>
따라서 우리는 TypeScript 파일을 Editor에서 작업한 후 TypeScript 컴파일러, 즉 Tranpiler를 설치하여 브라우저, NodeJS가 실행할 수 있는 plain한 JavaScript로 바꿔준다고 생각해 주시면 되겠습니다.

## 설치 및 사용

먼저 JavaScript 실행 환경은 크게 두 가지로 정리할 수 있습니다.

#### NodeJS

- Chrome V8 JavaScript Engine을 사용하여 JavaScript를 해석하고 OS level에서의 API를 제공하는 서버사이드용 JavaScript 런타임 환경
- npm(nvm) 설치하면 됩니다.

#### Browser

- HTML을 동적으로 만들기 위해 브라우저에서 JavaScript를 해석하고, DOM(Document Object Model)을 제어할 수 있도록 하는 JavaScript 런타임 환경
- 크롬 사용하면 됩니다.

<br>

설치는 npm을 사용하시면 됩니다. -g는 내 PC어디에서는 typescript를 불러서 사용하겠다는 뜻입니다.

> **npm i typescript -g**

## 간단한 컴파일러 사용 예제

- TypeScript 컴파일러를 -g 로 설치후,

  - cli 명령어로 파일 컴파일 (tsc 명령어 사용)
  - 특정 프로젝트 폴더에서 TypeScript 컴파일러 설정에 맞춰 컴파일
  - 특정 프로젝트 폴더에서 TypeScript 컴파일러 설정에 맞춰 컴파일 (watch 모드)

- 프로젝트에 TypeScript 컴파일러 설치 후,
  - .bin 안의 명령어로 파일 컴파일
  - npm 스크립트로 파일 컴파일
  - 프로젝트에 있는 TypeScript 설정에 맞춰, npm 스크립트로 컴파일
  - 프로젝트에 있는 TypeScript 설정에 맞춰, npm 스크립트로 컴파일 (watch 모드)

#### TypeScript 전역 설치

tsc 명령어를 사용하면 모든 TypeScript 파일이 한번에 컴파일 되어야 할텐데, 특정 프로젝트 파일에서는 한번에 수행되지 않을 수 있습니다. 따라서 어떤 방식으로 컴파일 할 것인지에 대한 설정 파일을 추가해주어야 합니다. 보통 설정 파일을 default로 추가해주는 명령어가 있습니다.

> **tsc -\-init**

이 명령어를 실행하면, `tsconfig.json`파일이 생성이 됩니다. 이제 이 파일이 있는 폴더에서 `tsc`명령어를 실행하면 tsconfig.json에 맞게 컴파일러가 실행됩니다. <br>
만약, test.ts파일을 변경한다면, 즉시 test.js 파일도 생성되도록 하는 명령어를 입력해보겠습니다.

> **tsc -w**

test.ts 파일을 저장하면 바로 test.js파일이 생성되는 것을 확인할 수 있습니다.

#### 프로젝트에 TypeScript 컴파일러 설치

먼저 npm 프로젝트를 시작해주어야 합니다. 명령어로 다음과 같이 입력합시다.

> **npm init -y**

이제 이 npm 프로젝트 파일에 TypeScript 컴파일러를 설치해주도록 하겠습니다. 개발자에게만 필요한 패키지이니, `-D` 플래그를 붙여서 설치해줍시다.

> **npm install typescript -D**

이 상태에서는 TypeScript 컴파일러는 node_modules안에 존재하게 됩니다. 이전에 사용했던 `tsc`명령어는 파일로 존재하고 있는데,

1. node_modules안에 .bin폴더에 존재하고,
1. node_modules안에 typescript 폴더 안에 존재합니다.

어떤 방법으로 실행하든, 프로젝트 내에서 실행하기 위해선 tsc를 입력하기 위해 꽤 긴 경로를 입력해야 합니다. 이를 해결하기 위해, npm에서는 편리한 기능을 제공합니다. 이를 이용하여 tsc 파일을 사용해주도록 하겠습니다.

> **npx tsc**

그리고 설정 파일을 만들어 주어야겠죠? 다음 명령어를 입력합시다.

> **npx tsc -\-init**

TypeScript 파일 변화를 watch 모드로 확인하려면 다음 명령어를 입력하면 됩니다.

> **npx tsc -w**

package.json 파일의 "scripts"속성을 바꿔보겠습니다. 다음과 같이 입력해줍시다.

```json
"scripts": {
    "build": "tsc",
    "build:watch": "tsc -w"
  }
```

이렇게 tsc만 입력해도, npm이 알아서 node_modules안의 .bin 폴더를 찾아 tsc 파일을 실행하게 됩니다.

## First Type Annotation

Type Annotation은 TypeScript가 가진 고유의 기능입니다. JavaScript와 가장 차별화되는 부분이라고 생각합니다. Type이라는 요소가 코드 상에 드러나는 방식입니다.

코드로 한번 보시죠

```typescript
let a = "Wuk";
a = 39;
```

다음과 같이 입력하면, JavaScript에선 아무런 error메시지도 발생시키지 않았지만, TypeScirpt에선 빨간줄이 생깁니다. 컴파일시에 문제가 생겼다는 의미입니다. 한번 살펴보죠

> **Type "number" is not assignable to type "string"**

숫자값은 문자열 값을 할당할 수 없다는 에러가 발생했습니다. TypeScript는 변수를 생성한 값을 만나면 _(ex. let a = ...)_ 임의로 변수에 특정한 타입을 지정해줍니다. 따라서 변수 a의 Type은 문자열로 지정이 되는 것입니다.
<br>
그렴, 선언과 할당을 따로 해준다면 어떻게 될까요? 그래도 에러가 발생할까요? `let a;`와 같이 선언만 해주게 된다면, a에는 `any`라는 Type이 들어가게 됩니다. 아직 any에 대해 배우지는 않았지만, any라는 Type만 이후에 변수 a 에 들어갈 수 있다는 뜻이겠네요.
<br>
그러니 선언할 때에 다음과 같이 작성해주도록 하겠습니다. 그 후 값을 할당해 주겠습니다.

```typescript
let a: string;
a = "wuk";
```

이제 에러가 발생하지 않습니다. 이를 **Type Annotation** 이라고 합니다.<br>
함수를 선언할 때 인수를 Type Annotation으로 지정해줄 수도 있습니다. 함수를 호출할 때, 다른 Type을 입력한다면 에러가 발생하게 되겠죠.

```typescript
function hello(b: number) {
  console.log(b);
}

hello(39);
```

이와 같이 TypeScript는 Type Annotation을 사용해 JavaScript 와는 다르게 Type이 정확하게 지정될 것입니다.
다음 포스트에서 TypeScript의 Type에 대해서 알아보도록 하겠습니다.
