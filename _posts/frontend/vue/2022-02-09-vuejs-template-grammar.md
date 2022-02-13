---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
# accent_color: "#ccc"
# theme_color: "#ccc"
description: >
  VueJS의 Template Grammar에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue
---

# VueJS - Template Grammar

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

이번 시간부터는 `VueJS`의 <a href="https://v3.ko.vuejs.org/guide/template-syntax.html" target="_blank">**Template Grammar**</a>에 대해 알아보겠습니다.

1. toc
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


## 보간법(Interpolation)

#### 문자열

데이터 바인딩의 가장 기본 형태는 “Mustache”(이중 중괄호 구문{% raw %}{{ }}{% endraw %})기법을 사용한 문자열 보간법(text interpolation)입니다. 다음 코드로 이해해봅시다.

```html
<span>메시지: {% raw %}{{ msg }}{% endraw %}</span>
```

Mustache 태그는 해당 컴포넌트 인스턴스의 msg 속성 값으로 대체됩니다. 또한 msg 속성이 변경될 때 마다 갱신됩니다.

<br>
v-once 디렉티브를 사용하여 데이터가 변경되어도 갱신되지 않는 일회성 보간을 수행할 수 있습니다. 다만, 이런 경우 같은 노드의 바인딩에도 영향을 미친다는 점을 유의해야 합니다.

```html
<span v-once>결코 변하지 않을 것입니다: {% raw %}{{ msg }}{% endraw %}</span>
```

---

그럼, VueJS에서 실습을 수행해보도록 하겠습니다. 

```vue
<template>
  <h1 @click="add">{% raw %}{{ msg }}{% endraw %}</h1>
</template>

<script>
export default {
  data() {
    return {
      msg: "Hello world!"
    }
  },    
  // 속성
  methods: {
    add() {
      this.msg += "!"
    }
  }
}
</script>
```

막간을 이용해서, 위 코드에서 **this**는 어디를 가리킬까요? methods의 형제까지 찾을 수 있기 떄문에, **export default** 영역 안을 의미한다고 할 수 있겠습니다. 자 그럼 이 코드를 실행하면 어떤 결과가 도출될까요? <br>바로 msg에 표현되는 문자열을 클릭하면 `!`가 계속 추가되는 것을 볼 수 있을 것입니다.

---
그럼 이번에는 v-once 디렉티브를 사용해봅시다. 다음과 같은 코드를 작성하면 이제 `!`가 추가되지 않는것을 볼 수 있습니다. `v-once 디렉티브`를 사용하여 데이터가 변경되어도 갱신되지 않는 일회성 보간을 수행할 수 있습니다.

```vue
<template>
  <h1 v-once @click="add">
    {% raw %}{{ msg }}{% endraw %}
  </h1>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!',
    }
  },
  // 속성
  methods: {
    add() {
      this.msg += '!'
    },
  },
}
</script>

```


#### 원시 HTML

이중 중괄호는 데이터를 HTML이 아닌 **일반 텍스트로 해석**합니다. 실제 **HTML을 출력하려면 v-html 디렉티브를 사용**해야 합니다.

```html
<p>이중 중괄호 사용: {{ rawHtml }}</p>
<p>v-html 디렉티브 사용: <span v-html="rawHtml"></span></p>
```

다음과 같은 코드를 작성하면, 어떤 결과가 도출될까요? 브라우저에선 실제 HTML 파일이 아닌, 텍스트로 인식하여 출력되는 것을 볼 수 있습니다. 만약 이 코드를 사용하고 싶다면, `v-html 디렉티브`를 사용하면 됩니다.

```vue
<template>
  <h1 v-once @click="add">
    {% raw %}{{ msg }}{% endraw %}
  </h1>
  <h1 v-html="msg"></h1>
</template>

<script>
export default {
  data() {
    return {
      msg: '<div style="color: red;">Hello!!</div>',
    }
  },
  // 속성
  methods: {
    add() {
      this.msg += '!'
    },
  },
}
</script>

```

#### 속성 

Mustaches(이중 중괄호 구문)는 HTML 속성에 사용할 수 없습니다. 대신 `v-bind 디렉티브`를 사용하세요.

```html
<div v-bind:id="dynamicId"></div>
```

다시 VueJS실습을 수행해보겠습니다. **h1태그** 안에 `v-bind: class="msg"`라고 작성해주겠습니다. 저장하면 자동으로 `v-bind`가 사라지게 되는데 이는, `v-bind`의 약어가 `:`기호이기 때문에 발생하는 **eslint 규칙**이라고 할 수 있겠습니다.

```vue
<template>
  <h1 v-bind:class="msg">
    {% raw %}{{ msg }}{% endraw %}
  </h1>
</template>

<script>
export default {
  data() {
    return {
      msg: 'active'
    }
  },
  methods: {
    add() {
      this.msg += '!'
    }
  }
}
</script>

<style scoped>
  .active {
    color: royalblue;
    font-size: 100px;
  }
</style>
```

클래스 부분에는 **msg**에 해당되는, **'active'**라는 문자가 실제 클래스로 사용되었습니다.
브라우저에서의 출력결과를 한번 확인해볼까요? **h1태그**의 클래스가 `active`로 잘 설정된 것을 확인할 수 있겠습니다.

```html
<h1 class="active" data-v-7ba5bd90="">active</h1>
```

## 디렉티브

디렉티브는 `v-`로 시작하는 특수한 속성입니다. 디렉티브 속성 값은 **단일 JavaScript 표현식**이 됩니다. (나중에 설명할 v-for와 v-on은 예외입니다.) 디렉티브의 역할은 표현식의 값이 변경될 때 발생하는 부수 효과(side effects)들을 반응적으로 DOM에 적용하는 것입니다. 아래 예제에서 살펴보겠습니다.

```html
<p v-if="seen">이제 저를 볼 수 있어요.</p>
```
여기서 v-if 디렉티브는 표현식 seen 값의 참, 거짓 여부를 바탕으로 <p> 엘리먼트를 삽입하거나 제거합니다.

#### 전달인자
일부 디렉티브는 디렉티브명 뒤에 `콜론(:)`으로 표기되는 전달인자를 가질 수 있습니다. 예를 들어, `v-bind` 디렉티브는 반응적으로 HTML 속성을 갱신하는 데 사용합니다.


```html
<a v-bind:href="url"> ... </a>
```
여기서 `href`는 전달인자로, `v-bind` 디렉티브가 표현식 url의 값을 엘리먼트의 href 속성에 바인딩하도록 지시합니다.
<br>
또 다른 예로는 DOM 이벤트를 수신하는 v-on 디렉티브가 있습니다.

```html
<a v-on:click="doSomething"> ... </a>
```
여기서는 수신할 이벤트명이 전달인자입니다. 이벤트 핸들링에 대해서도 좀 더 자세히 살펴볼 것입니다.

#### 동적 전달인자
##### v-bind
JavaScript 표현식을 대괄호로 묶어 디렉티브 전달인자로 사용할 수도 있습니다.

```html
<a v-bind:[attributeName]="url"> ... </a>
```

여기서 `attributeName`은 **JavaScript 표현식으로 동적 변환**되며, 변환된 결과는 전달인자의 최종값으로 사용됩니다. 예를 들어, 여러분의 컴포넌트 인스턴스에 값이 `href`인 데이터 속성 `attributeName`이 있다면, 이 바인딩은 `v-bind:href`와 같습니다.
<br>
예제를 한번 살펴보죠.

```vue
<template>
  <h1 :[attr]="msg">{% raw %}{{ msg }}{% endraw %}</h1>
</template>

<script>
export default {
  data() {
    return {
      msg: 'active',
      attr: 'class',
    }
  },
  // 속성
  methods: {
    add() {
      this.msg += '!'
    },
  },
}
</script>

<style scoped>
.active {
  color: royalblue;
  font-size: 100px;
}
</style>
```

위 코드에서는 `data()`에 정의해 둔 attr가 h1태그의 attr에 **"class"**라는 문자열로 들어가게 됩니다. 

##### v-on
이와 유사하게, 동적인 이벤트명에 핸들러를 바인딩할 때 동적 전달인자 `v-on 디렉티브`를 활용할 수 있습니다.
```html
<a v-on:[eventName]="doSomething"> ... </a>
```
위 예제에서 `eventName`의 값이 `focus` 라면, **v-on:[eventName]은 v-on:focus와 같습니다.**<br>
다음과 같은 예제로 이해해봅시다.

```vue
<template>
  <h1 :[attr]="msg" @[event]="add">{% raw %}{{ msg }}{% endraw %}</h1>
</template>

<script>
export default {
  data() {
    return {
      msg: 'active',
      attr: 'class',
      event: 'click',
    }
  },
  // 속성
  methods: {
    add() {
      this.msg += '!'
    },
  },
}
</script>

<style scoped>
.active {
  color: royalblue;
  font-size: 100px;
}
</style>
```
전달인자를 다음과 같이 데이터와 대괄호를 사용하여 전달해줄 수도 있습니다. 그렇지만 출력결과를 예상해보면, 동일한 `msg` 데이터를 사용하고 있기 때문에 만약 클릭을 하게 된다면 **msg값 전체가 바뀌게 되어 클래스에 할당해 둔 스타일이 사라지게 됩니다.** 이 문제를 해결합시다. 

```vue
<template>
  <h1 :[attr]="'active'" @[event]="add">
    {% raw %}{{ msg }}{% endraw %}
  </h1>
</template>

<script>
export default {
  data() {
    return {
      msg: 'active',
      attr: 'class',
      event: 'click',
    }
  },
  // 속성
  methods: {
    add() {
      this.msg += '!'
    },
  },
}
</script>

<style scoped>
.active {
  color: royalblue;
  font-size: 100px;
}
</style>
```
다음과 같이 작성해주면, `'active'`는 하나의 문자데이터로서 인식되어, msg의 값에 따라 변하지 않습니다.




## 약어

`v-` 접두어는 템플릿에서 Vue 특정 속성을 식별하기 위한 시각적 신호 역할을 합니다. 이 기능은 VueJS를 사용하여 기존 마크업에 동적인 동작을 적용할 때 유용하지만, 일부 자주 사용되는 디렉티브에 대해서는 복잡하다고 느낄 수 있습니다. 동시에 **VueJS가 모든 템플릿을 관리하는 SPA (opens new window)를 구축하게 되면 v- 접두어의 필요성이 줄어듭니다.** 따라서 가장 자주 사용되는 두 개의 디렉티브 v-bind와 v-on에 대한 특별한 약어를 제공합니다.

#### v-bind 약어

```html
<!-- 전체 문법 -->
<a v-bind:href="url"> ... </a>

<!-- 약어 -->
<a :href="url"> ... </a>

<!-- 동적 전달인자와 함께 쓴 약어 -->
<a :[key]="url"> ... </a>
```

#### v-on 약어

```html
<!-- 전체 문법 -->
<a v-on:click="doSomething"> ... </a>

<!-- 약어 -->
<a @click="doSomething"> ... </a>

<!-- 동적 전달인자와 함께 쓴 약어 -->
<a @[event]="doSomething"> ... </a>
```

위 예제들이 일반적인 HTML과 조금 다르게 보일 수 있으나, 속성명에 쓴 `:`과 `@`은 유효한 문자이며, VueJS를 지원하는 모든 브라우저는 이를 올바르게 이해할 수 있습니다. 또한 최종 렌더링 된 마크업에는 나타나지 않습니다. **약어는 전적으로 선택사항이지만, 사용법에 점차 익숙해지면 편할 것입니다.**

