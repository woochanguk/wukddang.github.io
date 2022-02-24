---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  VueJS Clone 웹사이트 주변 페이지를 구성해보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue
  - movie-search
  - vue-clone
---

# VueJS Clone - Side Pages

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간에는 사이트의 설명탭을 구현해보겠습니다.


---

## Side Pages

### About

```js
export default {
  namespaced: true,
  // state: 데이터. 하나의 함수로 만들어 주면 됩니다.
  // 왜 함수로?
  // 객체 데이터는 하나의 참조형 데이터 -> 따라서 불변성 X
  // 그러므로 데이터의 불변성을 유지하려면, 함수로 만들어서 반환해주어야 
  // state에서 사용하는 데이터를 고유하게 유지할 수 있습니다.
  // 객체를 반환해주려면 ({}) 와 같이 작성해주어야 합니다.
  state: () => ({
    name: 'CHANGUK',
    email: 'dnr8874@gmail.com',
    blog: 'wukddang.github.io',
    phone: '+82-10-4332-8874',
    image: 'https://raw.githubusercontent.com/funkyblues/vue3-study/bb23abc5536bb1472fdd37e147867d5583ace843/src/assets/logo.jpg'
  })
}
```

```vue
<!-- About.vue -->
<template>
  <div class="about">
    <div class="photo">
      <Loader 
        v-if="imageLoading" 
        absolute />
      <img 
        :src="image" 
        :alt="name" />
    </div>
    <div class="name">
      {{ name }}
    </div>
    <div>{{ email }}</div>
    <div>{{ blog }}</div>
    <div>{{ phone }}</div>
  </div>
</template>

<script>
import Loader from "../components/Loader"
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
    image() {
      return this.$store.state.about.image
    },
    name() {
      return this.$store.state.about.name
    },
    email() {
      return this.$store.state.about.email
    },
    blog() {
      return this.$store.state.about.blog
    },
    phone() {
      return this.$store.state.about.phone
    }
  },
  // mounted된 후에 실행. (HTML 연결)
  mounted() {
    this.init()
  },
  methods: {
    async init() {
      await this.$loadImage(this.image)
      this.imageLoading = false
    }
  }
}
</script>
```

### 404 Page Not Found

<a href="https://router.vuejs.org/guide/essentials/dynamic-matching.html#catch-all-404-not-found-route" target="_blank">**Vue Router**</a> 문서를 참조해주세요. 소괄호 `()` 안에 정규표현식을 이용하여 경로를 매칭한 후 만들어둔 컴포넌트에 연결해줄 수 있습니다.
**Header**에 만든 웹사이트 이외의 주소로 접근하려 한다면, **404 error**를 띄워줍니다. 


```vue
<!--NotFound.vue-->
<template>
  <div class="not-found">
    <div class="status">
      404
    </div>
    <div class="message">
      Page Not Found!
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import "../scss/main.scss";

.not-found {
  line-height: 1.2;
  text-align: center;
  font-family: "Oswald", sans-serif;
  padding: 80px 20px;
  .status {
    font-size: 160px;
    color: $primary;
  }
  .message {
    font-size: 50px;
  }
}
</style>
```

```js
// routes/index.js
import { createRouter, createWebHashHistory } from 'vue-router'
import Home from './Home'
import Movie from './Movie'
import About from './About'
import NotFound from './NotFound'


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
    {
      // 위의 경로(컴포넌트)에 해당하지 않는 페이지들 전부를 의미.
      // : 뒤엔 아무글자나 적어도 된다.
      path: '/:notFound(.*)',
      component: NotFound
    }
  ]
})
```

### 부트스트랩 Breakpoint (반응형)
화면을 줄이거나 늘일 때 요소들이 자연스럽게 줄어들고 늘어날 수 있도록 **Bootstrap**의 <a href="https://getbootstrap.com/docs/5.1/layout/breakpoints/" target="_blank">**Breakpoint**</a>라는 기능을 사용하도록 하겠습니다. 그중에서도 **Min-width**, **Max-width**라는 내용을 참조하여 사용할 것입니다. 


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
          {{ nav.name }}
        </RouterLink>
      </div>
    </div>
    <div 
      class="user" 
      @click="toAbout">
      <img 
        :src="image" 
        :alt="name" />
    </div>
  </header>
</template>
<script>
...
</script>

<style lang="scss" scoped>
@import "../scss/main.scss";
  header {
    height: 70px;
    padding: 0 40px;
    display: flex;
    align-items: center;
    position: relative;
    .logo {
      margin-right: 40px;
    }
    .user {
      width: 60px;
      height: 60px;
      padding: 6px;
      border-radius: 50%;
      box-sizing: border-box;
      background-color: $gray-200;
      cursor: pointer;
      position: absolute;
      top: 30px;
      bottom: 0;
      right: 40px;
      margin: auto;
      transition: .4s;
      &:hover {
        // 10% 더 어두운 색상
        background-color: darken($gray-200, 10%);
      }
      img {
        width: 100%;
        border-radius: 50%;
      }
    }
    // Bootstrap Breakpoint 작성
    // Bootstrap 사이트에 있는 기준에 따라 sm보다 작으면 header가 사라지는 코드
    @include media-breakpoint-down(sm) {
      .nav {
        display: none;
      }
    }
  } 
</style>
```

사이트의 폭이 일정 크기 이상 줄어들면, `display: flex`를 `display: block`으로 변경하도록 하였습니다.
```vue
<!--Search.vue-->
<template>
  <div class="container">
    <input 
      v-model="title" 
      class="form-control"
      type="text"
      placeholder="Search for Movies, Series & more" 
      @keyup.enter="apply"/>
    <div class="selects">
      <select
        v-for="filter in filters"
        v-model="$data[filter.name]"
        :key="filter.name"
        class="form-select">
        <option 
          v-if="filter.name === 'year'"
          value="">
          All Years
        </option>
        <option
          v-for="item in filter.items"
          :key="item">
          {{ item }}
        </option>
      </select>
    </div>
    <button class="btn btn-primary" @click="apply">
      Apply
    </button>
  </div>
</template>

<script>
...
</script>

<style lang="scss" scoped>
@import "../scss/main.scss";
  .container {
    ...
    // breakpoint lg: viewport의 가로너비가 lg보다 작은경우
    @include media-breakpoint-down(lg) {
      // display: flex -> block. 수평 -> 수직
      display: block;
      input {
        margin-right: 0;
        margin-bottom: 10px;
      }
      .selects {
        margin-right: 0;
        margin-bottom: 10px;
        select {
          width: 100%;
        }
      }
      .btn {
        width: 100%;
      }
    }
  }
</style>
```

```vue
<!--Movie.vue-->
<template>
  <div class="container">
    ...
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
      <div class="specs">
        <div class="title">
          {{ theMovie.Title }}
        </div>
        <div class="labels">
          <span>{{ theMovie.Released }}</span>
          <span>{{ theMovie.Runtime }}</span>
          <span>{{ theMovie.Country }}</span>
        </div>
        <div class="plot">
          {{ theMovie.Plot }}
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
              <span>{{ score }}</span>
            </div>
          </div>
        </div>
        <div>
          <h3>Actors</h3>
          {{ theMovie.Actors }}
        </div>
        <div>
          <h3>Director</h3>
          {{ theMovie.Director }}
        </div>
        <div>
          <h3>Production</h3>
          {{ theMovie.Production }}
        </div>
        <div>
          <h3>Genre</h3>
          {{ theMovie.Genre }}
        </div>  
      </div>
    </div>
  </div>
</template>

<script>
...
</script>

<style lang="scss" scoped>
@import '../scss/main';
...
.movie-details {
  ...
  @include media-breakpoint-down(xl) {
    .poster {
      width: 300px;
      height: 300px * 3 / 2;
      margin-right: 40px;
    }
  }
  @include media-breakpoint-down(lg) {
    display: block;
    .poster {
      margin-bottom: 40px;
    }
  }
  @include media-breakpoint-down(md) {
    .specs {
      .title {
        font-size: 50px;
      }
      .ratings {
        .rating-wrap {
          display: block;
          .rating {
            margin-top: 10px;
          }
        }
      }
    }
  }
}
</style>
```

