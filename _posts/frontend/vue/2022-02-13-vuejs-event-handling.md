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

## Event Handling (이벤트 핸들링)

### Listening to Events (이벤트 청취)

`v-on 디렉티브`는 `@`기호로, DOM 이벤트를 듣고 트리거 될 때와 JavaScript를 실행할 때 사용합니다.
`v-on:click="methodName"` 나 줄여서 `@click="methodName"`으로 사용합니다.

```html
<div id="basic-event">
  <button @click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      counter: 1
    }
  }
}).mount('#basic-event')
```

<iframe src="https://codesandbox.io/embed/frosty-lake-khrk8?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="frosty-lake-khrk8"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

### Method Handler (메소드 이벤트 핸들러)

많은 이벤트 핸들러의 로직은 일반적으로 복잡하기 때문에, JavaScript를 `v-on 디렉티브(@)` 값으로 보관하는 것은 간단하지 않습니다. 따라서 `v-on 디렉티브(@)`는 호출하고자 하는 메소드의 이름을 받아서 사용합니다.

자 그럼 직접 코드를 작성해서 확인해보겠습니다. `<template>`안에 버튼을 만들어서 클릭하게 되면 `handler()`라는 메서드가 실행되도록 작성하였습니다. 다음과 같은 **이벤트 객체**가 출력됩니다. 
이것은 **Click 이벤트가 발생했을 때 Click에 대한 정보를 가지고 있는 하나의 객체 데이터**를 의미합니다. **codesandbox**를 이용해서 한번 확인해보면 좋을 것 같네요.

> **PointerEvent {isTrusted: true, pointerId: 1, width: 1, height: 1, pressure: 0, …}**

<iframe src="https://codesandbox.io/embed/awesome-saha-xr8zc?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="awesome-saha-xr8zc"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

코드에서 볼 수 있는 **event 객체**라는 것은 **JavaScript의 여러 이벤트들의 정보를 가지고 있는 데이터**라고 보시면 되겠습니다. 따라서 우리가 특정 메소드를 실행하면 그에 해당하는 내용이 첫 번째 인수로 들어올 수 있는 구조이고 이를 event라는 매개변수로 내부에서 활용할 수 있게 됩니다.

아래 코드도 살펴보시죠. handler() 메서드에 소괄호가 추가되어있습니다. 이 소괄호는 VueJS에서는 생략할 수 있습니다. 생략하게 되면 이벤트 객체가 출력이 되지만 handler() 메서드를 실행할 때 인수로 특정 문자열을 출력하고자 한다면 아래 코드와 같이 작성해주면 됩니다. 

하지만 이벤트 객체가 여전히 필요할 수도 있겠죠? VueJS에서는 `$event`라고 추가적으로 작성해주면, 이벤트 객체도 같이 사용할 수 있는 문법을 제공하고 있습니다.

<iframe src="https://codesandbox.io/embed/modest-tree-cglpd?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="modest-tree-cglpd"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>


만약 하나의 이벤트에 여러 개의 메소드를 작성하려면, 소괄호를 반드시 포함해 주어야 합니다. 

<iframe src="https://codesandbox.io/embed/bold-wozniak-0pkkd?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="bold-wozniak-0pkkd"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>


### Event Modifiers (이벤트 수식어)

이벤트 핸들러 내부에서 `event.preventDefault()` 또는 `event.stopPropagation()`를 호출하는 것은 매우 흔한 일입니다. 메소드 내에서 쉽게 이 작업을 할 수 있지만, DOM 이벤트 세부 사항을 처리하는 대신 데이터 로직에 대한 메소드만 사용할 수 있으면 더 좋습니다.

이 문제를 해결하기 위하여, VueJS는 `v-on 디렉티브`에 이벤트 수식어를 제공합니다. 수식어는 점으로 된 접미사 입니다.

- .stop
- .prevent
- .capture
- .self
- .once
- .passive