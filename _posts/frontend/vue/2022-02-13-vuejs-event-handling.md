---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
# accent_color: "#ccc"
# theme_color: "#ccc"
description: >
  VueJS의 Event Handling에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue
---

# VueJS - Event Handling

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

--- 

이번시간부터는 VueJS의 <a href="https://v3.vuejs-korea.org/guide/events.html#%E1%84%8B%E1%85%B5%E1%84%87%E1%85%A6%E1%86%AB%E1%84%90%E1%85%B3-%E1%84%8E%E1%85%A5%E1%86%BC%E1%84%8E%E1%85%B1" target="_blank">이벤트 핸들링</a>에 대해 알아보겠습니다.

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

## Event Handling (이벤트 핸들링)

---

### Listening to Events (이벤트 청취)

`v-on 디렉티브`는 `@`기호로, DOM 이벤트를 듣고 트리거 될 때와 JavaScript를 실행할 때 사용합니다.
`v-on:click="methodName"` 나 줄여서 `@click="methodName"`으로 사용합니다.

```vue
<template>
  <div>
    <button @click="counter += 1">Add 1</button>
    <p>The button above has been clicked {{ counter }} times.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      counter: 1,
    }
  },
}
</script>
```

---

### Method Handler (메소드 이벤트 핸들러)

많은 이벤트 핸들러의 로직은 일반적으로 복잡하기 때문에, JavaScript를 `v-on 디렉티브(@)` 값으로 보관하는 것은 간단하지 않습니다. 따라서 `v-on 디렉티브(@)`는 호출하고자 하는 메소드의 이름을 받아서 사용합니다.

자 그럼 직접 코드를 작성해서 확인해보겠습니다. `<template>`안에 버튼을 만들어서 클릭하게 되면 `handler()`라는 메서드가 실행되도록 작성하였습니다. 다음과 같은 **이벤트 객체**가 출력됩니다. 
이것은 **Click 이벤트가 발생했을 때 Click에 대한 정보를 가지고 있는 하나의 객체 데이터**를 의미합니다. **codesandbox**를 이용해서 한번 확인해보면 좋을 것 같네요. 예제 코드들을 codesandbox를 이용해서 확인해보세요.

> **PointerEvent {isTrusted: true, pointerId: 1, width: 1, height: 1, pressure: 0, …}**


```vue
<template>
  <button @click="handler">
    Click me!
  </button>
</template>

<script>
export default {
  methods: {
    handler(event) {
      console.log(event)
      console.log(event.target)
    }
  }
}
</script>
```

코드에서 볼 수 있는 **event 객체**라는 것은 **JavaScript의 여러 이벤트들의 정보를 가지고 있는 데이터**라고 보시면 되겠습니다. 따라서 우리가 특정 메소드를 실행하면 그에 해당하는 내용이 첫 번째 인수로 들어올 수 있는 구조이고 이를 event라는 매개변수로 내부에서 활용할 수 있게 됩니다.

아래 코드도 살펴보시죠. handler() 메서드에 소괄호가 추가되어있습니다. 이 소괄호는 VueJS에서는 생략할 수 있습니다. 생략하게 되면 이벤트 객체가 출력이 되지만 handler() 메서드를 실행할 때 인수로 특정 문자열을 출력하고자 한다면 아래 코드와 같이 작성해주면 됩니다. 

하지만 이벤트 객체가 여전히 필요할 수도 있겠죠? VueJS에서는 `$event`라고 추가적으로 작성해주면, 이벤트 객체도 같이 사용할 수 있는 문법을 제공하고 있습니다.

```vue
<template>
  <button @click="handler('hi', $event)">Click 1</button>
  <button @click="handler('what', $event)">Click 2</button>
</template>

<script>
export default {
  methods: {
    handler(msg, event) {
      console.log(msg)
      console.log(event)
    },
  },
}
</script>
```

만약 하나의 이벤트에 여러 개의 메소드를 작성하려면, 소괄호를 반드시 포함해 주어야 합니다. 

```vue
<template>
  <button @click="handlerA(), handlerB()">Click me!</button>
</template>

<script>
export default {
  methods: {
    handlerA() {
      console.log('A')
    },
    handlerB() {
      console.log('B')
    },
  },
}
</script>
```

---

### Event Modifiers (이벤트 수식어)

<a href="https://v3.vuejs-korea.org/guide/events.html#%E1%84%8B%E1%85%B5%E1%84%87%E1%85%A6%E1%86%AB%E1%84%90%E1%85%B3-%E1%84%89%E1%85%AE%E1%84%89%E1%85%B5%E1%86%A8%E1%84%8B%E1%85%A5" target="_blank">이벤트 수식어</a>에 대해 알아보겠습니다. 이벤트 핸들러 내부에서 `event.preventDefault()` 또는 `event.stopPropagation()`를 호출하는 것은 매우 흔한 일입니다. 메소드 내에서 쉽게 이 작업을 할 수 있지만, DOM 이벤트 세부 사항을 처리하는 대신 데이터 로직에 대한 메소드만 사용할 수 있으면 더 좋습니다.

이 문제를 해결하기 위하여, VueJS는 `v-on 디렉티브`에 이벤트 수식어를 제공합니다. 수식어는 점으로 된 접미사 입니다.

- **.stop**
- **.prevent**
- **.capture**
- **.self**
- **.once**
- **.passive**

```html
<!-- 클릭 이벤트 전파가 중단되었습니다. -->
<a @click.stop="doThis"></a>

<!-- 제출 이벤트가 페이지를 다시 로드하지 않습니다. -->
<form @submit.prevent="onSubmit"></form>

<!-- 수정자는 체이닝이 가능합니다. -->
<a @click.stop.prevent="doThat"></a>

<!-- 단순히 수식어만 사용이 가능합니다. -->
<form @submit.prevent></form>

<!-- 캡처 모드를 사용할 때 이벤트 리스너를 사용 가능합니다.-->
<!--즉, 내부 엘리먼트를 대상으로 하는 이벤트가 해당 엘리먼트에서 처리되기 전에 여기서 처리합니다. -->
<div @click.capture="doThis">...</div>

<!-- event.target이 엘리먼트 자체인 경우에만 트리거를 처리합니다.-->
<!-- 자식 엘리먼트에서는 처리되지 않습니다.-->
<div @click.self="doThat">...</div>
```
#### .prevent & .once

이벤트 객체 안의 `preventDefault()` 메서드를 실행한 결과 `<a>`태그가 실행되지 않습니다. 즉, 이미 정의된 기능을 동작시키지 않고 단순히 정의한 메서드만 실행하겠다는 내용으로 이해하면 되겠습니다. 
웹 개발을 하다보니 HTML이 가지고 있는 기본 기능들이 많이 있구나 하는 것을 알게 되었습니다. 따라서 필요에 따라 이런 기본기능들을 사용하거나, 아니면 임의로 만든 메서드를 실행하기 위해 기본기능을 사용하지 않거나를 선택할 수 있게끔, `preventDefault()`를 사용합니다. 
이 기능을, `@click.prevent`를 사용해도 정상작동하는 것을 볼 수 있습니다. 

이러한 이벤트 수식어는 **메소드 체이닝**을 통해 연결해서도 사용할 수 있습니다. `@click.prevent.once`를 사용하면 어떤 결과가 나오는지 아래 코드로 확인해보시면 좋을거 같습니다.

```vue
<template>
  <a href="https://naver.com" target="_blank" @click.prevent.once="handler">
    NAVER
  </a>
</template>

<script>
export default {
  methods: {
    handler(event) {
      // event.preventDefauit()
      console.log('ABC!')
    },
  },
}
</script>
```

#### .stop

##### Event Bubbling (이벤트 버블링)
.stop 에 대해서 알아봅시다. 아래 코드를 보시죠.

```vue
<template>
  <div class="parent" @click="handlerA">
    <div class="child" @click="handlerB"></div>
  </div>
</template>

<script>
export default {
  methods: {
    handlerA() {
      console.log('A')
    },
    handlerB() {
      console.log('B')
    },
  },
}
</script>

<style lang="scss" scoped>
.parent {
  width: 200px;
  height: 100px;
  background-color: royalblue;
  margin: 10px;
  padding: 10px;
  .child {
    width: 100px;
    height: 100px;
    background-color: orange;
  }
}
</style>
```
특정 영역을 누르면 특정 메서드가 실행되는 간단한 코드입니다. 그런데 문제가 있습니다. 주황색 영역을 누르니, B, A가 둘다 출력이 되네요. 왜 이런일이 생길까요? 이유는 **주황색 영역이 파란색 영역 안의 내용이라, 주황색을 누르면 파란색도 같이 눌리기 때문입니다.** <br>
이와 같이 하위 요소를 작동시켰을 때 상위 요소까지 이벤트가 전달되는 것을 **이벤트 버블링**이라고 합니다.
<br>
이 버블링을 방지하려면 handlerB에 event객체를 첫 번째 매개변수로 받도록 하고, 다음 코드를 작성해주면 됩니다.

> `event.stopPropagation()`

하지만 VueJS에서는 이렇게 메서드에 작성하는 것이 아닌, 이벤트 수식어로서 조금 더 간단하게 만들어줄 수가 있습니다. 다음과 같이요.

> `@click.stop="handlerB"`

---

#### .capture 

##### Event Capturing (이벤트 캡쳐링)
이벤트 캡쳐링은 이전의 이벤트 버블링과 반대되는, 부모 요소에서 자식 요소로 내려오는 개념이라고 보시면 되겠습니다. 코드를 보며 이해해 보겠습니다.

```vue
<template>
  <div class="parent" @click.capture="handlerA">
    <div class="child" @click="handlerB"></div>
  </div>
</template>

<script>
export default {
  methods: {
    handlerA() {
      console.log('A')
    },
    handlerB() {
      console.log('B')
    },
  },
}
</script>

<style lang="scss" scoped>
.parent {
  width: 200px;
  height: 100px;
  background-color: royalblue;
  margin: 10px;
  padding: 10px;
  .child {
    width: 100px;
    height: 100px;
    background-color: orange;
  }
}
/* 출력 결과
A
B */
</style>
```
`@click.capture="handlerA`으로 작성하게 되면, 이전에는 B, A 순으로 출력이 되었는데 A, B로 출력이 되는 것을 확인할 수 있습니다. 즉 B 영역은 A 영역에 포함되어 있기 때문에, `.capture`를 사용해서 A 영역을 먼저 실행한 후 B 영역을 실행하였다고 이해하시면 되겠습니다. A 라는 데이터만 출력하고 싶다면, **메소드 체이닝**을 이용해 다음과 같이 작성해주면 됩니다.

> `@click.capture.stop = "handlerA`

#### .self
`.self`를 이해하면서 각 영역을 클릭한 결과가 독립되도록 만들어 줍시다. 다음과 같이 코드를 변경해주면, 이제는 **주황색 영역을 클릭해도 A가 출력되지 않습니다.** 즉 `.self`는 겹치지 않고 자신의 요소만을 선택하였을 때 동작하도록 하는 수식어라고 할 수 있습니다. 

```vue
<template>
  <div class="parent" @click.self="handlerA">
    <div class="child"></div>
  </div>
</template>

<script>
export default {
  methods: {
    handlerA() {
      console.log('A')
    },
    handlerB() {
      console.log('B')
    },
  },
}
</script>

<style lang="scss" scoped>
.parent {
  width: 200px;
  height: 100px;
  background-color: royalblue;
  margin: 10px;
  padding: 10px;
  .child {
    width: 100px;
    height: 100px;
    background-color: orange;
  }
}
</style>
```
##### target & currentTarget
더 깊은 이해를 위해 아래 코드도 확인해 보시죠. 

```vue
<template>
  <div class="parent" @click="handlerA">
    <div class="child"></div>
  </div>
</template>

<script>
export default {
  methods: {
    handlerA(event) {
      console.log(event.target)
      console.log(event.currentTarget)
      console.log('A')
    },
    handlerB() {
      console.log('B')
    },
  },
}
</script>

<style lang="scss" scoped>
.parent {
  width: 200px;
  height: 100px;
  background-color: royalblue;
  margin: 10px;
  padding: 10px;
  .child {
    width: 100px;
    height: 100px;
    background-color: orange;
  }
}
</style>
```
이 상태에서 주황색 영역을 클릭하면 `event.target`은 **child**가 출력되고 `event.currentTarget`은 **parent**가 출력됩니다. 그리고 A는 출력되지 않는군요. <br> 
각 메서드의 정확한 의미를 아직은 알 수 없지만, `event.target` 과 `event.currentTarget`이 다르면 `@click.self`가 **동작하지 않는 것**을 확인할 수 있네요. <br>

파란색 영역을 클릭하면 `event.target` 과 `event.currentTarget`은 모두 **parent**로 출력되고 `@click.self`는 동작하기 때문에, **A가 출력**되게 됩니다.


#### .passive
마우스 가운데 휠을 누르면, event 객체가 10000번 출력되도록 하는 코드를 작성하여 브라우저에 부하를 가하도록 하겠습니다. 

```vue
<template>
  <div class="parent" @wheel.passive="handler">
    <div class="child"></div>
  </div>
</template>

<script>
export default {
  methods: {
    handler(event) {
      for (let i = 0; i < 10000; i += 1) {
        console.log(event)
      }    
    }
  },
}
</script>

<style lang="scss" scoped>
.parent {
  width: 200px;
  height: 100px;
  background-color: royalblue;
  margin: 10px;
  padding: 10px;
  // 자식 요소가 부모 요소보다 넘치는 구조이면, 스크롤 바 생성!
  overflow: auto;
  .child {
    width: 100px;
    height: 2000px;
    background-color: orange;
  }
}
</style>
```

이렇게 되면, 스크롤을 할 때 버벅이게 됩니다. 이는 **브라우저가 스크롤 하는 것을 화면에 출력하는 것과, event 객체를 10000번 출력하는 것을 동시에** 하고 있기 때문에 발생하게 되는 것입니다.  
이때 **.passive** 수식어를 사용하게 되면 기본적인 코드의 실행과 화면에 출력해주는 것을 **독립**해서 작동하게 할 수 있습니다. 결과적으로 viewport의 부하를 최대한 줄여주면서 코드 실행은 정상적으로 할 수 있게끔 따로 처리를 할 수 있습니다.<br>
즉, 사용자들에게 좀 더 쾌적한 서비스를 제공해줄 때 `.passive` 수식어를 사용하게 됩니다. 사용하지 않았을 때 보다 **5배정도** 빠르게 동작할 수 있다고 합니다.

---

### Key Modifier (키 수식어)
<a href="https://v3.vuejs-korea.org/guide/events.html#%E1%84%8F%E1%85%B5-%E1%84%89%E1%85%AE%E1%84%89%E1%85%B5%E1%86%A8%E1%84%8B%E1%85%A5" target="_blank">키 수식어</a>에 대해 알아보겠습니다.<br>
키보드 이벤트를 listening할 때, 종종 공동 키 코드를 확인해야 합니다.
VueJS에서 키 이벤트를 listening할 때 키 수식어로 `v-on(@)`을 더할 수 있습니다.

```html
<!-- 키가 'Enter'일때만 `vm.submit()`을 호출할 수 있습니다.-->
<input @keyup.enter="submit" />
```

<a href="https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values" target="_blank">KeyboardEvent.key</a>를 통해 노출된 유효 키 이름을 케밥 케이스로 변환하여 수식어로 사용할 수 있습니다. <br> 위의 예제에서, 핸들러는 `$event.key === PageDown` 일 때만 호출됩니다.

#### Key Aliases (키 명령어)
Vue는 가장흔히 사용되는 키에서 명령어를 제공합니다:
- **.enter**
- **.tab**
- **.delete** (“Delete” 와 “Backspace” 키 모두를 캡처합니다)
- **.esc**
- **.space**
- **.up**
- **.down**
- **.left**
- **.right**

#### System Modifier Keys (시스템 수식어 키목록)
다음 수식어를 사용해 해당 수식어 키가 눌러진 경우에만 마우스 또는 키보드 이벤트 리스너를 트리거 할 수 있습니다:

- **.ctrl**
- **.alt**
- **.shift**
- **.meta**

다음과 같은 코드를 작성할 수 있습니다.

```vue
<template>
  <input
    type="text"
    @keydown.ctrl.shift.a="handler" />
</template>

<script>
export default {
  methods: {
    handler() {
      console.log("Enter!!")
    }
  }
}
</script>
```