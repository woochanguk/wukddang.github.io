---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  VueJS Clone 웹사이트 검색창을 클론해봅시다.
invert_sidebar: false
categories:
  - frontend
  - vue-clone
---

# VueJS Clone - Search

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간에는 검색창을 구현해보도록 하겠습니다.

---
## Code tester 
이번시간의 결과 모습입니다.
<iframe src="https://codesandbox.io/embed/exciting-babbage-fkmwhz?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="exciting-babbage-fkmwhz"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

---

## Search

### Search - filter
이번시간에는 검색바를 만들어보도록 하겠습니다. **Search.vue**파일을 **components**폴더 안에 생성해줍시다. `v-model 디렉티브`를 사용해서 **title**을 양방향 데이터 바인딩을 이용하였습니다.

```vue
<template>
  <div class="container">
    <input 
      v-model="title" 
      type="text"
      placeholder="Search for Movies, Series & more" />
  </div>
</template>

<script>
export default {
  data() {
    return{
      title: ''
    }
  }
}
</script>
```

그리고 **Home.vue**컴포넌트에 연결하여 **Home**에서 출력될 수 있도록 해줍시다.
```vue
<template>
  <Headline />
  <Search />
</template>

<script>
import Headline from '../components/Headline.vue'
import Search from '../components/Search.vue'

export default {
  components: {
    Headline,
    Search
  }
}
</script>
```

**Search.vue**컴포넌트의 **input** 요소는 **default**값이기 때문에, **Bootstrap**을 이용해서 스타일 작업을 수행해 주겠습니다.

#### Form control

Bootstrap의 <a href="https://getbootstrap.com/docs/5.1/forms/form-control/" target="_blank">**Form controls**</a>를 살펴보면, 입력창에 스타일을 적용해서 사용할 수 있습니다. 

```html
<div class="mb-3">
  <label for="exampleFormControlInput1" class="form-label">Email address</label>
  <input type="email" class="form-control" id="exampleFormControlInput1" placeholder="name@example.com">
</div>
<div class="mb-3">
  <label for="exampleFormControlTextarea1" class="form-label">Example textarea</label>
  <textarea class="form-control" id="exampleFormControlTextarea1" rows="3"></textarea>
</div>
```
**input**요소에 `class="form-control"`을 사용해주면 primary색상을 가진채로 스타일이 잘 적용되는 것을 확인할 수 있습니다.

#### Select

Bootstrap의 <a href="https://getbootstrap.com/docs/5.1/forms/select/" target="_blank">**Select**</a>를 살펴보면, 선택창에 스타일을 적용해서 사용할 수 있습니다. 

```html
<select class="form-select" aria-label="Default select example">
  <option selected>Open this select menu</option>
  <option value="1">One</option>
  <option value="2">Two</option>
  <option value="3">Three</option>
</select>
```

그럼 **Search.vue**컴포넌트를 수정해서 선택창을 추가하고, 스타일도 적용하겠습니다. <br>
이제 선택창의 메뉴들이 어떻게 나타나게 될 것인지를 지정해주기 위해, **Search.vue**컴포넌트의 `<script>`태그 내부에 **type**, **number**, **year**등의 속성들을 작성해주었습니다. 각 속성들은 **filters** 배열데이터에 작성해둔 내용을 바탕으로  `v-for 디렉티브`를 통해 `<template>`태그에 작성해둔 `<select>`, `<option>`태그에 입력될 것입니다. <br>
연도를 선택하는 창에서는 **default**값으로 **All Years**가 나타날 수 있도록, 새로운 `<option>`태그를 작성해주었습니다. `v-if 디렉티브`로, `filter.name === 'year'` 인 경우에만 출력되도록 했습니다.


```vue
<template>
  <div class="container">
    <input 
      v-model="title" 
      class="form-control"
      type="text"
      placeholder="Search for Movies, Series & more" />
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
  </div>
</template>

<script>
export default {
  data() {
    return{
      title: '',
      type: 'movie',
      number: 10,
      year: '',
      filters: [
        {
          name: 'type',
          items: ['movie', 'series', 'episode']
        },
        {
          name: 'number',
          items: [10, 20, 30]
        },
        {
          name: 'year',
          items: (() => {
            const years = []
            const thisYear = new Date().getFullYear() 
            for (let i = thisYear; i >= 1985; i -= 1) {
              years.push(i)
            }
            return years
          })()
        }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
  .container {
    display: flex;
    > * {
      margin-right: 10px;
      font-size: 15px;
      &:last-child {
        margin-right: 0px;
      }
    }
    .selects {
      display: flex;
      select {
        width: 120px;
        margin-right: 10px;
        &:last-child {
          margin-right: 0px;
        }
      }
    }
  }
</style>
```
**type**, **number**, **year**가 `<select>`, `<option>`태그에 입력되기 위해서는 양방향 데이터 바인딩을 이용하여 동적으로 동작이 되어야 하는데, 현재 `v-for 디렉티브`로 **filter**를 통해 **filters** 배열 데이터에 접근할 수 있습니다. 또한 **VueJS**에서 제공하는 `$data`라는 속성과 `[filter.name]`을 이용하여 **속성의 이름을 사용**(type, number, year)하는 기능을 사용하여 동적으로 동작하는 기능을 구현하였습니다.<br>

참고로, `$data.type`과 같은 점표기법은 `$data.["type"]`로 작성될 수 있습니다.


### Search - Button
실제 영화검색이 가능하도록, **Apply** 버튼을 구현해보겠습니다. <a href="http://www.omdbapi.com/" target="_blank">**OMDbAPI**</a>를 사용하여 구현할 것이기 때문에, 해당 링크에서 **APIkey**를 받아 수행해주어야 합니다. 본격적인 구현 전에, **axios**패키지를 설치해주겠습니다.

> `npm i axios`

자 이제 **Search.vue** 파일에 코드를 작성해주겠습니다. `<script>`태그 내부에 다음 코드를 작성하였습니다. **Usage**에 있는 주소는 **http**로 나와있어서, **https**로 변경해주어야 합니다. **Apply** 버튼을 생성하였고 클릭했을 때, `apply()`메서드가 실행됩니다. <br>

또 엔터키를 누르고 올라올 때도 `apply()` 메서드가 실행되도록 작성하였습니다.

```vue
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
import axios from 'axios'
export default {
  data() {
    ...
  },
  methods: {
    async apply() {
      const OMDB_API_KEY = '7035c60c'
      const res = await axios.get(`https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${this.title}&type=${this.type}&y={this.year}&page=1`)
      console.log(res)
    }
  }
}
</script>
```
**Usage**에 나와있는 **apikey**를 사용하는 주소를 입력하였고, 영화제목을 받을 때 사용하는 **s**파라미터, **type**을 받는 **type**파라미터, 연도를 받는 **y**파라미터 등을 작성해주었습니다. 또 **async**, **await**키워드를 사용하여 **get**이 완료되어서 데이터를 할당해준 후에 다음 동작이 수행되도록 하였습니다. <br>

그럼 이제 검색창에 **frozen**을 검색하고 엔터를 눌러주면 다음과 같은 객체가 출력됩니다.

```json
{data: {…}, status: 200, statusText: '', headers: {…}, config: {…}, …}
config: {transitional: {…}, transformRequest: Array(1), transformResponse: Array(1), timeout: 0, adapter: ƒ, …}
data:
Response: "True"
Search: Array(10)
0: {Title: 'Frozen', Year: '2013', imdbID: 'tt2294629', Type: 'movie', Poster: 'https://m.media-amazon.com/images/M/MV5BMTQ1MjQwMTE5OF5BMl5BanBnXkFtZTgwNjk3MTcyMDE@._V1_SX300.jpg'}
1: {Title: 'Frozen II', Year: '2019', imdbID: 'tt4520988', Type: 'movie', Poster: 'https://m.media-amazon.com/images/M/MV5BMjA0YjYyZG…GYyMTJkXkEyXkFqcGdeQXVyNDg4NjY5OTQ@._V1_SX300.jpg'}
2: {Title: 'Frozen', Year: '2010', imdbID: 'tt1323045', Type: 'movie', Poster: 'https://m.media-amazon.com/images/M/MV5BMTc5MTg0ODgxMF5BMl5BanBnXkFtZTcwODEzOTYwMw@@._V1_SX300.jpg'}
3: {Title: 'The Frozen Ground', Year: '2013', imdbID: 'tt2005374', Type: 'movie', Poster: 'https://m.media-amazon.com/images/M/MV5BYzM3Mjc1ZD…zliMTdkXkEyXkFqcGdeQXVyMTAxNDE3MTE5._V1_SX300.jpg'}
4: {Title: 'Frozen River', Year: '2008', imdbID: 'tt0978759', Type: 'movie', Poster: 'https://m.media-amazon.com/images/M/MV5BMTk2NjMwMDgzNF5BMl5BanBnXkFtZTcwMDY0NDY3MQ@@._V1_SX300.jpg'}
5: {Title: 'Frozen Fever', Year: '2015', imdbID: 'tt4007502', Type: 'movie', Poster: 'https://m.media-amazon.com/images/M/MV5BMjY3YTk5Mj…TZmOWRlXkEyXkFqcGdeQXVyNDgyODgxNjE@._V1_SX300.jpg'}
6: {Title: "Olaf's Frozen Adventure", Year: '2017', imdbID: 'tt5452780', Type: 'movie', Poster: 'https://m.media-amazon.com/images/M/MV5BOWQ1NjNiZT…Tg1MGQ4XkEyXkFqcGdeQXVyMTA4NDI1NTQx._V1_SX300.jpg'}
7: {Title: 'Frozen Stiff', Year: '2002', imdbID: 'tt0301634', Type: 'movie', Poster: 'https://m.media-amazon.com/images/M/MV5BMTk5NDc0MjU3Nl5BMl5BanBnXkFtZTcwNDc3NTU3OQ@@._V1_SX300.jpg'}
8: {Title: 'Frozen Land', Year: '2005', imdbID: 'tt0388318', Type: 'movie', Poster: 'https://m.media-amazon.com/images/M/MV5BMTczMjgyMjQxNF5BMl5BanBnXkFtZTcwNjUxMjU3Mg@@._V1_SX300.jpg'}
9: {Title: 'A Frozen Flower', Year: '2008', imdbID: 'tt1155053', Type: 'movie', Poster: 'https://m.media-amazon.com/images/M/MV5BMWZhNjQ4ZT…TMwNzNkXkEyXkFqcGdeQXVyMzQ2MDUxMTg@._V1_SX300.jpg'}
length: 10
[[Prototype]]: Array(0)
totalResults: "305"
[[Prototype]]: Object
headers: {cache-control: 'public, max-age=86400', content-type: 'application/json; charset=utf-8', expires: 'Sat, 19 Feb 2022 12:23:05 GMT', last-modified: 'Sat, 19 Feb 2022 11:23:05 GMT'}
request: XMLHttpRequest {onreadystatechange: null, readyState: 4, timeout: 0, withCredentials: false, upload: XMLHttpRequestUpload, …}
status: 200
statusText: ""
[[Prototype]]: Object
```

영화목록들이 잘 출력되고 있습니다. 웹 브라우저와 웹 서버간 데이터 전송 API를 의미하는 개발자 도구의 **XHR(XMLHttpRequest)**을 클릭해보면, 해당 웹페이지에서 발생하는 데이터 요청을 확인할 수 있습니다. <br>

이제 **get Method**로 요청해서 얻은 영화목록들의 사진을 **viewport**에 출력해주면 마무리가 되겠네요. 출력되는 사진들은 **MovieList.vue** 컴포넌트로 연결해주고, 각각의 카드들은 **MovieItem.vue** 컴포넌트로 연결해줄 계획입니다. <br>

하지만 약간의 문제가 있습니다. **Search.vue** 컴포넌트에서 영화를 검색해서 데이터를 받게 되는데, 이 데이터를 **MovieList.vue** 컴포넌트에서 활용하여 화면에 출력해주어야 합니다. 따라서 데이터를 전송해줄 수 있어야 하는데, 부모-자식 간의 컴포넌트 관계도 아니고 상위-하위 컴포넌트 관계도 될 수 없습니다. <br> 

구현해보면 형제 컴포넌트가 되게 됩니다. **VueJS**에서는 상위-하위, 부모-자녀 컴포넌트 관계만 데이터를 전달할 수 있어 웹사이트 구현에 어려움이 있죠. 따라서 우리는 **Vuex**라는 **중앙집중화된 상태관리 패턴의 라이브러리**를 적용하여 기능을 구현해보겠습니다. 단순히 데이터를 한 곳에 모아 처리해주는 공식 라이브러리 입니다.