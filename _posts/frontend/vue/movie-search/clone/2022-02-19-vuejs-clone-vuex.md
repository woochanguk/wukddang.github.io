---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  VueX에 대해 알아봅시다.
invert_sidebar: false
categories:
  - frontend
  - vue
  - movie-search
  - vue-clone
---

# VueJS Clone - Vuex

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간에는 `Vuex`를 알아보겠습니다.

---

## Vuex(Store) 
### 개요
아래 사진을 먼저 보시죠.

<img src="/assets/img/vue/vuex-introduction.png" width="650" height="350"  >

최상위 컴포넌트로는 **App.vue**파일이 있고 *Home*, *Movie*, *About* 각각의 컴포넌트로 구분되고 있습니다. 

우리는 **부모-자식 컴포넌트간의 데이터 통신** 방법은 **props**, **emits**가 있고 **상위-하위 컴포넌트간 데이터 통신** 방법은 **provide**, **inject**가 있다고 배웠습니다만, 형제 컴포넌트간의 데이터 통신 방법은 따로 배우지 않았습니다. <br>

굳이 방법을 찾아보자면 **Search** 컴포넌트에서 부모 컴포넌트인 **Home**으로 데이터를 전달한 후 **MovieList** 컴포넌트로 전달하는 방식으로 하면 가능은 합니다만, **Search**에서 **About**으로 데이터를 옮겨야 한다면 굉장히 번거롭게 되겠죠? 굳이 불필요한 데이터 통신을 하지 않는 방법은 없을까요? <br>
이 때 사용할 수 있는 것이 바로 `Vuex`라고 하는 **중앙집중식 상태관리 라이브러리**라고 하고 통상적으로 이런 방식들을 **Store**라고 합니다.

<img src="/assets/img/vue/vuex-store.png" width="650" height="350"  >
위의 그림과 같이 데이터를 **중앙집중화**하여 관리할 수 있는 장소를 만들게 됩니다. 이후에 직접 해당 컴포넌트들로 데이터를 연결하여 쓸 수 있게 되는 것입니다. 이와 같이 **Store**라는 중간 매개체를 하나만 두어서 프로젝트 내부의 어떤 곳의 컴포넌트로도 데이터를 전달할 수 있다는 장점이 있습니다. <br>
**Movie**, **About** 컴포넌트와 관련된 데이터들은 따로 관리하는게 편하기 때문에, 각각을 모듈로 만들어서 관리할 것입니다. 여기서 데이터는 Store에서 **상태**라고 불립니다. 따라서 이렇게 데이터를 관리하는 개념을**중앙집중식 상태관리 패턴**이라고 하고, `Vuex`를 **중앙집중식 상태관리 패턴 라이브러리**라고 하게 되는 것입니다.

### 구성
자, 이제 프로젝트에 **vuex**를 설치해줍시다. 

>`npm i vuex@next`

그리고 **src**폴더 안에 **store**폴더를 생성해주고 **index.js**파일을 생성하겠습니다. 이 파일 안에서 기본적인 **Store**의 구성을 작성해주도록 하겠습니다. <br>
**Vuex**로부터 **createStore**메서드를 객체 구조분할로 가져오도록 하겠습니다. 그리고 **export default**로 **createStore**를 내보낼 것인데, 이 함수의 인수로 **객체 리터럴**을 만들어서 **modules**라는 속성을 추가해주고 **modules** 내부에 **Movie**, **About**과 같은 데이터 모듈들이 연결되는 구조라고 이해하시면 되겠습니다.<br>

이 **index.js**파일을 **main.js**에 연결을 해주어야 사용이 가능하기 때문에, **main.js**파일을 다음과 같이 작성해주었습니다.

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './routes/index.js'
import store from './store/index.js'

createApp(App)
  .use(router)
  .use(store)
  .mount('#app')
```
여기서 팁을 드리자면, 특정 폴더내부의 **index.js**파일을 가져올 때에는 **index.js**를 생략할 수 있습니다. 아래처럼 작성할 수 있다는 의미지요.

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './routes'
import store from './store'

createApp(App)
  .use(router)
  .use(store)
  .mount('#app')
```
현재는 **Store**만 연결되어 있는 것이고, 우리가 사용해야 할 **module**이 없는 상황이니 새로운 파일을 작성해주도록 하겠습니다. **movie.js**, **about.js**파일을 생성해주고 각각의 파일은 **Movie**, **About** 컴포넌트의 데이터를 관리해주는 역할로 사용하도록 합시다.<br>

먼저 **movie.js**파일입니다.

```js
export default {
  namespaced: true,
  state: () => ({
    movies: []
  }),
  getters: {
    movieIds(state) {
      return state.movies.map(m => m.imdbID)
    }
  },
  mutations: {
    resetMovies(state) {
      state.movies = []
    }
  },
  actions: {
    searchMovies(context) {
      context.state
      context.getters
      context.commit
    }
  }
}
```
- `namespaced` : **movie.js**가 하나의 **Store**에서 모듈화되어 사용될 수 있다는 것을 명시적으로 나타내는 옵션 입니다. 
  - 이 값에 `true`를 입력하면, **store**폴더의 **index.js**의 **modules**부분에 명시하여 별개의 개념으로 사용할 수 있습니다.

- `state` : 취급해야하는 각각의 **데이터**들을 의미합니다. 
  - 화살표 함수로 작성하여, **movies**라는 객체(배열) 데이터를 반환할 수 있는 구조로 만들었습니다.

- `getters` : **계산된 데이터**를 만들어내는 것을 의미합니다. (**computed**) 
  - **state**를 인수로 받게 됩니다.
  - **movies**라는 배열 데이터에서 `map()` 메서드를 이용하면 **imdbID**값만으로 이루어진 **새로운 배열 데이터**를 반환해줍니다. 반환 값을 movieIds로 받아 사용하겠다는 의미입니다.

- `mutations` : **메서드**와 유사합니다. 내부에 각각 함수들을 만들어서 **movie.js**의 데이터들을 활용할 수 있습니다.
  - **state**를 인수로 받게 됩니다.
  - **mutations**를 이용해서 데이터들을 변경시킬 수 있습니다. 즉 **getters**, **actions** 또는 **다른 컴포넌트에서는 데이터를 수정하는 것이 허용되지 않습니다.**
  - **state**의 **movies**부분을 새로운 배열로 초기화해줄 수 있습니다.

- `actions` : **메서드**와 유사합니다. 내부에 각각 함수들을 만들어서 **movie.js**의 데이터들을 활용할 수 있습니다. 
  - **context** 객체 데이터를 인수로 받게 됩니다.
  - 기본적으로 **actions**로 생성된 함수들은 **비동기**로 동작합니다.



## Vuex Helpers

**About.vue** 컴포넌트를 보면, **computed** 옵션에 데이터를 불러와 사용하고 있는데 일일이 다 입력하기에는 좀 번거로울 수 있는 상황입니다. 따라서 이번시간에는 **Vuex Helpers**를 사용해보도록 하겠습니다. <br>

컴포넌트를 수정해주면서 이해해보겠습니다.

```vue
<!--About.vue-->
<script>
import { mapState } from 'vuex'
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
    // about모듈에 있는 상태들을 배열 데이터로 불러오기
    // 문자데이터로 state들을 가져오기
    // mapState() 함수가 해당 state들을 객체 데이터로 반환해줌

    // computed옵션에 다른 데이터를 사용해줄 수도 있기 때문에, 
    // 아래처럼 전개연산자로 하나의 객체 데이터 내부에 작성하는 방식으로 작성해주는 게 좋다.
    ...mapState('about', [
      'image',
      'name',
      'email',
      'blog',
      'phone'
    ])
  },
    // image() {
    //   return this.$store.state.about.image
    // },
    // name() {
    //   return this.$store.state.about.name
    // },
    // email() {
    //   return this.$store.state.about.email
    // },
    // blog() {
    //   return this.$store.state.about.blog
    // },
    // phone() {
    //   return this.$store.state.about.phone
    // }
  //},
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

```vue
<!--Header.vue-->
<script>
import { mapState } from 'vuex'
import Logo from './Logo.vue'
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
          // 정규표현식 사용
          // ^ : 특정 표현식으로 시작한다는 의미. escape \를 사용해서 / 로 시작한다는 의미로 작성
          // 
          path: /^\/movie/ // '/movie'
        },
        {
          name: 'About',
          href: '/about'
        },
      ]
    }
  },
  computed: {
    ...mapState('about', [
      'image',
      'name'
    ]),
    ...mapState('movie', [
      'movies',
      'loading',
      'message',
      'theMovie'
    ])
    // image() {
    //   return this.$store.state.about.image
    // },
    // name() {
    //   return this.$store.state.about.name
    // }
  },
  methods: {
    isMatch(path) {
      if (!path) {
        return false
      }
      console.log(this.$route)
      return path.test(this.$route.fullPath)
    },
    toAbout() {
      console.log('!!!')
      // router 플러그인 사용
      // push로 페이지 이동 가능. RouterLink 컴포넌트를 꼭 사용할 필요 없음
      this.$router.push('/about')
    }
  }
}
</script>
```

```vue
<!--MovieList.vue-->
<script>
import { mapState } from 'vuex'
import MovieItem from './MovieItem.vue'
import Loader from './Loader'
export default {
  components: {
    MovieItem,
    Loader
  },
  computed: {
    ...mapState('movie', [
      'movies',
      'message',
      'loading'
    ])
    // movies() {
    //   return this.$store.state.movie.movies
    // },
    // message() {
    //   return this.$store.state.movie.message
    // },
    // loading() {
    //   return this.$store.state.movie.loading
    // }
  }
}
</script>
```

```vue
<!--Movie.vue-->
<script>
import { mapState, mapActions } from 'vuex'
import Loader from '../components/Loader'
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
    ...mapState('movie', [
      'theMovie',
      'loading'
    ])
    // theMovie() {
    //   return this.$store.state.movie.theMovie
    // },
    // loading() {
    //   return this.$store.state.movie.loading
    // }
  },
  created() {
    console.log(this.$route)
    // this.$store.dispatch('movie/searchMovieWithId', {
    this.searchMovieWithId({
      id: this.$route.params.id
    })
  },
  methods: {
    ...mapActions('movie', [
      'searchMovieWithId'
    ]),
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
**State**의 데이터를 가져올 때는 유용하지만 **Actions**나 **Mutations**에 **Helpers**를 사용하면 직관적이지 않게 된다는 단점이 존재하므로, 데이터를 가져올 때만 **Helpers****(mapState)**를 사용하는 것을 추천합니다.

## Vuex 핵심 정리

### Vuex Options
<img src="/assets/img/vue/vuex-options.png" width="450" height="350"  >

- `state` : **VueJS**의 데이터와 유사합니다.
- `getters` : **VueJS**의 **computed** 옵션과 유사합니다. **state**를 계산된 형태로 사용하기 위해 **getters**를 이용합니다.
- `mutations` : **state**를 변경할 수 있는 권한을 갖고 있어, **state**를 변경하는 용도로 사용되는 메서드입니다.
- `actions` : 일반적인 메서드를 정의해줍니다.

### Vuex Options Communication
**Vuex**의 **Option**들은 다음과 같이 통신합니다.

<img src="/assets/img/vue/vuex-state-getters.png" width="400" height="300"  >
<img src="/assets/img/vue/vuex-state-mutations.png" width="400" height="300"  >


<img src="/assets/img/vue/vuex-options-diagram.png" width="600" height="400"  >

- **actions** 옵션은 첫 인수로 **context**를 받게 됩니다. **context**를 살펴보면 **state**, **getters**, **commit(mutations)**, **dispatch(actions)**의 내용들이 들어있습니다. 
- 필요한 내용만을 사용하려면 객체 구조분할을 수행해주면 됩니다.

### Vuex Options Description

<img src="/assets/img/vue/vuex-options-description.png" width="600" height="400"  >

- **mapState**, **mapGetters**의 경우 **VueJS** 컴포넌트의 **computed** 옵션에 작성해주어야 합니다.

- **mapMutations**, **mapActions**의 경우 **VueJS** 컴포넌트의 **methods** 옵션에 작성해주어야 합니다.