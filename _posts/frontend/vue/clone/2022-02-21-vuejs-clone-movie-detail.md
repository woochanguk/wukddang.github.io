---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  VueJS Clone 영화 상세 페이지를 구현해봅시다.
invert_sidebar: false
categories:
  - frontend
  - vue
  - vue-clone
---

# VueJS Clone - Movie Detail

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

## Movie About

### 단일 영화 상세 정보 가져오기
<a href="http://www.omdbapi.com/" target="_blank">**OMDb API**</a>사이트에서 **Parameters**를 확인해보면, **i Parameter**를 사용해서 영화의 상세 정보를 확인할 수 있습니다. 이것을 사용해서 먼저 콘솔 창에 상세 정보를 가져오도록 하겠습니다. 먼저 **movie.js**파일을 다음과 같이 작성해주겠습니다.

#### movie.js
```js
export default {
  ...,
  actions: {
    async searchMovies({ state, commit }, payload) {
      ...
    }
    async searchMovieWithId( { state, commit }, payload) {
      if (state.loading) {
        return
      }
      commit('updateState', {
        theMovie: {},
        loading: true
      })

      try {
        const res = await _fetchMovie(payload)
        commit('updateState', {
          theMovie: res.data
        })
      } catch (error) {
        commit('updateState', {
          theMovie: {}
        })
      } finally {
        commit('updateState', {
          loading: false
        })
      }
    }
  }
}

function _fetchMovie(payload) {
  const { title, type, year, page, id } = payload
  const OMDB_API_KEY = 'a0a1e9a9'
  const url = id 
    ? `https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&i=${id}` 
    : `https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=${page}`

  return new Promise((resolve, reject) => {
    axios.get(url)
      .then(res => {
        if (res.data.Error) {
          reject(res.data.Error)
        }
        resolve(res)
      })
      .catch(err => {
        reject(err.message)
      })
  })
}
```
- `searchMovieWithId()` 메서드
  - **context** 인수 안의 **state**와 **commit**을 객체 구조분할하여 인수로 받았습니다.
  - `searchMovies()` 메서드와 동일하게, **loading**중이면 함수를 바로 중단하도록 조건문을 작성하였습니다.
  - 메서드를 실행했을 때, 이전에 조회된 영화의 상세목록을 보지 않도록 초기화를 수행하였습니다.
  - **payload** 객체 안의 **id**만 사용하기 때문에 굳이 객체 구조분할을 수행할 필요 없이 그 자체를 인수로서 사용하였습니다.

- `_fetchMovie()` 메서드
  - 삼항 연산자를 사용하여 **id**가 있을 때와 없을 때를 구분하여 **url**을 정의해 주었습니다.

- **state** 옵션
  - 새로운 **theMovie** 객체 데이터가 추가되었습니다.

#### index.js
**Header** 부분(컴포넌트)의 **Movie**를 클릭했을 때, 영화 상세 설명 페이지로 이동이 되어야 합니다. 그러기 위해서는 먼저 **url**주소에 영화의 **imdbID**가 나타나야 하므로 
**routes** 폴더 안의 **index.js**파일을 다음과 같이 **routes** 옵션 안의 **path**를 수정해주어야 합니다. 그럼 아래와 같은 형태의 **url**로 접속했을 때, **Movie** 컴포넌트가 실행이 될 것입니다.

```js
import { createRouter, createWebHashHistory } from 'vue-router'
import Home from './Home'
import Movie from './Movie'
import About from './About'


export default createRouter({
  history: createWebHashHistory(),
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
  ]
})
```

#### Movie.vue
**movie.js**에서 작성된 **searchMovieWithId**는 **Movie** 컴포넌트에서 동작되어야 합니다. 따라서 **Movie.vue** 컴포넌트를 다음과 같이 수정해주었습니다.

```vue
<template>
  <h1>Movie!</h1>
</template>

<script>
export default {
  created() {
    console.log(this.$route)
    this.$store.dispatch('movie/searchMovieWithId', {
      id: this.$route.params.id
    })
  }
}
</script>
```

- `created()` **라이프사이클 훅**
  - 데이터가 생성된 후에 처리를 해주어야 하므로, `created()` 라이프사이클 훅을 사용하였습니다.
  - `this.$route`가 어떤 형태로 출력되는지 확인하였습니다.
    - **fullPath**: "/movie/tt4520988"
    - **hash**: "" 
    - **href**: "#/movie/tt4520988" 
    - **matched**: [{…}] 
    - **meta**: {}
    - **name**: undefined 
    - **params**: {id: 'tt4520988'} 
    - **path**: "/movie/tt4520988"
    - **query**: {} 
    - **redirectedFrom**: undefined
    - **[[Prototype]]**: Object
  - **store** 내부 **movie.js**의 **actions** 안에 있는 `searchMovieWithId() `메서드를 실행시키기 위해, 
- **routes** 폴더  **index.js**파일의 **routes** 배열 데이터에 작성해둔 객체를 보면 `path: '/movie/:id'` 라는 코드가 있습니다. 이 `:id`는 **url**에 입력되는 `/movie/`뒤의 주소를 데이터로 받아 저장하게 됩니다. 



먼저 **imdbID**로 영화를 검색했을 때, 상세 내용들이 잘 출력되는지를 확인해보도록 하겠습니다. <br>

**movie.js**에 작성한 **state**옵션에 **theMovie**로 객체 데이터를 정의하였습니다. 그 후 `created()` 라이프사이클 훅을 정의해서 데이터가 생성된 후에 **url** 주소값에서 **id**를 찾아 그 값을 **id**에 할당해주는 코드를 아래와 같이 작성하였습니다.
```vue
<script>
export default {
  computed: {
    theMovie() {
      return this.$store.state.movie.theMovie
    },
  },
  created() {
    console.log(this.$route)
    this.$store.dispatch('movie/searchMovieWithId', {
      id: this.$route.params.id
    })
  },
}
</script>

```
### 스켈레톤 UI & Loader

#### Loader 
이전에 코드마다 직접 작성해 주었던, **Loading** 상태를 출력하는 코드를 `Loader 컴포넌트`로 작성하여 다른 컴포넌트에서도 가져다 사용할 수 있도록 하였습니다.<br>

클래스를 데이터 바인딩을 통해 얻어올 수 있도록 하였고 `absolute: absolute, fixed: fixed` 와 같이 속성값과 데이터가 같아서 축약하여 작성했습니다. 기억해두면 좋은 것은, **VueJS**에서는 자체적으로 **camelCase**를 **dash-case**로 변경해줄 수 있습니다.
```vue
<template>
  <div 
    :style="{
      width: `${size}rem`,
      height: `${size}rem`,
      zIndex
    }"
    :class="{ 
      absolute, 
      fixed 
    }" 
    class="spinner-border text-primary"></div>
</template>

<script>
export default {
  props: {
    size: {
      type: Number,
      default: 2,
    },
    absolute: {
      type: Boolean,
      default: false
    },
    fixed: {
      type: Boolean,
      default: false
    },
    zIndex: {
      type: Number,
      default: 0
    },
  }
}
</script>

<style lang="scss" scoped>
.spinner-border {
  margin: auto;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  &.absolute {
    position: absolute;
  }
  &.fixed {
    position: fixed;
  }
}
</style>
```

#### 스켈레톤 UI

**Movie** 버튼을 눌렀을 때 데이터가 불러와지는 것을 기다리는 동안, 기본적인 **HTML** 구조만을 보여주는 형태의 코드를 작성하겠습니다. 이를 **스켈레톤 UI**라고 합니다. <br>

먼저 **loading**이 **true** 이면 **skeleton ui**를 출력하도록 하고 그렇지 않으면 상세정보를 출력하도록 하였습니다.
```vue
<template>
  <div class="container">
    <template 
      v-if="loading">
      <div class="skeletons">
        <div class="skeleton poster"></div>
        <div class="specs">
          <div class="skeleton title"></div> 
          <div class="skeleton spec"></div> 
          <div class="skeleton plot"></div> 
          <div class="skeleton etc"></div> 
          <div class="skeleton etc"></div> 
          <div class="skeleton etc"></div> 
        </div>
      </div>
      <Loader 
        :size="3"
        :z-index="9"
        fixed />
    </template>
    ...
</template>
<style lang="scss" scoped>
@import '~/scss/main';
.container {
  padding-top: 40px;
}
.skeletons {
  display: flex;
  .poster {
    flex-shrink: 0;
    width: 500px;
    height: 500px * 3 / 2;
    margin-right: 70px;
  }
  .specs {
    flex-grow: 1;
  }
  .skeleton {
    border-radius: 10px;
    background-color: $gray-200;
    &.title {
      width: 80%;
      height: 70px;
    }
    &.spec {
      width: 60%;
      height: 30px;
      margin-top: 20px;
    }
    &.plot {
      width: 100%;
      height: 250px;
      margin-top: 20px;
    }
    &.etc {
      width: 50%;
      height: 50px;
      margin-top: 20px;
    }
  }
}
```

### 영화 상세 페이지 정리

#### Ratings 데이터 출력 

**github**에 올라와있는 이미지를 사용해서 **Ratings** 데이터와 함께 출력하도록 하겠습니다. **theMovie.Ratings**의 배열은 객체로 이루어져 있기 때문에, 사용하기 전에 바로 객체 구조분할 하고 쓰겠다는 의미로 `{ Source: name, Value: score } in theMovie.Ratings`와 같이 작성하였습니다. **github**에 올라와있는 **png**파일의 이름을 데이터를 입력해주기 위하여 **데이터바인딩**을 이용하였습니다.
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
        :style="{ backgroundImage: `url(${requestDiffSizeImage(theMovie.Poster)})` }" 
        class="poster"></div>
      <div class="specs">
        <div class="title">
          {% raw %}{{ theMovie.Title }}{% endraw %}
        </div>
        <div class="labels">
          <span>{% raw %}{{ theMovie.Released }}{% endraw %}</span>
          <span>{% raw %}{{ theMovie.Runtime }}{% endraw %}</span>
          <span>{% raw %}{{ theMovie.Country }}{% endraw %}</span>
        </div>
        <div class="plot">
          {% raw %}{{ theMovie.Plot }}{% endraw %}
        </div>
        <div class="ratings">
          <h3>Ratings</h3>
          <div class="rating-wrap">
            <div
              v-for="{ Source: name, Value: score } in theMovie.Ratings"
              :key="name" 
              :title="name"
              class="rating">
              <img 
                :src="`https://raw.githubusercontent.com/ParkYoungWoong/vue3-movie-app/master/src/assets/${name}.png`"
                :alt="name" />
              <span>{% raw %}{{ score }}{% endraw %}</span>
            </div>
          </div>
        </div>
        <div>
          <h3>Actors</h3>
          {% raw %}{{ theMovie.Actors }}{% endraw %}
        </div>
        <div>
          <h3>Director</h3>
          {% raw %}{{ theMovie.Director }}{% endraw %}
        </div>
        <div>
          <h3>Production</h3>
          {% raw %}{{ theMovie.Production }}{% endraw %}
        </div>
        <div>
          <h3>Genre</h3>
          {% raw %}{{ theMovie.Genre }}{% endraw %}
        </div>  
      </div>

    </div>
  </div>
</template>

<script>
import Loader from '~/components/Loader'
export default {
  components: {
    Loader
  },
  computed: {
    theMovie() {
      return this.$store.state.movie.theMovie
    },
    loading() {
      return this.$store.state.movie.loading
    }
  },
  created() {
    console.log(this.$route)
    this.$store.dispatch('movie/searchMovieWithId', {
      id: this.$route.params.id
    })
  }
}
</script>

```
#### 더 높은 해상도의 영화 포스터 가져오기
AWS에서는 <a href="https://heropy.blog/2019/07/21/resizing-images-cloudfrount-lambda/" target="_blank">**Lambda@edge**</a>로 실시간 이미지 리사이징 기능을 지원합니다.
```vue
<script>
import Loader from '~/components/Loader'
export default {
  components: {
    Loader
  },
  computed: {
    ...
  },
  created() {
    ...
  },
  methods: {
    requestDiffSizeImage(url, size = 700) {
      return url.replace('SX300', `SX${size}`)
    }
  }
}
</script>
```
