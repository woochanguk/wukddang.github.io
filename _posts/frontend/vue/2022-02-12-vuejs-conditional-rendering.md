---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/vue/vuejs.jpg') center/cover
#   overlay: false
# accent_color: "#ccc"
# theme_color: "#ccc"
description: >
  VueJS의 Template Grammar에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - vue
---

# VueJS - Conditional Rendering

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.


--- 

이번시간부터는 VueJS의 <a href="" target="_blank">조건부 렌더링</a>에 대해 알아보겠습니다.

1. table of contents
{:toc .large-only}
---
## Conditional Rendering (조건부 렌더링)
v-if 디렉티브는 조건에 따라 블록을 렌더링하기 위해 사용됩니다. 블록은 디렉티브의 표현식이 true 값을 반환할 때만 렌더링됩니다