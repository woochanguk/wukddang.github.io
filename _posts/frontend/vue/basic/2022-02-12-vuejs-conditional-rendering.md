---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
# accent_color: "#ccc"
# theme_color: "#ccc"
description: >
  VueJS의 Conditional Rendering에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue
  - vue-basic
---

# VueJS - Conditional Rendering

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

--- 

이번시간부터는 VueJS의 <a href="https://v3.vuejs-korea.org/guide/conditional.html#v-show" target="_blank">조건부 렌더링</a>에 대해 알아보겠습니다.

1. table of contents
{:toc .large-only}

---

## Conditional Rendering (조건부 렌더링)

### v-if 디렉티브
`v-if 디렉티브`는 블록을 조건에 따라 렌더링하기 위해 사용됩니다. 블록은 디렉티브의 표현식이 true 값을 반환할 때만 렌더링됩니다.

```html
<h1 v-if="awesome">Vue is awesome!</h1>
```
`v-else`와 함께 `“else 블록”`을 추가하는 것도 가능합니다. `v-if`의 값이 true 이면 **Vue is awesome!**이 출력될 것이고, `v-if`의 값이 false이면 v-else블록의 값이 출력되겠죠.

```html
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

그럼 아래와 같은 코드도 작성할 수 있겠죠?

```vue
<template>
  <button @click="handler">
    Click me!
  </button>
  <h1 v-if="isShow">
    Hello?!
  </h1>
  <h1 v-else-if="count > 3">
    Count > 3
  </h1>
  <h1 v-else>
    Good~
  </h1>
</template>

<script>
export default {
  data() {
    return {
      isShow: true,
      count: 0
    }
  },
  methods: {
    handler() {
      this.isShow = !this.isShow
      this.count += 1
    }
  }
}
</script>
```
`<h1>`에 `v-if` 디렉티브로 isShow데이터를 할당하여, 만약 데이터가 true이면 값을 출력하고, false이면 `v-else `디렉티브의 값을 출력하게되고, <br> 또 handler() 메서드가 호출될 때 마다 count의 값이 하나씩 증가하여 만약 3보다 크다면 `Count > 3`를 출력하는 코드를 작성하였습니다.
`<script>`의 methods 옵션을 사용해서 `handler()` 메서드를 정의하였습니다. 이 메서드는 Boolean 데이터 isShow가 false이면 true, true이면 false를 반환해주게 됩니다. 이제 `<template>` 안에서 `<button>`을 누르면 이 메서드가 동작되도록 코드를 작성하였습니다.

<br>
`<template>`를 사용하면 요소들을 한번에 제어할 수 있습니다. `<div>`를 사용해도 되지 않겠느냐고 생각할 수 있겠지만, `<div>`로 작성한 코드를 개발자 도구로 살펴보면 클릭할 때 마다 `<div>`생성되었다가 사라지게 됩니다. 물론 <div>를 사용해도 정상적으로 작동하지만 **최적화를 위해** `<template>`을 사용하는게 추천됩니다. <br> 단, VueJS의 최상위 `<template>`에는 `v-if`가 적용되지 않습니다.

### v-show 디렉티브

<br>
`v-if`로 요소를 보이게 했다가 사라지게 할 수 있지만, VueJS는 다른 디렉티브도 제공합니다. 바로 `v-show`입니다. `v-show`를 사용하면 해당 태그가 **display: none**으로 설정됩니다. 즉, 차이점은 `v-show`가 있는 요소는 항상 렌더링 되고 **DOM**에 남아있다는 점입니다. `v-show`는 단순히 요소에 **display CSS 속성**을 토글합니다. 또 v-show는 `<template>` 엘리먼트를 지원하지 않으며, `v-else`와 함께 쓸 수 없습니다.


### v-if vs v-show
`v-if`는 **"실제(real)" 조건부 렌더링**입니다. 전환 도중 조건부 블록 내부의 이벤트 리스너 및 자식 컴포넌트들이 올바르게 제거되고 다시 생성되기 때문입니다.

또한 `v-if`는 **게으릅니다(lazy)**. 초기 렌더링 시, 조건이 거짓(false)이면 아무 작업도 하지 않습니다. 조건부 블록은 조건이 처음으로 참(true)이 될 때까지 렌더링되지 않습니다.

이에 비해 `v-show`는 **훨씬 간단**합니다. 요소는 CSS 기반 전환으로 초기 조건과 관계없이 항상 렌더링됩니다. `v-show`는 요소를 DOM에 우선 렌더링하고 조건에 따라 CSS **display:block/display:none** 속성을 전환합니다.

일반적으로 `v-if`는 전환 비용이 높은 반면, `v-show`는 초기 렌더링 비용이 높습니다. 그러므로 **무언가를 자주 전환**해야 한다면 `v-show`를 사용하는 게 좋고, **런타임 시 조건이 변경되지 않는다면** `v-if`를 사용하는 게 더 낫습니다.
