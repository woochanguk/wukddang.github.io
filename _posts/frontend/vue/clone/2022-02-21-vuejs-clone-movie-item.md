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
(codesandbox 오류. 추후 업데이트)
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