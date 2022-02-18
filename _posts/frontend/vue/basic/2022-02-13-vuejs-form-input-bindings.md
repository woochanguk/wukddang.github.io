---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
# accent_color: "#ccc"
# theme_color: "#ccc"
description: >
  VueJS의 Form Input Binding에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue-basic
---

# VueJS - Form Input Binding

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

--- 

이번시간부터는 VueJS의 <a href="https://v3.vuejs-korea.org/guide/forms.html#%E1%84%91%E1%85%A9%E1%86%B7-%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%85%E1%85%A7%E1%86%A8-%E1%84%87%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%83%E1%85%B5%E1%86%BC" target="_blank">폼 입력 바인딩</a>에 대해 알아보겠습니다.

1. table of contents
{:toc .large-only}

---

## Code tester 
여기서 예제코드를 테스트 해주세요
<iframe src="https://codesandbox.io/embed/frosty-lake-khrk8?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="frosty-lake-khrk8"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>


## Form Input Binding (폼 입력 바인딩)

### One-way Data Binding (단방향 데이터 바인딩)
다음과 같이 코드를 작성해보겠습니다. 
```vue
<template>
  <input 
    type="text"
    value="Hello world!" />
</template>

<script>
export default {
  data() {
    return {

    }
  }
}
</script>
```
이와 같이 {% raw %} **\<input\>** {% endraw %}에 value를 작성해도 되지만, <script> 안에 **msg**문자열을 작성한 후 `v-bind 디렉티브`를 사용해서 값을 넣어줄 수도 있습니다. 또 **msg**문자열을 **<h1>**안에 작성해줄 수도 있습니다. 

```vue
<template>
  <h1>{% raw %}{{ msg }}{% endraw %}</h1>
  <input 
    type="text"
    :value="msg" />
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>
```
이와 같이 데이터가 한 방향으로만 이동하면서 출력하고 있는 것을 **단방향 데이터 바인딩**이라고 합니다. 왜 단방향이냐면, {% raw %} **\<input\>** {% endraw %}의 텍스트를 아무리 바꾸어도 **<h1>**가 변하지 않기 때문이죠. <br>

### Two-way Data Binding (양방향 데이터 바인딩)

이 상태에서 `@input="handler"`라는 코드를 {% raw %} **\<input\>** {% endraw %}안에 추가해보겠습니다. 이 코드의 의미는, {% raw %} **\<input\>** {% endraw %}에 특정 데이터가 입력되기 시작하면, 그러한 모든 입력들은 모두 {% raw %} **\<input\>** {% endraw %}이란 이벤트로 인식이 됩니다. 

```vue
<template>
  <h1> {% raw %}{{ msg }}{% endraw %}</h1>
  <input 
    type="text"
    :value="msg" 
    @input="handler"/>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  },
  methods: {
    handler(event) {
      console.log(event.target.value)
    }
  }
}
</script>
```
`handler()` 메서드는 `event 객체` **target**의 **value**를 출력하라는 명령입니다. 이제 이 코드를 가지고 this를 통해 msg의 값에 `<input>`태그에 입력하는 값을 할당해주는 코드도 작성하면 어떻게 될까요?

```vue
<template>
  <h1>{% raw %}{{ msg }}{% endraw %}</h1>
  <input 
    type="text"
    :value="msg" 
    @input="handler"/>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  },
  methods: {
    handler(event) {
      console.log(event.target.value)
      this.msg = event.target.value
    }
  }
}
</script>
```
확인해보면, `<h1>`태그의 값도 따라서 변경이 되는 것을 알 수 있습니다. <br>
`this.msg = event.target.value` 코드로 인해 **msg**데이터가 변경되면, 반응성으로 인해 화면이 바뀌어질 수 있는 구조입니다. 이와 같은 상황에서는 **양방향 데이터 바인딩**이 이루어졌다고 할 수 있겠습니다. <br>

#### 인라인 방식

위와 같은 방식으로 코드를 작성해도 되지만, VueJS에서는 **인라인 방식의 코드**도 지원합니다. 아래 예제를 보시죠.

```vue
<template>
  <h1>{% raw %}{{ msg }}{% endraw %}</h1>
  <input 
    type="text"
    :value="msg" 
    @input="msg = $event.target.value"/>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>
```

#### v-model

`v-model 디렉티브`를 사용하면 양방향 데이터 바인딩을 적용하면서도 코드를 더 간략화 해줄 수 있습니다. 

```vue
<template>
  <h1>{% raw %}{{ msg }}{% endraw %}</h1>
  <input 
    type="text"
    v-model="msg"/>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>
```
`v-model 디렉티브`를 활용한 추가적인 기능도 살펴봅시다. 아래쪽에 추가적인 `<input>`태그를 작성하고 `v-model="checked"`를 작성하였습니다. 그리고 `<script>`태그에 **checked: false**를 작성해주면, 이 `<input>`태그는 `<script>`태그의 **checked**와 연결됩니다. <br>
그 후 checkbox를 클릭해주면 true, false를 출력해주는 토글로 작동하게 됩니다.

```vue
<template>
  <h1>{% raw %}{{ msg }}{% endraw %}</h1>
  <input 
    type="text"
    v-model="msg" />
  <h1>{{ checked }}</h1>
  <input 
    type="checkbox"
    v-model="checked" />
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!',
      checked: false
    }
  }
}
</script>
```

이런 v-model을 사용할 때 주의할 점이 있습니다. 한글을 작성할 때는, **한 글자가 완성될 때 까지 적용이 되지 않습니다.** 따라서 한글을 입력할 때는, 양방향 데이터 바인딩의 인라인 예제에서 나온 방식처럼 작성해야 바로바로 적용이 됩니다. 

##### v-model 수식어
###### v-model.lazy
v-model의 수식어를 살펴보도록 하겠습니다. 위에서 나왔던, 양방향 데이터 바인딩의 인라인 예제의 `@input`부분을 `@change`로 수정하고 설명을 진행하겠습니다.

```vue
<template>
  <h1>{% raw %}{{ msg }}{% endraw %}</h1>
  <input 
    type="text"
    :value="msg" 
    @change="msg = $event.target.value"/>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>
```
위와 같이 작성하면, 이제는 `<input>`에 입력하는 대로 바로 변경되지는 않지만, `ESC`, `Tab` 혹은 `Enter`키를 누르면 변경사항이 적용되는 것을 확인할 수 있습니다. <br>
이 코드를 `v-model 디렉티브`의 양방향 데이터 바인딩의 단축버전으로는 어떻게 만들 수 있을까요? `v-model.lazy`를 사용해주면 됩니다.

```vue
<template>
  <h1>{% raw %}{{ msg }}{% endraw %}</h1>
  <input 
    type="text"
    v-model.lazy="msg"/>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>
```
###### v-model.number
만약 **msg**데이터에 숫자를 입력하면 문자로 인식할까요 숫자로 인식할까요? 아래의 코드를 실행해보면 알 수 있지만, **string**을 출력해줍니다.

```vue
<template>
  <h1>{{ msg }}</h1>
  <input 
    type="text"
    v-model="msg"/>
</template>

<script>
export default {
  data() {
    return {
      msg: 123
    }
  },
  watch: {
    msg() {
      console.log(typeof this.msg)
    }
  }
}
</script>
```
따라서 만약 숫자 데이터를 사용하고 싶다면 `v-model.number`를 사용해주어야 합니다. 즉 `<input>`태그에 숫자 데이터를 계속 사용하려 한다면, `v-model.number` 코드를 사용하면 되는 것이죠.

###### v-model.trim
JavaScript에서 지원하는 trim()메서드를 사용해도 되지만, VueJS도 동일한 기능을 지원해줍니다. 
```vue
<template>
  <h1>{{ msg }}</h1>
  <input 
    type="text"
    v-model.trim="msg"/>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  },
  watch: {
    msg() {
      console.log(typeof this.msg)
    }
  }
}
</script>
```
`v-model.trim`을 사용하면, 앞 뒤의 공백이 항상 제거된 상태로 유지되기 때문에 아무리 공백을 추가해도 값이 변경되지 않았다고 판단하게 됩니다.