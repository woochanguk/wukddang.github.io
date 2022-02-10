---
layout: post
image: /assets/img/ts/typescript.png
# accent_image:
#   background: url('/assets/img/javascript.png') center/cover
#   overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  TypeScript
invert_sidebar: false
categories:
  - frontend
  - ts
---

# TS - Basic Types

안녕하세요. 성장하는 것을 즐기는 `changuk`이라고 합니다. 현재 `fastcampus`에서 `frontend` 개발을 공부하고 있습니다. 강의를 들으면서 해온 것들을 작성하여 지식을 공유하고 또 제가 잊었을 때 다시 와서 볼 수 있도록 내용들을 정리하려고 합니다.

---

이번에는 `TypeScript`의 `Basic Types`에 대해 알아보겠습니다.

1. table of contents
{:toc .large-only}
---

## TypeScript Types vs JavaScript Types

TypeScript와 JavaScript의 Type이 어떻게 다른지를 알고 가는게 중요합니다.
기본적으로 TypeScript는 `Static Types`(정적 타입)이고, JavaScript는 `Dynamic Types`(동적 타입) 입니다.

|      언어      | TypeScript | JavaScript |
| :------------: | :--------: | :--------: |
|   정적 타입    |     O      |     X      |
|   동적 타입    |     X      |     O      |
| Type 설정 시점 |  개발 중   |   런타임   |

코드를 통해 더 자세히 비교해봅시다.

```javascript
function add(n1, n2) {
  if (typeof n1 != "number" || typeof n2 !== "number") {
    throw new Error("Incorrect Input!");
  }
  return n1 + n2;
}

const result = add(39, 28);
```

JavaScript는 런타임 상에서 n1, n2가 number가 아닐 때, throw를 호출하도록 합니다. 그러나 TypeScript에서는 그러한 과정이 필요없이, n1, n2에 number라는 Type을 지정해주어서, 런타임 상에서의 확인이 아닌, 개발 중에 직접 확인해줄 수 있습니다. 따라서 먼저 에러를 고쳐줄 수 있겠죠.

```typescript
function add(n1: number, n2: number) {
  return n1 + n2;
}

const result = add(39, 28);
```

---

## TypeScript Data Types

TypeScript에서 프로그램 작성을 위해 기본적으로 제공하는 Data Type입니다. **사용자가 만든 Type들도 결국 이 기본 자료형들로 나누어집니다.** 당연히, JavaScript 기본 자료형을 포함하게 됩니다. (superset)

- ECMAScript 표준에 따른 기본자료형 6가지가 있습니다.
  - Boolean
  - Number
  - String
  - Null
  - Undefined
  - Symbol (ECMAScript 6에 추가)
  - Array: object 형
- 또한, 프로그래밍을 도울 몇 가지 Type이 더 제공됩니다.
  - Any, Void, Never, Unknown
  - Enum
  - Tuple: object 형

---

## Primitive Types

Primitive Types이란 object와 레퍼런스 형태가 아닌, 실제 값을 저장하는 자료형을 의미합니다. TypeScript는 JavaScript 처리 방식*(프로토타입?)*을 이용하여 Primitive Type의 내장 함수를 사용할 수 있습니다.

- (ES2015 기준) 6가지
  - boolean
  - number
  - string
  - symbol (ES2015)
  - null
  - undefined

내장 함수 예제는 아래 코드를 보도록 하겠습니다.

```typescript
let name = "wuk";
name.toString();
```

- literal 값으로 Primitive Type의 Sub Type을 나타낼 수 있습니다.

```typescript
true;
("hello");
3.14;
null;
undefined;
```

- 또는 Primitive Type을 Wrapper 형태의 객체로 만들 수도 있습니다.

```typescript
new Boolean(false); // typeof new Boolean(false): "object"
new String("world"); // typeof new String("world): "object"
new Number(42); // typeof new Number(42): "object"
```

하지만 TypeScript에서는 **객체 형태로 Number, String, Boolean Type을 저장하는 것을 권장하지 않습니다.** **TypeScript의 핵심 primitive Types는 모두 소문자**입니다. Number, String, Boolean, Symbol 또는 Object 유형이 위에서 권장한 소문자 버전과 동일하다 생각할 수 있지만 **이러한 유형들은 primitives를 나타내지 않으며 타입으로 사용해서는 안됩니다.**

---

## boolean Type

TypeScript의 boolean Type에 대해 알아보겠습니다. boolean.ts파일을 만들고 다음 내용을 입력해보죠.

```typescript
let isDone: boolean = false;
isDone = true;
console.log(typeof isDone); // "boolean"
```

위 코드를 입력하고, JavaScript로 컴파일 해주면 됩니다. 그 후에 node boolean.js를 입력하면 위와 같이 잘 작동합니다.
여기서 대문자 Boolean과 비교해보도록 하겠습니다.

```typescript
let isOk: Boolean = true;
let isNotOk: boolean = new Boolean(true); // error
```

마지막 라인에서 error가 발생하게 됩니다. True 값을 가진 Wrapper 객체를 Primitive Type인 boolean에 할당하려고 하지만 다음과 같은 오류 메시지가 발생합니다.

> **Type 'Boolean' is not assignable to type 'boolean'. 'boolean' is a primitive, but 'Boolean' is a wrapper object. Prefer using 'boolean' when possible.**

---

## number Type

TypeScript의 number Type에 대해 알아보겠습니다. number.ts파일을 만들고 다음 내용을 입력해보죠. 다음 코드들 모두 TypeScript 내에서 정상작동합니다.

```typescript
let decimal: number = 6; // 10진수 리터럴
let hex: number = 0xf00d; // 16진수 리터럴
let binary: number = 0b1010; // 2진수 리터럴
let octal: number = 0o744; // 8진수 리터럴
let notANumber: number = NaN;
let underscoreNum: number = 1_000_000;
```

요약하자면,

- JavaScript와 같이, TypeScript의 **모든 숫자는 부동 소수점 갑**입니다.
- TypeScript는 16진수 및 10진수 리터럴 외에도, ECMAScript2015에 도입된 2진수 및 8진수를 지원합니다.
- NaN도 지원합니다.
- 1_000_000과 같은 표기가 가능합니다.

---

## string Type

TypeScript의 string Type에 대해 알아보겠습니다. 다른 언어에서와 마찬가지로 텍스트 형식을 참조하기 위해 `string Type`을 사용합니다. JavaScript에서와 마찬가지로, TypeScript는 문자열 데이터를 둘러싸기 위해 큰 따옴표(")나 작은 따옴표(')를 사용합니다.
<br>
예제를 코드로 작성해보겠습니다. string.ts 파일을 만들어주고 다음 내용을 입력해 보죠.

```typescript
let fullName: string = "Changuk Woo";
let age: number = 39;
let sentence: string = `Hello, My name is ${fullName}. 
I'll be ${age + 1} years old next month.`;
console.log(sentence);

// 출력 결과 :
// Hello, My name is Changuk Woo.

// I'll be 40 years old next month.
```

---

## symbol Type

TypeScript의 symbol Type에 대해 알아보겠습니다. 먼저, Symbol은 ECMAScript 2015에 추가되었습니다. 이 Symbol은 **new Symbol로 사용할 수 없고**, **Symbol을 함수**로 사용해서 symbol Type을 만들어낼 수 있습니다.

```typescript
console.log(Symbol("foo") === Symbol("foo"));

// 출력 결과 :
// false
```

같은 함수에 같은 인자를 넣었지만 출력 결과는 false로 나타나고, 다른 symbol인 것을 확인할 수 있습니다.
정리하면 다음과 같습니다. <br>
Symbol은,

- Primitive Type의 값을 담아서 사용합니다.
- 고유하고 수정 불가능한 값으로 만들어줍니다.
- 그래서 주로 접근을 제어하는 데 쓰는 경우가 많았습니다.

예제로 확인해봅시다.

```typescript
console.log(Symbol("foo") === Symbol("foo"));

const sym: symbol = Symbol(); // 아무것도 입력하지 않아도 생성됩니다.

const obj = {
  [sym]: "value",
};

obj[sym];
```

배열에서 "value"값을 찾으려 한다면, `obj["sym"]`이 아닌, 다음과 같이 `obj[sym]`이라고 입력해야 가능합니다.

---

## null & undefined Type

TypeScript의 null & undefined Type에 대해 알아보겠습니다.
TypeScript에서 undefined와 null은 실제로 각각 undefined 및 null이라는 Type을 가집니다. 그러나 나중에 살펴볼 void와 마찬가지로 그 자체로는 그다지 유용하지 않습니다.

```typescript
// 이 변수들에 할당할 수 있는 것들은 거의 없습니다.
let u: undefined = undefined;
let n: null = null;
```

tsconfig.json파일에서 설정을 하지 않으면, undefined나 null은 모든 Type들의 subtype이 될 수 있습니다. 이렇게 되면 문제가 많이 발생할 수 있기 때문에, 컴파일 옵션에서 `--strictNullChecks`를 사용하면 null과 undefined는 void나 자기 자신들에게만 할당할 수 있습니다. ,<br>

이 경우, null과 undefined를 할당할 수 있게 하려면, `union type`을 이용해야 합니다. 아래 코드를 보시죠

```typescript
let MyName: string = null; // error 발생
let u: undefined = null; // error 발생

let v: void = undefined; // OK

let union: string | null = null; //union type!

union = "wuk";
```

string type과 null type 모두를 가질 수 있다고 정의해주는 union type 입니다.

#### null in JavaScript

JavaScript에선 null 이라는 값으로 할당된 것을 null이라고 합니다. 이는, 무언가가 있는데 사용할 준비가 덜 된 상태라고 정의할 수 있겠습니다. null type은 null 값만 가질 수 있습니다. 또 **런타임에서 typeof 연산자를 이용하여 확인하면, object라고 출력됩니다.**

```typescript
let n: null = null;

console.log(n); // null
console.log(typeof n); // object
```

#### undefined in JavaScript

JavaScript에서 값을 할당하지 않은 변수는 undefined 라는 값을 가지게 됩니다. 이는 무언가가 아예 준비가 되지 않은 상태로서, object의 property가 없을 때도 undefined 입니다. 또 **런타임에서 typeof 연산자를 이용하여 확인하면, undeinfed로 출력됩니다.**

```typescript
let u: undefined = undefined;

console.log(u); // undefined
console.log(typeof u); // undefined
```

---

## object Type

TypeScript의 object Type에 대해 알아보겠습니다. object는 다음과 같이 생성할 수 있습니다.

```typescript
// create by object literal
const person1 = { name: "changuk", age: 26 };
// person1의 type은 "object"가 아닙니다
// person1은 "{ name: "changuk", age: 26}" Type 입니다.

// create by Object.create
const person2 = Object.create({ name: "changuk", age: 26 });
```

object 리터럴로 생성한 person1 상수는 object type이 아닙니다. 코드에 나와있는대로 **"{ name: "changuk", age: 26}" Type**입니다. 내장 전역 객체 Object로 생성하면, {}안의 object 리터럴을 통해 객체를 만들어내게 됩니다. 그럼, {}안에는 어떤 Type이 들어가야 할까요?<br>
실제로 Object.create()라는 함수안에는 object type 또는 null type을 입력하게 되어 있습니다. 여기서 object type이란, object 리터럴로 구체적인 타입이 있는 것이 아니라 primitive type이 아닌 것이라는 의미를 가지고 있습니다. <br>다음은 **object.create()**의 설명입니다.

> **(method) ObjectConstructor.create(o: object \| null): any (+1 overload)**

그러니, 아래와 같은 코드는 동작하지 않겠지요? 이런 상황을 막아주기 위해 object type이 존재하는 것입니다.

```typescript
const person2 = Object.create(39);
```

정리하자면,

- object
  - **primitive type이 아닌 것**을 나타내고 싶을 때 사용하는 타입
- **non-primitive type**

  - **not number**
  - **not string**
  - **not boolean**
  - **not bigint**
  - **not symbol**
  - **not null**
  - **not undefined**

<br>
그럼 몇 가지 코드를 보면서 이해를 해봅시다.

```typescript
let obj: object = {};

obj = { name: "Changuk" };

obj = [{ name: "Changuk" }];

obj = 39; // Error

obj = "Mark"; // Error

obj = true; // Error

obj = 100n; // Error

obj = Symbol(); // Error

obj = null; // Error

obj = undefined; // Error
```

자, 그럼 다음과 같은 함수도 작성할 수 있을 것입니다.

```typescript
declare function create(o: object | null): void;

create({ prop: 0 });
create(null);
create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error

// Object.create
Object.create(0); // Error
```

---

## Array Type

TypeScript의 Array Type에 대해 알아보겠습니다. JavaScript에서와 마찬가지로, 같은 Type의 요소들을 모아놓은 자료형을 의미합니다. 주의해야 할 점은, **Array 안의 요소들은 전부 같은 Type이라는 것입니다.** 하나의 Type으로 묶을 수 없는 것은 Array가 아니라는 뜻이지요.

#### Array

- 원래 JavaScript에서 array는 객체입니다.
- TypeScript에서 Array를 사용하는 방법은 다음과 같습니다.
  - Array \<type\>
  - type [ ]

웬만해서는 첫 번쨰 방법을 사용하여 작성하는것 추천합니다.

```typescript
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

그럼 만약, Array 안에 다른 Type도 넣고 싶다고 한다면 어떻게 해주어야 할까요? 다음과 같이 작성해주시면 됩니다.

```typescript
let list: (number | string)[] = [1, 2, 3, "4"];
```

그렇다면, Array 중에 한번은 number, 한번은 string이 나오는 그런 자료가 있지 않을까요? 그럴 땐 어떻게 다뤄주어야 할까요? <br>
TypeScript는 고맙게도 그러한 자료를 다룰 수 있는 Type이 있습니다. 다음 단락에서 계속 알아보겠습니다.

---

## Tuple Type

TypeScript의 Tuple Type에 대해 알아보겠습니다.

```typescript
let x: [string, number];

x = ["hello", 39];

x = [10, "hi"]; // Error
x[2]; // Error: Tuple type '[string, number]' of length '2' has no element at index '2'.
```

Tuple Type에는 길이가 존재합니다. 따라서 `x[2]`이렇게 입력하여도, index 2 이후에는 값을 지정할 수 없도록 type이 undefined 되어 있습니다. 에러 메시지를 보시죠

> **Tuple type '[string, number]' of length '2' has no element at index '2'.**

계속해서 예제를 보시겠습니다.

```typescript
const person: [string, number] = ["changuk", 39];

const [first, second, third] = person;
```

Tuple Type을 정의하고, Destructuring을 수행하였습니다.
first에는 string, second에는 number가 할당되었지만, third를 입력하자 위의 예제와 동일한 에러가 발생하였습니다.

> **Tuple type '[string, number]' of length '2' has no element at index '2'.**

정리하자면 Tuple은,

- 길이가 정해져있고,
- 앞 뒤의 Type이 정확하게 할당되고, 다를 수 있습니다.

---

## any Type

TypeScript의 Tuple Type에 대해 알아보겠습니다. any의 뜻 처럼 정말로 어떤 것이든 들어올 수 있는 경우가 있고, 아닐 수도 있습니다. 그러나 개발자가 무심코 `any`를 사용하게 되면 전체적인 Type system이 문제를 일으킬 수도 있습니다. 그렇기 때문에, any를 정확하게 알고 쓰는것이 중요합니다.<br>
예제를 살펴보겠습니다.

```typescript
function returnAny(message): any {
  console.log(message);
}

const any1 = returnAny("리턴은 아무거나");
```

returnAny의 결과는 Type이 `any`라고 출력됩니다. 결과가 무엇인지 규정되어있지 않고 무엇이든 들어올 수 있기 때문에, 어떤일을 하든 제한을 주지 않게됩니다. 밑의 코드처럼 사용할 수도 있다는 의미입니다. 어떠한 Type적인 에러를 나타내지 않습니다.

```typescript
any1.toString();
```

any Type은 어떤것을 할 지 모른다는 의미이죠. 모른다는 것은 아무것도 할 수 없다는 것이 아닌, 어떤것이든 할 수 있다는 의미입니다. 이런 경우, 최대한 any를 쓰지 않는것이 Type system을 안전하게 유지하고 코드를 사용하는 사람들에게 정확한 가이드를 줄 수 있는 방법입니다.

하지만 any를 쓸 수 밖에 없는 곳이 있습니다. 우리가 만든 함수를 다시 보겠습니다.

```typescript
function returnAny(message): any {
  console.log(message);
}

const any1 = returnAny("리턴은 아무거나");
```

우리 함수는 내부에서 log만 출력하고 있습니다. 이러한 경우, message는 number, string일 규정이 필요가 없습니다. 그저 log로 표현되는 기능만 하는 역할이기에, 위 함수에서의 **message는 any일 수 있습니다.**<br>
결론적으로 인자로 들어오는 message가 어떤 타입이든 가능하다고 추론할 수 있겠습니다. 이럴 때 개발자가 `message: any`라고 입력해주면 error가 사라지게 됩니다. 이러한 기능을 tsconfig.json안에 있는 컴파일 옵션, `"noImplicitAny": true`라고 합니다.
<br>
다음과 같이 정리할 수 있겠습니다.

#### any

- 어떤 타입이어도 상관없는 타입입니다.
- 이것을 최대한 쓰지 않는것이 핵심입니다.
- 왜냐하면 컴파일 타임에 타입 체크가 정상적으로 이뤄지지 않기 때문입니다.
- 그래서 컴파일 옵션 중에는 any를 써야하는데 쓰지 않으면 오류를 출력하는 옵션이 존재합니다.
  - **noImplicitAny**

any는 개체를 통해서 계속해서 전파됩니다. 편리하다고 any를 남용하면 Type 안전성을 잃는 대가로 돌아오게 됩니다. Type 안전성은 TypeScript를 사용하는 주요 동기 중 하나이며 정말로 필요하지 않은 경우엔, any를 사용하지 말아야 겠습니다.

<br>
any가 전파되는 것을 예제를 통해 보도록 하겠습니다.

```typescript
let looselyTyped: any = {};

const d = looselyTyped.a.b.c.d;
```

위와 같이 아무런 의미가 없는 잘못된 코드라도 에러가 출력되지 않게 됩니다.<br>
사실 any라는 Type은 존재할 수 밖에 없는 Type입니다. 예를 들어 API를 통해 동적인 데이터가 들어올 수도 있기 때문이죠. 그러므로 어떤 지점에는 any를 사용했다면, 그 누수를 막는 지점을 만들어내는것이 굉장히 중요합니다. 코드를 살펴보죠.

```typescript
function leakingAny(obj: any) {
  const a = obj.num;
  const b = a + 1;
  return b;
}

const c = leakingAny({ num: 0 });
c.indexOf("0");
```

위와 같은 코드에서는 a, b, c는 모두 any Type이 됩니다. 또 c에 `indexOf()`를 수행해주어도 어떠한 에러가 출력되지 않습니다. 이렇게 되면 JavaScript를 사용하는 것과 다를바가 없죠. 다음과 같이 수정해줍시다.

```typescript
function leakingAny(obj: any) {
  const a: number = obj.num;
  const b = a + 1;
  return b;
}

const c = leakingAny({ num: 0 });
c.indexOf("0");
```

a의 Type을 number로 해주는 순간 이제 누수가 막힙니다. b, c의 Type도 number가 되고, 이젠 `c.indexOf("0")`에서 에러가 출력됩니다. <br>
이렇게 누수를 막아주는것도 방법일 수 있지만, 사실 위의 코드에서 함수에 obj를 사용하기 전에 TypeGuard같은 형식으로 Type을 분리하여 지정할 수 있는 **unknown**형식을 사용하는게 추천됩니다.

---

## unknown Type

TypeScript의 unknown Type에 대해 알아보겠습니다. unknown Type은 any Type의 불안정한 요소를 조금이나마 해소하고자 하는 대체제라고 생각해주시면 되겠습니다.

간략히 설명해 보겠습니다.

- **응용 프로그램을 작성할 때 모르는 변수의 Type을 묘사해야 할 수도 있습니다.**
- **이러한 값은 동적 콘텐츠*(예: 사용자로부터, 또는 우리 API)* 의 모든 값을 의도적으로 수락하기를 원할 수 있습니다.**
- **이런 경우, 컴파일러와 미래의 코드를 읽는 사람에게 이 변수가 무엇이든 될 수 있음을 알려주는 Type을 제공하기를 원하므로 unknown Type을 제공합니다.**

<br>
코드를 보며 이해해 보겠습니다. 다음과 같이 입력해봅시다.

```typescript
declare const maybe: unknown;

const aNumber: number = maybe; // Error
```

에러가 출력됩니다. 다음과 같은 메시지가 출력이 되는군요.

> **Type 'unknown' is not assignable to type 'number'.**

그렇다면 다음과 같이 코드를 작성해보겠습니다.

```typescript
declare const maybe: unknown;

if (maybe === true) {
  const aBoolean: boolean = maybe;
}
```

if 문 안에서는 maybe가 true이기 때문에, aBoolean이라는 상수에 값을 대입해줄 수 있습니다.

또 다음 예제를 살펴봅시다.

```typescript
// typeof typeguard
if (typeof maybe === "string") {
  const aString: string = maybe;
}
```

이렇게 작성하면 **에러가 출력되지 않습니다.** typeof에서 maybe가 오직 string Type인 경우에만 if 문이 실행되기 때문이죠. 위와 같이 maybe가 특정 조건인 경우에만 정상 작동하도록 하는 코드를 **TypeGuard**라고 부릅니다. <br>
정리해보면 다음과 같습니다.

#### unknown

- TypeScript 3.0 버전부터 지원
- any와 짝으로 any 보다 Type-safe한 Type
  - any와 같이 아무 것이나 할당할 수 있다.
  - 컴파일러가 Type을 추론할 수 있게끔 Type의 유형을 좁히거나
  - Type을 확정해주지 않으면 다른 곳에 할당할 수 없고 사용할 수 없다.
- unknown Type을 사용하면 runtime error를 줄일 수 있을 것이다.
  - 사용 전에 데이터의 일부 유형의 검사를 수행해야 함을 알리는 API에 사용할 수 있을 것이다.

---

## never Type

TypeScript의 never Type에 대해 알아보겠습니다. 일반적으로 return에 사용됩니다. 코드로 알아보시죠. return에 사용되기 때문에 함수를 만들어보도록 하겠습니다.

```typescript
function error(message: string): never {
  throw new Error(message);
}
```

never를 return한다는 것은 아무것도 return하지 않는다는 의미입니다. 그러니 함수의 body부분이 끝나지 않아야 합니다. (아니면 에러가 출력) throw를 하게 되면 그 밑의 라인으로는 내려오지 않게 되고, 위의 함수는 throw하고 끝나버리기 때문에 어떠한 형태도 return되지 않도록 never라는 키워드를 사용한다고 이해하시면 되겠습니다.

```typescript
function error(message: string): never {
  throw new Error(message);
}

function fail() {
  return error("failed");
}
```

위와 같이 error함수를 사용하면서 return을 하게되는 경우에도 TypeScript는 function fail()을 **never Type**으로 추론하게 됩니다.

```typescript
function infiniteLoop(): never {
  while (true) {}
}
```

무한루프도 다음과 같이 지정해줄 수 있겠습니다.<br>
정리해보겠습니다.

#### never

- never Type은 모든 Type의 subtype이며, 모든 Type에 할당할 수 있습니다.
- 하지만, never에는 어떤 것도 할당할 수 없습니다.
- any조차 never에게 할당할 수 없습니다.
- 잘못된 Type을 넣는 실수를 막고자 할 때 사용하기도 합니다.

```typescript
let a: string = "hello";

if (typeof a !== "string") {
  a;
}
```

위 코드에서 a는 never Type이 됩니다.

```typescript
declare const b: string | number;

if (typeof b !== "string") {
  b;
}
```

위 코드에서 b는 number Type이 됩니다.

---

## void Type

TypeScript의 void Type에 대해 알아보겠습니다. `void`는 어떠한 Type도 가지지 않는 빈 상태를 의미합니다. 값은 없고 Type만 있어서 void라는 값을 쓸 수는 없습니다. 일반적으로 어떤 변수에 void를 annotation하는게 아닌, 값을 반환하지 않는 일종의 undefined를 return하는 상태라고 볼 수 있습니다. 그런 경우 return Type으로만 사용하고 다른 경우는 거의 사용하지 않습니다.<br>
원래 다른 프로그래밍 언어들에서는 void를 return Type으로 많이 사용하지만, TypeScript에서는 undefined라는 같은 의미의 상태가 있기 때문에 void라는 값을 가질 필요가 없었습니다. 그러나 다른 언어에서 사용하고 있기 때문에 의미상 void라는 Type을 가져온 것 뿐입니다.
<br>
코드를 보며 이해해보죠

```typescript
function returnVoid(message: string) {
  console.log(message);
}
```

message를 string으로 받아 출력만 하는 함수입니다. 이 경우, 보통은 Type 추론에 의해 returnVoid 함수의 return Type은 추론이 됩니다.

> **function returnVoid(message: string): void**

다음과 같이 return만 적고 아무런 값을 적지 않아도, return Type은 void로 추론이 됩니다.

```typescript
function returnVoid(message: string) {
  console.log(message);
  return;
}
```

이번에는 void 함수를 값으로 할당해 보도록 하겠습니다. 어떻게 될까요? 동일하게 void가 할당이 됩니다.

```typescript
const r = returnVoid("리턴이 없다.");
```

> **const r: void**

void를 가지고 다른 작업에 활용할 수 없다는 의미로 받아들이시면 됩니다. 상수 r에는 다른 값을 할당할 수도 없습니다.

```typescript
const r: undefined = returnVoid("리턴이 없다."); // Error
```

void로 지정된 함수는 return을 가지고 아무것도 하지 않겠다는 명시적인 의미입니다. 그리고 return 키워드 뒤에 들어갈 수 있는 유일한 값은 `undefined` 뿐입니다.

```typescript
function returnVoid(message: string): void {
  console.log(message);
  return undefined;
}
```
