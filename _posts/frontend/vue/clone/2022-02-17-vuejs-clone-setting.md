---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  VueJS Clone 프로젝트 세팅을 해봅시다.
invert_sidebar: false
categories:
  - frontend
  - vue-clone
---

# VueJS Clone - Setting

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간부터는 `VueJS`를 사용하여 웹사이트를 클론해보도록 하겠습니다.

---
## Code tester 
여기서 예제코드를 테스트 해주세요
(완성된 **MyBtn.vue**파일을 components폴더에 작성해두었습니다.)
<iframe src="https://codesandbox.io/embed/adoring-panka-fpye0g?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="adoring-panka-fpye0g"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

---

## Vue-Router

웹사이트 URL을 살펴보면, 주소가 `/`로 나뉘어져 있는 것을 알 수 있습니다. 이것은 웹사이트에서 페이지를 나누는 방식인데 이것을 **Router**기술이라고 합니다. 이제 VueJS로 웹사이트 클론을 할 때 Router 기술을 사용하기 위해, <a href="https://router.vuejs.org/installation.html" target="_blank">**Vue-Router**</a>패키지를 사용하도록 하겠습니다. 

> `npm i vue-router@4`

설치가 완료되었다면, **main.js**파일로 router를 가져오도록 합시다. **main.js** 파일을 다음과 같이 작성해줍시다.

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './routes/index.js'

createApp(App).use(router).mount('#app')
```
`use() 메서드`는 현재 프로젝트의 **특정 플러그인을 연결**할 때 사용하는 메서드입니다. 따라서 **Vue-Router**로 구성한 페이지를 관리해주는 특정 플러그인을 **router**라는 이름으로 가져와서 연결하는 개념입니다. 그러면 위의 경로에 맞게 **index.js**파일을 생성해주겠습니다. <br>

객체 구조분해를 사용해서 **createRouter**, **createWebHashHistory** 메서드를 가져오겠습니다. 이 두 개의 메서드를 사용해서 기본적인 페이지를 구성하고 **createRouter** 메서드를 **export default**를 사용해서 내보내도록 하겠습니다. 이 내보내진 메서드는, **main.js**에서 **router**라는 이름으로 사용되게 됩니다.

```js
import { createRouter, createWebHashHistory } from 'vue-router'
import Home from './Home'
import About from './About'


export default createRouter({
  history: createWebHashHistory(),
  routes: [
    {
      path: '/',
      component: Home
    },
    {
      path: '/about',
      component: About
    }
  ]
})
```
`createRouter()` 메서드 내부에는 다음과 같이 작성해주도록 하겠습니다. **history**와 **routes**라는 옵션을 작성하였는데 **history**는 **hash모드**, **history모드**로 구분됩니다. 일단 프로젝트 개발 시에는 hash모드를 사용하는 것이 편하기 때문에, `createWebHashHistory()` 메서드를 사용해주면 되겠습니다. <br>

**hash모드**는, `#` 키워드를 붙여주는 역할을 하는데 이는 웹사이트 특정페이지에서 새로고침을 할 떄, 페이지가 없다고 표시되는 것을 방지하기 위함입니다. <br>
**routes**내부에는 객체데이터 형태로 **path**옵션과 **component**옵션을 작성할 것인데, 특정 경로로 가면(**path**) 특정 파일을 실행(**component**)하겠다는 의미로 이해해주시면 되겠습니다. **routes**폴더 내부에 **Home.vue**, **About.vue**파일을 작성하면 됩니다. <br>

이렇게 **Vue-Router**를 활용한 기본적인 페이지 구성을 완성했습니다. 이 내용이 정상적으로 동작할 수 있도록 **App.vue**파일에 `<RouterView>`라는 컴포넌트를 작성하여 다음과 같이 만들어주겠습니다.

```vue
<template>
  <RouterView />
</template>
```
프로젝트를 실행할 때 기본적으로 **main.js**가 동작하게 되면 **App.vue**라는 파일이 실행되고, **VueJS**에서 제공하는 전체 영역에서 사용가능한(전역화) `RouterView 컴포넌트`를 사용하여 **Home**, **About**페이지를 출력하는 개념으로 구성됩니다. <br>

개발 서버를 열어서 해당 URL을 작성해주면 이동하는것을 알 수 있습니다. 이와같이 웹사이트의 페이지들을 구성해줄 수 있습니다.




## BootStrap
**VueJS Clone** 프로젝트에 **Bootstrap**을 설치하여 사용해보도록 하겠습니다. 먼저 **Bootstrap** 패키지를 설치해줍시다.

> `npm i bootstrap@5`

**main.scss**파일에 **Bootstrap**을 가져오도록 `@import` 문법을 사용해주면 됩니다. 

```scss
@import '../../node_modules/bootstrap/scss/bootstrap';
```

이렇게 만들어진 **main.scss**파일을 **App.vue**파일에 연결해주겠습니다. 
```vue
<template>
  <RouterView />
</template>

<style lang="scss">
@import "./scss/main.scss"
</style>
```
**main.scss**파일은 계속 적용되어야 하므로 **scoped** 옵션은 삭제하는게 편합니다. 이제 **Home.vue**파일을 다음과 같이 작성해주고 웹사이트를 확인하면, **Bootstrap**이 잘 적용되어 있는 것을 확인할 수 있습니다.

```vue
<template>
  <h1>Home!</h1>
  <div class="btn btn-primary">
    Home
  </div>
</template>
```

이제 이 스타일들을 기호에 맞게 수정하겠습니다. 다음 <a href="https://getbootstrap.com/docs/5.1/customize/sass/#variable-defaults">링크</a>를 보면, **Customize**에 관한 내용들이 있는 것을 확인할 수 있습니다. **Variable defaults**안의 **Required** 내용만을 복사해서 가져오도록 하겠습니다. 이 내용들을 **main.scss**파일의 내부에 다음과 같이 작성하고, **primary 클래스**를 갖는 요소들은 `#fdc000` 색상을 적용해주도록 하겠습니다.

```scss
// Default variable overrides
$primary: #fdc000;

// Required
@import '../../node_modules/bootstrap/scss/functions';
@import '../../node_modules/bootstrap/scss/variables';
@import '../../node_modules/bootstrap/scss/mixins';
@import '../../node_modules/bootstrap/scss/root';

@import '../../node_modules/bootstrap/scss/bootstrap';
```

**node_modules** 내부의 **bootstrap**폴더의 **scss**폴더를 보면 **_variables.scss**라는 파일이 있습니다. 파일을 열어서 색이 지정되어 있는 변수들을 보면, **!default**라는 옵션이 있습니다. 이는, 해당 색상이 재정의될 수 있다는 것을 명시해 둔 것입니다. **scss** 문법이니, 기억해둡시다. <br>

위의 코드에서는 variables파일을 읽어들이기 전에 `$primary`를 먼저 새로운 값으로 할당해 준 것이므로 재정의된 값이 사용된 것이라고 할 수 있습니다. **functions**, **variables**, **mixins**등 여러가지 스타일을 정의해 주고, 맨 아래쪽에 **bootstrap**이라는 기본 스타일을 가져오도록 하는 것입니다.<br>

추가적으로, **Bootstrap**을 커스터마이징할 때, 다음 코드를 **Required**라는 주석이 달린 코드들 보다 밑에 작성해주는게 좋습니다. 그렇지 않으면 오류가 나게 됩니다. 

```scss
$theme-colors: (
  "primary":    $primary,
  "secondary":  $secondary,
  "success":    $success,
  "info":       $info,
  "warning":    $warning,
  "danger":     $danger,
  "light":      $light,
  "dark":       $dark
) !default;
```

하지만, `$primary`와 같이 아직 정의되지 않은 값들을 사용하는게 아닌, `#fff`등과 같은 특정 색을 사용한다고 한다면, **Required** 코드 밑에 작성해줄 필요는 없습니다.