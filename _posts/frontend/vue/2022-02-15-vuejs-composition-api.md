---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
# accent_color: "#ccc"
# theme_color: "#ccc"
description: >
  VueJS의 Composition API에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue
---

# VueJS - Composition API

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

--- 

이번시간부터는 VueJS의 <a href="https://v3.ko.vuejs.org/guide/composition-api-introduction.html#%E1%84%89%E1%85%A9%E1%84%80%E1%85%A2" target="_blank">컴포지션 API</a>에 대해 알아보겠습니다.

1. table of contents
{:toc .large-only}

---

## Code tester 
여기서 예제코드를 테스트 해주세요
(완성된 **MyBtn.vue**파일을 components폴더에 작성해두었습니다.)
<iframe src="https://codesandbox.io/embed/frosty-lake-khrk8?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="frosty-lake-khrk8"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

## Composition API (컴포지션 API)