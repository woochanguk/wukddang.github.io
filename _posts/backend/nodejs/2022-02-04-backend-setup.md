---
layout: post
image: /assets/img/nodejs/NodeJS.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  NodeJS Setup을 해봅시다.
invert_sidebar: false
categories:
  - backend
  - nodejs
---

# NodeJS - Setup

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `backend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

이번시간은 `NodeJS SETUP`을 진행하겠습니다.

1. table of contents
{:toc .large-only}

---

## Setup

높은 생산성을 위해서는 개발환경을 잘 세팅해두는 것이 필수적이라고 할 수 있습니다. 개발을 편하게 하는 패키지들 중 하나인 **Formatter**와 **Linter**를 먼저 설치해주도록 하겠습니다. <br>
**Formatter**는 띄어쓰기나 들여쓰기가 잘 되어 있는지, 비주얼적인 요소를 바로바로 맞춰주는 패키지라고 할 수 있고, **Linter**는 자체적으로 권장하는 문법에 맞지 않으면 에러를 발생시켜서 수정하도록 하는 패키지라고 할 수 있습니다.**npm**으로 개발 의존성 패키지가 되도록 설치하도록 하겠습니다.
이 두 패키지는 설치해주고 **VSCode Extensions**를 따로 설치해 주셔야 합니다.

> **npm i -D prettier eslint**

---

그리고 **Airbnb**에서 제공하는 <a href="https://github.com/airbnb/javascript" target="_blank">**JavaScript Style Guide**</a>를 설치하도록 하겠습니다. **JavaScript** 개발에 가장 합리적인 접근이라고 하네요. 또 이 플러그인을 연결해줄 수 있도록 새로운 패키지도 설치하겠습니다.

> **npm i -D eslint-config-airbnb-base eslint-plugin-import**

---

**prettier**와 **eslint**가 서로 충돌할 수 있기 때문에, 다음 플러그인도 설치해 주어야 합니다.

> **npm i -D eslint-config-prettier**

---

**eslint**가 **NodeJS**환경에서도 잘 동작할 수 있는 플러그인도 설치하겠습니다.

> **npm i -D eslint-plugin-node**

---

당장 **TypeScript**를 사용하는 것은 아니지만, **TypeScript**의 **Type Checking**을 이용하기 위해, 이 패키지도 설치하도록 하죠.

> **npm i -D typescript**

---

그리고 **Type Checking**을 할 수 있도록 다음 라인을 **js**파일의 처음에 입력해주면 됩니다.

```javascript
//@ts-check
```

또, **node**환경에서의 주로 사용하는 객체들에 대한 **Type** 정보들에 **Type Checking**을 수행해주기 위해서 아래 패키지도 또 설치하도록 하겠습니다.

> **npm i -D @types/node**

**prettier**, **eslint** 각각의 설정 파일을 다음과 같이 만들도록 하겠습니다. <br>

**.prettierrc**<br>
**.eslintrc.js**

**prettier**의 설정은, 본인의 기호에 맞게 세팅해주면 됩니다. 기본적인 설정만 이용해도 만족감은 상당하죠. <br>
**.eslintrc.js**파일에는 설정을 좀 추가하도록 하겠습니다.

```javascript
module.exports = {
  // 이 순서대로 작성해 주어야 한다.
  extends: ['airbnb-base', 'plugin:node/recommended', 'prettier'],
}
```

---
**.vscode** 폴더를 생성해서 내부에 **settings.json**파일을 만들어주면, 해당 프로젝트 폴더에서만 적용되는 **setting**파일을 갖게 됩니다. 파일을 생성하고 다음 내용을 입력해줍시다.

```json
{
  "[javascript]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

마지막으로 **jsconfig.json**파일을 생성하고 다음 내용을 기입하여 설정을 마무리 해주도록 하겠습니다. 엄격한 컴파일 옵션을 설정해주고, **src** 밑의 모든 파일들을 확인해주겠다는 의미로 이해하시면 될 것 같습니다.

```json
{
  "compilerOptions": {
    "strict": true
  },
  "include": [
    // src밑의 모든 파일들
    "src/**/*"
  ]
}
```

**Setup**내용을 간략하게 표로 정리했습니다.

#### VSCode JavaScript Development Setup

|                                     | Formatting  |                                                 Linting                                                 | Type Checking |
| :---------------------------------: | :---------: | :-----------------------------------------------------------------------------------------------------: | :-----------: |
|             **Package**             |  prettier   |                                                 eslint                                                  |  typescript   |
| **<br>Additional <br>dependencies** |             | eslint-config-airbnb-base<br /> eslint-config-prettier<br/> eslint-plugin-import<br/>eslint-plugin-node |  @types/node  |
|           **Config file**           | .prettierrc |                                              .eslintrc.js                                               | jsconfig.json |
|      **VSCode<br>extensions**       |      O      |                                                    O                                                    |       X       |
