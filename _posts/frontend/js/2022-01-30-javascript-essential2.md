---
layout: post
image: /assets/img/js/javascript.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  안녕하세요?!
invert_sidebar: false
categories:
  - frontend
  - js
---

# JS - Class

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

## 생성자 함수(Prototype)

이번 시간에는 javascript의 class에 대해 알아보겠습니다.

1. table-of-contents
{:toc .large-only}

---

먼저 코드를 살펴보겠습니다.

```javascript
const changuk = {
  firstName: 'Changuk',
  LastName: 'Woo',
  getFullName: function () {
    return `${this.firstName} ${this.LastName}`
  },
}
console.log(changuk)

// 출력 결과 :
// Object
// LastName: "Woo"
// firstName: "Changuk"
// getFullName: ƒ getFullName()
// [[Prototype]]: Object
```

일단 `const changuk`이라고 상수가 선언되어 있습니다. 그리고 상수에 `{}`를 사용한 객체 데이터가 할당되어 있습니다. 객체 데이터를 출력한 결과는 아래 출력 결과로 확인해볼 수 있고, `firstName`, `LastName`으로 된 것은 **속성(Property)**이라고 할 수 있습니다. 또, 그런 속성에 함수가 할당되어 있다면 이는 더 이상 속성이라고 부르지 않고 **메서드(Method)** 라고 부르게 됩니다. 속성과 메서드를 통틀어 **멤버(Member)**라고 부르기도 합니다. <br> 메서드 부분을 잠깐 볼까요? 문자열 interpolation을 사용한 건 알겠는데, `this`라는 용어가 사용되었습니다. 일단 유추해보자면, 현재 객체 데이터로 정의된 `changuk`을 지칭하는 거라고 생각해볼 수 있을 것 같습니다. this를 사용하는 것 대신에, 객체이름을 직접적으로 명시하는 게 더 직관적일 수 있겠지만, 코드를 작성하면서 객체의 이름은 변할 수 있습니다. 따라서 현재의 객체를 의미하고자 할 때는 **this**를 사용해주는 게 좋습니다. **return** 결과를 보고자 한다면 다음과 같이 작성해주면 되겠습니다.

```javascript
console.log(changuk.getFullName)

// 출력 결과 : Changuk Woo
```

만약, `changuk`이외에 다른 사용자들도 `getFullName`을 사용하려면 어떻게 해야 할까요? const changuk했던 것처럼 일일이 상수를 선언해주어야 할까요? 그렇게 해도 동작은 하겠지만, 메모리의 낭비가 심해질 것입니다. 같은 로직을 사용하는데 동일한 부분이 반복되게 되니까요. 그러니 이제 우리는 메모리의 효율적인 관리를 위해 JavaScript의 Class를 사용해보도록 하겠습니다. 아래 코드를 잘 보도록 합시다.

```javascript
function user(first, last) {
  this.firstName = first
  this.lastName = last
}

user.prototype.getFullName = function () {
  return `${this.firstName} ${this.lastName}`
}

const changuk = new user('Changuk', 'Woo')
const amy = new user('Amy', 'Clarke')
const neo = new user('Neo', 'Smith')

console.log(changuk)
console.log(amy)
console.log(neo)

// 출력 결과 :

// user {firstName: 'Changuk', lastName: 'Woo'}
// firstName: "Changuk"
// lastName: "Woo"
// [[Prototype]]: Object
// getFullName: ƒ ()
// constructor: ƒ user(first, last)
// [[Prototype]]: Object

// user {firstName: 'Amy', lastName: 'Clarke'}
// firstName: "Amy"
// lastName: "Clarke"
// [[Prototype]]: Object
// getFullName: ƒ ()
// constructor: ƒ user(first, last)
// [[Prototype]]: Object

// user {firstName: 'Neo', lastName: 'Smith'}
// firstName: "Neo"
// lastName: "Smith"
// [[Prototype]]: Object
// getFullName: ƒ ()
// constructor: ƒ user(first, last)
// [[Prototype]]: Object
```

다음과 같은 결과가 출력되었습니다.
코드에서 볼 수 있듯이 우리는 first, last 인수에 각각 문자열을 입력해주었고, **new**라는 키워드를 사용하여 **user**함수를 실행하였습니다. 이때의 user함수를 **생성자 함수**라고 부릅니다. 그럼 무엇이 생성된다는 의미인가요? 바로 객체 데이터가 생성된다는 의미입니다. 객체 생성은 사실 `{}`기호 하나만으로도 쉽게 할 수 있었죠? 이렇게 특정한 기호를 통해 어떤 데이터를 생성하는 것을 **리터럴**방식이라고 부릅니다. "A", [](배열 데이터) 와 같이 문자를 생성하는 것도 리터럴방식을 이용하는 것이라고 볼 수 있겠습니다. <br>
이야기가 좀 길어졌습니다. 결국 **this**라는 것은 생성자 함수를 통해서 할당되는 앞의 객체부분을 지칭하는 것입니다. **new** 키워드를 통해 생성자 함수로 실행한 결과를 반환하여 할당된 변수를 생성자 함수의 **인스턴스(instance)**라고 부르게 됩니다. <br>
가운데에 user.prototype.getFullName이라고 정의한 부분을 살펴봅시다. firstName, lastName과 같은 부분은 user라는 생성자 함수가 실행될 때 마다 입력받는 것이 달라질 수 있기 때문에 통일하여 메모리를 관리하기는 어렵지만, getFullName의 경우 기능이 동일하기 때문에 통일화하여 메모리를 효과적으로 관리할 수 있게 됩니다. 결국 user함수의 `prototype`속성 부분에 `getFullName`을 할당해주면, 몇개의 객체를 만들던 간에, getFullName을 사용하는 메모리는 딱 한번만 사용하게 됩니다. user함수를 통해 만들어진 데이터들의 getFullName 메소드들은, 이미 만들어져 있는 getFullName함수를 **참조**하는 것이 됩니다.

이와 같은 이유 때문에, **JavaScript를 프로토타입 기반 언어**라고 부르기도 합니다. 기본적인 내용만 살펴보았지만 (user.)prototype을 사용하고, user생성자 함수로 인스턴스를 만들어내는 이러한 개념들을 `JavaScript의 Class`라고 부릅니다. 엄밀히 말하면 다른 프로그래밍 언어의 클래스와는 다른 개념이긴 합니다.
간단하게 하나만 더 알아보고 가겠습니다.
배열데이터를 하나 생성해보도록 하죠.

```javascript
const a = [1, 2, 3]
a

// 출력 결과 :

// (3) [1, 2, 3]
// 0: 1
// 1: 2
// 2: 3
// length: 3
// [[Prototype]]: Array(0)
// at: ƒ at()
// concat: ƒ concat()
// constructor: ƒ Array()
// copyWithin: ƒ copyWithin()
// entries: ƒ entries()
// every: ƒ every()
// fill: ƒ fill()
// filter: ƒ filter()
// find: ƒ find()
// findIndex: ƒ findIndex()
// findLast: ƒ findLast()
// findLastIndex: ƒ findLastIndex()
// flat: ƒ flat()
// flatMap: ƒ flatMap()
// forEach: ƒ forEach()
// includes: ƒ includes()
// indexOf: ƒ indexOf()
// join: ƒ join()
// keys: ƒ keys()
// lastIndexOf: ƒ lastIndexOf()
// length: 0
// map: ƒ map()
// pop: ƒ pop()
// push: ƒ push()
// reduce: ƒ reduce()
// reduceRight: ƒ reduceRight()
// reverse: ƒ reverse()
// shift: ƒ shift()
// slice: ƒ slice()
// some: ƒ some()
// sort: ƒ sort()
// splice: ƒ splice()
// toLocaleString: ƒ toLocaleString()
// toString: ƒ toString()
// unshift: ƒ unshift()
// values: ƒ values()
// Symbol(Symbol.iterator): ƒ values()
// Symbol(Symbol.unscopables): {copyWithin: true, entries: true, fill: true, find: true,            findIndex: true, …}
// [[Prototype]]: Object
```

굉장히 많은 함수들이 Prototype으로 배열 안에 들어가 있는 것을 확인할 수 있습니다. 이 내용 중 하나를 한번 사용해봅시다. **includes** 메서드는 배열 안에 특정 데이터가 있는지 없는지를 알려줍니다. 4라는 데이터는 없으므로 false값이 나오게 됩니다. 또, 2라는 데이터는 있으므로 true값이 나옵니다.

```javascript
const a = [1, 2, 3]
a.includes(4)
a.includes(2)

// 출력 결과 :
// false
// true
```

생성자 함수는 new라는 키워드로 생성을 했습니다. 그런데 생성자 함수는 일반적으로 일반함수와 구분이 안되기 때문에, JavaScript 개발자들은 관행적으로 new키워드를 사용하는 생성자 함수엔 `Pascal-Case`로 첫 문자를 대문자로 사용해오고 있습니다.

---

## this

이번엔 this키워드에 대해 알아보도록 하겠습니다. <br>(참고로, 일반 함수를 정의할 때, function 키워드를 생략해줄 수 있습니다.)

```javascript
const changuk = {
  name: 'Changuk',
  normal: function () {
    console.log(this.name)
  },
  arrow: () => {
    console.log(this.name)
  },
}
changuk.normal()
changuk.arrow()

// 출력 결과 :
// Changuk
// undefined
```

같은 로직이지만, 출력 결과는 다르군요. 일반(Normal) 함수는 **호출 위치에서 this가 정의**됩니다. 이게 무슨 의미일까요? 바로 `changuk.normal()`로 호출하는 위치에서 this가 정의된다고 이해하시면 됩니다. 따라서 **changuk**이 **this**이고 이 부분에서 name을 꺼내 쓰는 것이기 때문에 잘 출력이 된다고 볼 수 있습니다. 반면, 화살표(Arror) 함수는 **선언된 범위에서 this를 정의**하게 됩니다. 위 코드상에서는 선언된 범위안의 this가 존재하지 않기 때문에 undefined라는 값이 출력되는 것을 알 수 있습니다.

```javascript
const changuk = {
  name: 'Changuk',
  normal: function () {
    console.log(this.name)
  },
  arrow: () => {
    console.log(this.name)
  },
}
changuk.normal()
changuk.arrow()

const amy = {
  name: 'Amy',
  normal: changuk.normal,
  arrow: changuk.arrow,
}

amy.normal()
amy.arrow()

// 출력 결과 :
// Amy
// undefined
```

`const amy`는 changuk의 normal, arrow함수를 각각 할당받았습니다. 그 후 출력 결과를 보면 이전의 코드와 동일합니다.

---

이번에는 prototype을 사용하는 생성자 함수에서 this가 어떻게 정의되어야 하는지를 살펴봅시다.

```javascript
function User(name) {
  this.name = name
}
User.prototype.normal = function () {
  console.log(this.name)
}
User.prototype.arrow = () => {
  console.log(this.name)
}

const changuk = new User('Changuk')

changuk.normal()
changuk.arrow()

// 출력 결과 :
// Changuk
// undefined
```

이전과 동일한 결과를 보여줍니다. 일반(Normal)함수, 화살표(Arrow) 함수를 상황에 맞게 사용해야 합니다.
새로운 예제를 통해 더 깊게 이해해 봅시다.

```javascript
const timer = {
  name: 'Changuk!',
  timeout: function () {
    //   setTimeout(함수, 시간);
    setTimeout(function () {
      console.log(this.name)
    }, 2000)
  },
}
timer.timeout()

// 출력 결과 :
// undefined
```

undefined가 출력되었습니다. this가 사용된 부분은 일반 함수로 만들어져 있고, 일반 함수는 호출 위치에서 this가 정의되므로, 함수가 어디에서 호출되는지를 알아야 합니다. setTimeout의 내부에서 callback으로 호출되어 실행이 될텐데, 만약 우리가 이 this.name을 timer객체 데이터의 name을 지칭하여 출력하기를 원했다면, 일반 함수로 callback을 사용하면 안됩니다. 왜냐하면 이 일반 함수는 setTImeout 함수 내부의 어딘가에서 실행될 것이기 때문이죠. 따라서 일반 함수를 화살표 함수로 변경해 주면 해결될 것 같습니다.

```javascript
const timer = {
  name: 'Changuk!',
  timeout: function () {
    //   setTimeout(함수, 시간);
    setTimeout(() => {
      console.log(this.name)
    }, 2000)
  },
}
timer.timeout()

// 출력 결과 :
// undefined
```

앞서 말씀드린 내용과 동일합니다. 화살표 함수는 자신이 선언된 함수 범위내에서 this를 정의합니다. 위의 코드를 보면, 일반 함수 내에 화살표 함수가 정의되어 있는데, 이 일반 함수의 범위에서 this가 정의됩니다. 일반 함수의 범위 내에서 this를 찾으면 timer 객체 데이터를 가리키게 되므로, **this는 timer가 됩니다**.

---

## ES6 Classes

ES6의 JavaScript Class 패턴에 대해 알아보겠습니다. 이전코드를 조금 수정해보도록 하겠습니다. 익숙하진 않지만, 훨씬 직관적인것 같습니다. 추후 `react`를 배울 때 이런 문법을 많이 사용하게 될 것입니다.

```javascript
class User {
  constructor(first, last) {
    this.firstName = first
    this.lastName = last
  }
  getFullName() {
    return `${this.firstName} ${this.lastName}`
  }
}

const changuk = new User('Changuk', 'Woo')
const amy = new User('Amy', 'Clarke')
const neo = new User('Neo', 'Smith')

console.log(changuk)
console.log(amy)
console.log(neo)

// 출력 결과 :
// Changuk WOo
// Amy Clarke
// Neo Smith
```

위와 같은 코드로 작성하여도 동일한 결과를 출력하는 것을 볼 수 있습니다.

## 상속(확장)

이번에는 상속의 개념에 대해 알아보겠습니다. 아래 코드를 보면서 설명하겠습니다.

```javascript
class Vehicle {
  constructor(name, wheel) {
    this.name = name
    this.wheel = wheel
  }
}

const myVehicle = new Vehicle('운송수단', 2)
console.log(myVehicle)

// 출력 결과 :
// Vehicle {name: '운송수단', wheel: 2}
// name: "운송수단"
// wheel: 2
// [[Prototype]]: Object

class Bicycle extends Vehicle {
  constructor(name, wheel) {
    super(name, wheel)
  }
}

const myBicycle = new Bicycle('삼천리', 2)
const daughtersBicycle = new Bicycle('세발', 3)

// 출력 결과 :

// Bicycle {name: '삼천리', wheel: 2}
// name: "삼천리"
// wheel: 2
// [[Prototype]]: Vehicle

// Bicycle {name: '세발', wheel: 3}
// name: "세발"
// wheel: 3
// [[Prototype]]: Vehicle

console.log(myBicycle)
console.log(daughtersBicycle)

class Car extends Vehicle {
  constructor(name, wheel, license) {
    super(name, wheel)
    this.license = license
  }
}

const myCar = new Car('벤츠', 4, true)
const daughtersCar = new Car('포르쉐', 4, false)

console.log(myCar)
console.log(daughtersCar)

// 출력 결과 :
// Car {name: '벤츠', wheel: 4, license: true}
// license: true
// name: "벤츠"
// wheel: 4
// [[Prototype]]: Vehicle

// Car {name: '포르쉐', wheel: 4, license: false}
// license: false
// name: "포르쉐"
// wheel: 4
// [[Prototype]]: Vehicle
```

코드를 보면 가운데 Bicycle이라는 클래스가 정의되어 있고 그 부분을 보면, extends 라는 키워드가 있습니다. 이는 확장 또는 상속이라는 의미로 설명될 수 있습니다. 그 뒤에는 Vehicle이라는 클래스가 있습니다. Vehicle 클래스를 Bicycle 클래스로 확장(상속)을 해서 **Vehicle의 내용을 Bicycle 클래스 내부에서 사용하겠다**는 의미가 됩니다. 그래서 Bicycle 내부에서는 `super`라는 함수를 사용할 수 있습니다. 이 `super`는 확장된 클래스인, `Vehicle`을 의미하게 되는 거죠. 따라서 Bicycle이 받은 **name**, **wheel** 인수를 Vehicle에 입력하게 되고, 내부 로직이 실행될 수 있는 구조를 갖게 됩니다. 클래스를 사용한다면 미리 만들어진 정보에 추가적인 기능을 추가하여 확장할 수 있다고 생각해주시면 됩니다.
