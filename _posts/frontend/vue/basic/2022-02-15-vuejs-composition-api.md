---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
# accent_color: "#ccc"
# theme_color: "#ccc"
description: >
  VueJS의 Composition API에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue-basic
---


# VueJS - Composition API

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

--- 

이번시간부터는 VueJS의 <a href="https://v3.ko.vuejs.org/guide/composition-api-introduction.html#%E1%84%89%E1%85%A9%E1%84%80%E1%85%A2" target="_blank">컴포지션 API</a>에 대해 알아보겠습니다.

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

## Composition API (컴포지션 API)
이번시간에는 컴포지션 API에 대해 알아보겠습니다. 먼저 App.vue파일을 다음과 같이 작성해주죠

```vue
<template>
  <h1 @click="increase">
    {% raw %}{{ count }}{% endraw %} / {% raw %}{{ doubleCount}}{% endraw %}
  </h1>
  <h1>
    {% raw %}{{ message }}{% endraw %} / {% raw %}{{ reversedMessage }}{% endraw %}
  </h1>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello world!',
      count: 0
    }
  },
  computed: {
    doubleCount() {
      return this.count * 2
    },
    reversedMessage() {
      return this.message.split('').reverse().join('')
    }
  },
  methods: {
    increase() {
      this.count += 1
    }
  }
}
</script>
```

HTML에 출력하려는 것이 무엇인지를 `<script>`태그 내에 정의된 내용들을 통해 알아봅시다. **count**가 1이면, **doubleCount**는 2가 되는 계산된 데이터로 사용하겠다는 의미입니다. 또 **reversedMessage**는 **message**를 뒤집어준 데이터를 사용하겠다는 의미입니다. <br>
`<h1>`태그를 클릭했을 때 `increase()` 메서드가 실행이 됩니다. <br>

이렇게 코드를 살펴보았을 때, 각각 구분되는 기능을 수행하는 코드들이 있습니다. 지금처럼 단순한 상태라면 상관없겠지만, 로직이 복잡해진다면 코드를 이해할 때 굉장히 번거롭게 될 것입니다. 이를 좀 더 해석하기 좋은 형태로 만들어주기 위해서 **컴포지션 API**를 사용하게 됩니다. 이전의 **App.vue**파일을 다음과 같이 바꿔줄 수 있습니다. <br>

이와같이, 우리가 배워온 내용들을 어떻게 컴포지션 API로 변경해줄 수 있을지를 배워봅시다.


```vue
<template>
  <h1 @click="increase">
    {% raw %}{{ count }}{% endraw %} / {% raw %}{{ doubleCount}}{% endraw %}
  </h1>
  <h1>
    {% raw %}{{ message }}{% endraw %} / {% raw %}{{ reversedMessage }}{% endraw %}
  </h1>
</template>

<script>
export default {
  setup() {
    const message = ref('Hello World!')
    const reversedMessage = computed(() => {
      return message.value.split('').reverse().join('')
    })

    const count = ref(0)
    const doubleCount = computed(() => count.value * 2)
    function increase() {
      count.value += 1
    }

    return {
      message,
      reversedMessage,
      count,
      doubleCount,
      increase
    }
  }
}
</script>
```

### 반응형 데이터(반응성, Reactivity)
먼저 간단한 반응형 데이터를 출력해주는 **VueJS**파일을 확인해봅시다. 
```vue
<template>
  <h1 @click="increase">
    {% raw %}{{ count }}{% endraw %}
  </h1>
</template>

<script>
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increase() {
      this.count += 1
    }
  }
}
</script>
```
이 코드를 **컴포지션 API**를 사용해서는 어떻게 구현해줄 수 있을까요? **VueJS**에서 제공해주는 **ref** 기능을 사용하면 구현할 수 있습니다. 왜냐면 **ref 키워드를 사용하지 않으면 반응성을 가지지 못하기 때문**이죠. <br>
`ref()` 메서드를 실행한 후 초기화 할 값을 인수로 넘겨주면 반응성을 가지는 데이터가 **count**에 할당되게 됩니다. 그리고 해당 데이터에 접근을 하려면 `count.value`를 사용해주면 됩니다. 그 후 이 데이터를 반환해주게 되면 `<template>` 에서 사용할 땐 **value**를 붙여줄 필요는 없습니다.
```vue
<template>
  <h1 @click="increase">
    {% raw %}{{ count }}{% endraw %}
  </h1>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {
    let count = ref(0)
    function increase() {
      count.value += 1
    }
    
    return {
      count,
      increase
    }
  }
}
</script>
```
처음 작성했던 **VueJS**파일과 동일한 기능을 하도록 만들어주었습니다.

### 기본 옵션과 라이프사이클
**원래 작성했던 방식의 VueJS**파일과 **컴포지션 API** **방식으로 작성한 VueJS**파일을 비교하면서 컴포지션 API를 익혀봅시다.

- **기존 VueJS**

```vue
<template>
  <h1 @click="increase">
    {% raw %}{{ count }}{% endraw %} / {% raw %}{{ doubleCount}}{% endraw %}
  </h1>
  <h1>
    {% raw %}{{ message }}{% endraw %} / {% raw %}{{ reversedMessage }}{% endraw %}
  </h1>
</template>

<script>
export default {
  data() {
    return {
      count: 0,
      message: 'Hello world!'
    }
  },
  computed: {
    doubleCount() {
      return this.count * 2
    },
    reversedMessage() {
      return this.message.split('').reverse().join('')
    }
  },
  watch: {
    message(newValue) {
      console.log(newValue)
    }
  },
  created() {
    console.log(this.message)
  },
  mounted() {
    console.log(this.count)
  },
  methods: {
    increase() {
      this.count += 1
    },
    changeMessage() {
      this.message = 'Good?!'
    }
  }
}
</script>
```

- **컴포지션 API**

```vue
<template>
  <h1 @click="increase">
    {% raw %}{{ count }}{% endraw %} / {% raw %}{{ doubleCount}}{% endraw %}
  </h1>
  <h1>
    {% raw %}{{ message }}{% endraw %} / {% raw %}{{ reversedMessage }}{% endraw %}
  </h1>
</template>

<script>
import { ref, computed, watch, onMounted } from 'vue'

export default {
  setup() {
    const count = ref(0)
    const doubleCount = computed(() => {
      return count.value * 2
    })
    function increase() {
      count.value += 1
    }

    const message = ref('Hello world!')
    const reversedMessage = computed(() => {
      return message.value.split('').reverse().join('')
    })
    watch(message, newValue => {
      console.log(newValue)
    })
    function changeMessage() {
      message.value = 'Good?!'
    }

    console.log(message.value)

    
    onMounted(() => {
      console.log(count.value)
    })

    return {
      count,
      doubleCount,
      increase,
      message,
      reversedMessage,
      changeMessage,
    }
  }
}
</script>
```

중간에 작성해둔 **created**, **mounted**와 같은 라이프사이클 훅은 아래 표를 참조해주세요. 다음과 같이 매핑됩니다.

|기존 라이프사이클 옵션|컴포지션 API 라이프사이클 옵션|
|:----:|:-----:|
|beforecreate|setup()|
|created|setup()|
|beforeMount|onBeforeMount|
|mounted|onMounted|
|beforeUpdate|onBeforeUpdate|
|updated|onUpdated|
|beforeUnmount|onBeforeUnmount|
|unmounted|onUnmounted|
|errorCaptured|onErrorCaptured|
|renderTracked|onRenderTracked|
|renderTriggered|onRenderTriggered|

`setup()`은 `beforeCreate`, `created` 라이프사이클 훅 사이에 실행되는 시점이므로 명시적으로 정의할 필요가 없습니다. 즉 두 훅은 `setup` 내부에 작성해주면 됩니다.

### Props / Context
**컴포지션 API**의 **Props**와 **Context**의 개념에 대해 개략적으로 알아보겠습니다. 다음 작성할 파일들을 **컴포지션 API 방식**으로 작성해보죠.
먼저, **App.vue**파일을 다음과 같이 작성하였습니다.

```vue
<template>
  <MyBtn
    class="changuk"
    style="color: red;"
    color="#ff0000"
    @hello="log">
    Apple
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
      console.log("Hello world!")
    }
  }
}
</script>
```
`<MyBtn>` 컴포넌트에 여러 속성들을 입력해주었습니다. `@hello`라는 속성을 실행하게 되면 `log()`메서드가 출력됩니다. 또 속성의 상속도 구현을 해주었습니다. `$attrs`객체는 이전에 다루었지만 한번 더 짚고 넘어가자면, **부모 컴포넌트에서 작성한 모든 속성들을 받는다**는 의미로 생각해주시면 되겠습니다. <br>

**MyBtn.vue**파일도 다음과 같이 작성해주겠습니다.

```vue
<template>
  <div
    v-bind="$attrs"
    class="btn"
    @click="bye">
    <slot></slot>
  </div>
</template>

<script>
export default {
  inheritAttrs: false,
  props: {
    color: {
      type: String,
      default: 'gray'
    }
  },
  emits: ['hello'],
  mounted() {
    console.log(this.color)
    console.log(this.$attrs)
  },
  methods: {
    bye() {
      this.$emit('hello')
    }
  }
}
</script>
```
이전에 배웠던 내용들입니다. 자 그럼 **MyBtn.vue**파일을 컴포지션 API를 적용하면 어떻게 변경해줄 수 있을까요? 다음과 같이 작성해주면 됩니다.

```vue
<template>
  <div
    v-bind="$attrs"
    class="btn"
    @click="hello">
    <slot></slot>
  </div>
</template>

<script>
// 컴포지션 API로 작성해주면??
import { onMounted } from 'vue'

export default {
  inheritAttrs: false,
  props: {
    color: {
      type: String,
      default: 'gray'
    }
  },
  emits: ['hello'],

  // 컴포지션 API로 작성해주면??
  // 컴포지션 API에선 this메서드 사용 못하니까 props로 받아주기
  setup(props, context) {
    function bye() {
      context.emit('hello')
    }
    onMounted(() => {
      console.log(props.color)
      console.log(context.attrs)
    })
  return {
    bye
  }
  }
}
</script>
```
컴포지션 API로 사용하게 되면 this메서드를 사용하지 못하므로, `setup()`메서드를 정의해주어 **props**, **context**인자를 받도록 해줍니다. **props**를 이용해서 상속받은 **props**옵션을 사용해줄 수 있고 **context**를 이용해서 상속받은 모든 속성들을 사용해줄 수 있게 됩니다. 이전에 **methods** 옵션을 사용해서 생성했던 함수도 대체해 주었습니다. 마지막으로 정의한 함수를 반환해주면 됩니다. 