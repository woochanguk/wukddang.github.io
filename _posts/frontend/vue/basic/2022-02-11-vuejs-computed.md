---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
accent_color: "#ccc"
theme_color: "#ccc"
description: >
  VueJS의 Computed 옵션을 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue
  - vue-basic
---

# VueJS - Computed

## Computed 

이번시간에는 VueJS의 Computed 옵션에 대해 알아보겠습니다. 

1. table of contents
{:toc .large-only}

## Code tester 
여기서 예제코드를 테스트 해주세요
(완성된 **MyBtn.vue**파일을 components폴더에 작성해두었습니다.)
<iframe src="https://codesandbox.io/embed/frosty-lake-khrk8?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="frosty-lake-khrk8"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

---


다음과 같이 `App.vue` 파일을 작성해주겠습니다.

```vue
<template>
  <div></div>
</template>

<script>
export default {

}
</script>

```
그 후 components 폴더 안에 `Fruits.vue`파일을 생성하여 다음과 같은 코드를 작성하고, `<script>` 안에 data() 메서드(옵션)를 추가하고 그 부분의 return을 객체 리터럴로 반환하는 배열 데이터를 선언해주겠습니다. 이전에, 배열 데이터를 반복적으로 출력할 때, `v-for 디렉티브`를 사용할 수 있다고 배웠으니 v-for도 사용할 수 있겠지요. <br>
그 후 key값에 `fruit`를 `v-bind`로 할당해 줍니다. v-bind는 생략할 수 있었죠? 그 후 `<li>` 안에
이중 중괄호 구문으로 fruit 데이터를 출력하겠습니다.

```vue
<template>
  <section>
    <h1>Fruits</h1>
    <ul>
      <li v-for="fruit in fruits" :key="fruit"></li>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    return {
      fruits: ['Apple', 'Banana', 'Cherry'],
    }
  },
}
</script>
```

이제 `App.vue`로 `Fruits.vue`파일을 가져오도록 하죠. `export default`안에 components 객체 데이터를 생성하고 안에 Fruits라는 component를 연결할 수 있습니다. 그리고 `<template>`안에 `<Fruits />`라고 선언해주면, 화면에 잘 출력되는것을 볼 수 있습니다.

```vue
<template>
  <Fruits />
</template>

<script>
import Fruits from `~/components/Fruits`
export default {
  components: {
    Fruits
  }
}
</script>

```

fruits라는 데이터가 존재할 때만, `<template>`안의 데이터가 출력되길 원한다면 다음과 같이 작성해주면 되겠죠

```vue
<template>
  <section v-if="fruits.length > 0">
    <h1>Fruits</h1>
    <ul>
      <li v-for="fruit in fruits" :key="fruit">
        {{ fruit }}
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    return {
      fruits: [],
    }
  },
}
</script>

```

이번엔 `Fruits.vue`의 `export default` 안에 computed라는 옵션을 생성해주도록 하겠습니다. 그리고 computed안에 `hasFruit()`란 메서드도 추가하였습니다. 이 메서드에는 fruits의 길이가 0보다 큰 경우를 비교연산자로 반환하게 됩니다. 그럼 이 메서드를 어떻게 사용할까요? 다음과 같이 위에서 작성한것과 비슷하게, `fruits.length`를 대체할 수 있습니다.
```vue
<template>
  <section v-if="hasFruit">
    <h1>Fruits</h1>
    <ul>
      <li v-for="fruit in fruits" :key="fruit">
        {{ fruit }}
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    return {
      fruits: ['Apple', 'Banana', 'Cherry'],
    }
  },
  computed: {
    hasFruit() {
      return this.fruits.length > 0
    },
  },
}

// 출력 결과: 
// Fruits
// Apple
// Banana
// Cherry 
</script>

```
위와 같이 computed옵션을 이용하여 우리는 VueJS의 계산된 데이터를 사용할 수 있습니다. <br>
computed옵션을 사용하여 다른 메서드를 추가해보도록 하겠습니다. this를 사용하여 fruits 데이터를 가져온 후 `map()` 메서드를 통해 `fruit`라는 각각의 아이템을 새로운 배열 데이터를 반환하겠습니다. <br> `split()` 메서드를 사용하여 과일이름을 하나의 문자로 나누어서 배열로 생성해준 후에, reverse()를 사용하여 배열을 뒤집어주고, join을 사용하여 다시 합쳐주도록 하겠습니다. <br>

그 후 뒤집어진 문자열을 출력하여 비교해 보도록 하겠습니다.


```vue
<template>
  <section v-if="hasFruit">
    <h1>Fruits</h1>
    <ul>
      <li v-for="fruit in fruits" :key="fruit">
        {{ fruit }}
      </li>
    </ul>
  </section>
  <template>
  <section v-if="hasFruit">
    <h1>Fruits</h1>
    <ul>
      <li v-for="fruit in fruits" :key="fruit">
        {{ fruit }}
      </li>
    </ul>
  </section>
  <section>
    <h1>Reverse Fruits</h1>
    <ul>
      <li v-for="fruit in reverseFruits" :key="fruit">
        {{ fruit }}
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    return {
      fruits: ['Apple', 'Banana', 'Cherry'],
    }
  },
  computed: {
    hasFruit() {
      return this.fruits.length > 0
    },
    reverseFruits() {
      return this.fruits.map(fruit => {
        // 'Apple' => ['A', 'p', 'p', 'l', 'e']
        // => ['e', 'l', 'p', 'p', 'A'] => 'elppA'
        return fruit.split('').reverse().join('')
      })
    }
  },
}
// 출력 결과: 
// Fruits
// Apple
// Banana
// Cherry 
// Reverse Fruits
// elppA
// ananaB
// yrrehC
</script>

```

#### Caching(캐싱)

Computed의 캐싱에 대해서도 알아보겠습니다. `<template>`에 같은 메서드를 반복해서 작성하면 동일한 연산의 낭비가 발생하여 웹 어플리케이션의 속도가 느려질 수 있습니다. 이럴 때 Computed 캐싱을 사용해주면 효율적입니다.<br>
다음과 같이 작성해주면 **computed 옵션에 작성한 reversedMessage()메서드가 연산을 수행한 후에** 계산된 문자열이 출력되게 됩니다. 이렇게 되면 추가적인 문자열 연산을 수행할 필요가 없게 되는것이지요. <br>
VueJS 앱을 만들 때 데이터를 최적화하기 위해 **계산된 데이터를 캐싱 기능을 통해 사용하게 된다**는 의미입니다.

```vue
<template>
  <h1>{{ reversedMessage }}</h1>
  <h1>{{ reversedMessage }}</h1>
  <h1>{{ reversedMessage }}</h1>
  <h1>{{ reversedMessage }}</h1>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello Computed!',
    }
  },
  computed: {
    reversedMessage() {
      return this.msg.split('').reverse().join('')
    },
  },
  methods: {
    reverseMessage() {
      return this.msg.split('').reverse().join('')
    },
  },
}
</script>

```

#### Getter, Setter
Computed의 gettter, setter에 대해서도 알아보겠습니다. 
<br> 다음 코드를 이해해보죠. 먼저, methods 옵션 안에 `add()` 메서드를 정의해주고, `<template>`안의 `<button>`에 `@click="add"`를 사용해주면 메서드가 실행됩니다. 그러면 **reservedMessage**에 데이터가 추가될까요? 

```vue
<template>
  <button @click="add">ADD</button>
  <h1>{{ reversedMessage }}</h1>
  <h1>{{ reversedMessage }}</h1>
  <h1>{{ reversedMessage }}</h1>
  <h1>{{ reversedMessage }}</h1>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello Computed!',
    }
  },
  computed: {
    reversedMessage() {
      return this.msg.split('').reverse().join('')
    },
  },
  methods: {
    add() {
      this.reversedMessage += '!?'
    },
  },
}
</script>
```
아무리 버튼을 눌러도 변하는 것은 아무것도 없습니다. 이유는 브라우저의 콘솔이 알려주고 있군요.

> **[Vue warn]: Write operation failed: computed property "reversedMessage" is readonly.**

computed옵션은 읽기 전용이기 때문에, 즉 우리가 할당연산자를 통해 어떤 값을 할당하더라도 그것이 반응적으로 동작하는 구조는 아니라는 뜻이지요. 따라서 computed 옵션은 값을 **얻어내는(getter)** 용도로만 쓰인다는 의미가 됩니다. <br> 하지만 다음과 같이 코드를 작성하면 어떻게 될까요?
이번에는 `<template>`안의 **reversedMessage**가 변해서 출력됩니다.

```vue
<template>
  <button @click="add">ADD</button>
  <h1>{{ reversedMessage }}</h1>
  <h1>{{ reversedMessage }}</h1>
  <h1>{{ reversedMessage }}</h1>
  <h1>{{ reversedMessage }}</h1>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello Computed!',
    }
  },
  computed: {
    reversedMessage() {
      return this.msg.split('').reverse().join('')
    },
  },
  methods: {
    add() {
      this.msg += '!?'
    },
  },
}
</script>

```

data 옵션에 사용한 msg 데이터의 경우에는, 값을 얻어내는(getter) 용도로 사용해줄 수도 있고 값을 지정해주는(setter) 용도로 사용해줄 수도 있습니다. <br>
computed 옵션은 기본적으로 값을 얻어내는 것만 가능하지만,  VueJS 문법을 통해 지정해주는 용도를 사용할 수도 있습니다.
<br> 다음과 같이 `get()`, `set()` 메서드를 선언해주고, `set()` 메서드에는 value 값을 받아 값을 지정해주는 의미로 코드를 작성하였습니다. 그리고 `methods` 옵션은 msg를 **reversedMessage**로 작성하였습니다. 과연 이제 버튼을 눌렀을 때, reversedMessage가 변하는 지를 확인해봅시다.

```vue
<template>
  <button @click="add">ADD</button>
  <h1>{{ reversedMessage }}</h1>
  <h1>{{ reversedMessage }}</h1>
  <h1>{{ reversedMessage }}</h1>
  <h1>{{ reversedMessage }}</h1>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello Computed!',
    }
  },
  computed: {
    // reversedMessage() {
    //   return this.msg.split('').reverse().join('')
    // },
    reversedMessage: {
      get() {
        return this.msg.split('').reverse().join('')
      },
      set(value) {
        // msg = '!detupmoC olleH'
        // value = '!detupmoC olleH!?'
        this.msg = value
      }
    }
  },
  methods: {
    add() {
      this.reversedMessage += '!?'
    },
  },
}
</script>

```

보여드리지 못하는게 정말 아쉽네요. 나중에는 클립을 따서 gif로 보여드리는 방법을 찾아봐야겠습니다... 결과를 말씀드리면, **잘 변하면서 출력됩니다!!** 그럼 코드를 하나씩 살펴보겠습니다.

---
먼저 computed 옵션 안의 **reversedMessage**는 msg에서 받은 값인 `'Hello Computed'`를 이용하여 `get()` 메서드를 통해 문자열을 뒤집어주고 `set()` 메서드를 통해 값을 지정해주게 됩니다. 그 이후에 
버튼을 클릭하게 되면 methods 옵션 안의 `add()` 메서드가 실행되고, **reversedMessage**에 `'!?'`를 추가한 후 `set()`메서드를 이용해서 msg에 값을 할당해준 뒤 `get()` 메서드를 실행해서 문자열이 추가된 데이터를 뒤집은 후 브라우저에 출력해줍니다.

---
일반적으로 computed 옵션에 get(), set() 메서드를 사용하는게 일반적인 경우 잘 활용되지는 않지만, 뒤에 Vuex라는 중앙 집중식 저장소를 사용할 때는 종종 유용하게 사용될 수 있습니다. 다음과 같은 패턴이 있다는 것 정도만 일단 이해하고 넘어갑시다.


## Watch 
Watch 옵션에 대해 알아보겠습니다. 아래 코드를 확인해보죠.
watch 옵션은 특정한 데이터들의 변경사항을 감시하는 용도의 옵션입니다. watch 부분에 msg라는 메서드를 명시해주고, 변경사항을 한번 확인해보겠습니다. **(메서드의 이름을 꼭 msg로 해주어야 변경사항을 감지할 수 있습니다!)**
```vue
<template>
  <h1 @click="changeMessage">
    {{ msg }}
  </h1>
  <!-- Hello? -->
  <h1>{{ reversedMessage }}</h1>
  <!-- ?olleH -->
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello?',
    }
  },
  computed: {
    reversedMessage() {
      return this.msg.split('').reverse().join('')
    },
  },
  watch: {
    msg() {
      console.log('msg: ', this.msg)
      // msg: Good!
    }
  },
  methods: {
    changeMessage() {
      this.msg = 'Good!'
    },
  },
}
</script>

```
코드를 설명해보겠습니다. `<template>`에서 `<h1>`를 클릭하면 `changeMessage()` 메서드가 실행됩니다. 그런데 `<script>`내부를 보시면 `watch` 옵션이 작성되어 있고 msg 데이터의 변경사항을 감지하는 코드가 작성되어 있습니다. 따라서 changeMessage()가 실행되기 전에, **msg** 데이터는 감시되고 있는 상황이지요. <br>
이 상황에서, `changeMessage()` 메서드가 실행되면 `msg` 데이터는 `Good!`으로 바뀌게 되고 watch 옵션은 msg데이터를 감시하고 있으므로 브라우저에 출력되는 msg도 바뀌게 되는 코드입니다.

---

또 watch 옵션은 계산된 데이터(computed)도 감시할 수 있습니다. 아래의 코드를 보시죠. 

```vue
<template>
  <h1 @click="changeMessage">
    {{ msg }}
  </h1>
  <!-- Hello? -->
  <h1>{{ reversedMessage }}</h1>
  <!-- ?olleH -->
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello?',
    }
  },
  computed: {
    reversedMessage() {
      return this.msg.split('').reverse().join('')
    },
  },
  watch: {
    msg() {
      console.log('msg: ', this.msg)
      // msg: Good!
    },
    reversedMessage() {
      console.log('reversedMessage: ', this.reversedMessage)
      // reversedMessage: !dooG
    },
  },
  methods: {
    changeMessage() {
      this.msg = 'Good!'
    },
  },
}
</script>
```
결과적으로, watch 옵션을 사용하면 computed 옵션으로 작성한 읽기 전용 데이터도 변경되는 것을 확인할 수 있습니다. <br> 추가적으로 알아야 할 것은, watch 옵션 메서드들의 첫 번째 매개변수로는 실제로 변경된 값을 사용할 수 있습니다. 아래의 코드는 위의 코드와 동일한 결과를 보여주겠죠?

```vue
<script>
export default {
  data() {
    return {
      msg: 'Hello?',
    }
  },
  computed: {
    reversedMessage() {
      return this.msg.split('').reverse().join('')
    },
  },
  watch: {
    msg(newValue) {
      console.log('msg: ', newValue)
      // msg: Good!
    },
    reversedMessage(newValue) {
      console.log('reversedMessage: ', newValue)
      // reversedMessage: !dooG
    },
  },
  methods: {
    changeMessage() {
      this.msg = 'Good!'
    },
  },
}
</script>
```