---
layout: post
image: /assets/img/bootstrap/Bootstrap-logo.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  Bootstrap을 공부해봅시다.
invert_sidebar: false
categories:
  - frontend
  - bootstrap
---

# Bootstrap

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

이번시간에는 `bootstrap`에 대해 알아보도록 하겠습니다. 홈페이지의 Docs를 살펴보면 더 자세히 설명되어 있으니, 여긴 참고만 하시고 Docs를 살펴보시는 걸 추천드려요!

1. table of contents
{:toc .large-only}
---

<a href="https://getbootstrap.com/" target="_blank">`Bootstarp`</a>사이트에서 참조하였습니다.
Introduction을 참조하면 사용법이 나와있으니 따라서 하면 될 것 같아요.

---

## 버튼과 버튼 그룹

<a href="https://getbootstrap.com/docs/5.1/components/buttons/" target="_blank">버튼과 버튼 그룹</a>링크 입니다.
CSS와 JS를 CDN을 이용하여 연결하고 예제를 시작해봅시다.
HTML 파일 내에 다음 코드를 넣어보죠.

```html
<button type="button" class="btn btn-primary">Primary</button>
<button type="button" class="btn btn-secondary">Secondary</button>
<button type="button" class="btn btn-success">Success</button>
<button type="button" class="btn btn-danger">Danger</button>
<button type="button" class="btn btn-warning">Warning</button>
<button type="button" class="btn btn-info">Info</button>
<button type="button" class="btn btn-light">Light</button>
<button type="button" class="btn btn-dark">Dark</button>

<button type="button" class="btn btn-link">Link</button>
```

bootstrap은 특정 클래스에 해당하는 CSS작업이 완료된 상태입니다. 그러니 이러한 클래스들을 사용하면 편리하겠죠!
또 btn-group 클래스를 활용하면 버튼들을 그룹화하여 사용할 수도 있습니다.

---

## 드롭다운과 리스트

<a href="https://getbootstrap.com/docs/5.1/components/dropdowns/" target="_blank">드롭다운과 리스트</a> 링크입니다.

```html
<!-- Example split danger button -->
<div class="btn-group">
  <button type="button" class="btn btn-danger">Action</button>
  <button
    type="button"
    class="btn btn-danger dropdown-toggle dropdown-toggle-split"
    data-bs-toggle="dropdown"
    aria-expanded="false"
  >
    <span class="visually-hidden">Toggle Dropdown</span>
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">Action</a></li>
    <li><a class="dropdown-item" href="#">Another action</a></li>
    <li><a class="dropdown-item" href="#">Something else here</a></li>
    <li><hr class="dropdown-divider" /></li>
    <li><a class="dropdown-item" href="#">Separated link</a></li>
  </ul>
</div>
```

---

## 양식

<a href="https://getbootstrap.com/docs/5.1/forms/overview/" target="_blank">양식</a> 링크입니다.

---

## 모달

<a href="https://getbootstrap.com/docs/5.1/components/modal/" target="_blank">모달</a> 링크입니다.

---

## Tooltips

<a href="https://getbootstrap.com/docs/5.1/components/tooltips/" target="_blank">Tooltips</a> 링크입니다.<br>
Tooltips의 경우 많은 부분에서 사용될 수 있기 때문에, 성능이슈가 발생할 수 있기 떄문에 사용자가 JavaScript로 초기화해야 동작될 수 있다고 합니다

---

## npm을 이용하여 bootstrap 사용

CDN방식이 아닌, 이제 npm을 활용하여 bootstrap을 활용해봅시다.

> **npm install bootstrap**

이렇게 npm을 활용하면 귀찮다고 생각할 수 있지만, 우리가 필요로하는 기능만 가져와서 사용할 수 있기 때문에 기능적인 측면에서 훨씬 이득입니다. 그리고 우리의 입맛에 맞게 커스터마이징도 가능하구요.

---

## 테마 색상 커스터마이징

<a href="https://getbootstrap.com/docs/5.1/customize/overview/" target="_blank">커스터마이징</a> 링크입니다.

Docs를 확인하면 어떻게 커스터마이징을 할 수 있는지에 대한 내용들이 있습니다. 확인해보면서 하나하나 사용해가면 좋을것 같네요.
<br>
다음 <a href="https://getbootstrap.com/docs/5.1/customize/sass/#maps-and-loops" target="_blank">링크</a>를 선택하면 어떻게 색을 커스터마이징 할 수 있는지를 알려주는군요.

---

## 성능 최적화 (트리쉐이킹)

bootstrap에서 필요로 하는 기능만 가져와서 쓸 수 있는 기능인 Optimize에 대해 알아보겠습니다.
<a href="https://getbootstrap.com/docs/5.1/customize/optimize/" target="_blank">Optimize</a> 링크입니다.

#### Lean Sass imports

최대한 간결하게 SCSS의 여러 기능들을 가져와 쓸 수 있는 옵션들입니다. 초기화를 해야하는 컴포넌트 내용들은 문서를 확인하고 사용하면 될 것 같습니다.

#### Modal component Optimization

<a href="https://getbootstrap.com/docs/5.0/components/modal/#live-demo">Modal component Optimization</a> 링크입니다.

현재 Lean imports를 사용하고 있으므로, Modal component를 추가해도 정상적으로 작동하지 않는 모습을 볼 수 있습니다. 다음 내용을 import 해주시면 되겠습니다.

> **import Modal from "bootstrap/js/dist/modal**
