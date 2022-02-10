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

# VueJS - Template Grammar

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

이번시간부터는 `VueJS`의 **Template Grammar**에 대해 알아보겠습니다.

1. toc
{:toc .large-only}

---

## 보간법(Interpolation)

#### 문자열

데이터 바인딩의 가장 기본 형태는 “Mustache”(이중 중괄호 구문{% raw %}{{ }}{% endraw %})기법을 사용한 문자열 보간법(text interpolation)입니다. 다음 코드로 이해해봅시다.

```html
<span>메시지: {% raw %}{{ msg }}{% endraw %}</span>
```

Mustache 태그는 해당 컴포넌트 인스턴스의 msg 속성 값으로 대체됩니다. 또한 msg 속성이 변경될 때 마다 갱신됩니다.

<br>
v-once 디렉티브를 사용하여 데이터가 변경되어도 갱신되지 않는 일회성 보간을 수행할 수 있습니다. 다만, 이런 경우 같은 노드의 바인딩에도 영향을 미친다는 점을 유의해야 합니다.

```html
<span v-once>결코 변하지 않을 것입니다: {% raw %}{{ msg }}{% endraw %}</span>
```
