---
layout: post
current: post
cover: assets/built/images/web-app.png
navigation: True
title: To-Do 웹 App 만들기 - 1
date: 2021-12-30 12:00:00
tags: [Web-programming]
class: post-template
subclass: "post tag-Web-programming"
author: changuk
---

{% include webprogramming-table-of-contents.html %}

## 나만의 웹 app 개발 시작

노마드코더 **[Vanilla JS로 크롬 app 만들기]**를 완강하였다.. 앱의 심미성을 높이기 위해... CSS작업을 진행해야 한다! 작업을 하면서 CSS스킬이 점차 향상될 것이다. CSS와 같이 개인적으로 또한 배경 이미지를 화면의 크기에 상관없이 여백이 없도록, 필요한 기능을 추가하는 것 등등.. 본인의 마음에 맞게 앱을 수정할 것이다.

---

### GitHub을 활용한 웹사이트 퍼블리싱

GitHub에서는 아주 좋은 기능을 지원하고 있다. GitHub에선 repository에서 `gh-pages`라는 특별한 이름을 가진 branch를 가지고 있으면, 공짜로 Static 호스팅을 할 수 있도록 해준다. 참고로 <code class="post-full-content">Static Website</code>는 프론트엔드의 필수요소라고 할 수 있는, <br><code class="post-full-content">HTML/CSS/JavaScript</code>로만 이루어진 사이트를 의미한다.
To-Do 웹 app 폴더는 GitHub repository로 존재하고 있고, `gh-pages`로 branch를 만들어, localhost로서가 아닌, GitHub로 이 app을 실행할 수 있도록 만들어야 한다. <br> 다음 순서로 진행해주면 된다.

1. <code class="post-full-content">gh-pages</code>로 branch 생성
2. 업데이트 사항이 존재하면, <code class="post-full-content">gh-pages branch</code>로 `commit`
3. <code class="post-full-content">{사용자 이름}.github.io/{레포지토리 이름}</code> 으로 접속 가능

---

~~... 이렇게 진행하려 했는데, 아무래도 진행에 더딘감이 있어 프론트엔드 예제를 더 다루고 돌아와야 할 것 같다...~~

**2022년 1월 9일** 현재 어느정도 방향을 잡고 To-Do 어플 작성을 시작하고 있다. 블로그에 어플 개발 과정을 꾸준히 기록하여 복습도 하고, 나중에 발생할 수 있는 삽질도 방지하도록 해야겠다.

---

{% gist 51bf4816a930625fb5b4249443c6a92a %}

(현재 작성 중)
<br>
[**바닐라 Javascript: To-Do App build**](https://funkyblues.github.io/mymomentum-2022)
