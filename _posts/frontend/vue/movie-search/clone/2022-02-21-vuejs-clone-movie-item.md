---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  VueJS Clone 영화 아이템을 구현해봅시다.
invert_sidebar: false
categories:
  - frontend
  - vue
  - movie-search
  - vue-clone
---

# VueJS Clone - Movie Item

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간에는 영화 아이템을 구현해보겠습니다.


---
## Code tester 
이번시간의 결과 모습입니다.
(codesandbox 오류. 추후 업데이트) <br>
<!-- <iframe src="https://codesandbox.io/embed/exciting-babbage-fkmwhz?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="exciting-babbage-fkmwhz"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe> -->

---

## Movie Item

### 기본 출력
영화 아이템을 출력하기 위해, **MovieItem.vue** 컴포넌트를 다음과 같이 수정하겠습니다.
```vue
<template>
  <div 
    :style="{ backgroundImage: `url(${movie.Poster})` }"
    class="movie">
    <div class="info">
      <div class="year">
        {% raw %}{{ movie.Year }}{% endraw %}
      </div>
      <div class="title">
        {% raw %}{{ movie.Title }}{% endraw %}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    movie: {
      type: Object,
      default: () => ({})
    } 
  }
}
</script>

<style lang="scss" scoped>
@import "../scss/main.scss";
  .movie {
    $width: 200px;
    width: $width;
    height: $width * 3 / 2;
    margin: 10px;
    border-radius: 4px;
    background-color: $gray-400;
    background-size: cover;
    position: relative;
    .info {
      background-color: rgba($black, .3);
      width: 100%;
      padding: 14px;
      font-size: 14px;
      text-align: center;
      position: absolute;
      left: 0;
      bottom: 0;
    }
  }
</style>
```
- 포스터의 크기가 각각 다르기 때문에, `v-bind 디렉티브`를 이용하여 배경이미지로 설정하였습니다. 
- 3:2 비율을 갖도록 `$width`로 값을 변수화 하였습니다.

## 텍스트 말줄임 표시와 배경 흐림 처리

아래 코드를 보면 쉽게 이해할 수 있습니다.
{% include codepen.html hash="WNXExNa" title="hello" %}

- <a href="https://developer.mozilla.org/ko/docs/Web/CSS/white-space" target="_blank">**white-space**</a> 
- <a href="https://developer.mozilla.org/ko/docs/Web/CSS/overflow" target="_blank">**overflow**</a>
- <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/text-overflow" target="_blank">**text-overflow**</a>
- <a href="https://developer.mozilla.org/ko/docs/Web/CSS/backdrop-filter" target="_blank">**backdrop-filter**</a>

위에서 배운 내용을 바탕으로 **MovieItem.vue** 컴포넌트의 스타일을 수정하도록 하겠습니다.
```vue
<style lang="scss" scoped>
@import "../scss/main.scss";
  .movie {
    $width: 200px;
    width: $width;
    height: $width * 3 / 2;
    margin: 10px;
    border-radius: 4px;
    background-color: $gray-400;
    background-size: cover;
    position: relative;
    &::after {
      content: "";
      opacity: 0;
      position: absolute;
      top: -8px;
      bottom: -8px;
      left: -8px;
      right: -8px;
      border: 8px solid $primary;
      border-radius: 6px;
      transition: opacity 0.2s ease-in-out;
    }
    &:hover::after {
      opacity: 0.5;
    }
    .info {
      background-color: rgba($black, .3);
      width: 100%;
      padding: 14px;
      font-size: 14px;
      text-align: center;
      position: absolute;
      left: 0;
      bottom: 0;
      backdrop-filter: blur(3px);
      .year {
        color: $primary;
      }
      .title {
        color: $white;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
    }
  }
</style>
```

### 에러 메시지 출력 

영화 목록을 제대로 받아오지 못하게 된다면 **에러 메시지를 출력**해주도록, **movie.js**와 **MovieList.vue** 컴포넌트를 수정하도록 하겠습니다.

#### movie.js
```js
export default {
  ...,
  actions: {
    async searchMovies({ state, commit }, payload) {
      commit('updateState', {
        message: ''
      })
      try {
        const res = await _fetchMovie({
          ...payload,
          page: 1
        })
        const { Search, totalResults } = res.data
        console.log(res)
        commit('updateState', {
          movies: _uniqBy(Search, 'imdbID'),
        })
  
        const total = parseInt(totalResults, 10)
        const pageLength = Math.ceil(total / 10) 

        if (pageLength > 1) {
          for (let page = 2; page <= pageLength; page += 1) {
            if (page > (payload.number / 10)) {
              break
            }
            const res = await _fetchMovie({
              ...payload,
              page // page: page
            })
            const { Search } = res.data
            commit('updateState', {
              movies: [
                ...state.movies, 
                ..._uniqBy(Search, 'imdbID')
              ]
            })
          }
        }
      } catch (message) {
        commit('updateState', {
          movies: [],
          message 
        })
      }
    }
  }
}
...
```
- `searchMovies()`가 실행되면 바로 `commit()` 메서드를 호출하여 **message**를 빈 문자열로 바꾸었습니다.

#### MovieList.vue
```vue
<template>
  <div class="container">
    <div 
      :class="{ 'no-result': !movies.length }" 
      class="inner">
      <div
        v-if="message" 
        class="message">
        {% raw %}{{ message }}{% endraw %}
      </div>
      <div
        v-else 
        class="movies">
        <MovieItem 
        v-for="movie in movies" 
        :key="movie.imdbID"
        :movie="movie"/>
      </div>
    </div>
  </div>
</template>

<script>
import MovieItem from './MovieItem.vue'
export default {
  components: {
    MovieItem
  },
  computed: {
    movies() {
      return this.$store.state.movie.movies
    },
    message() {
      return this.$store.state.movie.message
    }
  }
}
</script>

<style lang="scss" scoped>
@import "~/scss/main";
  .container {
    margin-top: 30px;
    .inner {
      background-color: $gray-200;
      padding: 10px 0;
      border-radius: 4px;
      text-align: center;
      &.no-result {
        padding: 70px 0;
      }
    }
    .message {
      color: $gray-400;
      font-size: 20px;
    }
    .movies{
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
    }
  }
</style>
```
- **movies** 배열이 비어있는 경우, **no-result** 클래스를 추가하도록 문자열 데이터를  `v-bind 디렉티브`로 연결해주었습니다.

- **message**가 있는 경우와 없는 경우를 구분하도록 `v-if, v-else 디렉티브`를 사용했습니다.

### 로딩 애니메이션
영화 목록 데이터를 받아오는 동안 로딩 애니메이션을 출력하도록 **movie.js**와 **MovieItem.vue** 컴포넌트를 수정하였습니다.
```js
export default {
  ...,
  actions: {
    async searchMovies({ state, commit }, payload) {
      if (state.loading) {
        return 
      }
      commit('updateState', {
        message: '',
        loading: true
      })
      try {
        const res = await _fetchMovie({
          ...payload,
          page: 1
        })
        const { Search, totalResults } = res.data
        console.log(res)
        commit('updateState', {
          movies: _uniqBy(Search, 'imdbID'),
        })
  
        const total = parseInt(totalResults, 10)
        const pageLength = Math.ceil(total / 10) 

        if (pageLength > 1) {
          for (let page = 2; page <= pageLength; page += 1) {
            if (page > (payload.number / 10)) {
              break
            }
            const res = await _fetchMovie({
              ...payload,
              page // page: page
            })
            const { Search } = res.data
            commit('updateState', {
              movies: [
                ...state.movies, 
                ..._uniqBy(Search, 'imdbID')
              ]
            })
          }
        }
      } catch (message) {
        commit('updateState', {
          movies: [],
          message 
        })
      } finally {
        commit('updateState', {
          loading: false
        })
      }
    }
  }
}
```
- 사용자가 엔터를 여러번 누르거나 **apply** 버튼을 여러번 눌러서 함수를 계속 실행시킬 수 있습니다. 이런 문제점을 방지하도록 `searchMovies()` 메서드의 제일 앞부분에 조건문을 추가하여 **loading**이 **true**이면 함수를 종료하도록 하였습니다.
- **finally** 구문을 사용하여 `searchMovies()` 메서드가 실행 종료되면 (에러든 정상 동작이든) **loading** 데이터를 **false**로 변경하도록 수정하였습니다.


#### MovieList.vue
```vue
<template>
  <div class="container">
    <div 
      :class="{ 'no-result': !movies.length }" 
      class="inner">
      <div 
        v-if="loading"
        class="spinner-border text-primary">
      </div>
      <div
        v-if="message" 
        class="message">
        {% raw %}{{ message }}{% endraw %}
      </div>
      <div
        v-else 
        class="movies">
        <MovieItem 
        v-for="movie in movies" 
        :key="movie.imdbID"
        :movie="movie"/>
      </div>
    </div>
  </div>
</template>
<script>
...
</script>
<style lang="scss" scoped>
...
</style>
```
- 영화 목록을 **GET Request**로 받아올 때 로딩 애니메이션을 출력하도록 `<div>태그`를 사용하였습니다.



## Vue 플러그인 (이미지 로드 이벤트) 

이번에는 <a href="https://v3.ko.vuejs.org/guide/plugins.html" target="_blank">**VueJS 플러그인**</a>을 직접 만들어서 영화 검색 목록들의 포스터가 생성될 때 까지 로딩 애니메이션을 각각 적용해주도록 하겠습니다. 일반적으로 `$`기호로 시작한다면, 그것을 플러그인이라고 유추할 수 있습니다.<br>

먼저 **MovieItem.vue** 컴포넌트를 수정해보겠습니다.
**movie.Poster**가 렌더링되어 출력되기 전까지는 로딩 애니메이션을 출력해주는 코드를 작성해보겠습니다.

**plugins**폴더를 만들어 **loadImage.js**파일을 다음과 같이 작성해주겠습니다. 그리고 마무리된 후에 **main.js** 파일에 연결해주도록 하겠습니다. 

```js
//loadImage.js
export default {
  install(app) {
    app.config.globalProperties.$loadImage = (src) => {
        return new Promise((resolve) => {
          const img = document.createElement('img')
          img.src = src
          img.addEventListener('load', () => {
            resolve()
        }) 
      })
    }
  }
}

// main.js
import { createApp } from 'vue'
import App from './App.vue'
import router from './routes'
import store from './store'
import loadImage from './plugins/loadImage'

createApp(App)
  .use(router) // $route, $router
  .use(store) // $store
  .use(loadImage) // $loadImage
  .mount('#app')

```
- `new Promise()`
  - `Promise()` 생성자를 만들어, **async/await**가 동작할 수 있는 구조를 만들었습니다.
  - 잘 이행되었을 때를 체크하기위해, **resolve** 인수를 받도록 하였습니다.
  - **'load'**가 완료되면, 잘 마무리 되었다는 의미로 `resolve()` 메서드를 실행하도록 했습니다.

```vue
<!--Movie.vue-->
<template>
  <div 
    :style="{ backgroundImage: `url(${movie.Poster})` }"
    class="movie">
    <Loader 
      v-if="imageLoading"
      :size="1.5"
      absolute />
    <div class="info">
      <div class="year">
        {% raw %}{{ movie.Year }}{% endraw %}
      </div>
      <div class="title">
        {% raw %}{{ movie.Title }}{% endraw %}
      </div>
    </div>
  </div>
</template>

<script>
import Loader from './Loader.vue'
export default {
  components: {
    ...
  },
  data() {
    return {
      imageLoading: true
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    async init() {
      await this.$loadImage(this.movie.Poster)
      this.imageLoading = false
    }
  }
}
</script>
```
- `imageLoading` 
  - **imageLoading** 데이터를 사용하여 현재 포스터 이미지의 로딩 상태를 체크하도록 하였습니다.
- `mounted() 라이프사이클 훅`
  - `created 라이프사이클 훅`은 컴포넌트 생성 직후 동작하기 때문에 **HTML** 구조와 연결되지 않습니다.
  - 즉, **HTML** 구조와 연결된 직후를 의미하기 위해서 `mounted 라이프사이클 훅`을 사용하였습니다.

- `init()`
  - **methods** 옵션 내부에 `init()` 함수를 만들었습니다.
  - 비동기로 작성하여 해당 영화의 포스터가 다 로드될 때까지 기다린 후, 완료가 되면 **imageLoading**을 **false**로 변경하였습니다.


### 영화 포스터가 존재하지 않는 경우
영화 포스터가 존재하지 않는 경우에는 로딩 애니메이션이 계속 실행됩니다. 따라서 예외 처리를 해주도록 코드를 작성하였습니다.<br>

**MovieItem.vue**컴포넌트의 코드를 다음과 같이 수정하였습니다.
```vue
<template>
  <div 
    :style="{ backgroundImage: `url(${movie.Poster})` }"
    class="movie">
    <Loader 
      v-if="imageLoading"
      :size="1.5"
      absolute />
    <div class="info">
      <div class="year">
        {% raw %}{{ movie.Year }}{% endraw %}
      </div>
      <div class="title">
        {% raw %}{{ movie.Title }}{% endraw %}
      </div>
    </div>
  </div>
</template>
<script>
import Loader from './Loader.vue'
export default {
  components: {
    Loader
  },
  props: {
    movie: {
      type: Object,
      default: () => ({})
    } 
  },
  data() {
    return {
      imageLoading: true
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    async init() {
      const poster = this.movie.Poster
      if (!poster || poster === 'N/A') {
        this.imageLoading = false
      } else {
        await this.$loadImage(this.movie.Poster)
        this.imageLoading = false
      }
    }
  }
}
</script>
```

**Movie**탭을 눌렀을 때, 해당 상세 설명창의 포스터도 로딩 애니메이션이 출력되게끔 **Movie.vue** 컴포넌트에 `<Loader>` 컴포넌트를 추가하였습니다. 그리고 **imageLoading** 데이터를 정의한 후 `v-if 디렉티브`로 **imageLoading**의 상태에 따라 출력되게 했습니다.

```vue
<template>
  <div class="container">
    <template>
      ...
    </template>
    <div 
      v-else 
      class="movie-details">
      <div 
        :style="{ backgroundImage: `url(${requestDiffSizeImage(theMovie.Poster, 1000)})` }" 
        class="poster">
        <Loader 
          v-if="imageLoading"
          absolute />
      </div>
      ...
</template>
<script>
import Loader from '~/components/Loader'
export default {
  components: {
    Loader
  },
  data() {
    return {
      imageLoading: true
    }
  },
  computed: {
    ...
  },
  created() {
    ...
  },
  methods: {
    requestDiffSizeImage(url, size = 700) {
      if (!url || url === 'N/A') {
        this.imageLoading = false
        return ''
      }
      const src = url.replace('SX300', `SX${size}`)
      this.$loadImage(src)
        .then(() => {
          this.imageLoading = false
        })
      return src
    }
  }
}

</script>
```
- `requestDiffSizeImage()`
  - **loadImage** 플러그인을 사용할 때, **async/await** 키워드를 사용하면 이미지가 다 불러와지고 나서 **return**을 해주게 됩니다. 
  - 따라서 여기서는 **src**를 먼저 반환해주고, `then()`키워드를 통해 이미지가 다 불러와지고 난 후에 **imageLoading**을 변경해주도록 하였습니다.


## Nav 경로 일치 및 활성화 

검색된 영화 목록들 중 영화를 클릭하면 실제로 해당 **item** 의 상세 페이지로 이동할 수 있도록. `<RouterLink>` 컴포넌트를 연결하도록 코드를 변경하였습니다.
```vue
<template>
  <RouterLink 
    :to="`/movie/${movie.imdbID}`"
    :style="{ backgroundImage: `url(${movie.Poster})` }"
    class="movie">
    <Loader 
      v-if="imageLoading"
      :size="1.5"
      absolute />
    <div class="info">
      <div class="year">
        {% raw %}{{ movie.Year }}{% endraw %}
      </div>
      <div class="title">
        {% raw %}{{ movie.Title }}{% endraw %}
      </div>
    </div>
  </RouterLink>
</template>

<script>
import Loader from './Loader.vue'
export default {
  components: {
    Loader
  },
  props: {
    movie: {
      type: Object,
      default: () => ({})
    } 
  },
  data() {
    return {
      imageLoading: true
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    async init() {
      const poster = this.movie.Poster
      if (!poster || poster === 'N/A') {
        this.imageLoading = false
      } else {
        await this.$loadImage(this.movie.Poster)
        this.imageLoading = false
      }
    }
  }
}
</script>
```

잘 작동하지만, 약간의 문제점이 있습니다. **Header**의 **Movie** 탭이 활성화 되어 있지 않는군요. **Header.vue** 컴포넌트를 수정합시다.

```vue
<!--Header.vue-->
<template>
  <header>
    <Logo />
    <div class="nav nav-pills">
      <div
        v-for="nav in navigations"
        :key="nav.name"
        class="nav-item">
        <RouterLink 
          :to="nav.href"
          active-class="active"
          :class="{ active: isMatch(nav.path)}"
          class="nav-link">
          {% raw %}{{ nav.name }}{% endraw %}
        </RouterLink>
      </div>
    </div>
  </header>
</template>

<script>
import Logo from './Logo'
export default {
  components: {
    Logo
  },
  data() {
    return {
      navigations: [
        {
          name: 'Search',
          href: '/'
        },
        {
          name: 'Movie',
          href: '/movie/tt4520988',
          path: /^\/movie/ // '/movie'
        },
        {
          name: 'About',
          href: '/about'
        },
      ]
    }
  },
  methods: {
    isMatch(path) {
      if (!path) {
        return false
      }
      console.log(this.$route)
      return path.test(this.$route.fullPath)
    }
  }
}
</script>
```
- `Movie tab`
  - 현재 겨울왕국 2의 내용만 `class="active"`로 작성되어 있어서 발생한 상황입니다.
  - 따라서 **navigations** 옵션 안에 정규표현식을 사용하여
    **/movie**로 시작하는 경로를 찾도록 하였습니다.
  - 찾은 경로가 실제 경로와 맞는지를 확인하는 `isMatch()` 메서드를 작성하였습니다.