---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
# accent_color: "#ccc"
# theme_color: "#ccc"
description: >
  VueJS의 Component Basic에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue
---

# VueJS - Component Basic

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

--- 

이번시간부터는 VueJS의 <a href="https://v3.vuejs-korea.org/guide/forms.html#%E1%84%91%E1%85%A9%E1%86%B7-%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%85%E1%85%A7%E1%86%A8-%E1%84%87%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%83%E1%85%B5%E1%86%BC" target="_blank">폼 입력 바인딩</a>에 대해 알아보겠습니다.

1. table of contents
{:toc .large-only}

---

## Code tester 
여기서 예제코드를 테스트 해주세요
(완성된 **MyBtn.vue**파일을 components폴더에 작성해두었습니다.)
<iframe src="https://codesandbox.io/embed/frosty-lake-khrk8?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="frosty-lake-khrk8"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

## Component-basic (컴포넌트 기초)
### props
이번시간에는 VueJS의 <a href="https://v3.vuejs-korea.org/guide/component-basics.html#%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB-%E1%84%8B%E1%85%A8%E1%84%8C%E1%85%A6" target="_blank">컴포넌트 기초</a>에 대해 알아보겠습니다.
특정 패턴 혹은 데이터가 반복되는 부분이 있을 때 컴포넌트로 미리 코드를 만들게 된다면, 코드의 재활용성이 높아질 수 있습니다. 그러나 완전 동일한 내용이라면 그다지 도움이 되지 않을 것입니다. 임의로 수정할 수 있으면 좋을텐데요. <br>

물론 가능합니다. 아래 코드를 살펴봅시다. 먼저, **MyBtn.vue**파일을 **App.vue**파일에서 사용할 수 있도록 import 해주어야 합니다. 그 후 `template`태그에 `<MyBtn />`과 같이 작성해주면 되겠죠. <br> 두 번째 버튼이 `royalblue`로 출력되려면 먼저 다음과 같이 작성해주어야 합니다.

```vue
<template>
  <MyBtn />
  <MyBtn color="royalblue" />
  <MyBtn />
  <MyBtn />
</template>

<script>
import MyBtn from '~/components/MyBtn'
export default {
  components: {
    MyBtn
  }
}
</script>
```
그리고 **MyBtn.vue** 컴포넌트가, 위에서 작성한 스타일을 받을 수 있도록 `props`라는 옵션에 **color**객체를 추가하였습니다. 다음과 같이 작성해주면 되겠습니다. 

```vue
<template>
  <div
    :style="{ backgroundColor: color }"
    class="btn">
    Apple
  </div>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: 'gray'
    }
  }
}
</script>

<style scoped>
  .btn {
    display: inline-block;
    margin: 4px;
    padding: 6px 12px;
    border-radius: 4px;
    background-color: gray;
    color: white;
    cursor: pointer;
  }
</style>
```
`<template>`이 style 데이터를 받아서 변경될 수 있도록 `v-bind 디렉티브`를 사용하여 `backgroundColor: color`를 적용하도록 하였습니다. <br>
요약하자면, 위와 같이 App.vue(부모 컴포넌트)에서 MyBtn.vue(자식 컴포넌트)로 특정한 데이터를 전달할 때 쓰는 용도로 사용되어서 `부모-자식간 데이터 통신`방법이라고도 이야기 합니다. <br>

이번에는 `<script>`태그의 `data()`에 **"color"**값을 정의하여 `<template>`태그에 지정해주도록 하겠습니다. 어디까지나 **"color"**는 문자 데이터이기 때문에, `v-bind 디렉티브`를 사용해주어야 합니다.

```vue
<template>
  <MyBtn />
  <MyBtn :color="color"/>
  <MyBtn />
  <MyBtn />
</template>

<script>
import MyBtn from '~/components/MyBtn'
export default {
  components: {
    MyBtn
  }
}
</script>
```

새로운 props를 한번 생성해서 더 이해해보도록 합시다. **App.vue** 파일의 세 번째 **MyBtn**에 **large** 속성을 추가해주겠습니다. 색상도 변경해주죠.
```vue
<template>
  <MyBtn />
  <MyBtn :color="color"/>
  <MyBtn 
    large 
    color="royalblue"/>
  <MyBtn />
</template>

<script>
import MyBtn from '~/components/MyBtn'
export default {
  components: {
    MyBtn
  },
  data() {
    return {
      color: '#000'
    }
  }
}
</script>
```

그리고 **MyBtn.vue**파일에 다음 내용을 추가해주었습니다. 속**성과 데이터의 이름이 같아서** `{ large: large }`를 축약하여 `{ large }`로 작성되었다는 것을 말씀드리고 넘어가겠습니다. 또한 코드의 간략화를 위해 **SCSS**문법을 사용했습니다.

```vue
<template>
  <div
    :class="{ large }"
    :style="{ backgroundColor: color }"
    class="btn">
    Apple
  </div>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: 'gray'
    },
    large: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style scoped lang="scss">
  .btn {
    display: inline-block;
    margin: 4px;
    padding: 6px 12px;
    border-radius: 4px;
    background-color: gray;
    color: white;
    cursor: pointer;
    &.large {
    font-size: 20px;
    padding: 10px 20px;
    }
  }
</style>
```

각 버튼들의 텍스트도 임의로 변경해줄 수는 없을까요? <br> 가능합니다. 먼저 **App.vue**파일을 다음과 같이 수정해주겠습니다.

```vue
<template>
  <MyBtn>Banana</MyBtn>
  <MyBtn :color="color">
    <span style="color: red;">Banana</span>
  </MyBtn>
  <MyBtn large color="royalblue">Apple</MyBtn>
  <MyBtn>Cherry</MyBtn>
</template>

<script>
import MyBtn from '~/components/MyBtn'
export default {
  components: {
    MyBtn
  },
  data() {
    return {
      color: '#000'
    }
  }
}
</script>
```

그리고 **MyBtn.vue** 파일돌 다음과 같이 작성해주겠습니다. 
```vue
<template>
  <div
    :class="{ large }"
    :style="{ backgroundColor: color }"
    class="btn">
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: 'gray'
    },
    large: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style scoped lang="scss">
  .btn {
    display: inline-block;
    margin: 4px;
    padding: 6px 12px;
    border-radius: 4px;
    background-color: gray;
    color: white;
    cursor: pointer;
    &.large {
    font-size: 20px;
    padding: 10px 20px;
    }
  }
    
</style>
```
코드를 살펴보면, `<slot>`이란 태그를 추가해주었죠? 이 태그 안으로 **App.vue**파일에서 작성한 `<MyBtn></MyBtn>`태그 안의 내용들이**(html 태그 포함)** 들어가게 됩니다. 이렇게 텍스트도 부모 컴포넌트에서 변경해줄 수 있습니다.

### Attribute Inheritance (속성 상속)

`App.vue` 파일을 새롭게 작성해 주겠습니다.