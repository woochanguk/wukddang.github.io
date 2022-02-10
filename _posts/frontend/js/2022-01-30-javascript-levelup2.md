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

# JS - Arrays

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

배열을 다루는 메서드들에 대해 알아보겠습니다. <a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array" target="_blank">`MDN문서`</a>를 참조했습니다.

1. table of contents
{:toc .large-only}
---

## forEach

**forEach() 메서드는 주어진 함수를 배열 요소 각각에 대해 실행합니다.** forEach()의 첫 번째 인수는 배열 item의 이름, 두 번째 인수는 반복되는 횟수를 의미합니다. 배열 item의 갯수만큼 특정 callback 함수를 반복적으로 실행하고자 할 때 사용하는 메서드라고 이해해주시면 되겠습니다.

```javascript
const numbers = [1, 2, 3, 4];
const fruits = ["Apple", "Banana", "Cherry"];

fruits.forEach(function (element, index) {
  console.log(element, index);
});

// 출력 결과 :
// Apple 0
// Banana 1
// Cherry 2
```

---

## map

map() 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 **호출한 결과를 모아 새로운 배열을 반환**합니다. 참고로 forEach는 따로 반환되는 값이 없기 때문에, 변수에 값을 지정해줄 수 없습니다. map() 메서드는 forEach() 메서드와 비슷하지만 조금 다릅니다. map() 메서드는 원본에 영향을 끼치지 않습니다.

```javascript
const numbers = [1, 2, 3, 4];
const fruits = ["Apple", "Banana", "Cherry"];

const a = fruits.forEach(function (fruit, index) {
  console.log(`${fruit}-${index}`);
});
console.log(a);

const b = fruits.map(function (fruit, index) {
  return `${fruit}-${index}`;
});
console.log(b);

// 출력 결과 :
// undefined
// 0: "Apple-0"
// 1: "Banana-1"
// 2: "Cherry-2"
// length: 3
// [[Prototype]]: Array(0)
```

fruits 배열 내에 있는 아이템의 갯수 만큼 map() 메서드 내의 callback함수가 실행됩니다. 위의 예제에서는 총 3번 실행된다고 할 수 있겠습니다.
<br>
또 map() 메서드를 사용해서 다음과 같은 방식으로도 배열을 만들 수 있습니다.

```javascript
const numbers = [1, 2, 3, 4];
const fruits = ["Apple", "Banana", "Cherry"];

const b = fruits.map(function (fruit, index) {
  return {
    id: index,
    name: fruit,
  };
});
console.log(b);

// 출력 결과 :
// 0: {id: 0, name: 'Apple'}
// 1: {id: 1, name: 'Banana'}
// 2: {id: 2, name: 'Cherry'}
// length: 3
// [[Prototype]]: Array(0)
```

이번엔 일반 함수를 화살표 함수로 한번 바꿔보도록 하겠습니다. 좀 더 간결하게 표현이 가능할 거에요.

```javascript
const numbers = [1, 2, 3, 4];
const fruits = ["Apple", "Banana", "Cherry"];

const a = fruits.forEach((fruit, index) => {
  console.log(`${fruit}-${index}`);
});
console.log(a);

const b = fruits.map((fruit, index) => ({
  id: index,
  name: fruit,
}));
console.log(b);
```

---

## filter

filter() 메서드는 **주어진 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환**합니다. filter() 메서드는 원본에 영향을 끼치지 않습니다.

```javascript
const numbers = [1, 2, 3, 4];
const fruits = ["Apple", "Banana", "Cherry"];

const a = numbers.map((number) => number < 3);

console.log(a);

const b = numbers.filter((number) => number < 3);

console.log(b);

// 출력 결과 :
// (4) [true, true, false, false]
// 0: true
// 1: true
// 2: false
// 3: false
// length: 4
// [[Prototype]]: Array(0)

// (2) [1, 2]
// 0: 1
// 1: 2
// length: 2
// [[Prototype]]: Array(0)
```

---

## find(), findIndex()

find() 메서드는 **주어진 판별 함수를 만족하는 배열의 첫 번째 요소 값을 반환**합니다. 그런 요소가 없다면 undefined를 반환합니다.<br>
findIndex() 메서드는 **주어진 판별 함수를 만족하는 배열의 첫 번째 요소에 대한 인덱스를 반환**합니다. 만족하는 요소가 없으면 -1을 반환합니다. 먼저 find() 메서드에 대해 알아보죠

```javascript
const numbers = [1, 2, 3, 4];
const fruits = ["Apple", "Banana", "Cherry"];

const a = fruits.find((fruit) => /^B/.test(fruit));
console.log(a);

// 출력 결과 :
// Banana
```

`/^B/`는 대문자 B로 시작하는 문자를 의미하는 정규표현식입니다. findIndex() 메서드에 대해 알아보겠습니다.

```javascript
const numbers = [1, 2, 3, 4];
const fruits = ["Apple", "Banana", "Cherry"];

const b = fruits.findIndex((fruit) => /^C/.test(fruit));
console.log(b);

// 출력 결과 :
// 2
```

---

## includes

includes() 메서드는 **배열이 특정 요소를 포함하고 있는지 판별**합니다.

```javascript
const numbers = [1, 2, 3, 4];
const fruits = ["Apple", "Banana", "Cherry"];

const a = numbers.includes(3);
console.log(a);

const b = fruits.includes("changuk");
console.log(b);

// 출력 결과 :
// true
// false
```

---

## push, unshift

push() 메서드는 **배열의 끝에 하나 이상의 요소를 추가하고, 배열의 새로운 길이를 반환**합니다.
unshift() 메서드는 새로운 요소를 배열의 맨 앞쪽에 추가하고, 새로운 길이를 반환합니다.
push(), unshift() 모두 원본을 수정하기 때문에 주의해야합니다.

```javascript
const numbers = [1, 2, 3, 4];
const fruits = ["Apple", "Banana", "Cherry"];

numbers.push(5);
console.log(numbers);

numbers.unshift(0);
console.log(numbers);

// 출력 결과 :
// (5) [1, 2, 3, 4, 5]
// (6) [0, 1, 2, 3, 4, 5]
```

---

## reverse

reverse() 메서드는 **배열의 순서를 반전**합니다. 첫 번째 요소는 마지막 요소가 되며 마지막 요소는 첫 번째 요소가 됩니다. 원본을 수정하기 때문에 주의해야 합니다.

```javascript
const numbers = [1, 2, 3, 4];
const fruits = ["Apple", "Banana", "Cherry"];

numbers.reverse();
fruits.reverse();

console.log(numbers);
console.log(fruits);

// 출력 결과 :
// (4) [4, 3, 2, 1]
// (3) ['Cherry', 'Banana', 'Apple']
```

---

## splice

splice() 메서드는 **배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경**합니다. 원본을 수정하기 때문에 주의해야 합니다.
splice() 메서드의 첫 번째 인수는 배열의 변경을 시작할 인덱스입니다. 배열의 길이보다 큰 값이라면 실제 시작 인덱스는 배열의 길이로 설정됩니다. 음수인 경우 배열의 끝에서부터 요소를 세어나갑니다. 값의 절대값이 배열의 길이 보다 큰 경우 0으로 설정됩니다. 그리고 메서드의 두 번째 인수는 배열에서 제거할 요소의 수입니다. 세 번째 인수는 배열에 추가할 요소입니다. 아무 요소도 지정하지 않으면 splice()는 요소를 제거하기만 합니다.

```javascript
const numbers1 = [1, 2, 3, 4];
const numbers2 = [1, 2, 3, 4];
const numbers3 = [1, 2, 3, 4];
const fruits = ["Apple", "Banana", "Cherry"];

numbers1.splice(2, 1);
numbers2.splice(2, 0, 999);
numbers3.splice(2, 1, 99);
fruits.splice(2, 0, "Mango");
console.log(numbers1);
console.log(numbers2);
console.log(numbers3);
console.log(fruits);
// 출력 결과 :
// (3) [1, 2, 4]
// (5) [1, 2, 999, 3, 4]
// (4) [1, 2, 99, 4]
// (4) ['Apple', 'Banana', 'Mango', 'Cherry']
```
