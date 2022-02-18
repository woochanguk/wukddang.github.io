---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
# accent_color: "#ccc"
# theme_color: "#ccc"
description: >
  VueJS의 클래스와 스타일 바인딩에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue-basic
---

# VueJS - Class & Style binding


안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

<a href="https://kr.vuejs.org/v2/guide/class-and-style.html" target="_blank">클래스와 스타일 바인딩</a>에 대해 알아보겠습니다.

1. table of contents
{:toc .large-only}

---

먼저 아래 코드를 먼저 살펴볼까요?

```vue
<template>
  <h1 @click="activate">Hello?! ({{ isActive }})</h1>
</template>

<script>
export default {
  data() {
    return {
      isActive: false,
    }
  },
  methods: {
    activate() {
      this.isActive = true
    },
  },
}
</script>
```
`<h1>`을 클릭하면, activate() 메서드가 실행되어서 isActive의 값이 true로 변경되는 코드군요. 추가적으로 `<style>`도 추가해봅시다. 그리고 `active`클래스를 추가해줍시다. **class**가 객체 데이터를 취급할 수 있도록 `v-bind` 디렉터리를 사용하겠습니다. 

```vue
<template>
  <h1 
    :class="{ active: isActive}"
    @click="activate">
    Hello?! ({{ isActive }})
  </h1>
</template>

<script>
export default {
  data() {
    return {
      isActive: false,
    }
  },
  methods: {
    activate() {
      this.isActive = true
    },
  },
}
</script>

<style scoped>
.active {
  color: red;
  font-weight: bold;
}
</style>

```
위의 코드를 설명해보겠습니다. `class`로 **active**라는 속성을 추가하고 싶은데, 이 **active**는 **isActive**의 영향을 받아서 만약 **isActive**가 **true**이면 **active**가 추가되고 **false**면 **active**가 추가되지 않는 그런 구조로서, **클래스 바인딩**을 사용했다고 생각하면 되겠습니다. 클릭해보면, 클래스의 스타일에 맞게 브라우저에 출력되는 것을 확인할 수 있습니다. <br> 


서론이 길었습니다. 위와 같이 클래스 바인딩은 JavaScript처럼 HTML, CSS를 다루는 개념이다라고 이해하시면 되겠습니다. 다른 이야기지만 VueJS, ReactJS, SvelteJS는 HTML, CSS구조도 JavaScript의 데이터로 이해하여 작성하게 됩니다.


---

**데이터 바인딩**은 엘리먼트의 **클래스 목록**과 **인라인 스타일**을 조작하기 위해 일반적으로 사용됩니다. 이 두 속성은 `v-bind`를 사용하여 처리할 수 있습니다. 우리는 표현식으로 최종 문자열을 계산하면 됩니다. 그러나 문자열 연결에 간섭하는 것은 짜증나는 일이며 오류가 발생하기 쉽습니다. 이러한 이유로, **Vue는 class와 style에 v-bind를 사용할 때 특별히 향상된 기능을 제공**합니다. 표현식은 문자열 이외에 객체 또는 배열을 이용할 수 있습니다.

### HTML 클래스 바인딩

#### 객체 구문
클래스를 동적으로 토글하기 위해 `v-bind:class`에 객체를 전달할 수 있습니다. (`v-bind`는 생략가능합니다!)

```html
<div v-bind:class="{ active: isActive }"></div>
```
위 구문은 active 클래스의 존재 여부가 데이터 속성 isActive 의 Boolean 데이터 값에 의해 결정되는 것을 의미합니다.

객체에 필드가 더 있으면 여러 클래스를 토글 할 수 있습니다. 또한 `v-bind:class` 디렉티브는 일반 class 속성과 공존할 수 있습니다. 그래서 다음과 같은 템플릿이 가능합니다. 또한 ''를 사용해서 `text-danger`와 같은 클래스 명도 사용할 수 있겠구요. 즉, 특정한 데이터들을 기준으로 하여 해당 요소의 특정 클래스 이름이 연결 될 것인지, 아닐것인지를 정의할 수 있습니다.

```html
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

그리고 바인딩 된 객체는 인라인 일 필요는 없습니다.

```html
<div v-bind:class="classObject"></div>
```

```js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```
또한 객체를 반환하는 computed 옵션에도 바인딩 할 수 있습니다. 이것은 일반적이고 강력해서 많이 사용되는 패턴입니다.

```html
<div v-bind:class="classObject"></div>
```

```js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```
computed 옵션은 classObject() 메서드 안의 `active`, `test-danger` 옵션들을 갖게 되는데 , `active`의 경우 **this**를 사용하여 `isActive`의 값이 **true**이고, `error`의 값이 **false**인 경우에만 **해당 내용이 true**가 되고 **active가 적용**되는 방식이라고 이해하시면 되겠습니다. 

#### 배열 구문
배열을 `v-bind:class` 에 전달하여 클래스 목록을 지정할 수 있습니다. **activeClass**에는 **active**가, **errorClass**에는 **text-danger**가 할당되겠죠?

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```

```js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```
결과는 아래와 같이 렌더링됩니다.

```html
<div class="active text-danger"></div>
```

### 인라인 스타일 바인딩

#### 객체 구문
`v-bind:style` 객체 구문은 매우 간단합니다. **JavaScript 객체라는 점을 제외하면 CSS와 거의 비슷**합니다. CSS 속성 이름에는 camelCase와 kebab-case (따옴표를 함께 사용해야 합니다)를 사용할 수 있습니다.
```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

```js
data: {
  return {
    activeColor: 'red',
    fontSize: 30
  }

}
```
위와 같이 작성해도 잘 작동하지만, 스타일 객체에 직접 바인딩 하여 템플릿이 더 간결하도록 만드는 것이 좋습니다. 이와 같이, **style도 JavaScript에서 제어할 수 있습니다.**

```html
<div v-bind:style="styleObject"></div>
```

```js
data: {
  return {
    styleObject: {
    color: 'red',
    fontSize: '13px'
    }
  }
}
```

그럼 간단하게 실습을 진행해 봅시다.

```vue
<template>
  <h1 
    :style="{ 
      color: color,
      fontSize: fontSize 
    }" 
    @click="changeStyle">
    Hello?! 
  </h1>
</template>

<script>
export default {
  data() {
    return {
      color: 'orange',
      fontSize: '30px'
    }
  },
  methods: {
    changeStyle() {
      this.color = 'red',
      this.fontSize = '50px'
    }
  }
}
</script>
```
`<template>`안의 `<h1>`은 `v-bind` 디렉티브를 사용하여 color, fontSize 스타일을 사용하였습니다. 또 `v-on` 디렉티브를 사용하여 `<h1>`을 클릭하면 **changeStyle()** 메서드가 실행되게끔 작성하였습니다. 스타일도 동적으로 변경이 가능하죠.

<br>
스타일을 배열 구문으로도 작성해봅시다. 그리고 새로운 데이터도 추가해서 더 이해해 보겠습니다. 새로운 스타일을 적용하려면 새로운 클래스를 추가해 주어야겠죠? 배열 구문을 사용하여 스타일을 적용해봅시다. 

```vue
<template>
  <h1 
    :style="fontStyle"
    @click="changeStyle">
    Hello?! 
  </h1>
</template>

<script>
export default {
  data() {
    return {
      fontStyle: {
        color: 'orange',
        fontSize: '30px'
      }
    }
  },
  methods: {
    changeStyle() {
      this.fontStyle.color = 'red',
      this.fontStyle.fontSize = '50px'
    }
  }
}
</script>
```
위와 같이, 여러 개의 스타일을 적용하려면 배열 구문을 사용하면 됩니다.