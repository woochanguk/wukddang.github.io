---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  SPA(Single Page Application)에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue
  - vue-clone
---

# VueJS - SPA(Single Page Application)

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간에는 **SPA(Single Page Application)**에 대해 알아보겠습니다.

---

## Traditional Web Application



<img src="/assets/img/vue/spa/Traditional-web-app.png" width="450" height="350"  >

- 전통적인 웹 어플리케이션은 사이트의 특정 페이지를 방문하려면 **HTML**파일을 계속 서버에 요청했어야 했습니다.

---

<img src="/assets/img/vue/spa/Traditional-web-app-2.png" width="450" height="350"  >

## SPA(Single Page Application)


<img src="/assets/img/vue/spa/SPA.png" width="450" height="350"  >

- **SPA**에서는 페이지 요청이 아닌, 기존 페이지와 다른 부분을 서버에 **AJAX** 요청을 수행하여 **JSON** 파일을 받게 됩니다. 

---

<img src="/assets/img/vue/spa/SPA-2.png" width="450" height="350"  >

- **Header**, **Footer** 컴포넌트 사이에 `<RouterView>` 컴포넌트를 연결하여 페이지의 내용만 변경하는 구조를 가지고 있습니다.

## SPA의 장단점

### 장점

1. 빠르고 자연스런 전환으로 **훌륭한 사용자 경험****(User Experience; UX)**을 제공합니다.

2. 더 적게 요청해서 **빠른 렌더링을 가능**하게 해줍니다.

3. **컴포넌트 단위의 개발**로 **생산성**을 향상시켜줍니다.

4. 프론트엔드 벡엔드 개발의 구분이 명확해져서 **분업화**를 수월하게 만들었습니다.

### 단점

1. **최초 로딩이 느립니다.**
  - 당장 사용자에게 보이지 않는 컨텐츠는 느리게 가져오는 **Lazy loading**를 수행하여 보완할 수 있습니다.
  - 한번 로드된 페이지의 경우 **브라우저 캐싱**을 통해 **필요한 내용을 저장**하여 화면이 빠르게 출력되도록 보완할 수 있습니다.

2. **index.html** 파일 하나만을 가지고 있기 때문에, 세부적으로 분리된 사이트에 **검색엔진 최적화****(Search Engine Optimization; SEO)** 를 수행하기가 어렵습니다.
  - **SSR**, **Serverless Functions**로 보완해줄 수 있습니다.

3. **모든 데이터가 노출**되어 보안상의 위험이 존재합니다.
  - **비즈니스 로직을 최소화** 하는 것으로 보완해줄 수 있습니다.

---

<img src="/assets/img/vue/spa/SPA-detail-2.PNG" width="450" height="350"  >

- 현재 우리가 **VueJS**로 만든 프로젝트에서는, 사용자가 **APIkey**까지 **Netlify**로 부터 받아 **OMDbAPI**로 부터 영화들을 받아오고 있습니다. 
- 따라서 **Netlify**로 부터 **HTML파일**을 받는 과정에서 해킹의 위험이 존재하고 있습니다.

<img src="/assets/img/vue/spa/SPA-serverless-functions.PNG" width="450" height="350"  >

- **Netlify**에서 제공하는 서버리스 함수를 사용하게 되면, **APIkey**를 해당 함수에서 보관할 수 있습니다.
- 따라서 사용자의 컴퓨터가 해킹되더라도 **APIkey**를 지킬 수 있습니다.
<br>


즉, **서버가 없이 동작**할 수 있는 특정함수를 사용할 수 있고 별도의 서버를 구축하지 않고도 벡엔드에서 동작하는 명령**(API)**을 만들어 **SPA**와 연결해줄 수 있습니다.

## Netlify Serverless 함수 세팅

<a href="https://docs.netlify.com/functions/overview/" target="_blank">**Netlify Document**</a>에서 **Function**을 보면 **Default deployment options**란 것이 있습니다. **Netlify**에서 사용할 수 있는 함수는 **AWS** 서버에서 사용이 가능하다고 설명하고 있습니다.<br>

실제 우리의 프로젝트에서 **Netlify**의 함수를 사용할 수 있도록 설정하겠습니다.

---

1. 프로젝트의 작업 경로에 **netlify.toml**파일을 생성하였습니다.

2. 파일 내부에 다음 코드를 작성하였습니다.

```toml
[build]
  functions = "functions"
```
3. 위의 파일에서 작성한 **functions**라는 이름과 동일한 폴더를 프로젝트 작업 경로에 생성해주도록 하겠습니다. 

4. 폴더 내부에 **hello.js** 파일을 생성하고 내부에 함수를 작성해서 기본 내보내기로 설정해주었습니다. (해당 함수는 **async** 키워드를 사용해서 비동기로 설정해주어야 합니다.)

5. 객체 데이터를 반환하는데, 데이터 내부의 *body*옵션은 **serverless** 함수로 응답시켜 줄 데이터를 명시해주는 곳입니다. 
  - 문자 데이터만 할당해줄 수 있습니다.
  - 만약 객체 데이터를 할당해주고 싶다면, `JSON.stringify()` 메서드에 입력해서 실행해야합니다.

6. **Github** 저장소에 **Push**를 해주고 **url** 주소창에 **#**을 지우고 `/.netlify/functions/hello`를 작성해주면 우리가 **body**에 작성한 내용이 객체로 출력되는 것을 확인할 수 있습니다.

이러한 **Serverless** 함수를 이용해서 **APIkey**를 보호하는 함수를 작성해보도록 하겠습니다.

## Netlify-CLI 구성

<a href="https://cli.netlify.com/netlify-dev" target="_blank">**Netlify CLI**</a> 문서를 참조하면 많은 정보를 확인할 수 있습니다.<br>



<img src="/assets/img/vue/spa/netlify-cli.PNG" width="600" height="450"  >

위의 그림을 보면, 우리가 이전에 만들었던 **VueJS** 프로젝트에서는 바로 브라우저에 출력했지만 **Netlify-CLI**를 사용하면 프로젝트를 **Netlify Dev와** 연결된 후에 브라우저에 출력됩니다. <br>
이렇게되면 **localhost**에서도 앞전에 만들었던 **functions** 폴더를 이용할 수 있습니다.

**Netlify CLI** 문서를 확인하면서 **netlify.toml** 파일을 수정해주도록 하겠습니다.

```toml
# Netlify Dev
# https://cli.netlify.com/netlify-dev/#netlifytoml-dev-block

# 제품 모드
[build]
  command = "npm run build"
  functions = "functions" # Netlify 서버리스 함수가 작성된 디렉토리를 지정합니다.
  publish = "dist" 프로젝트 빌드 결과의 디렉토리를 지정합니다.

# 개발 모드
[dev]
  framework = "#custom" # 감지할 프로젝트 유형을 지정합니다. 앱 서버 및 `targetPort` 옵션을 실행하는 명령 옵션은
  command = "npm run dev" # 연결할 프로젝트의 개발 서버를 실행하는 명령(스크립트)을 지정합니다.
  targetPort = 8080 # 연결할 프로젝트 개발 서버의 포트를 지정합니다.
  port = 8888 # 출력할 Netlify 서버의 포트를 지정합니다.
  publish = "dist" # 프로젝트의 정적 콘텐츠 디렉토리를 지정합니다.
  autoLaunch = false # Netlify 서버가 준비되면 자동으로 브라우저를 오픈할 것인지 지정합니다.
```

**package.json** 파일도 약간의 수정이 필요합니다.

```json
{
  ...
  "scripts": {
    "dev": "webpack-dev-server --mode development",
    "dev:netlify": "netlify dev",
    "build": "webpack --mode production",
    "lint": "eslint --fix --ext .js,.vue"
  }
  ...
}
```
- `netlify dev`를 실행하기 위해서는 **netlify-cli**가 필요합니다. 

> `npm i -D netlify-cli`

이제 개발 서버를 열면, **localhost:8888** 포트로 출력이 되고 있는것을 확인할 수 있습니다.

### 영화 정보 반환 API 만들기

영화의 검색 요청을 처리해줄 수 있는 **Serverless** 함수를 만들도록 하겠습니다. **functions** 폴더 내부에 **movie.js**파일을 생성하도록 하겠습니다.

```js
const axios = require('axios')

exports.handler = async function (event, context) {
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

- **serverless** 함수는 **NodeJS** 환경으로 동작합니다.
- **serverless** 함수는 **async**로 비동기 함수가 되어야 합니다.
- 이전에 **store**에 작성해둔 **movie.js**의 **_fetchMovie**함수가 이제 **serverless** 함수가 되어야 하기 때문에, 해당 내용을 옮겨주도록 하겠습니다.
- **serverless** 함수는 브라우저에서 동작해야하기 때문에, `require()` 함수를 이용해서 **axios**를 사용하도록 하겠습니다.

우리가 작성하려는 함수는 이미 비동기 함수로 정의되었기 때문에, 이전에 작성해두었던 `then()`, `catch()` 메서드는 필요가 없습니다. 따라서 다음과 같이 작성해주겠습니다.

```js
// /store/movie.js
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
      Object.keys(payload).forEach(key => {
        state[key] = payload[key]
      }) 
    },
    resetMovies(state) {
      state.movies = []
      state.message = _defaultMessage
      state.loading = false
    }
  },
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
        // error로 받는 객체 데이터를 바로 객체 구조분할을 통해 message로 사용하겠다는 의미입니다.
      } catch ( { message } ) {
        commit('updateState', {
          // 현재 _fetchMovie가 netlify의 serverless 함수와 연결되어 동작하고 있기 떄문에,
          // 네트워크로 주고 받은 객체 데이터의 속성을 가져와서 할당해 주어야 합니다.
          movies: [],
          message
        })
      } finally {
        commit('updateState', {
          loading: false
        })
      }
    },
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
        console.log(res)
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

async function _fetchMovie(payload) {
  // 이젠 POST Method로 데이터를 넘겨주어야 함
  // axios의 post 메서드를 통해 serverless 함수 주소로 특정 데이터를 전송
  // get : 쿼리스트링에 정보를 담아서 클라이언트에서 서버로 요청하는 것
  // post : payload에 담아서 전송(?). 서버에서 클라이언트로 보내주는 것

  // 비동기로 동작하는 개념
  return await axios.post('/.netlify/functions/movie', payload)
}
```

**serverless** 함수에서 작성한 `console.log(event)`에서 많은 데이터를 출력해준 것을 확인할 수 있습니다. (직접 한번 확인해보세요!) 출력된 **JSON** 데이터의 **body** 속성을 보시면 **serverless** 함수 내부에 정의된 대로, `_fetchMovie()` 메서드를 호출한 후에 데이터를 반환하여 **body** 속성으로 전달해준 것을 볼 수 있습니다.

## 로컬 & 서버의 환경변수 구성

개발한 웹사이트를 배포할 때 **APIkey**가 노출되는 위험이 존재하여 **serverless** 함수 내부에 **APIkey**를 선언하고 **serverless** 함수로 **GET Method**를 수행하도록 작성하였습니다. 하지만 **Github**과 같은 원격 저장소에 배포할 파일들을 업로드 해야하기 때문에, 여전히 해킹의 위험이 존재한다고 할 수 있습니다.