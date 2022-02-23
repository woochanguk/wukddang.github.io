---
layout: post
image: /assets/img/vue/vuejs.jpg
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  Vue Router를 정리해봅시다.
invert_sidebar: false
categories:
  - frontend
  - vue
  - vue-clone
---

# VueJS Clone - Vue Router

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간에는 `Vue Router`를 정리하여 이해해보도록 하겠습니다.

---

## Vue Router

### {% raw %}\<RouterView\>{% endraw %}
우리는 **App.vue**에서 `<RouterView>` 컴포넌트를 한번 사용했습니다. **Header**와 **Footer**사이에 존재하고 변경되는 부분은 전부 `<RouterView>` 컴포넌트에 존재하고 있었는데, 요약하자면
> **{% raw %}\<RouterView\>{% endraw %}** : 페이지가 출력(렌더링)되는 영역 컴포넌트

### {% raw %}\<RouterLink\>{% endraw %}
> **{% raw %}\<RouterLink\>{% endraw %}** : 페이지 이동을 위한 링크 컴포넌트


### $route
> **$route** : 접근된 **Route**(페이지) 정보를 가지는 객체(**조회용**)

### $router 
> **$router** : 접근된 **Route**(페이지) **조작**을 위한 객체
  - 많은 메서드들을 가지고 있습니다.
  - 프로젝트에서는 `push()` 메서드를 사용할 수 있었습니다.
