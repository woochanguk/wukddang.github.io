---
layout: post
current: post
cover: assets/built/images/wetube-clone.png
navigation: True
title: Wetube 클론코딩 - SET UP
date: 2022-01-25 00:00:00
tags: [Clone]
class: post-template
subclass: "post tag-Clone"
author: changuk
---

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `nomadcoders`에서 `Wetube` 클론코딩을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

{% include wetube-clone-table-of-contents.html %}

## NODEJS란?

`NodeJS` 홈페이지에 들어가면 알 수 있듯이, NodeJS란 **크롬 V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임**입니다.
<br>
런타임(`Runtime`)이란 프로그램이 실행되고 있는 동안의 동작이라고 정의되어 있는데, 일반적으로 **프로그래밍 언어가 구동되는 환경**이라고 생각하시면 됩니다. 즉, 원래 웹사이트와 간단하게 상호작용하기 위해 만들어졌던 자바스크립트를 이젠 웹사이트에서만이 아닌 다른 환경에서도 실행할 수 있도록 만들어 주었다는 의미로 받아들이시면 됩니다.<br>
이런 환경을 만듦으로써, 이젠 자바스크립트 언어만으로도 벡엔드(`express`)를 구성하고, 심지어는 `IOS`, `안드로이드` 어플도 만들 수 있게 되었다고 합니다(`React-native`). 아직 모바일 앱은 못만들어 봤지만... 차차 공부해가면 다 해낼 수 있을 겁니다 헤헤.
<br>
그럼 잘 작동하는 지 확인도 해볼겸, NodeJS에서 실행될 첫 번째 JavaScript 파일을 한번 만들어서 실행해 보겠습니다.

```javascript
console.log("Hello NodeJS");
```

콘솔 창에 `Hello NodeJS`라는 문자열을 출력하는 `index.js`파일입니다. 출력결과를 한번 보죠.
<img src="../assets/built/wetube/SETUP/first-js-node.png" width="600" height="50" />
콘솔 창에 잘 출력되었습니다.

---

## NPM이란?

이름에서 알 수 있듯, `npm(node package manager)`은 자바스크립트 언어를 위한 패키지 매니저입니다. 다른 개발자들은 이`npm`이라는 곳에 자신들이 만든 패키지들을 업로드해주는데 그것들을 활용하면 정말 간단히 말해 개발을 아주 편하게 할 수 있도록 도와줍니다. 추후 모듈들을 소개하며 각각의 기능들을 살펴보면 어떤 의미인지 와닿을 것이라고 생각합니다~~(공부가 부족하다고는 말 못해 읍읍)~~. 우리는 이 패키지 매니저에서 개발을 편하게 해주는 모듈을 가져다 쓸 수도 있고, 또는 특정 개발(ex. 벡엔드)에 꼭 필요한 모듈을 가져다 쓸 수도 있습니다.

그럼 `npm`을 어떻게 사용하냐? `work directory`의 콘솔창에 `npm init`이라 입력하면 `package.json`파일을 자동으로 생성해줍니다 <br>(약간의 설정을 요구함).
<img src="../assets/built/wetube/SETUP/npm-init.png" width="800" height="300" />
모든 값들을 무시하고 엔터만 계속 치면, package.json파일이 생성됩니다.  
<img src="../assets/built/wetube/SETUP/npm-init-fin.png" width="800" height="450" />
`npm`을 사용하여 모듈들을 설치한다면 이 `package.json`파일에 설치된(할) 모듈들의 이름들이 입력됩니다.
~~(`package.json`은 아주 많은 모듈들의 이름을 받게 될 운명에 처해있죠 후후...)~~

이처럼, `npm`은 여러 모듈들을 설치하게 도와주는 패키지 매니저로 생각하면 됩니다.
`NodeJS`로 `app` 개발을 한다는 것은 레고를 만드는걸로 비유할 수 있습니다. 하나씩 하나씩 쌓아 올라가면서 만들어야 하거든요. 보다시피 처음은 굉장히 간단하죠? 하지만 레고를 만들 때 아무 생각없이 하다 보면 뭘 만들고 있는지 잊는 것 처럼, `NodeJS`로 개발을 할 때에도 마찬가지입니다. 개발자가 어떤 행동을 할 때 스스로 어떤 결과를 발생시키는지 인지하고 있지 않다면 어떤 무시무시한 일이 발생할 지... 무튼 각 세부내용들을 잘 숙지하여 불상사가 없도록 합시다. ㅎㅎ ~~(그래서 블로그 글을 쓰고있다고는 말 못해 읍읍)~~

---

## package.json의 기능 (script)

package.json 파일을 보시면 `"script"`라고 하는 부분이 있습니다. 원래 NodeJS환경에서 index.js라는 자바스크립트 파일을 사용하려면, 콘솔창에다가 `node index.js`라고 입력해주어야 index.js를 실행할 수 있었습니다. 하지만 이제 package.json의 `script` 기능을 사용하면 npm 명령어로 파일을 실행할 수 있습니다. 나중에 알아볼 수 있겠지만, 서버를 시작하는 script, CSS를 압축하는 script, 웹사이트를 빌드하고 서버에 배포하는 script 등등... 여러 script를 만들어서 사용할 수 있습니다. 와우! script 기능으로 개발 시간을 상당히 아낄 수 있습니다. script안에 다음과 같이 작성해 봅시다.

```json
"scripts": {
  "win": "node index.js"
}
```

그 후, 콘솔 창에 `npm run win`을 입력하시면, node index.js를 입력하지 않아도, index.js를 실행해주는 것을 볼 수 있습니다! 물론, 한 파일 가지고는 node index.js를 쓰든 npm run win을 쓰든 똑같은거 아니냐고 할 수 있지만 script의 장점은 다른 명령어를 추가해도 `npm run win` 한번이면 원하는 기능을 다 작동시켜주는 것에 있으니까요. script를 잘 이용하도록 합시다.

---

## Express 설치

script도 알았겠다, 우리의 첫 `module`인 `express`를 설치해 봅시다. 이 모듈이 없으면 서버를 만들 수가 없습니다... 첫 등장으로부터 꽤 시간이 지났지만 여전히 쓰이고 있는 모듈로서, JavaScript로 벡엔드 개발을 한다면 절대 없어서는 안되는 친구입니다. npmjs에서 express가 현재 얼마나 사용되고 있는지 한번 보시죠. <a href="https://www.npmjs.com/package/express" target="_blank">`EXPRESS`</a><br>
서두가 길었습니다. 콘솔 창에 `npm install express`를 입력합시다. i로 하셔도 되고, install 이라고 하셔도 됩니다. 둘 다 동일하게 작동합니다.
<img src="../assets/built/wetube/SETUP/install-express.png" width="800" height="350" />
설치가 완료되었습니다. 우리의 package.json의 `"dependencies"`를 보면 express가 잘 설치되어 있습니다!
<br>
왼쪽의 현재 `work directory`를 한 번 살펴보시죠.
<img src="../assets/built/wetube/SETUP/node-modules.png" width="200" height="200" />
`node_modules`가 생성되었습니다. 이게 뭘까요?? `npm`으로 설치한 모듈들은 모두 `node_modules`에 설치가 됩니다. 그런데 우리가 설치한 건 `express`뿐인데, 처음 보는 모듈들도 많이 있습니다. 이것은 `express`의 `package.json`을 보면 확실하게 알 수 있습니다. `package.json` 안의 `"dependencies"`를 한번 봅시다.
<img src="../assets/built/wetube/SETUP/express-dependencies.png" width="300" height="400" />
상당히 많은 모듈의 이름들이 적혀있습니다. 이것은 바로 `express`를 사용하기 위해 필요한 모듈들 입니다. `node_modules`에는 해당 모듈들을 사용할 때 필요한 모듈들도 설치가 되어 있습니다. 다시 말씀드리면 express를 설치할 때 express를 사용하기 위해 필요한 모듈들도 같이 설치가 되고, 설치가 된 모듈들을 사용할 때 필요한 모듈들도 설치되는... 것이죠. 뭐, 어떤 모듈들이 설치되어야 하는지 알 필요는 없습니다. **`npm`이 알아서 해주니까요!** <br> 또 여기서 아시면 좋을게 `"devDependencies"`라는 것이 있습니다. 이것은 개발자가 필요로 하는 모듈들을 설치해주는 명세서라고 보시면 됩니다. 프로젝트를 배포한 후 작동시킬 때 필요한 것이 아니라, 개발에 편의를 줄 수 있는 모듈을 개발자의 기호에 맞게 설치해달라고 package.json에게 전달해주는 것이죠.

---

## npm install

문제가 있습니다. node_modules는 데이터가 너무 많아요. express 하나만 설치했는데도 폴더의 갯수가 너무 많아요. 이 상태로는 `github`에 업로드 하기가 꺼려집니다. 다행이도 npm이 다 해결해줍니다. npm은 package.json파일의 `"dependencies"`와 `"devDependencise"`만 있으면 프로젝트에 필요한 모든 파일들을 알아서 설치해줍니다! 그러니 node_modules폴더를 공유해 줄 필요가 없는거죠. 다음 명령만 콘솔창에 입력해주면 됩니다.

```console
npm i // 또는 npm install
```

`node_modules`를 `github`에 업로드 하지 않으려면, `.gitignore` 파일을 생성하고 파일 안에 `node_modules`를 입력해주면 됩니다.

---

## Babel

`SETUP`에서 `Babel`을 빼놓을 수 없습니다. JavaScript는 1995년 처음 발표되었습니다. 그 후 계속해서 업데이트를 해오고 있고 현재 버전은 <a href="https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8" target="_blank">`ECMAScript 2020`</a> 입니다. 업데이트를 계속함으로 인해 개발할 때 불편한 부분들도 개선해나가고, 효율도 향상시켜주기 때문에 업데이트는 반드시 필요한 것이라고 할 수 있지요. 문제는 **모든 브라우저가 이 최신 버전의 JavaScript를 지원해주지 않는다**는 것에 있습니다. 개발자들은 최신 버전의 JavaScript언어를 사용하고 싶지만 웹 app이 동작할 브라우저가 항상 최신 버전 JavaScript를 지원해 줄 것이라는 걸 기대할 수는 없기에 이전 버전의 JavaScript코드로 변환을 해주어야 합니다. 또한 `NodeJS`조차 최신 버전 JavaScript를 이해하지 못할 수도 있습니다. 이 때 `Babel`이 사용되는 것이지요. <a href="https://babeljs.io/setup#installation" target="_blank">`Babel`</a>을 설치해 봅시다.
<br>
링크로 들어가면 `Installation`에 다음과 같이 콘솔 창에 입력하라고 합니다. 그런데 못 본 명령어가 있습니다.

```console
npm install --save-dev @babel/core
```

`--save-dev`가 뭘까요? 앞서 말씀드렸듯이 해당 `@babel/core` 패키지를 `package.json`에 `devDependencies`로 설치하라는 명령어 입니다. 물론 `dependencies`나 `devDependencies` 둘 다 `node_modules`에 설치되기 때문에 큰 차이는 없을 수 있지만, 구분하고 있는 것 같습니다. 굳이 비유하자면 자동차의 기름이 `dependencies`고 자동차에서 나오는 음악이 `devDependencies`입니다.
<br>
@babel/core를 잘 설치해주고 나면, `babel.config.json`파일을 만들라고 합니다. 시작해봅시다. 저는 WSL(window subsystem for linux)를 사용해서 touch명령어를 쓸 수 있지만 window terminal을 사용하시는 분들은 직접 파일을 만드시면 됩니다.

```console
touch babel.config.json
```

그리고 파일을 열어 다음 코드를 입력해줍시다.

```json
{
  "presets": ["@babel/preset-env"]
}
```

하나 더 설치해줍시다.

```console
npm install @babel/preset-env --save-dev
```

`Babel`을 활용하면 최신의 JavaScript문법을 사용할 수 있다고 했지만, 설정을 해 줄 필요가 있습니다. 그래서 `babel.config.json`에다가 `presets`를 정의해 주었습니다.
`preset`은 `Babel`을 위한 엄청 거대한 플러그인이라고 생각하면 되는데, 우리는 <a href="https://babeljs.io/docs/en/babel-preset-env" target="_blank">`@babel/preset-env`</a>라는 것을 사용하게 됩니다. 궁금하시면 링크를 한번 보시죠. 무튼 이 `preset`은 최신 JavaScript 구문을 사용할 수 있도록 해줍니다.

설치도 다 되었으니 `Babel`을 사용해봅시다. JavaScript 파일 내에서 사용하지 않고, package.json의 script를 이용해서 babel로 컴파일하도록 하겠습니다. 이를 위해 <a href="https://babeljs.io/setup#installation">`nodemon`</a>을 사용할 겁니다.
`@babel/core`는 이미 설치했으니까, `@babel/node`만 설치합시다.

```console
npm install @babel/node --save-dev
```

이제 `script`를 수정해볼까요? development를 뜻하는 `dev`명령어로 바꿔주고 시작해봅시다. 그 후 우리는 babel로 컴파일 해 줄것이기 때문에 같이 적어줍시다.

```json
"scripts": "babel-node index.js"
```

babel도 다 세팅해주었겠다, 이제 최신 문법을 사용할 수 있습니다. 바로 `import`입니다! 원래 express를 사용하던 방법을 한번 볼까요?

```javascript
const express = require("express");
const app = express();
```

다음과 같이 express라는 상수를 선언해주고, package.json에 명시되어 있는 `express`를 가져오는 코드였는데 최신 문법을 적용하면 다음과 같아집니다.

```javascript
import express from "express";
const app = express();
```

---

## nodemon이란?

아주 잘 진행되고 있습니다! 그런데 파일을 업데이트 해 줄 때마다 `npm run dev`를 실행하기가 굉장히 귀찮네요. 그러니 새로운 패키지인 `nodemon`을 설치해주겠습니다. 이 패키지를 사용하면 콘솔 창을 종료할 때 까지 계속해서 파일의 업데이트 상황을 체크해주고, 만약 업데이트가 된 파일이 있다면 파일을 재실행 시켜줍니다.

```console
npm install nodemon --save-dev
```

설치가 완료가 되었다면 package.json의 script를 다음과 같이 수정해줍시다.

```json
"scripts" : {
  "dev": "nodemon --exec babel-node index.js"
}
```

이제 콘솔 창에 `npm run dev`라고 실행해주면, nodemon이 적용된 채 작동하는 것을 볼 수 있고, 터미널이 종료되지 않는걸 볼 수 있습니다! 파일을 새로 저장할 때 마다 nodemon이 체크해주고 알아서 새로고침 해주는 것을 볼 수 있습니다.

---

`SETUP`이 정말 기네요. 이제 시작입니다. 힘내고 계속 다음으로 넘어가겠습니다. 저도 포기하지 않고 Wetube 클론코딩을 마무리 할 때까지 기운내도록 하겠습니다.
