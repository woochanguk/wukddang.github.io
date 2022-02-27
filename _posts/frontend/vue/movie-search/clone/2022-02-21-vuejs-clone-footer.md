---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  VueJS Clone Footer를 구현해봅시다.
invert_sidebar: false
categories:
  - frontend
  - vue
  - movie-search
  - vue-clone
---

# VueJS Clone - Footer

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간에는 Footer를 구현해보겠습니다.


---
## Code tester 
이번시간의 결과 모습입니다.
(codesandbox 오류. 추후 업데이트) <br>
<!-- <iframe src="https://codesandbox.io/embed/exciting-babbage-fkmwhz?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="exciting-babbage-fkmwhz"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe> -->

---

## Footer

웹사이트의 **Footer**를 생성해주도록 하겠습니다.

### Footer.vue
```vue
<template>
  <footer>
    <Logo />
    <a 
      href="https://github.com/funkyblues" 
      target="_blank">
      (c){% raw %}{{ new Date().getFullYear() }}{% endraw %} Changuk
    </a>
  </footer>
</template>

<script>
import Logo from '~/components/Logo'
export default {
  components: {
    Logo

  }
}
</script>

<style lang="scss" scoped>
  footer {
    padding: 70px 0;
    text-align: center;
    opacity: .3;
    .logo {
      display: block;
      margin-bottom: 4px;
    }
  }
</style>
```
- **Logo** 컴포넌트를 사용하게 되는데, 해당 컴포넌트안에 **.logo** 클래스를 갖는 태그를 `display: block`으로 동작되도록 하여 수직으로 쌓이게 하였습니다.


### App.vue
```vue
<template>
  <Header />
  <RouterView />
  <Footer />  
</template>

<script>
import Header from './components/Header'
import Footer from '~/components/Footer'
export default {
  components: {
    Header,
    Footer
  }
}
</script>

<style lang="scss">
  @import "./scss/main.scss";
</style>
```

**Footer**의 경우 모든 페이지에서 사용될 수 있기 때문에, **App.vue** 컴포넌트에 연결해주어서 사용하도록 하겠습니다.