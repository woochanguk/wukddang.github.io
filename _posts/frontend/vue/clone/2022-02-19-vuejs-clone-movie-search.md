---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  VueJS Clone 영화 검색을 구현해봅시다.
invert_sidebar: false
categories:
  - frontend
  - vue
  - vue-clone
---

# VueJS Clone - Movie Search

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간에는 영화 검색을 구현해보겠습니다.


---
## Code tester 
이번시간의 결과 모습입니다.
(codesandbox 오류. 추후 업데이트)
<!-- <iframe src="https://codesandbox.io/embed/exciting-babbage-fkmwhz?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="exciting-babbage-fkmwhz"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe> -->

---

## Movie Search
먼저 **Search.vue** 컴포넌트에서 methods 옵션 내부에 작성한 `searchMovies()` 메서드를 **movie.js**의 **actions** 옵션으로 옮겨주도록 하겠습니다. 그리고 내부에 작성한 **this** 키워드도 지워주었습니다. <br>
그러면 이제 **title**, **type**, **year**를 입력할 수 있는 구조를 작성해주어야 하므로 **context**, **payload**라는 매개변수를 받도록 하겠습니다. <br>

```js
export default {
  ...
  actions: {
    async searchMovies(context, payload) {
      const { title, type, number, year} = payload
      const OMDB_API_KEY = '7035c60c'
      const res = await axios.get(`https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=1`)
    }
  }
}
```

**context**에는 **state**, **getters**, **mutations**를 활용할 수 있는 내용들이 들어가 있고 **payload**에는 `searchMovies()` 메서드가 실행될 때 인수로 들어온 특정한 데이터들이 들어오게 되고, **객체 구조분해**를 통해서 사용할 수 있게 됩니다. <br> 이제 이 actions를 동작버튼을 눌렀을 때 실행되도록 만들어주면 되겠죠?

**Search.vue** 컴포넌트로 돌아와서 **methods** 옵션에 다음 내용을 작성해주도록 하겠습니다. **main.js**파일에 우리가 정의해 둔 **store**에 접근해서 `dispatch()` 메서드를 통해 **actions**를 실행하도록 하겠습니다. 실행할 **action**은 **movie.js**파일 안에 들어있는데 우리는 **store**폴더 안의 **index.js**에 **movie**로서 **import** 해주었기 때문에, **movie**라는 이름을 사용해야 합니다. 그리고 `searchMovies()` **action**을 실행해주면 되겠습니다.

(참고로 **Store**의 **Mutations**를 실행할 때는 `.commit()` 메서드를, **Actions**를 실행할 때는 `.dispatch()` 메서드를 사용합니다.)

```vue
<script>
  ...
  methods: {
      async apply() {
        this.$store.dispatch('movie/searchMovies', {
          title: this.title,
          type: this.type,
          number: this.number,
          year: this.year,
        })
      }
    }
  ...
</script>
```
`dispatch()` 메서드에는 객체 데이터인 **payload** 매개변수를 전달해줄 수 있으므로, **title**, **type**, **number**, **year**를 인수로 전달해주면 되겠습니다. **Search.vue**컴포넌트 위에 작성해 둔 `data()` 메서드에는 **title**, **type**, **number**, **year** 데이터가 정의되어 있고 이 데이터들은 `<template>`태그에서 `v-for, v-model 디렉티브`를 통해 반복 출력, 양방향 데이터 바인딩으로 연결되어 있습니다. 따라서, 이 데이터들은 **this** 키워드를 이용해서 받아올 수 있습니다. <br>


**movie** 모듈의 `searchMovies()` 메서드는 이 객체 데이터를 인수로 받아서 사용할 수 있게 됩니다. 비동기 방식을 이용해서 영화 데이터를 받아서 **res**에 할당하면 **res** 객체 데이터의 **data**속성을 통해 **OMDbAPI에서 제공하는** **Search**와 **totalResults**라는 데이터를 활용할 수 있게 됩니다. **API**를 통해서 전달받은 **Search** 데이터를 **state**에 정의해 둔 **movies**로 할당해주기 위해서는 **mutations** 옵션에 데이터를 전달해주면 됩니다. `searchMovies()`에서 받은 **context** 매개변수에는 `commit()` 이라는 메서드가 있습니다. 이 메서드를 이용해서 **mutations** 옵션 내부의 `updateState()` 메서드를 실행할 수 있고 이 메서드를 통해 **APIkey**를 통해 받아온 데이터를 원래의 데이터들에 할당해줄 수 있게 됩니다. <br>

`commit()` 메서드를 실행할 때 인수로서 객체 데이터를 전송해주고 있기 때문에, `Object.keys()`를 사용해서 **객체 데이터의 속성의 이름값으로 이루어지는 새로운 배열 데이터**를 만들어 주겠습니다. 현 상황에서는 **movies** 이름만 가지는 배열데이터가 생성되겠네요. 추후 추가될 데이터도 고려해서 ,`forEach()`를 통해 반복적으로 동작할 수 있도록 구문을 작성해 주었습니다.

```js
import axios from 'axios'

export default {
  namespaced: true,
  state: () => ({
    movies: [],
    message: '',
    loading: false,
  }),
  getters: {},
  mutations: {
    updateState(state, payload) {
      Object.keys(payload).forEach(key => {
        state[key] = payload[key]
      }) 
    },
    ...
  },
  actions: {
    async searchMovies({ commit }, payload) {
      const { title, type, number, year} = payload
      const OMDB_API_KEY = 'a0a1e9a9'
      const res = await axios.get(`http://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=1`)
      console.log(res)
      const { Search, totalResults } = res.data
      commit('updateState', {
        movies: Search,
      })
    }
  }
}
```

이제 데이터를 업데이트해줄 수 있는 코드는 작성해주었으니, 컴포넌트에서 데이터를 사용하면 되겠습니다. **MovieList.vue**컴포넌트에서 **computed** 옵션안에 `movies()` 메서드를 정의해주고, `this.$store.state.movie.movies`를 반환해주도록 하겠습니다. **반응성을 유지**하도록 계산된 데이터인 **computed** 옵션을 사용해준다고 이해하시면 되겠습니다. **this** 키워드를 통해 **$store**로 접근하여 **movie.js**로 만들어준 **movie**모듈의 **state**에 정의된 **movies** 배열 데이터를 반환해주겠다는 의미입니다. <br>

`<template>`태그의 `<MovieItem />` 컴포넌트에 작성된 **movies**는 바로 **computed** 옵션에서 계산된 **movies** 배열 데이터를 가져다가 사용한다는 개념이 되겠지요. 이 배열 데이터의 각 요소들을 자식 컴포넌트인 `<MovieItem />`컴포넌트로 전달해주기 위해 **props**를 사용하면 되겠습니다.


```vue
<template>
  <div class="container">
    <div class="inner">
      <MovieItem 
        v-for="movie in movies" 
        :key="movie.imdbID"
        :movie="movie"/>
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
    }
  }
}
</script>
```

자, 거의 다 왔습니다. `<MovieItem>` 컴포넌트만 마무리해주면 됩니다. <MovieList>` 컴포넌트에서 받은 객체 데이터인 **movie**를 **props**를 통해 type과 default를 
지정해주면 끝납니다. 객체 데이터를 그냥 반환하면 경고가 발생하여서, 추후 문제가 생기지 않도록 함수로 만들어 객체 데이터를 반환하였습니다.
```vue
<template>
  <div>{{ movie.Title }}</div>
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
```
마지막으로 `<template>` 태그에 **movie** 객체 데이터의 **Title**속성을 참조하는 코드를 작성하여 영화이름을 출력해주었습니다.


### 영화 검색 추가요청
영화검색 목록을 10개만이 아니라 20개, 30개도 출력할 수 있도록 코드를 작성해주도록 하겠습니다. <br>

총 페이지 수를 의미하는 **totalResults** 데이터는 문자 데이터이기 때문에, 10진수 정수 데이터로 변환하여 상수 **total**에 할당해주었고, 검색해야 할 페이지의 수를 의미하는 상수 **pageLength**는 **total**을 10으로 나누어 할당해주었습니다. 만약, 한번에 출력되었으면 하는 영화목록의 수가 20, 30개라면, 반복문을 통해서 출력될 수 있게 하였고 반복문 내부의 코드는 크게 어렵지 않으니 보면 이해할 수 있을 것입니다. <br>

다만 **context** 매개변수에 **state**를 객체 구조분할 해주어야 사용 가능하므로 **state**도 가져오면 되겠습니다. **이전의 영화 목록들을 제거하지 않고 추가**해주어야 하기 때문에 **전개 연산자**를 사용하여 영화 목록들을 추가하였습니다.
```js
import axios from 'axios'

export default {
  ...
  actions: {
    async searchMovies({ state, commit }, payload) {
      const { title, type, number, year} = payload
      const OMDB_API_KEY = 'a0a1e9a9'
      const res = await axios.get(`http://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=1`)
      const { Search, totalResults } = res.data
      commit('updateState', {
        movies: Search,
      })

      const total = parseInt(totalResults, 10)
      const pageLength = Math.ceil(total / 10) 

      if (pageLength > 1) {
        for (let page = 2; page <= pageLength; page += 1) {
          if (page > (number / 10)) {
            break
          }
          const res = await axios.get(`http://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=${page}`)
          const { Search } = res.data
          commit('updateState', {
            movies: [...state.movies, ...Search]
          })
        }
      }
    }
  }
}
```


### 영화 목록에서 ID 중복 제거
영화 목록의 ID값이 중복되는 문제를 해결해주기 위해, 추가적인 코드를 작성해주도록 하겠습니다. 원래는 이렇게 중복값을 내보내는 DB의 코드를 수정하는게 최우선이지만, 현재 API를 받아서 사용하는 입장이기 때문에 프론트엔드에서 수정해서 사용하려고 합니다. <br> 우리는 <a href="https://lodash.com/docs/4.17.15#uniqBy" target="_blank">**lodash**</a> 라이브러리의 `_uniqBy()` 메서드를 사용해서 이 기능을 구현하겠습니다.

```js
import axios from 'axios'
import _uniqBy from 'lodash/uniqBy'

export default {
  ...,
  actions: {
    async searchMovies({ state, commit }, payload) {
      ...
      commit('updateState', {
        movies: _uniqBy(Search, 'imdbID'),
      })

      const total = parseInt(totalResults, 10)
      const pageLength = Math.ceil(total / 10) 
      if (pageLength > 1) {
        for (let page = 2; page <= pageLength; page += 1) {
          if (page > (number / 10)) {
            break
          }
          const res = await axios.get(`http://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=${page}`)
          const { Search } = res.data
          commit('updateState', {
            movies: [
              ...state.movies, 
              ..._uniqBy(Search, 'imdbID')
            ]
          })
        }
      }
    }
  }
}
```

### 영화 검색 코드 리팩토링

**store**폴더에 있는 **movie.js**파일을 다음과 같이 리팩토링을 수행하였습니다.

```js
import axios from 'axios'
import _uniqBy from 'lodash/uniqBy'

export default {
  namespaced: true,
  state: () => ({
    movies: [],
    message: '',
    loading: false,
  }),
  getters: {},
  mutations: {
    updateState(state, payload) {
      Object.keys(payload).forEach(key => {
        state[key] = payload[key]
      }) 
    },
    resetMovies(state) {
      state.movies = []
    }
  },
  actions: {
    async searchMovies({ state, commit }, payload) {
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
              page 
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

function _fetchMovie(payload) {
  const { title, type, year, page } = payload
  const OMDB_API_KEY = 'a0a1e9a9'
  const url = `http://www.omdbapi.com/?apikey=${OMDB_API_KEY}`

  return new Promise((resolve, reject) => {
    axios.get(url)
      .then(res => {
        console.log(res)
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

- `_fetchMovies()`
  - **movie.js** 내부에서만 사용할 `_fetchMovies()` 함수를 정의해서 **APIkey**와 **axios**를 반복해서 사용하는 코드를 줄였습니다.
  - 함수 내부에 **try**, **catch**를 활용하여 예외 처리를 수행하였습니다.
  - **url**의 주소를 **OMDB_API_KEY**만 남기고 다 없애버린 경우, 정상작동하지 않아야 함에도 불구하고 **GET Request**가 200번을 출력하여 `then()` 메서드로 넘어갔습니다.
    - `then()` 메서드 내부에서도 예외 처리가 필요하므로 `if()`문을 사용하여 **res.data.Error**가 **true**일 경우(문자열이 존재), `reject()` 메서드를 실행하도록 하였습니다.

- `actions` 옵션
  - `searchMovies()` 메서드가 비동기로 구현되어 있기 때문에, **try**, **catch**를 사용하여 예외 처리를 수행하였습니다.
  - 이미 위에서 **payload**를 인수로 받아서 사용중이기 때문에, 전개 연산자를 이용하여 **_fetchMovie**를 호출하였습니다.
  - error가 발생할 경우 `commit()` 메서드를 사용하여 변이 메서드인, `updateState()` 메서드를 호출하도록 하였습니다.
  - **state**에 정의해 둔 **message**에 **error**인 **message**를 할당하도록 했습니다.
  - 이제 **store**에 있는 **message**데이터를 활용해서, 우리가 사용하는 모든 컴포넌트에서 **error** 메시지를 활용 및 출력이 가능합니다.

---

**MovieList.vue** 파일도 수정해 주었습니다.
```vue
<template>
  <div class="container">
    <div class="inner">
      <div class="message">
        {% raw %}{{ message }}{% endraw %}
      </div>
      <MovieItem 
        v-for="movie in movies" 
        :key="movie.imdbID"
        :movie="movie"/>
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
```
- `computed`
  - **movie.js**에서 생성한 **message**를 양방향 데이터 바인딩으로 사용할 수 있도록 **computed** 옵션에 작성해주었습니다.