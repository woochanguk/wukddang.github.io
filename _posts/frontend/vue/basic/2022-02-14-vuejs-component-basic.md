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
  - vue-basic
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

물론 가능합니다. 아래 코드를 살펴봅시다. 먼저, **MyBtn.vue**파일을 **App.vue**파일에서 사용할 수 있도록 **import** 해주어야 합니다. 그 후 `template`태그에 `<MyBtn />`과 같이 작성해주면 되겠죠. <br> 두 번째 버튼이 `royalblue`로 출력되려면 먼저 다음과 같이 작성해주어야 합니다.

```vue
<template>
  <MyBtn />
  <MyBtn color="royalblue" />
  <MyBtn />
  <MyBtn />
</template>

<script>
import MyBtn from './components/MyBtn'
export default {
  components: {
    MyBtn
  }
}
</script>
```
그리고 위에서 작성한 스타일을 **MyBtn.vue** 컴포넌트가 받을 수 있도록 `props`라는 옵션에 **color**객체를 추가하였습니다. 다음과 같이 작성해주면 되겠습니다. 

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
요약하자면, `props`는 App.vue(부모 컴포넌트)에서 MyBtn.vue(자식 컴포넌트)로 특정한 데이터를 전달할 때 쓰는 용도로 사용되어서 `부모-자식간 데이터 통신`방법이라고도 이야기 합니다. <br>

이번에는 `<script>`태그의 `data()`에 **"color"**값을 정의하여 `<template>`태그에 지정해주도록 하겠습니다. 어디까지나 **"color"**는 문자 데이터이기 때문에, `v-bind 디렉티브`를 사용해주어야 합니다.

```vue
<template>
  <MyBtn />
  <MyBtn :color="color"/>
  <MyBtn />
  <MyBtn />
</template>

<script>
import MyBtn from './components/MyBtn'
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
import MyBtn from './components/MyBtn'
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

각 버튼들의 텍스트도 임의로 변경해줄 수는 없을까요? <br> 가능합니다. **App.vue**파일을 다음과 같이 수정해주면 됩니다.

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
import MyBtn from './components/MyBtn'
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

**App.vue** 파일을 새롭게 작성해 주겠습니다.

```vue
<template>
  <MyBtn class="heropy">
    Banana
  </MyBtn>
</template>

<script>
import MyBtn from './components/MyBtn'
export default {
  components: {
    MyBtn
  },
}
</script>
```

이 상태에서 **MyBtn.vue** 파일도 수정해주겠습니다. 그러면 App.vue에서 작성한 class인 **"heropy"**도 최상위 요소 `<div>`태그의 class로 들어가 있는 것을 확인할 수 있습니다.
```vue
<template>
  <!-- 최상위 요소 -->
  <div class="btn">
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
그러나 최상위 요소가 두개가 되면, `class="heropy"`는 사라지게 됩니다. 왜 그럴까요? **MyBtn.vue** 컴포넌트를 사용하는, **App.vue**에서 작성한 속성들이 어느 최상위 컴포넌트에 적용될 지를 정확히 모르기 때문에 그냥 렌더링이 됩니다. <br>
이와 같이 컴포넌트가 사용되는 곳에서 작성해둔 클래스나, 기타 속성들이 컴포넌트 내부에 있는 하나의 요소에 연결이 되는 것을 **속성 상속(Attribute Inheritance)**이라고 이야기 합니다. 
<br>

만약, 이러한 속성 상속을 일괄적으로 적용하지 않겠다고 한다면, `<script>`태그 내부에 **inheritance** 속성에 false값을 할당해주면 됩니다.

> `inheritAttrs: false`

그렇다면 우리가 App.vue에 작성한 내용을 컴포넌트의 특정 부분에 연결해주고 싶다면 어떻게 해야 할까요? 그 부분을 확인해봅시다. **MyBtn.vue**파일의 `<script>`태그 내부를 다음과 같이 작성해보겠습니다. 출력 결과를 한번 확인해보세요.

```vue
<script>
export default {
  inheritAttrs: false,
  created() {
    console.log(this.$attrs)
  }
}
// 출력 결과 :
// Proxy {class: 'heropy', style: {…}, __vInternal: 1}
// [[Handler]]: Object
// [[Target]]: Object
// class: "heropy"
// style:
// color: "red"
// [[Prototype]]: Object
// __vInternal: 1
// [[Prototype]]: Object
// [[IsRevoked]]: false
</script>
```
이 내용은, App.vue에서 작성한 `<MyBtn>`태그 내부의 속성들이 출력된 결과입니다. 즉 `this.$attrs`를 사용하면 다른 부분에서도 이 속성들을 적용해줄 수 있겠습니다. **MyBtn.vue**파일을 다음과 같이 작성해 줍시다. 출력결과를 확인해보세요. `<h1>`태그의 클래스가 **"heropy"**로 출력이 됩니다.

```vue
<template>
  <div class="btn">
    <slot></slot>
  </div>
  <h1 
    :class="$attrs.class"
    :style="$attrs.style">
    hello
  </h1>

</template>

<script>
export default {
  inheritAttrs: false,
  created() {
    console.log(this.$attrs)
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
이와 같이 `inheritAttrs: false`를 사용하면, 컴포넌트에서 작성한 클래스와 속성을 원하는 곳에 입력해줄 수 있습니다. 전체 속성을 한번에 적용해주고 싶다면, `<h1>`태그 안의 속성을 다음과 같이 작성해 주어야 겠지요

> `v-bind="$attrs"`


### 컴포넌트 - Emit
이전에 컴포넌트에 클래스와 스타일을 적용해주었던 것처럼, 이벤트들도 직접적으로 연결해줄 수 있습니다. 어떻게 할 수 있는지 확인해봅시다. <br>
먼저 **App.vue**파일을 다음과 같이 작성합니다.

```vue
<template>
  <MyBtn @changuk="log">
    Banana
  </MyBtn>
</template>

<script>
import MyBtn from './components/MyBtn'
export default {
  components: {
    MyBtn
  },
  methods: {
    log() {
      console.log('Click!!')
    }
  }
}
</script>
```
그리고 **MyBtn.vue** 컴포넌트의 `<script>`태그에 다음과 같이 배열을 작성하였습니다. **emits**옵션 안의 배열 속성인 `'click'`은 `@click`과 연결되는 의미로 이해하시면 되겠습니다. (물론 꼭 **click**이라는 이름을 가질 필요는 없습니다.) 이제, **ABC**를 클릭해주면 **App.vue** 파일의 **methods** 옵션 안에 있는`log()` 메서드가 실행되는 것을 볼 수 있습니다.

```vue
<template>
  <div class="btn">
    <slot></slot>
  </div>
  <h1 @click="$emit('click')">ABC</h1>
</template>

<script>
export default {
  emits: [
    'changuk'
  ]
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
하지만 MyBtn.vue파일의 h1태그를 다음과 같이 변경하면, 더블클릭을 해야지만 이벤트가 실행됩니다.

>`<h1 @dblclick="$emit('click')">ABC</h1>`

즉 **emits**를 사용할 때, 이벤트를 **전달**하는 것이라면 아무 이름이나 사용하여도 상관없지만 실제 이벤트를 **사용**할 때에는, 정확한 이름을 작성하여야 한다는 의미입니다. <br>

**App.vue**파일의 `log()메서드` 안에 **event** 객체를 출력하는 코드를 추가해서 **event** 객체가 어떻게 출력되는지 확인해봅시다. 

```vue
<template>
  <MyBtn @hi="log">
    Banana
  </MyBtn>
</template>

<script>
import MyBtn from '~/components/MyBtn'

export default {
  components: {
    MyBtn
  },
  methods: {
    log(event) {
      console.log('Click!!'),
      console.log(event)
    }
  }
}
</script>
```
현재는 **event** 객체를 출력했을 때, **undefined**만 출력되게 됩니다. `$emit()` 메서드의 두 번째 인자로서 데이터를 넘겨주면, 넘겨준 데이터가 출력됩니다.

---

최종적인 정리를 통해 `emit()`을 이해해봅시다. <br> 
먼저 **MyBtn.vue**파일을 다음과 같이 작성해주죠. `v-model 디렉티브를` 사용하여 `data()` 옵션에 작성한 **msg** 데이터를 **양방향 데이터 바인딩**으로 연결해주고 **watch** 옵션을 사용하여 **msg** 데이터의 변화를 감지하도록 작성하였습니다. <br>

```vue
<template>
  <div class="btn">
    <slot></slot>
  </div>
  <h1 @dblclick="$emit('hi', $event)">
    ABC
  </h1>
  <input 
    type="text" 
    v-model="msg" />
</template>

<script>
export default {
  emits: [
    'hi',
    'changeMsg'
  ],
  data() {
    return {
      msg: ''
    }
  },
  watch: {
    msg() {
      this.$emit('changeMsg', this.msg)
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
만약 **msg**데이터가 변화한다면 `$emit()`메서드를 실행해서  *emits** 옵션에 작성한 배열 속의 `changeMsg`메서드를 작동시킵니다. 이 메서드는 **App.vue** 파일에 연결되어 있는 `<MyBtn>`컴포넌트 안의 **@change-msg** 속성을 실행하게 됩니다. 이는 `"logMsg()"`메서드를 실행하게 되고 `logMsg()` 메서드는 **msg**를 데이터를 출력하게 됩니다. 이 출력하는 것은, **MyBtn.vue** 컴포넌트에 작성한 `this.$emit('changeMsg', this.msg)`의 this.msg로 입력됩니다. 즉 `console.log(msg)`가 **출력**되는 것이지요.

### 컴포넌트 - Slot
**App.vue**파일에 연결한 `<MyBtn>`컴포넌트 태그를 비워두면, 출력되는 결과가 사라지게 됩니다. 이를 방지하기 위해 `<slot>`태그를 사용합니다. 아래 예제를 보시죠. App.vue를 다음과 같이 수정해주겠습니다.

```vue
<template>
  <MyBtn />
</template>

<script>
import MyBtn from './components/MyBtn'

export default {
  components: {
    MyBtn
  }
}
</script>
```
그리고 MyBtn.vue파일을 다음과 같이 작성해주면, `<MyBtn>` 컴포넌트 태그를 비워도 기본 텍스트가 출력되는 것을 확인할 수 있습니다. 이를 **Fallback Contents**라고 합니다.

```vue
<template>
  <div class="btn">
    <slot>Apple</slot>
  </div>
</template>

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

`<slot>`태그의 추가적인 기능이 사실 더 중요합니다. 만약 출력하려는 데이터가, 코드를 작성한 순서에 관계없이 내가 원하는대로 출력하게 하려면 어떻게 작성해주어야 할까요? `<slot>` 태그를 사용하여 연결해주면 부모-자식 컴포넌트를 연결해주면 됩니다. 먼저 App.vue파일을 다음과 같이 작성합시다. <br>

참고로 `v-slot:` 속성은 `#`으로 축약할 수 있습니다.

```vue
<template>
  <MyBtn>
    <template v-slot:text>
      <span>Banana</span>
    </template>
    <template v-slot:icon>
      <span>(B)</span>
    </template>
  </MyBtn>
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

순서는 **Banana**가 앞에 있음에도, **(B)**가 먼저 출력되게 됩니다. 바로 **MyBtn.vue** 파일을 다음과 같이 작성해주면요.

```vue
<template>
  <div class="btn">
    <slot name="icon"></slot>
    <slot name="text"></slot>
  </div>
</template>

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
### 컴포넌트 - Provide, Inject

이번에는 컴포넌트의 Provide와 Inject에 대해 알아보겠습니다. **App.vue**파일을 다음과 같이 작성해주죠.

```vue
<template>
  <Parent :msg="message" />
</template>

<script>
import Parent from './components/Parent'
export default {
  components: {
    Parent,
  },
  data() {
    return {
      message: 'Hello world!',
    }
  },
}
</script>
```
**App.vue**파일은 `<Parent>` 컴포넌트를 연결하여 사용하고 있고 **message** 데이터를 **props**형태로 전달해주고 있습니다. <br>

**Parent.vue**파일을 다음과 같이 작성해주겠습니다. `<Child>` 컴포넌트를 연결하여 **props**로 받은 **msg** 데이터를 **props** 형태로 전달해주고 있습니다.

```vue
<template>
  <Child :msg="msg" />
</template>

<script>
import Child from './Child'

export default {
  components: {
    Child,
  },
  props: {
    msg: {
      type: String,
      default: '',
    },
  },
}
</script>
```
이번에는 **Child.vue**파일을 확인하겠습니다. **props**로 받은 **msg** 데이터를 
문자열로 출력해주고 있습니다. 그럼 어떤 문자가 출력이 될까요? App.vue파일에서 정의해 둔 "**Hello world!**"가 출력이 됩니다.
```vue
<template>
  <div>
    {% raw %}{{ msg }}{% endraw %}
  </div>
</template>

<script>
export default {
  props: {
    msg: {
      type: String,
      default: '',
    },
  },
}
</script>
```
결국 **App.vue**에서 정의한 문자열을 **Child** 컴포넌트에서 출력하기 위해서 데이터를 **props**를 이용해 한 단계씩 전달해주고 있다고 이해하시면 되겠습니다. 따라서 데이터를 사용하지 않는 **Parent** 컴포넌트에서도, **props** 옵션을 사용해서 데이터를 다뤄주어야 한다는 문제점이 존재합니다. <br>

이와 같은 부분들을 조금 더 개선하기 위해 사용하는 것이 바로 `Provide`, `Inject` 입니다. 그럼 **App.vue**파일을 수정해보겠습니다.

```vue
<template>
  <Parent />
</template>

<script>
import Parent from './components/Parent'
export default {
  components: {
    Parent,
  },
  data() {
    return {
      message: 'Hello world!',
    }
  },
  provide() {
    return {
      msg: this.message
    }
  }
}
</script>
```
`provide()` 메서드는 **this**를 이용하여 **message**를 사용할 수 있기 때문에 다음과 같이 작성하였습니다. 이렇게 되면, 이제는 더 이상 **Parent 컴포넌트를 중간 매개체로 사용할 필요가 없습니다**. 따라서 이전에 **props** 형태로 작성한 내용을 삭제해 주어도 됩니다. 또한 **Parent** 컴포넌트에서도 **props**를 전달해 줄 필요가 없기 때문에, 이전에 작성한 **props** 형태를 삭제해 주어도 됩니다. <br>

**App.vue**에서 `provide()` 메서드를 사용하여 문자 데이터를 **msg**로 제공하고 있기 때문에, **Child** 컴포넌트에서도 **msg**로 데이터를 받아야 합니다. 이 때 **Inject** 옵션을 사용해주게 됩니다. 다음과 같이 작성해주면 됩니다.

```vue
<template>
  <div>
    Child: {{ msg }}
  </div>
</template>

<script>
export default {
  inject: ['msg']
}
</script>
```
`['msg']`가 하나의 데이터처럼 취급이 되어, 이중 중괄호 구문으로 데이터를 출력할 수 있게 됩니다. 코드를 확인해보면, 정상적으로 **Child** 컴포넌트에서 동작하는 것을 알 수 있습니다. <br>

굉장히 편리하게 보입니다. 하지만 여기서도 주의해야 할 점이 하나 있습니다. 다음 **App.vue**파일을 보시죠

```vue
<template>
  <button @click="message= 'Good?'">
    Click!
  </button>
  <h1>App: {{ message }}</h1>
  <Parent />
</template>

<script>
import Parent from './components/Parent'
export default {
  components: {
    Parent,
  },
  data() {
    return {
      message: 'Hello world!',
    }
  },
  provide() {
    return {
      msg: this.message
    }
  }
}
</script>
```
버튼을 누르면 **message** 데이터가 변하게 됩니다. 결과는 한번 살펴보시고, 계속 진행하도록 하겠습니다. <br>

`App: `은 변경된 데이터가 출력이 되었습니다. 그러나, `provide()` 메서드로 전달된 Child 컴포넌트의 데이터는 변경되지 않았습니다. 즉, `provide()` 메서드로 전달할 때는 **반응성을 전달해줄 수 없습니다.** 반응성을 전달해주려면 다음과 같은 옵션을 가져와 주시면 되겠습니다.

> `import { computed } from vue`

이 후에, `this.message` 부분을 `computed()` 메서드로 변경해주면 되겠습니다. 

```vue
<template>
  <button @click="message= 'Good?'">
    Click!
  </button>
  <h1>App: {{ message }}</h1>
  <Parent />
</template>

<script>
import Parent from './components/Parent'
import { computed } from 'vue'
export default {
  components: {
    Parent,
  },
  data() {
    return {
      message: 'Hello world!',
    }
  },
  provide() {
    return {
      msg: computed(() => this.message)
    }
  }
}
</script>
```
마지막으로 **Child** 컴포넌트로 와서 **msg (객체) 데이터**의 값을 사용해야 하기 때문에, `value`라는 속성을 사용해주시면 됩니다.

```vue
<template>
  <div>
    Child: {{ msg.value }}
  </div>
</template>

<script>
export default {
  inject: ['msg']
}
</script>
```
즉 `Provide`, `Inject`로 받은 데이터를 반응성 있게 만들어주려면, **computed** 옵션을 통해 내부에 **callback 함수**를 실행하고 함수 내에 반응성을 가지도록 원하는 데이터를 작성하여 반환해 주시면 됩니다. 이렇게 되면 **계산된 데이터를 msg로 반환**해줄 수 있게되고 반환된 내용이 **Child** 컴포넌트로 전달되게 되는 것입니다.

### 컴포넌트 - Refs
**VueJS**에서 특정 요소나, 컴포넌트를 참조할 수 있는 `Refs`에 대해 알아보겠습니다. **App.vue** 파일을 확인해보시죠. 다음과 같이 요소를 선택해줄 수 있습니다.

```vue
<template>
  <h1 ref="hello">
    Hello world!
  </h1>
</template>

<script>
export default {
  mounted() {
    console.log(this.$refs.hello.textContent)
  }
}
</script>
```
다음과 같이 우리는, **VueJS**에서 제공하는 `ref`라는 속성을 통해 해당 요소를 참조할 수 있고 `$refs`라는 객체 데이터를 통해 저장되는 구조를 갖고 있습니다. <br>

유의할 점은 이러한 요소들은 **컴포넌트가 HTML 구조와 연결(mounted)된 직후에만 사용할 수 있습니다**.<br>

컴포넌트를 **ref**로 참조할 수도 있습니다. **components** 폴더에 새로운 **Hello.vue**파일을 생성하였습니다. **App.vue**로 사용해보도록 하죠.

```vue
<template>
  <Hello ref="hello" />
</template>

<script>
import Hello from './components/Hello'

export default {
  components: {
    Hello,
  },
  mounted() {
    console.log(this.$refs.hello)
  }
}
</script>
```
이와 같이 작성해주면 **VueJS**에서 사용해줄 수 있는 여러 객체들이 있습니다. 이 중 `$el`속성이 있는데, 이는 최상위 요소를 출력해줍니다. <br>

여기서도 주의해야 할 점이 있습니다. **컴포넌트를 참조할 때 최상위 요소가 하나일 경우는 거의 없을 것입니다**. 따라서 내가 원하는 부분만 참조할 수 있도록 작성해 주어야 합니다. 다음과 같이 작성한 **Hello.vue** 파일을 한번 보시죠.

```vue
<template>
  <h1>Hello~</h1>
  <h1 ref="good">Good?</h1>
</template>
```

두 번째 `<h1>` 태그를 사용하려면 어떻게 해야 할까요? **App.vue**에 다음과 같이 작성해주면 됩니다. `$ref` 속성을 **연속해서 사용**해주면 되는 것이죠.

```vue
<template>
  <Hello ref="hello" />
</template>

<script>
import Hello from './components/hello'

export default {
  components: {
    Hello,
  },
  mounted() {
    console.log(this.$refs.hello.$refs.good)
  }
}
</script>
```