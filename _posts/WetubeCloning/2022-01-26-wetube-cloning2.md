---
layout: post
current: post
cover: assets/built/images/wetube-clone.png
navigation: True
title: Wetube 클론코딩 - INTRODUCTION TO EXPRESS
date: 2022-01-25 18:00:00
tags: [Clone]
class: post-template
subclass: "post tag-Clone"
author: changuk
---

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `nomadcoders`에서 `Wetube` 클론코딩을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

{% include wetube-clone-table-of-contents.html %}

먼저 우리의 `index.js` 파일을 `server.js` 파일로 변경해줍시다. 그리고 앞으로는 `src(source)`파일을 만들어서 우리가 만들 모든 application들을 src에 넣겠습니다. 자, 그러면 모든 준비가 끝났습니다. 시작하죠

## 첫 Server 만들기

`express`를 import 해줍니다. `import from`을 사용하면 `express`는 `node_modules`내에서 검색하게 됩니다. 그 후 `express app`을 생성해주는 코드도 작성합니다.

```javascript
import express from "express;
const app = express();
```

이렇게 하면 express app이 만들어지게 됩니다 와우! 이제 이 `app`은 사용자의 요청을 listen 할 수 있어야 합니다. 여기서 생각해야할 문제가 있습니다. **app**이 **listen**한다고 했을 때, 과연 **무엇을** `listen`한다는 것일까요? 이를 위해서는 **서버**를 알아야 합니다.

## 서버(Server)

서버는 항상 켜져있는 컴퓨터라고 생각할 수 있습니다. 서버는 노트북으로 작동 될 수도 있고, 데스크탑으로 작동될 수도 있습니다. 일반적으로는 우리가 상상하는 키보드, 모니터도 없는 수 많은 하드디스크들로 이루어진 컴퓨터의 모습이 서버 역할을 하는 컴퓨터라고 할 수 있습니다. 정리하자면 서버는 항상 켜져있고 인터넷에 연결되어 있는 컴퓨터지요. 웹 app의 관점에서 서버의 기능을 살펴보도록 합시다.

#### request?

서버는 **request**를 **listening**하고 있습니다. 만약 `google.com` 사이트에 접속한다고 해봅시다. 그럼 우리는 지금 `google.com`에 `request`를 보낸 것입니다. 이것이 서버가 request를 listening 하고 있다는 의미입니다. 구글의 서버가 그 역할을 하고 있는 것이죠. 카카오톡에서 상대편에게 메시지를 보내는 것도 request입니다. 제가 서버에게 메시지를 보내면 서버는 제게 응답을 보내게 됩니다. 이것이 서버가 하는 일입니다. request를 듣고 답하는 것이지요. 우리는 이 서버가 사람들이 뭔가를 요청할 때 까지 기다리도록 해야 합니다. 바로 `express`에서 지원해주는 `app.listen()`으로 말이죠.

```javascript
app.listen();
```

listen()에는 `callback` 함수가 있습니다. 따라서 `callback`을 작성하기 전에 서버에게 어떤 port를 listening 할 지를 이야기 해 주어야 합니다. 컴퓨터에는 이미 수 많은 port가 있으니까요. `port`는 컴퓨터가 외부의 사이트나 다른 프로그램에 접속하기 위해 필요합니다. 우리의 경우 `4000`번 port번호를 사용하겠습니다. 그리고 서버가 잘 작동하는지를 한번 확인해보죠.

```javascript
import express from "express";
const app = express();
const handleListening = () => console.log("Server listening on port 4000🚀");
app.listen(4000, handleListening);
```

다음과 같이 작성한 후 콘솔 창을 확인해보면,`handleListening` `callback`함수가 잘 작동하는 것을 볼 수 있습니다! 우리는 port 4000번을 listening 하는 서버를 만들었습니다. 그럼 이제 서버에 어떻게 갈 수 있을까요? 일반적으로는 localhost로 접속할 수 있습니다. 한번 해보죠. 인터넷 URL에 `localhost:4000`이라고 입력해봅시다. 접속해보면, `Cannot GET/`이라는 텍스트가 우리를 반겨줍니다. 이게 뭘까요?? 이 친구는 일단 다음에 확인해보도록 합시다. **중요한 것은, 우리는 `express`를 사용하여 브라우저에서 접속할 수 있는 우리들만의 `server`를 만들었다는 것입니다!** 사실인지 아닌지는 nodemon을 종료해본다면 확실하게 알 수 있을거에요. 다음으로 넘어가기 전에, 코드를 좀 더 간결하게 만들어 봅시다.

```javascript
import express from "express";
const PORT = 4000;
const app = express();
app.listen(4000, function () {
  console.log(`Server listening on port ${PORT}🚀`);
});
```

---

## GET Requests

`localhost:4000`으로 접속하게 되면 `Cannot GET/`을 보게 됩니다. 하나씩 살펴 봅시다.

1. **Cannot : ~을 할 수 없다**
1. **GET : ~을 얻다/ 가져오다.**
1. **/ : ??**

**Cannot** 이나, **GET**은 대충 의미를 유추할 수 있을 것 같은데 `/`는 무슨의미일까요?? <br>
`/`는 **서버의 root, 혹은 첫 페이지**를 의미합니다. 예를 들어 `google.com`에 접속한다면, `google.com/`라고 요청하는 것과 같은 의미라는 소리지요. 생각보다 간단했네요. 이제 그럼 `GET`의 정확한 의미에 대해 알아봅시다. `GET`은 `HTTP method`라고 합니다. 갑자기 HTTP method라고 하니 무슨말인지 잘 모르겠네요. 먼저 <a href="https://ko.wikipedia.org/wiki/HTTP" target="_blank">`HTTP`</a>에 대해 알 필요가 있습니다. 링크에서 한번 훑어보고 옵시다. (HTTP에 관해선 추후 NETWORK 태그로 글을 정리할 계획입니다.) <br>간단하게 말씀드리자면, **HTTP는 개인과 서버 또는 서버끼리 소통하는 방법**이라고 할 수 있습니다. 사용자가 URL로 접속하려 할 때, 브라우저는 사용자를 위해 `HTTP REQUEST`를 만들어서 서버에 보내게 됩니다. 그러므로 **HTTP REQUEST는 웹사이트에 접속하고 서버에 정보를 보내는 방법**입니다.
링크에서 보면 알 수 있지만 `HTTP`는 많은 method를 가지고 있습니다. 여기서 우리가 사용하려는 **GET**은 **페이지를 가져와줘** 라는 의미로 사용될 수 있습니다.
<br>
그러니 이렇게 정의할 수 있습니다. 우리가 웹사이트에 접속할 때 브라우저는 우리를 어디론가 데려가는 것이 아닙니다. 대신 웹사이트를 `REQUEST`하고, 페이지를 가져다준다고 할 수 있습니다. 그럼 이제 할 일이 생겼군요. 우리가 만든 서버에 누군가가 `GET REQUEST`를 보낸다면 어떻게 해야하는지 알려주어야 하거든요.

## REQUEST & RESPONSE

이제 우리의 서버 어플리케이션이 `GET REQUEST`에 응답할 수 있도록 해줍시다. 준비가 되면 어플리케이션은 listen하기 시작할 것입니다. 그 전에 `REQUEST`와 `RESPONSE`를 좀 더 알아봐야 할 것 같네요.

#### REQUEST

`REQUEST`는 앞서 말씀드렸던 것과 같이 브라우저가 서버에게 웹사이트를 요청(REQUEST)하는 것이라고 이해해주시면 됩니다. 브라우저로부터 요청(REQUEST)를 받았다면 서버는 응답(RESPONSE)을 해주어야 겠지요? 일단은 간단한 `console.log()`만 출력해보도록 합시다. <br>

```javascript
import express from "express";
const PORT = 4000;
const app = express();

const handleHome = () => console.log("Somebody is trying to go home.");

app.get("/", handleHome);

app.listen(4000, function () {
  console.log(`Server listening on port ${PORT}🚀`);
});
```

우리가 만든 `express application`인 `app`을 사용하여 응답하도록 합시다. 이 `app`에는 많은 옵션들이 있는데 그 중 **get**을 사용하겠습니다. 이 `get`은 우리 `app`에게 만약 누군가가 특정 `route`로 (우리의 경우엔 `/(home)`) `GET REQUEST`를 보낸다면, 반응하는 `Callback` 함수를 호출하겠다는 의미입니다. 참고로 꼭 `Callback`에 함수를 사용해야 합니다. 함수를 보내지 않으면 실행할 수 없습니다. 결과를 볼까요? 실제로 작성해보고 결과를 한번 확인해 보시죠.

---

사이트를 확인해보면, 브라우저는 계속 로딩상태일 것입니다. 하지만 콘솔 창을 보면, `Callback` 함수로 작성했던 `handleHome`이 출력되어 있는걸 볼 수 있습니다. 여전히 브라우저는 먹통이지만, 우린 한 가지를 알아냈습니다. 작성한 코드로, 우리의 `express app`이 브라우저의 `GET REQUEST`에 반응했습니다! 그러니 `Cannot GET /`는 보이지 않는 것이지요 (물론 무한로딩 상태이지만요.) <br>이제 문제는 `GET REQUEST`에 반응하는 것이 아닌, 브라우저의 요청에 적절하게 대답해주지 못하는 것이 되었습니다. 브라우저는 `/`에 해당하는 웹페이지를 요구하는데 우리 `app`은 `handleHome`함수를 호출하여 `console.log(~~)`만 실행할 뿐 아무런 작업도 하고 있지 않거든요. 이제 코드를 더 작성하여, 브라우저에게 웹사이트만을 보내줘야하는 작업이 남았습니다.
<br>
더 넘어가기 전에, `GET REQUEST`가 동작하는 방식을 한번 정리해보겠습니다.

1. 브라우저는 사용자가 요구하는 URL을 받아서 서버에게 전송한다.
1. 서버는 브라우저가 요청하는 get method를 처리할 수 있는지를 확인한다.
   - 불가능하면 `Cannot GET /~~~`
   - 가능하면 해당`REQUEST`에 `RESPONSE(응답)`

`GET REQUEST`에는 어디로 가는지에 대한 `route`정보가 있습니다. `route`는 home을 의미하는 `/`일 수도 있고, `/login`일 수도 있고, `/profile`일 수도 있는것이지요.

이전에는 사용자가 `home`경로로 가려할 때 아무것도 몰라서 `Cannot GET /` 에러를 발생시켰지만, 이제 우리의 `server app`은 사용자가 `/`으로 가려 할 때, 무엇을 해야할 지 알고 있습니다. 그러니 이제는 더이상 에러가 발생하지 않습니다.

#### RESPONSE

현재 우리 서버의 문제점을 이야기해 볼게요. 사용자의 request를 받아들이고 listening하고 있지만, 아직 응답(response)이 없는 상태입니다. 그럼 어떻게 해야 응답할 수 있을까요?
`express`의 route handler에는 **request(req)**, **response(res)**라는 훌륭한 object가 두 개 있습니다. 우리가 작성한 코드로 간략하게 설명을 해보자면, `home`으로 `get request`가 오면, `express`는 `handleHome`에게 request, response object를 보내주게 됩니다. 코드를 실행한 후 request object를 확인해봅시다.

```javascript
import express from "express";
const PORT = 4000;
const app = express();

const handleHome = (req, res) => {
  console.log(req);
};

app.get("/", handleHome);

app.listen(4000, function () {
  console.log(`✔ Server listening on port ${PORT}🚀`);
});
```

콘솔 창을 확인해보면, 엄청난 양의 정보가 생긴것을 볼 수 있습니다. (ㄷㄷ) response object도 확인해봅시다.

```javascript
import express from "express";
const PORT = 4000;
const app = express();

const handleHome = (req, res) => {
  console.log(res);
};

app.get("/", handleHome);

app.listen(4000, function () {
  console.log(`✔ Server listening on port ${PORT}🚀`);
});
```

마찬가지로 정말 많은 정보가 생겼네요! 일단 req, res라는 object가 있다는 것을 확인해본 것으로 만족하고 다음으로 넘어가볼게요. 우리 무한로딩에 빠져있는 웹사이트를 쉬게 해주어야겠습니다. **서버는 브라우저로부터 `request`를 받게 되면, `response`를 `return`해주어야 합니다.** 그러한 의미를 코드에 담아봅시다. 방법은 크게 두가지가 있습니다.

```javascript
import express from "express";
const PORT = 4000;
const app = express();

const handleHome = (req, res) => {
  return res.end();
};

app.get("/", handleHome);

app.listen(4000, function () {
  console.log(`✔ Server listening on port ${PORT}🚀`);
});
```

결과를 한번 확인해볼까요? `res.end()`를 사용함으로써, 우리 서버는 이제 더 이상 무한 로딩에 빠져있지 않게 되었습니다! 결론적으로 말씀드리면 서버는 브라우저에게 아무것도 보내지 않고 request를 끝내버린 것입니다. 다른 방법도 한번 볼까요?

```javascript
import express from "express";
const PORT = 4000;
const app = express();

const handleHome = (req, res) => {
  return res.send("I still love you");
};

app.get("/", handleHome);

app.listen(4000, function () {
  console.log(`✔ Server listening on port ${PORT}🚀`);
});
```

이렇게 `res.send()`를 사용하는 방법도 있습니다. 무한로딩이 없어진 것은 물론이고, 웹사이트를 확인해보면 res.send안에 작성한 문자열이 웹사이트에 출력되어 있는 것을 볼 수 있습니다. 와우! <br>연습으로 route 하나를 더 추가해 봅시다. 이제 우리의 `express server`는 두 개의 URL`(/, /login)`을 이해할 수 있게 되었습니다.

```javascript
import express from "express";
const PORT = 4000;
const app = express();

const handleHome = (req, res) => {
  return res.send("I still love you");
};

const handleLogin = (req, res) => {
  return res.send("Login here");
};

app.get("/", handleHome);
app.get("/login", handleLogin);

app.listen(4000, function () {
  console.log(`✔ Server listening on port ${PORT}🚀`);
});
```

---

이런 방식으로 브라우저와 서버는 서로 상호작용을 하며 웹사이트를 사용자에게 보여주게 됩니다. 위의 예제처럼 `res.send()`를 사용하여 단순한 텍스트만 보여줄 수도 있고 html, 파일, 또는 status code등 많은 것들을 전달해줄 수 있습니다. 굉장히 복잡한 것 같지만 뭔가 재미있네요. **request를 받고 response한다.** 이것이 우리의 wetube app의 기저기능이 될 것입니다.

이렇게 차차 우리들만의 `wetube`를 만들어 가보도록 합시다. 다음시간에는 `middleware`에 대해 알아보도록 할게요. 꼭 만들어 봅시다. 포기하지 말고 화이팅!
