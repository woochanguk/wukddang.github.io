---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
# accent_color: "#ccc"
# theme_color: "#ccc"
description: >
  VueJS의 List Rendering에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue
---

# VueJS - List Rendering

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

--- 

이번시간부터는 VueJS의 <a href="https://v3.vuejs-korea.org/guide/list.html#%E1%84%85%E1%85%B5%E1%84%89%E1%85%B3%E1%84%90%E1%85%B3-%E1%84%85%E1%85%A6%E1%86%AB%E1%84%83%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC" target="_blank">조건부 렌더링</a>에 대해 알아보겠습니다.

1. table of contents
{:toc .large-only}

---

## List Rendering (리스트 렌더링)


### v-for로 요소에 배열 매핑

`v-for 디렉티브`를 사용하여 배열을 기반으로 리스트를 렌더링 할 수 있습니다. `v-for 디렉티브`는 **item in items** 형태로 특별한 문법이 필요합니다. 여기서 **items는 원본 데이터 배열이고 item은 반복되는 배열 요소의 별칭** 입니다.

#### 기본 사용방법

VueJS에서 실습을 수행해 보겠습니다. v-for 디렉티브에 소괄호를 사용해서 배열인자와 배열요소를 받아 출력하였습니다.

```vue
<template>
  <ul>
    <li
      v-for="( fruit, index ) in fruits"
      :key="fruit">
      {% raw %}{{ fruit }}-{{ index + 1 }}{% endraw %}
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      fruits: ["Apple", "Banana", "Cherry"]
    }
  }
}
// 출력 결과
// Apple-1
// Banana-2
// Cherry-3
</script>
```

이번에는 조금 더 복잡한 구조의 배열 데이터를 v-for로 출력해보겠습니다.

```vue
<template>
  <ul>
    <li
      v-for="fruit in newFruits"
      :key="fruit.id">
      {{ fruit.name }}
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      fruits: ["Apple", "Banana", "Cherry"]
    }
  },
  computed: {
    newFruits() {
      return this.fruits.map((fruit, index) => {
        return {
          id: index,
          name: fruit
        }
      })
    }
  }
}
</script>
```
computed 옵션에 newFruits() 메서드를 작성하였습니다. 이 메서드는 this를 이용하여 fruits라는 배열을 **map()메서드**를 사용하여 **id에 index**를, **name에 fruit**를 매핑시켜주는데
화살표 함수가 Callback 함수로 정의되어 있으므로 먼저 return 해주고 난 뒤에 매핑을 수행하게 되는 것입니다.


`:key`부분에는 고유한 값이 들어가야 하므로, **fruit** 객체 데이터에 들어있는 **id**라는 속성을 고유한 값으로 `:key`에 입력해주게 됩니다.

조금 더 나아가서, 각각의 id를 고유하게 만들어줄 수 있는 패키지를 설치하여 사용해보도록 하겠습니다.

> npm i -D shortid

이 패키지를 `<script>`태그 부분에 **import**한 후 이전에 `id: index`로 작성한 부분을 다음과 같이 바꿔줄 수 있습니다.

> **id: shortid.generate()**

그러면 이제 index라는 매개변수는 사용하지 않기때문에 제거해줄 수 있고 다음과 같은 코드로 요약해줄 수 있겠습니다. 어떤 **id**가 나타날지도 궁금하니 출력 결과도 보시죠.

```vue
<template>
  <ul>
    <li
      v-for="fruit in newFruits"
      :key="fruit.id">
      {{ fruit.name }}-{{ fruit.id }}
    </li>
  </ul>
</template>

<script>
import shortid from 'shortid'

export default {
  data() {
    return {
      fruits: ["Apple", "Banana", "Cherry"]
    }
  },
  computed: {
    newFruits() {
      return this.fruits.map(fruit => ({
        id: shortid.generate(),
        name: fruit
      }))
    }
  }
}
// 출력 결과
// Apple-Oeqebhyw2
// Banana-PdtU3Ida_z
// Cherry-FCfW3BuOvp
</script>
```
조금은 난해할 수 있지만, 어쨌든 고유한 id를 출력할 수 있게 되었습니다.

또 newFruits라는 **배열 데이터**에 각각의 요소가 **객체 데이터**라면 이 객체 데이터는 객체 구조분해 문법을 사용하여 각각을 이용해줄 수 있습니다.

```vue
<template>
  <ul>
    <li
      v-for="{ id, name } in newFruits"
      :key="id">
      {{ name }}-{{ id }}
    </li>
  </ul>
  <!-- Apple-l2SVrftxD -->
  <!-- Banana-X3A8Usah6T -->
  <!-- Cherry-rMd8xJaYgr -->
</template>
```


이번에는 `<template>` 태그 안에 `<button>` 태그를 사용하여  버튼을 클릭하였을 때 **handler() 메서드**가 실행되도록 하였고 이 메서드는 **data()**에 정의된 **fruits 배열에 특정 요소를 추가해주는 push() 메서드를 작성**하였습니다. <br>
 이전에도 말씀드렸지만 이를 **반응성(Reactivity)**이라고 합니다.


### 배열 변경 감지

#### 변이 메소드

Vue는 감시중인 배열의 변이 메소드를 래핑하여 뷰 갱신을 트리거합니다(**Reactivity**). 래핑된 메소드는 다음과 같습니다.

- push() 
  - 배열의 가장 뒤에 새로운 요소를 추가해주는 메서드
- pop()
  - 배열의 가장 뒤의 요소를 반환해주는 메서드
- shift()
  - 배열의 가장 앞의 요소를 반환해주는 메서드
- unshift()
  - 배열의 가장 앞에 요소를 추가해주는 메서드 
- splice()
  - 인덱스를 이용해서 요소를 추가하거나 삭제할 때 사용하는 메서드
- sort()
  - 배열을 정렬해주는 메서드
- reverse() 
  - 배열을 뒤집어주는 메서드

##### 배열 대체

이름에서 알 수 있듯 **변이 메소드는 호출된 원본 배열을 변형**합니다. 이와 비교하여 **변형을 하지 않는 방법**도 있습니다. 바로 `filter()`, `concat()` 와 `slice()` 입니다. 이 방법을 사용하면 원본 배열을 변형하지 않고 **항상 새 배열을 반환**합니다. 변형이 없는 방법으로 작업할 때는 이전 배열을 새 배열로 바꿀 수 있습니다. 아래 예시를 살펴보죠

```js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

이렇게 하면 Vue가 기존 DOM을 버리고 전체 목록을 다시 렌더링 한다고 생각할 수 있습니다. 다행히도, 그렇지는 않습니다. Vue는 **DOM 요소 재사용을 극대화**하기 위해 몇가지 똑똑한 구현을 하므로 **배열을 겹치는 객체를 포함하는 다른 배열로 대체하는 것은 효율적인 작업**입니다.