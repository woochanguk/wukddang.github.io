---
layout: post
image: /assets/img/nodejs/NodeJS.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  JavaScript의 Prototype에 대해 알아보겠습니다.
invert_sidebar: false
categories:
  - backend
  - nodejs
---

# JavaScript Basic - Prototype

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `backend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

1. table of contents
{:toc .large-only}
---

이번시간에는 **JavaScript**에서 상속을 구현하는 개념인 **Prototype**에 대해 알아보겠습니다.

---
## Code tester 
코드 예제는 여기서 확인해주세요.
<iframe src="https://codesandbox.io/embed/jovial-hermann-yk8xfq?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="blog-js"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>
---

## Prototype
먼저 특정 **Class**를 정의하면서 시작해보겠습니다. 다른 언어에서는 그렇지 못하지만 **JavaScript ES5 syntax**를 사용하면 함수를 정의하듯이 **Class**를 사용할 수 있습니다.**(ES5 syntax)** 물론 **JavaScript**에서도 **Class**라는 키워드를 제공하지만, 실제로 나타내는 바는 **Prototype**을 기반으로 한 **function**이기 때문에 큰 차이가 없다고 할 수 있겠습니다.

```js
function Student(name) {
  this.name = name;
}

const me = new Student("Changuk");
console.log(me);
```
다음과 같이 작성하고 출력해보면, 어떤 생성자(**Constructor**, 함수)로부터 만들어졌는지를 적어주고 객체로 표시해줍니다. **new** 키워드를 사용하여 함수를 사용해주면, 함수는 생성자로서 동작하게 됩니다. 따라서 **this** 키워드는 새로 생성되는 객체에게 바인딩되고 여러개의 객체가 생성되면, 각각의 객체에게 **this** 키워드를 사용할 수 있게 되는 것입니다.<br>

해당 내용을 **Vscode**에서 **debug**모드로 실행해보면 더 잘 이해할 수 있습니다. 현재 **new**로 생성한 **me**는 **Student**이자, **객체**입니다. 무슨 말이냐면, 객체 **me**는 **Student** 함수 내부의 `this.name = name;`이라는 기능과 객체가 가지는 기능들을 모두 사용할 수 있다는 뜻이지요. **Prototype Chaining**이라는 기능을 통해 먼저 선언된 순서대로 기능들을 찾아서 **Student**와 **객체**의 기능을 이용할 수 있게 됩니다.<br>

추가적으로 환영하는 코드를 작성해보겠습니다. 우리가 생성하려는 **Student**에 특정 코드를 추가해주기 위해서는 **prototype**을 이용해서 다음과 같이 작성하면 됩니다.
 
```js
// Prototype

function Student(name) {
  this.name = name;
}

Student.prototype.greet = function greet() {
  return `Hi, ${this.name}`;
};

const me = new Student("Changuk");
console.log(me);
```

이런 **Prototype Chain**의 특성을 통해 간단한 상속개념을 구현해보도록 하겠습니다.
```js
// Prototype

function Person(name) {
  this.name = name;
}

Person.prototype.greet = function greet() {
  return `Hi, ${this.name}`;
};

function Student(name) {
  this.__proto__.constructor(name);
}

Student.prototype.study = function study() {
  return `${this.name} is studying.`;
};

// Student, Person의 연결관계 구현
// Student(Person) prototype 객체의 prototype을 다시 설정.
Object.setPrototypeOf(Student.prototype, Person.prototype);

const me = new Student("Changuk");
console.log(me.greet());
console.log(me.study());
```
`Object.setPrototypeOf()` 를 사용해서 `Student.prototype`을 `Person.prototype`으로 연결 해주었습니다. **debug** 모드에서 실행해보면 **Student**의 **prototype**이 **Person**으로 설정되어 있는 것을 확인할 수 있습니다.<br>

**new** 키워드로 생성한 **Student**는 `this.__proto__.constructor(name)`을 통해  **Person** 객체로 **name**을 전달해주게 됩니다. 그러면 `study()`, `greet()`을 모두 사용할 수 있게 되는 거죠. <br>

최신의 **JavaScript** 문법을 사용해주면 다음과 같이 코드를 작성할 수 있습니다. 그러나 **debug**모드에서 확인해보면, 이전과 같은 형태로 **Prototype Chain**이 작성되어 있는것을 확인할 수 있습니다. 결국 **Class**도 **Prototype**을 이용하는 것이라고 볼 수 있겠습니다.

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `Hi, ${this.name}.`;
  }
}
class Student extends Person {
  constructor(name) {
    super(name);
  }

  study() {
    return `${this.name} is studying.`;
  }
}

const me = new Student("Changuk");
console.log(me.study());
console.log(me.greet());
```