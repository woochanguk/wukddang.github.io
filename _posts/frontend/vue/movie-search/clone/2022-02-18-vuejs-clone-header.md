---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  VueJS Clone 웹사이트 헤더를 클론해봅시다.
invert_sidebar: false
categories:
  - frontend
  - vue
  - movie-search
  - vue-clone
---

# VueJS Clone - Header

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간에는 헤더를 구성해보도록 하겠습니다.

---
## Code tester 
이번시간의 결과 모습입니다.
<iframe src="https://codesandbox.io/embed/xenodochial-dirac-5z5cxd?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="xenodochial-dirac-5z5cxd"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

---

## Header

### Header - Nav

웹사이트의 Header를 구성해봅시다. 먼저, <a href="https://getbootstrap.com/docs/5.1/components/navs-tabs/#pills" target="_blank">**Bootstrap**</a>의 Docs를 확인해보죠. 만들려는 사이트의 버튼으로 적합한 모양인 것 같습니다. 이걸 사용합시다. 

```html
<ul class="nav nav-pills">
  <li class="nav-item">
    <a class="nav-link active" aria-current="page" href="#">Active</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link disabled">Disabled</a>
  </li>
</ul>
```
**class**에 **active**가 있는 **item**을 사용하겠다는 의미로 이해하시면 되겠습니다. 이제 **components**폴더 안에 **Header.vue**파일을 생성하고 내용을 추가해 줍시다. <br>

`<a>`태그를 사용하여 직접 **URL**을 작성하면서 페이지를 구분해주어도 되지만, 계속 태그를 작성하기는 번거롭기 때문에 **VueJS**에서는 `<RouterLink>`라는 컴포넌트를 지원해줍니다. 해당 컴포넌트를 작성하고, **class**를 **nav-link**로 구성해주면 되겠습니다. <br>
그리고 `<script>`태그의 `data()` 메서드 내부에 **return**되는 **navigations** 옵션속에, 페이지들을 구분해두었습니다. 이 내용들을 `<template>`태그 내부에서 사용해주면 되는 것입니다. <br>
**navigations** 옵션의 객체를 하나씩 조회하기 위해 `v-for 디렉티브`를 사용했고, 특정한 값을 의미해주기 위해 `v-bind 디렉티브`로 **key**속성에 **navigations** 옵션 각 객체들의 **name**을 할당해주었습니다. **class**는 **Bootstrap** 코드처럼 `nav-item`으로 작성해주었구요. 이제 <RouterLink>컴포넌트에 `v-bind 디렉티브`를 활용해서 **to**속성에 **URL**주소를 할당해주었습니다.

```vue
<template>
  <header>
    <div class="nav">
      <div
        v-for="nav in navigations"
        :key="nav.name"
        class="nav-item">
        <RouterLink 
          :to="nav.href"
          class="nav-link">
          About
        </RouterLink>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  data() {
    return {
      navigations: [
        {
          name: 'Search',
          href: '/'
        },
        {
          name: 'Movie',
          href: '/movie'
        },
        {
          name: 'About',
          href: '/about'
        },
      ]
    }
  }
}
</script>
```

자, 다음과 같이 잘 작성해주었다면 이제 **App.vue**파일에 연결해주도록 합시다. <br> `<Header>` 컴포넌트를 `<script>`태그 내부의 **components**옵션 속에 작성해주고, `<template>`태그 내부의 `<RouterView>`컴포넌트 위쪽에 작성해주면 화면에 출력이 될 것입니다. <br> 만약 페이지가 변경된다면, 변경되는 페이지는 `<RouterView>`에서 변경되어 출력될 것이고, `<Header>`는 페이지가 변경되어도 상관없이 **그대로 출력**이 될 것입니다.

```vue
<template>
  <Header />
  <RouterView />
</template>

<script>
import Header from './components/Header'
export default {
  components: {
    Header
  }
}
</script>


<style lang="scss">
@import "./scss/main.scss"
</style>
```

만들어진 웹사이트의 구조를 살펴봅시다. 만약 About 페이지가 열려있으면, `class="nav"`의 **about** 버튼이 열려있는 상태라는 **class**의 요소들을 확인할 수 있습니다. 다른 페이지가 열려있다면 다른 페이지에 아래 **class**요소들이 들어가 있게 되는 것이죠.

```html
<div class="nav-item">
  <a href="#/about" class="router-link-active router-link-exact-active nav-link" aria-current="page">
    About
  </a>
</div>
```

navigation 버튼들을 active한 상태로 출력하고 싶은데, 출력하려면 어떻게 하는게 좋을까요?
<a href="https://router.vuejs.org/guide/#html">**Vue Router**</a> 링크를 보면, **Vue-Router**는`<router-link>`기능을 제공해주고 있습니다. <br>여담으로 링크에선 `<RouterView>`컴포넌트를 **dash**케이스로 작성하였지만 3버전에서는 **Pascal**케이스로 작성하여도 됩니다. 우리는 `<RouterLink>`컴포넌트의 **active-class**기능을 사용할 것입니다. <br>

**Header.vue**파일의 `<template>`태그 부분을 다음과 같이 변경해주면 되겠습니다. 이제 버튼을 클릭하면, 배경색이 가득 차있는 채로 출력이 될 것입니다.

```vue
<template>
  <header>
    <div class="nav nav-pills">
      <div
        v-for="nav in navigations"
        :key="nav.name"
        class="nav-item">
        <RouterLink 
          :to="nav.href"
          active-class="active"
          class="nav-link">
          {{ nav.name }}
        </RouterLink>
      </div>
    </div>
  </header>
</template>
```

### Header - Logo and Google Fonts
이번에는 웹사이트의 로고를 만들어 줍시다. img파일이 아닌, 직접 작성할 수 있는 글자이므로 **Google Fonts**의 도움을 받아 만들어보죠. <a href="https://fonts.google.com/" target="_blank">**Google Fonts**</a> 사이트에서 **Roboto**폰트 400, 700과 **Oswald**폰트 500사이즈를 받아서 **index.html**파일에 `<link>`태그로 삽입해줍시다. <br>

우리의 웹사이트 전체에서 **Roboto**폰트를 사용해주기 위해 **index.html**에 `<style>`태그를 추가해주겠습니다.

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Webpack project!</title>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/reset-css@5.0.1/reset.min.css"
    />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Oswald:wght@500&family=Roboto:wght@400;700&display=swap"
      rel="stylesheet"
    />
    <style>
      body {
        line-height: 1.4;
        font-family: 'Roboto', sans-serif;
      }
    </style>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```
다 완료가 되었다면, **components**폴더에 **Logo.vue**파일을 생성하고 다음 내용을 입력하도록 하겠습니다. **Logo**의 폰트는 **Oswald**를 사용하겠습니다. <br>

일전의 기본색을 우리가 원하는 색상으로 변경해서 작성해주었던, **main.scss**파일을 가져와서 사용하겠습니다.

```vue
<template>
  <RouterLink
    to="/"
    class="logo">
    <span>OMDbAPI</span>.COM
  </RouterLink>
</template>

<style lang="scss" scoped>
@import "~/scss/main";

.logo {
  font-family: "Oswald", sans-serif;
  font-size: 20px;
  color: $black;
  span {
    color: $primary;
  }
}
</style>
```
마지막으로 **Header.vue**파일에 **Logo.vue**를 연결해주면 우리가 작성한 내용이 잘 나오게 될 것입니다. 스타일도 변경해주고, 보기좋게 만들어 줍시다.

```vue
<template>
  <header>
    <Logo />
    <div class="nav nav-pills">
      <div
        v-for="nav in navigations"
        :key="nav.name"
        class="nav-item">
        <RouterLink 
          :to="nav.href"
          active-class="active"
          class="nav-link">
          {{ nav.name }}
        </RouterLink>
      </div>
    </div>
  </header>
</template>

<script>
import Logo from './Logo'
export default {
  components: {
    Logo
  },
  data() {
    return {
      navigations: [
        {
          name: 'Search',
          href: '/'
        },
        {
          name: 'Movie',
          href: '/movie'
        },
        {
          name: 'About',
          href: '/about'
        },
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
  header {
    height: 70px;
    padding: 0 40px;
    display: flex;
    align-items: center;
    .logo {
      margin-right: 40px;
    }
  }
</style>
```

### Header - Headline
**Header**밑의 내용을 추가해주는 코드를 작성해봅시다. **components**폴더 안에 **Headline.vue**파일을 생성해주고 다음과 같이 작성해줍시다.

```vue
<template>
  <h1>
    <span>OMDb API</span> <br/>
    THE OPEN<br/> 
    MOVIE DATABASE
  </h1>
  <p>
    The OMDb API is a RESTful web service to obtain movie information, all content and images on the site are contributed and maintained by our users.<br/>
    If you find this service useful, please consider making a one-time donation or become a patron.
  </p>
</template>

<style lang="scss" scoped>
@import "~/scss/main";
  h1 {
    line-height: 1;
    font-family: "Oswald", sans-serif;
    font-size: 80px;
    span {
      color: $primary;
    }
  }
  p {
    margin: 30px 0;
    color: $gray-600;
  }
</style>
```
작성해주고 **Home.vue**컴포넌트에 연결해주면, 잘 출력되는것을 확인할 수 있습니다. 사이트를 보면 **Headline**이 여백없이, 왼쪽에 붙어있습니다. **Headline**을 웹사이트 가운데로 정렬해주고 싶어지네요. 해봅시다. 바로 **Bootstarp**의 <a href="https://getbootstrap.com/docs/5.1/layout/containers/" target="_blank">**containers**</a>를 사용해주면 됩니다. <br>

**Headline.vue**파일을 다음과 같이 수정해주었습니다.
```vue
<template>
  <div class="container">
    <h1>
      <span>OMDb API</span> <br/>
      THE OPEN<br/> 
      MOVIE DATABASE
    </h1>
    <p>
      The OMDb API is a RESTful web service to obtain movie information, all content and images on the site are contributed and maintained by our users.<br/>
      If you find this service useful, please consider making a one-time donation or become a patron.
    </p>
  </div>
</template>

<style lang="scss" scoped>
@import "~/scss/main";
  .container {
    padding-top: 40px;
  }
  h1 {
    line-height: 1;
    font-family: "Oswald", sans-serif;
    font-size: 80px;
    span {
      color: $primary;
    }
  }
  p {
    margin: 30px 0;
    color: $gray-600;
  }
</style>
```
이제 여백이 자동으로 생기면서 가운데 정렬이 되어있습니다. 다시 위의 링크로 들어가서 **container**의 내용을 살펴보면 여러개의 .container class 속성들이 존재하는데, 간략하게 말씀드리면 **Extra small**, **Small**은 모바일, **Medium**, **Large**는 태블릿, **X-Large**, **XX-Large**는 데스크탑에 적용되는 사항이라고 생각해주시면 되겠습니다. 물론 우리가 원하는 크기로 설정을 해줄 수도 있죠. 추후에 적용해보도록 하겠습니다.

### Header - userImage

웹사이트 우측 상단에 나타나고, 클릭하면 **About** 페이지로 이동하는 사용자의 이미지를 출력하겠습니다. 다음과 같이 **Header.vue** 컴포넌트를 수정하겠습니다.

```vue
<!--Header.vue-->
<template>
  <header>
    <Logo />
    <div class="nav nav-pills">
      <div
        v-for="nav in navigations"
        :key="nav.name"
        class="nav-item">
        <RouterLink 
          :to="nav.href"
          active-class="active"
          :class="{ active: isMatch(nav.path)}"
          class="nav-link">
          {{ nav.name }}
        </RouterLink>
      </div>
    </div>
    <div 
      class="user" 
      @click="toAbout">
      <img 
        :src="image" 
        :alt="name" />
    </div>
  </header>
</template>

<script>
import Logo from './Logo.vue'
export default {
  components: {
    Logo
  },
  data() {
    ...
  },
  computed: {
    image() {
      return this.$store.state.about.image
    },
    name() {
      return this.$store.state.about.name
    }
  },
  methods: {
    isMatch(path) {
      if (!path) {
        return false
      }
      console.log(this.$route)
      return path.test(this.$route.fullPath)
    },
    toAbout() {
      console.log('!!!')
      this.$router.push('/about')
    }
  }
}
</script>
<style>

</style>
```
`toAbout()` 메서드를 사용하여 해당 기능을 구현하였습니다.
- `toAbout()`
  - **methods** 옵션 내부에 해당 메서드를 정의했고 **router** 플러그인에서 제공하는 메서드 `push()`를 사용해서 about페이지로 이동하였습니다.

- **computed 옵션**
  - **store**에 저장한 **about** 모듈 내부의 데이터중 **image**와 **name**을 사용하여 이미지를 출력하였습니다.