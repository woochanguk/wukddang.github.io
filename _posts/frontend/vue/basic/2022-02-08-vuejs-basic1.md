---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
accent_color: "#ccc"
theme_color: "#ccc"
description: >
  VueJS를 시작해봅시다.
invert_sidebar: false
categories:
  - frontend
  - vue-basic
---

# VueJS - Start

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

이번시간부터는 `VueJS`에 대해 알아보겠습니다.

1. toc
{:toc .large-only}

---

## VueJS 시작하기

<a href="https://v3.ko.vuejs.org/guide/instance.html" target="_blank">**VueJS**</a>는 한글 가이드를 제공하고 있습니다.
<br>
VueJS 설치는, CDN으로 가져오는 방식과 npm프로젝트에 설치하는 방식 두가지로 나뉩니다. CDN방식은 한글 가이드를 따라서 해봤는데... 잘 안되더라고요. 그래서 아래 링크를 사용해주시면 되겠습니다. 저는 codepen.io를 사용했습니다.

**https://unpkg.com/vue@next**

#### Vue CLI

VueJS는 CLI로 간단하게 **SPA(Single Page Application: 단일 페이지 애플리케이션)**를 빠르게 구축할 수 있는 **Vue CLI**를 지원합니다. 한번 알아보겠습니다.<br>
먼저, @vue/cli를 전역 설정으로 설치해주겠습니다.

> **npm i -g @vue/cli**

그 후 다음 링크로 가서 <a href="https://cli.vuejs.org/guide/creating-a-project.html" target="_blank">**Vue-CLI**</a>를 자세히 알아보도록 하겠습니다<br>
VueJS는 다음 명령어로 사용자가 원하는 이름으로 프로젝트를 간단하게 만들 수 있다고 합니다. 저는 다음 이름으로 만들도록 하겠습니다.

> **vue create vue3-test**

vue3를 사용할 것이기 때문에, vue3를 선택하겠습니다. 일단 프로젝트로 이동하도록 하죠. <br>프로젝트의 package.json파일을 확인하니, **"scripts"**옵션에 여러가지 속성들이 있는 것을 확인할 수 있습니다.

- **"serve"**: 개발 서버를 시작
- **"build"**: 제품화
- **"lint"**: VueJS 코드가 특정 규칙에 맞게 작성이 되었는지

참고로 Vue-CLI는 내부적으로 Webpack을 사용하고 있습니다. <br>
앞서 말씀드렸던 lint의 설정 관련 내용은 **"eslintConfig"**라는 하위 옵션에서 찾을 수 있습니다. 옵션 하에 있는 **"rules"**속성에 여러가지 코드 규칙들을 추가적으로 입력할 수 있습니다. babel도 설치되어 있군요.

---

디테일한 설치관련 내용들은 Vue-CLI에서 알아서 해주기 때문에 입문자 입장에서는 구성옵션들을 신경쓰지 않고 바로 프로젝트를 만들 수 있는 장점이 있는 반면, 세부적인 옵션들을 정리해가며 나만의 프로젝트를 만들고 싶은 분들은 좀 아쉬울 수 있는 부분입니다.

---

public 폴더의 index.html을 보면 **\<body\>**안에 **id="app"**이 만들어진 것을 볼 수 있습니다. 그리고 src 폴더에 가면 **App.vue**파일도 생성된 것을 알 수 있습니다. 다만 파일이 하나도 highlighting이 되어있지 않네요... VSC 플러그인 **Vetur**를 설치해 줍시다.

---

**.vue**파일은 크게 **\<template\>, \<script\>, \<style\>**의 세 가지로 나누어져 있는것을 알 수 있습니다. 일단은 정확하게 이해할 수는 없는 문법으로 이루어져 있는 것 같습니다. 차차 이해해 봅시다.

---

#### Vue3 Webpack Template

이전에 만들었던 Webpack Template을 Github에서 가져오도록 하겠습니다. `git clone`명령어가 아닌, `npx degit`을 이용하여 버전 복사가 아닌 최신 파일만 복사해오겠습니다.

> **npx degit funkyblues/fastcampus-frontend vue3-webpack-template**

자 그럼 이 Webpack 프로젝트를 우리의 VueJS 프로젝트에 맞게 수정을 해보겠습니다. 먼저 **src**폴더를 생성하고 폴더 안에 **App.vue**와 **main.js**파일을 만들겠습니다. 이전의 **js**폴더는 제거해주겠습니다. 작성한 파일들이 정상적으로 작동할 수 있도록, VueJS 패키지를 설치하겠습니다. 다음과 같이 작성해야 최신의 3버전이 설치될 겁니다. <br>또 VueJS는 개발에만 사용되는 것이 아니고 **실제 브라우저에서 동작하는 프레임워크**이기 때문에, 일반 의존성으로 설치해주어야 합니다.

> **npm i vue@next**

---

VueJS 파일을 프로젝트 내부에서 관리하려면, Webpack과 관련된 추가적인 패키지들을 설치해야 합니다. **@vue/compiler-sfc**패키지가 VueJS 파일을 컴파일해서 브라우저에서 동작할 수 있는 형태로 바꿔준다고 이해하시면 됩니다.

> **npm i -D vue-loader@next vue-style-loader @vue/compiler-sfc**

위에서 바꾼 상황들을 반영하여 **webpack.config.js**파일도 수정해주도록 하겠습니다.

```javascript
const { VueLoaderPlugin } = require("vue-loader");
...

module.exports = {
  ...

  module: {
    rules: [
      {
        test: /\.vue$/,
        use: "vue-loader",
      },
      {
        test: /\.s?css$/,
        use: [
          "vue-style-loader",
          "style-loader",
          "css-loader",
          "postcss-loader",
          "sass-loader",
        ],
      },
    ],
  },
  plugins: [
    new HtmlPlugin({
      template: "./index.html",
      favicon: "./static/images/logo.jpg",
    }),
    new CopyPlugin({
      patterns: [{ from: "static" }],
    }),
    new VueLoaderPlugin(),
  ],
};
```

.vue로 끝나는 파일들을 찾는 정규 표현식을 작성하였고, 찾은 VueJS파일들을 읽는 역할을 하는 **vue-loader**에게 전달해줍니다. 그리고 **vue-style-loader**를 순서를 잘 맞추어서 맨 마지막에 vue-style이 적용되도록 하였습니다. **plugin**에 vue-loader에서 가져온 모듈을 **생성자 함수**로 추가하였습니다.

---

출력이 잘 되는지, App.vue에 테스트로 작성해보겠습니다.

```vue
<template>
  <h1>{{ message }}</h1>
</template>

<script>
export default {
  data() {
    return {
      message: "Hello Vue!!!",
    };
  },
};
</script>
```

**\<template\>** 옵션을 작성하고 안에 **\<h1\>** 태그를 입력해줍니다. 그리고 아래에 **\<script\>**옵션을 작성하고 안에 **export default**를 설정하여 객체 리터럴을 반환하는 코드를 작성하겠습니다. 먼저 **data()**메소드를 작성하고 이 메소드 안에서 객체 리터럴을 반환하겠습니다. 해당 객체는 **message**라는 속성을 갖습니다. 그리고 **\<h1\>** 태그에 **\{\{ message \}\}**를 작성해줍니다.

---

main.js에서 App.vue라는 파일이 Vue 프로젝트의 시작이 되도록 설정하겠습니다. **App.vue**에서 작성한 내용들은, `export default`를 통해 `createApp()`함수의 인자로 전달되었습니다. 또한 CSS 선택자를 이용하여 `mount()`함수를 사용해줄 수 있습니다.

```javascript
import { createApp } from "vue";
// import App from "./App.vue";
// 이렇게확장자가 명시되어 있지 않으면 오류남.
import App from "./App";
// -> webpack을 통해 개선해줄 수 있다
createApp(App).mount("#app");
```

편의를 위한 설정을 추가하겠습니다. App.vue, main.js 와 같이, 확장자를 계속 추가해가며 import를 하게되면 여간 귀찮은 일이 아닙니다. 따라서 **webpack.config.js**파일을 다음과 같이 설정해주도록 하겠습니다.

```javascript
module.exports = {
    resolve: {
    extensions: [".js", ".vue"],
  },
  ...
}
```

---

이제 App.vue에 연결할 새로운 Vue 파일들, 즉, **컴포넌트(Component)**를 만들어보겠습니다. **src폴더 안에 components폴더**를 만들겠습니다. 그 안에 예시로 **Helloworld.vue**파일을 만들어 보겠습니다.<br>
그리고 static 폴더 안의 images 폴더를 src 폴더로 옮기고 폴더 이름을 **assets**로 변경하겠습니다. .vue확장자를 갖는 파일 내에서 경로를 명시하기 위해선 또 역시 **Webpack의 세팅**이 필요합니다. 새로운 패키지를 추가해주어야 합니다.

> **npm i -D file-loader**

설치가 완료되었으면, **webpack.config.js**파일을 수정하겠습니다. `module.exports` 객체 안의 module속성에 다음 내용을 추가해주도록 하겠습니다.

```javascript
module.exports = {
  ...
  module: {
    ...
    {
      test: /\.(png|jpe?g|gif|webp)$/,
      use: "file-loader",
    },
  }
}

```

그리고 **webpack.config.js**내에 다음과 같이 경로 별칭도 설정해주도록 하겠습니다. **alias** 옵션안에 경로 별칭을 설정할 수 있는데, 예를 들어 **"~"**의 경우, 상수 **path**의 **내장함수 resolve()**를 사용하여 현재 **webpack.config.js**가 있는 경로에 **"src"**를 더한 것을 나타내고 있다고 할 수 있습니다.

```javascript
module.exports = {
  resolve: {
    ...
    alias: {
      "~": path.resolve(__dirname, "src"),
      assets: path.resolve(__dirname, "src/assets"),
    },
  },
};
```

---

이전과 동일하게, **file-loader**에게 test에서 지정한 파일들이 전달됩니다. <br>
그럼 이제 그림 파일들이 잘 나타나는지 확인하기 위해 **HelloWorld.vue**파일을 다음과 같이 작성하겠습니다.

```javascript
<template>
  <img src="~/assets/logo.jpg" alt="TY" />
</template>
```

그리고 App.vue파일로 가져오도록 하겠습니다.

```vue
<template>
  <h1>{{ message }}</h1>
  <HelloWorld />
</template>

<script>
import HelloWorld from "~/components/HelloWorld";
export default {
  components: {
    HelloWorld,
  },
  data() {
    return {
      message: "Hello Vue!!!",
    };
  },
};
</script>
```

**components** 속성을 선언하고 객체 데이터로 생성하였고, 그 안에 가져온 HellowWorld를 입력하였습니다. **\<template\>** 옵션에 **\<HelloWorld /\>**를 작성해줍시다. 개발 서버를 동작시키면, 그림이 잘 나오는 것을 확인할 수 있습니다.

#### ESLint

개발 의존성 패키지로 ESLint를 설치하겠습니다.

> **npm i -D eslint eslint-plugin-vue babel-eslint**

설치가 완료되었다면, 프로젝트 경로에 다음 파일을 하나 생성해주겠습니다. <br>
**.eslintrc.js**

그리고 다음과 같이 작성해주도록 하죠.

```javascript
module.exports = {
  env: {
    browser: true,
    node: true,
  },
  extends: [
    // "plugin:vue/vue3-essential", // Lv1
    "plugin:vue/vue3-strongly-recommended", // Lv2
    // "plugin:vue/vue3-recommended", // Lv3
    "eslint:recommended",
  ],
  parserOptions: {
    parser: "babel-eslint",
  },
  rules: {},
};
```

**env** 옵션은 브라우저, Node에서 동작하는 모든 전역 개념을 코드검사를 할 것인지 설정하는 부분입니다. 그리고 **extends** 옵션에는 vue 코드 규칙과 js 코드 규칙을 명시해 줍니다. <br>vue의 코드규칙은, 플러그인으로 설치한 eslint-plugin-vue의 코드규칙 3가지 중 2번째 단계의 규칙을 사용하겠습니다. <br>
**parserOptions** 에는 기본적인 코드를 분석할 수 있는 분석기를 지정해주어야 합니다. **babel**을 사용하여 구문분석을 수행하고 **eslint**를 동작시키기 위해 다음과 같이 **parser**를 지정해 주었습니다. <br>
**rules**에는 eslint에서 제공하는 규칙을 사용자가 변경해야 할 때, 작성해주면 되겠습니다.

---

<a href="https://eslint.vuejs.org/rules/" target="_blank">**vue의 코드규칙**</a>에 관한 내용은 이 링크에서 확인해볼 수 있습니다. 이제 우리는 이 규칙에 맞게 **VueJS**의 코드를 작성해야 하는 것입니다. <br>
이번에는 <a href="https://eslint.org/docs/rules/" target="_blank">**eslint rules**</a>를 검색해서 한번 확인해보죠. 그러나 이러한 eslint의 경우 어디까지나 권장사항이기 때문에, 프로젝트 팀의 합의에 맞게 규칙들을 만들어야 할 것입니다.

---

###### ESLint rules Settings

만약, ESlint의 규칙이 맘에 들지 않는다면, 수정해서 사용할 수 있습니다. 어떻게 할 수 있을까요? 이전에 방문했던 <a href="https://eslint.org/docs/rules/" target="_blank">**eslint rules**</a>를 다시 방문해봅시다. <br>
예시로 검색에 **html-closing-bracket-newline**을 입력해보겠습니다. 닫히는 꺽쇠괄호를 붙여줄 것인지, 아닌지에 대한 옵션인 것 같네요. 아래로 보다 보면 **Options**가 나타나게 됩니다.<br>
**.eslintrc.js**파일의 rules 안쪽에 다음 내용을 입력해주겠습니다.

```json
rules: {
    "vue/html-closing-bracket-newline": ["error", {
      "singleline": "never",
      "multiline": "never"
    }]
  },
```

다음과 같이 본인의 기호에 맞게 설정을 바꿀 수가 있습니다.

---

###### ESLint 자동수정

우리가 설정해 준 ESlint문법을 파일이 지키고 있지 않다면, 자동으로 변경해서 저장해주는 설정을 해주겠습니다.
<br>
VSC의 Settings로 들어가서 Open Settings(JSON)을 찾아서 클릭하겠습니다. (변경될 수 있음.) 이곳에 새로운 설정을 추가해주도록 하겠습니다.

```json
"editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
```

이와같이 기본적인 ESLint 코드규칙만 잘 명시해주면 편리하게 JS, VueJS를 이용할 수 있습니다.

---

## 프로젝트 버전 맞추기

[`Download from Here`](https://github.com/ParkYoungWoong?tab=repositories)

---

## 조건문 & 반복문

본격적인 VueJS의 문법을 배우기 이전에, 기본적인 기능을 한번 살펴보도록 하겠습니다. 클릭해서 숫자가 올라가는 template을 작성해보았습니다. 만약, 4이상이면 **4보다 크다**라는 문자열이 출력되려면 어떤 방식으로 VueJS 파일을 작성하면 될까요? 다음 코드를 보며 이해하시면 되겠습니다. `v-if`라는 **디렉티브(Directive)**를 사용해주면 됩니다.

```vue
<template>
  <h1 @click="increase">
    {{ count }}
  </h1>
  <div v-if="count > 4">4보다 큽니다!</div>
</template>
```

반복되는 데이터를 이용하려면 반복문을 처리하는 문법이 필요합니다. VueJS에서 반복문은 어떻게 사용할까요? `v-for` 디렉티브를 사용합니다.
새로운 데이터를 배열로 만들어봅시다. 데이터를 반복할 때는 각각의 데이터가 고유한지를 증명하기 위해 `:key`를 제공해주어야 합니다.

```vue
<template>
  <h1 @click="increase">
    {{ count }}
  </h1>
  <div v-if="count > 4">4보다 큽니다!</div>
  <ul>
    <li v-for="fruit in fruits" :key="fruit">
      {{ fruit }}
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      count: 0,
      fruits: ["Apple", "Banana", "Cherry"],
    };
  },
  methods: {
    increase() {
      this.count += 1;
    },
  },
};
</script>

<style lang="scss">
h1 {
  font-size: 50px;
  color: royalblue;
}
ul {
  li {
    font-size: 40px;
  }
}
</style>
```

다음과 같이 스타일 속성에는 SCSS로 해석해달라는 내용을 추가하였고,브라우저에는 배열 순서대로 출력되는 것을 확인할 수 있습니다.

---

이번에는 별개의 Component를 사용해서 배열 데이터를 출력해보도록 하죠. component폴더 안에 **Fruit.vue**파일을 만들도록 하겠습니다. 그리고 다음과 같이 작성해주겠습니다.

```vue
<template>
  <ul>
    <li>{{ name }}</li>
  </ul>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      default: "",
    },
  },
};
</script>
```

**props**라는 옵션을 설정해주겠습니다. props는 객체 데이터이고, 내부에는 name이라는 String 데이터를 받도록 하였습니다. props는 default value를 제공하여야 하기 때문에, 위와 같이 작성하였습니다.
작성한 파일을 **App.vue**에서 사용할 수 있게끔, 가져오도록 하죠.

```vue
<script>
import Fruit from '~/components/Fruit'

export default {
  components: {
    Fruit: Fruit,
  },

...

```

components라는 옵션부분에 객체 리터럴로 등록을 해주어야 사용이 가능합니다. component의 이름을 Fruit라고 명시해주고, Data도 Fruit라고 하겠습니다. <br>위의 코드는 다음과 같이 요약할 수 있겠죠? Fruit라는 이름으로 Fruit component를 활용하겠습니다.

```vue
<script>
import Fruit from '~/components/Fruit'

export default {
  components: {
    Fruit,
  },

...
```

**\<template\>**칸으로 가서 다음과 같이 hello component를 사용하겠다는 의미로, **\<hello\>** 옵션으로 작성하겠습니다. `:name="fruit"`속성도 추가하겠습니다. 이 name속성은 **Fruit.vue**의 props 옵션에 작성되어 있습니다. <br>

```vue
<template>
  ...

  <ul>
    <hello v-for="fruit in fruits" :key="fruit" :name="fruit">
      {{ fruit }}
    </hello>
  </ul>
</template>
```

즉, 다음과 같이 정리할 수 있겠습니다. VueJS 파일인 **Component**를 만들고 **App.vue**로 가져와서 연결한 후에는 **HTML 부분에서 사용할 수 있다**는 의미입니다. <br>설명한 바와 같이, Component를 여러 개 만들어서 그것을 *App.vue*또는 *Component*끼리 연결하여 웹사이트를 개발할 수 있다는 의미입니다.

---

Fruit.vue의 스타일을 지정해주도록 하겠습니다. 여기서 유의해야 할 점이 있습니다. 아래 코드를 보시죠. 다음과 같이 작성하면, **App.vue파일의 h1태그도 변경**되는 불상사가 일어납니다.

```vue
<style lang="scss">
h1 {
  color: red !important;
}
</style>
```

그러니, **scoped**란 속성을 추가해주어야 합니다. 그럼 **Fruit.vue** 파일내에서만 작동하게 되는거죠. **유효범위**가 현재 Component다라고 이해하시면 되겠습니다.

```vue
<style scoped lang="scss">
h1 {
  color: red !important;
}
</style>
```
