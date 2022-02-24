---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  VueJS Clone 웹사이트를 사용자 친화적이게 만드는 과정을 수행하겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue
  - movie-search
  - vue-clone
---

# VueJS Clone - UI Improvement

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간에는 웹사이트를 사용자 친화적이게끔 수정하도록 하겠습니다.


---

## 검색정보 초기화

**movie.js**파일을 다음과 같이 수정하여 **resetMovies** 함수가 **message**, **loading**, **movies**를 모두 초기화하도록 하였습니다.
```js
//movie.js
import axios from 'axios'
import _uniqBy from 'lodash/uniqBy'

const _defaultMessage = 'Search for the movie title!'


export default {
  namespaced: true,
  state: () => ({
    movies: [],
    message: _defaultMessage,
    loading: false,
    theMovie: {}
  }),
  getters: {},
  mutations: {
    updateState(state, payload) {
      ...
    },
    resetMovies(state) {
      state.movies = []
      state.message = _defaultMessage
      state.loading = false
    }
  },
  actions: {
    ...
  }
}
```

**Home.vue**컴포넌트를 수정하여 **Home** 화면이 열릴 때 마다 `created() 라이프사이클 훅`이 실행되어 기본적인 데이터를 초기화할 수 있도록 하였습니다.
```vue
<!--Home.vue-->
<template>
  <Headline />
  <Search />
  <MovieList />
</template>

<script>
import Headline from '../components/Headline.vue'
import Search from '../components/Search.vue'
import MovieList from '../components/MovieList.vue'

export default {
  components: {
    Headline,
    Search,
    MovieList
  },
  // 
  created() {
    this.$store.commit('movie/resetMovies')
  }
}
</script>
```


## 페이지 전환 스크롤 위치 복구

<a href="https://router.vuejs.org/" target="_blank">**Vue Router**<a> 문서에 소개되어 있는 **Scroll Behavior** 메서드를 사용하도록 하겠습니다.

```js
// routes/index.js
import { createRouter, createWebHashHistory } from 'vue-router'
import Home from './Home'
import Movie from './Movie'
import About from './About'
import NotFound from './NotFound'


export default createRouter({
  history: createWebHashHistory(),
  scrollBehavior() {
    return { top: 0 }
  },
  routes: [
    {
      path: '/',
      component: Home
    },
    {
      path: '/movie/:id',
      component: Movie
    },
    {
      path: '/about',
      component: About
    },
    {
      path: '/:notFound(.*)',
      component: NotFound
    }
  ]
})
```