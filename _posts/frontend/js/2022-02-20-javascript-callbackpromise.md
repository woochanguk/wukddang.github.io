---
layout: post
image: /assets/img/js/javascript.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  JavaScript Callback, Promise에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - frontend
  - js
---

# JS - Callback & Promise

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. toc
{:toc .large-only}

---

이번시간에는 JavaScript의 callback, promise에 대해 알아보겠습니다.

---
## Code tester 
코드 예제는 여기서 확인해주세요.
<iframe src="https://codesandbox.io/embed/jovial-hermann-yk8xfq?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="blog-js"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

## 비동기 - Callback & Promise

다음 코드를 보시죠. 
```js
function a() {
  console.log("A");
}

function b() {
  console.log("B");
}

a();
b();
// 출력 결과 : 
// A
// B
``` 
위의 코드처럼, 순서대로 동작하는 방식을 **동기방식(Synchronous)**이라고 합니다. 그러면 이번에는 `setTimeout()`메서드를 활용해서 1초뒤에 실행되는 코드를 작성해보겠습니다. 어떤 결과가 출력될까요? B가 먼저 출력됩니다. 

```js
function a() {
  setTimeout(() => {
    console.log("A");
  }, 1000);
}

function b() {
  console.log("B");
}

a();
b();
// 출력 결과 :
// B
// A
```
이와 같이 함수의 기본적인 처리에 지연이 있다면 **비동기(Asynchronous)**라는 개념을 도입해서 처리를 수행하게 됩니다. 

### callback
비동기 처리 방식으로 먼저 **callback**에 대해 알아보겠습니다. 다음 코드를 살펴보죠.
```js
function a(callback) {
  setTimeout(() => {
    console.log("A");
    callback()
  }, 1000);
}

function b() {
  console.log("B");
}

a(function () {
  b()
})
// 출력 결과 :
// A
// B
```
먼저 실행되길 원하는 코드가 출력된 후에 다음 코드가 실행될 수 있도록, `a()` 함수는 인수로 **callback**을 받고  **callback**을 함수처럼 실행하였습니다. 그 후 `a()` 함수를 호출하였는데, `b()`를 호출하는 익명함수를 인수로 받았습니다. 즉, `a()` 함수가 호출되고 `A`가 출력된 후에 **callback**으로 입력된 `function() {b()}` 가 호출되는 구조를 지니는 것입니다.


이 **callback**함수로 인수를 전달해줄 수도 있습니다. 아래 코드를 봅시다. 문자 데이터 **str**을 인수로 `callback()` 함수로 전달해주면 **callback**으로 입력된 `function(event) {}`가 받은 인수를 출력하는 구조의 함수입니다.
```js
function a(callback) {
  const str = 'Hello A'
  setTimeout(() => {
    console.log("A")
    callback(str)
  }, 1000);
}

function b() {
  console.log("B")
}

a(function (event) {
  console.log(event)
  b()
})
// 출력 결과 :
// A
// Hello A
// B
```
이러한 **callback**의 원리를 이용해서 비동기 방식의 코드를 구성해줄 수 있습니다. 하지만 이런 방식이 계속되면 문제가 생깁니다. 다음 코드를 보시죠.

```js
function a(callback) {
  setTimeout(() => {
    console.log("A")
    callback()
  }, 1000)
}

function b(callback) {
  setTimeout(() => {
    console.log("B")
    callback()
  }, 1000)
}

function c(callback) {
  setTimeout(() => {
    console.log("C")
    callback()
  }, 1000)
}

function d(callback) {
  setTimeout(() => {
    console.log("D")
    callback()
  }, 1000)
}

a(function () {
  b(function () {
    c(function () {
      d(function () {
        console.log('Done!')
      })
    })
  })
})
```
간단한 코드죠? 1초뒤에 **A**가 출력되고, 1초뒤에 **B**가 출력되고... 마지막으로 **Done!**이 출력되는 구조입니다. 하지만 코드구조가 복잡해지고 지금보다 더 많은 **callback**을 사용하게 된다면 결코 간단한 코드가 아니게 될 것입니다. 이를 보통 **callback 지옥**이라고 표현합니다.

### Promise 객체
**Promise**를 사용하게 되면 **async**, **await** 키워드를 사용하여 비동기 처리를 수행할 수 있게 됩니다. 아래 코드로 살펴봅시다. **Promise**객체를 생성자 함수를 이용하여 인스턴스를 생성해주었고, 내부에는 1초뒤에 `A`를 출력해주는 코드를 작성하였습니다. <br>

 `A`가 출력되고 난 이후에 다른 함수를 호출하고 싶다면 **resolve**인수를 사용해주면 됩니다. `a()` 함수는 **Promise** 객체를 사용해서 출력된 결과를 반환해주게 됩니다. **return**을 작성해주지 않으면 **async**, **await**를 사용할 수 없으니 작성해주세요
```js
function a() {
  return new Promise(function (resolve) {
    setTimeout(() => {
      console.log('A')
      resolve('Hello A')
    }, 1000)
  })
}

function b() {
  console.log('B')
}

async function test() {
  const res = await a()
  console.log('res:', res)
  b()
}

test()
// 출력 결과 :
// A
// res: Hello A
// B
```
위의 코드에서는 `resolve()`라는 함수가 실행이 완료될 때 까지 기다리도록 **async**, **await** 키워드를 사용해주었습니다. 또 **callback**에서 사용한 것처럼, **resolve**도 인수를 받을 수 있습니다. <br>

위의 코드에서 1초가 지나고 **'A'**가 출력된 후에 **resolve**에 입력한 인수인, 문자 데이터 **'Hello A'**가 전달되어 상수 **res**로 할당되었습니다. 그 후에 **res** 데이터가 출력되고 **'B'**가 출력되었습니다. 즉  **callback**의 기능을 **Promise**도 구현해줄 수 있습니다.

```js
function a() {
  return new Promise(function (resolve) {
    setTimeout(() => {
      console.log("A");
      resolve("Hello A");
    }, 1000);
  });
}
function b() {
  return new Promise(function (resolve) {
    setTimeout(() => {
      console.log("B");
      resolve("Hello B");
    }, 1000);
  });
}
function c() {
  return new Promise(function (resolve) {
    setTimeout(() => {
      console.log("C");
      resolve("Hello C");
    }, 1000);
  });
}
function d() {
  return new Promise(function (resolve) {
    setTimeout(() => {
      console.log("D");
      resolve("Hello D");
    }, 1000);
  });
}

async function test() {
  const h1 = await a();
  const h2 = await b();
  const h3 = await c();
  const h4 = await d();
  console.log("Done!");
  console.log(h1, h2, h3, h4)
}
test();
// 출력 결과 :
// A
// B
// C
// D
// Done!
// Hello A Hello B Hello C Hello D
```

## 비동기 - 예외 처리(then, catch, finally)
<a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise" target="_blank">**Promise**</a>와 관련된 내용들을 살펴봅시다. **Promise**의 인스턴스 메서드로는 `catch()`, `then()`, `finally()`가 존재합니다.

- `then()`
- `catch()`
- `finally()` 

순으로 중요하다고 생각해주시면 될 것 같아요.

### then()

```js
function a() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("A");
      resolve();
    }, 1000);
  });
}
async function test() {
  a().then(() => {
    console.log("B");
  });
}

test();
// 출력 결과
// A
// B
```
다음과 같이 작성해주면, `"A"`가 실행되고 난 이후에 `"B"`가 실행되는 코드라고 이해하시면 되겠습니다. `then()` 메서드를 사용해서 다음과 같이 순차적으로 실행되는 코드도 작성해줄 수 있겠습니다. 하지만 **callback 지옥**과 패턴이 동일하기 때문에 이렇게 작성하는 것은 권장하지 않습니다.

```js
function a() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("A");
      resolve();
    }, 1000);
  });
}

function b() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("B");
      resolve();
    }, 1000);
  });
}

function c() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("C");
      resolve();
    }, 1000);
  });
}

function d() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("D");
      resolve();
    }, 1000);
  });
}

function test() {
  a().then(() => {
    b().then(() => {
      c().then(() => {
        d().then(() => {
          console.log('Done!')
        })
      })
    })
  })
}
```
대신 아래의 코드처럼 화살표 함수와 함께 작성해주면 더 깔끔해집니다.
```js
function a() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("A");
      resolve();
    }, 1000);
  });
}

function b() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("B");
      resolve();
    }, 1000);
  });
}

function c() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("C");
      resolve();
    }, 1000);
  });
}

function d() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log("D");
      resolve();
    }, 1000);
  });
}

function test() {
  a()
    .then(() => b())
    .then(() => c())
    .then(() => d())
    .then(() => {
      console.log("Done!");
    });
  // a().then(() => {
  //   return b()
  // }).then(() => {
  //   return c()
  // }).then(() => {
  //   return d()
  // }).then(() => {
  //   console.log('Done!')
  })
}

test();
```
위와 같이, **async**, **await** 비동기 패턴을 사용할 수 없을 때 `then()` 메서드를 체이닝 형식으로 작성해주면 됩니다. 단, 체이닝 형식으로 사용하기 위해서는 `then()` 메서드의 내부에서 새로운 **promise** 객체가 반환되어야 합니다.

### catch()
`catch()` 메서드를 사용하면, 예외 처리를 할 수 있는 **reject** 매개변수를 받아서 사용할 수 있습니다. 아래 코드를 보시죠. 함수 `a()`를 살펴보면, **number** 인수를 받습니다. **if**문에 걸리면, `reject()` 함수가 실행되고, 함수는 종료되게 되죠. 함수 `test()`를 호출했을 때, **resolve** 매개변수가 실행되면 `then()` 메서드가 동작하고 **reject** 매개변수가 실행되면 `catch()` 메서드가 동작하게 됩니다. 

```js
function a(number) {
  return new Promise((resolve, reject) => {
    if (number > 4) {
      reject();
      return;
    }
    setTimeout(() => {
      console.log("A");
      resolve();
    }, 1000);
  });
}

function test() {
  a(7)
    .then(() => {
      console.log("Resolve!");
    })
    .catch(() => {
      console.log("Reject!");
    });
}

test();
```

### finally()
`finally()` 메서드는 **resolve**, **reject** 중 어떤 매개변수가 동작하든지에 관계없이 **제일 마지막에 동작하는 메서드**입니다. 다음 코드로 이해하시면 되겠습니다. 즉, **then()**, **catch()**, **finally()** 로 예외 처리를 수행해 줄 수 있는 것이지요.

```js
function a(number) {
  return new Promise((resolve, reject) => {
    if (number > 4) {
      reject();
      return;
    }
    setTimeout(() => {
      console.log("A");
      resolve();
    }, 1000);
  });
}

function test() {
  a(1)
    .then(() => {
      console.log("Resolve!");
    })
    .catch(() => {
      console.log("Reject!");
    })
    .finally(() => {
      console.log("Done!");
    });
}

test();
```

## 비동기 - 예외 처리(async, await)
이번에는 **async**, **await**를 활용한 예외 처리에 대해서도 간략하게 알아보겠습니다. 아래 코드를 살펴보죠. `finally()` 메서드는 동일하게 작성해주었습니다.

```js
function a(number) {
  return new Promise((resolve, reject) => {
    if (number > 4) {
      reject();
      return;
    }
    setTimeout(() => {
      console.log("A");
      resolve();
    }, 1000);
  });
}
test();

async function test() {
  **try** {
    await a(8);
    console.log("Resolve!");
  } catch (error) {
    console.log("Reject!");
  } finally {
    console.log('Done!')
  }
}
```

## 비동기 처리 예제
비동기 패턴들을 사용해서 **API**를 처리하는 방법을 연습해보도록 하겠습니다. (**CDN** 방식으로 **axios** 라이브러리를 가져왔습니다.) 

```js
function fetchMovies(title) {
  const OMDB_API_KEY = "123";
  return new Promise(async (resolve, reject) => {
    try {
      const res = await axios.get(
        `https://omdbapi.com?apikey=${OMDB_API_KEY}&s=${title}`
      );
      resolve(res);
    } catch (error) {
      console.log(error.message);
      reject("Changuk?!");
    }
  });
}

async function test() {
  try {
    const res = await fetchMovies("frozen");
    console.log(res);
  } catch (changuk) {
    console.log(changuk);
  }
}
test();

function hello() {
  fetchMovies("jobs")
    .then((res) => console.log(res))
    .catch((changuk) => {
      console.log(changuk);
    });
}
hello();
```
- `fetchMovies()` 함수를 작성하였습니다. 
- 생성자 함수로 **Promise** 객체를 생성하고 **resolve**, **reject** 인수를 받도록 하였습니다. 
- **Promise** 내부에 **callback**을 **비동기 함수**를 구현했습니다. 
- **try**, **catch**를 사용하여 예외 처리를 하는 구문을 작성하였고 **try**에는 **axios** 라이브러리로 **GET Request**를 요청하여 받은 데이터를 **res**에 할당해주었습니다. 
- 성공적인 반환을 의미하는 **resolve**를 호출하고 인수로 res를 받았습니다.


- 함수 `test()`는 **fetchMovies**에 `"frozen"`문자 데이터를 인수로 보내서 **async**, **await**를 활용한 **try**, **catch**로 예외 처리를 구현하였습니다.

- 함수 `hello()`는 **fetchMovies**에 `"jobs"` 문자 데이터를 인수로 보내서 **then**, **catch**를 활용하여 예외 처리를 구현하였습니다.

---
#### 참조

- **APIkey**를 잘못된 값으로 입력하면 **401 에러**가 호출됩니다.