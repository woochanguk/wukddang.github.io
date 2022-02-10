---
layout: post
current: post
cover: assets/built/images/wetube-clone.png
navigation: True
title: Wetube 클론코딩 - MIDDLEWARES(EXPRESS)
date: 2022-01-29 00:00:00
tags: [Clone]
class: post-template
subclass: "post tag-Clone"
author: changuk
---

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `nomadcoders`에서 `Wetube` 클론코딩을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

{% include wetube-clone-table-of-contents.html %}

## MIDDLEWARE

본격적으로 웹사이트를 만들기 전에, middleware의 개념을 이해해야 합니다. `middleware`는 이름에 뜻이 있습니다. **중간에 있는 소프트웨어**라는 의미입니다. 그럼 무엇의 중간에 있다는 의미일까요?? 이전 챕터에서의 내용 기억나시나요?? **브라우저가 무언가를 `request`하면 서버는 거기에 `response`해준다.** **middleware**는 바로 **request, response**사이에 있습니다. 브라우저가 request하고 서버가 response하기 전, 그 사이에 middleware가 있는 것입니다. 하나 기억하고 넘어가면 좋을 것 같습니다.

> **모든 middleware는 handler고 모든 handler는 middleware다.**

그리고 이제부턴 **handler**라는 말 대신 **controller**라는 용어로 사용하겠습니다. 실제로 **handler**는 **controller**이거든요. 추후 `MVC`를 설명할 때 괴리감을 줄이고자, 이렇게 사용하도록 하겠습니다. 그러니 다음과 같이 이야기 할 수 있겠죠.

> **모든 controller는 middlerware고 모든 middleware는 controller다.**

원래 controller에는 두 개의 argument인 request(req), response(res)외에도 하나가 더 있습니다.
