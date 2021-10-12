---
title: RxJS란 무엇인가?
date: 2018-08-21 14:51:12
thumbnail: https://cdn-images-1.medium.com/fit/t/1600/480/1*gD37OB2-PtMqZdk3X1YnEQ.png
tags: [RxJS, Observable, Observer, Subscription, Subject]
categories: [RxJS]
---

`Observable`을 사용하여 비동기 및 이벤트 기반의 프로그램을 작성하기 위한 라이브러리이다. 동기/비동기/이벤트 등 다양한 코드를 동일한 인터페이스로 작성할 수 있다는 점이 매우 인상적이다. `RxJS`는 *Iterator Pattern*과 *Observer Pattern*을 결합하여 이벤트들을 관리하기 위한 효울적인 방법을 제공한다. `RxJS`의 주요 개념은 다음와 같다.

<!-- more -->

1. ``Observable``: 미래에 발생할 이벤트, 값들을 모아놓은 컬렉션이다.
2. ``Observer``: `Observable`이 배달한 값을 읽을 수 있도록 하는 콜백들의 컬렉션이다.
3. `Subscription`: `Observable`의 실행을 나타낸다. 기본적으로 `Observable`의 실행 취소에 적합하다.
4. `Operators`: 함수형 프로그램을 제공하기 위한 순수 함수들의 모음이다.
5. `Subject`: `Observer`이자 `Observable`이다. 다수의 `Observable`에 브로드캐스팅 할 수 있는 유일한 방법이다.
6. `Scheduler`: `setTimeout`, `requestAnimationFrame`과 같은 비동기 함수의 동시성을 제어할 수 있다.

### 예제

#### 버튼의 이벤트 리스너

일반적으로 이벤트 리스너를 만드는 코드는 다음과 같다.

```javascript
const button = document.querySelector('button');
button.addEventListener('click', () => console.log('Clicked!'));
```

RxJS를 쓴다면 이것을 `Observable`로 변환 할 수 있다.

```javascript
const { fromEvent } = rxjs;

const button = document.querySelector('button');
fromEvent(button, 'click')
  .subscribe(() => console.log('Clicked!'));
```


#### 순수성

RxJS는 순수함수를 제공하기 때문에 넘어오는 값은 모두 독립적(불변)이다.
불변이라는것은 에러가 날 확률이 적어진다는 것과 같다.
 

일반적인 가변 변수를 사용하는 코드이다.

```javascript
var count = 0;
var button = document.querySelector('button');
button.addEventListener('click', () => console.log(`Clicked ${++count} times`));
```

RxJS를 쓴다면 변수를 불변적으로 관리할 수 있다.

```javascript
const { fromEvent } = rxjs;
const { scan } = rxjs.operators;

const button = document.querySelector('button');
fromEvent(button, 'click').pipe(
  scan(count => count + 1, 0)
)
.subscribe(count => console.log(`Clicked ${count} times`));
```

`scan`은 배열의 `reduce`와 유사하게 동작한다.


#### 흐름

RxJS는 다양한 오퍼레이터를 통해 이벤트흐름을 제어할 수 있다.

그 예로 1초가 지나야 사용자의 클릭을 허용하는 코드가 있다.

```javascript
var count = 0;
var rate = 1000;
var lastClick = Date.now() - rate;
var button = document.querySelector('button');
button.addEventListener('click', () => {
  if (Date.now() - lastClick >= rate) {
    console.log(`Clicked ${++count} times`);
    lastClick = Date.now();
  }
});
```

이것을 RxJS를 이용한다면 다음의 코드로 바꿀 수 있다.

```javascript
const { fromEvent } = rxjs;
const { throttleTime, scan } = rxjs.operators;

const button = document.querySelector('button');
fromEvent(button, 'click').pipe(
  throttleTime(1000),
  scan(count => count + 1, 0)
)
.subscribe(count => console.log(`Clicked ${count} times`));
```

`throttleTime` 외에도 `delay`, `debounceTime`, `take`, `takeUntil`, `distinct`, `distinctUntilChanged` 등 엄청나게 많은 오퍼레이터를 제공한다

#### 유연한 값

구독이 일어나기 전 오퍼레이터들을 이용하여 값들을 미리 변환시킬 수 있다.

클릭할 때마다 마우스의 X Position을 더해주는 코드는 아래와 같다.

```javascript
let count = 0;
const rate = 1000;
let lastClick = Date.now() - rate;
const button = document.querySelector('button');
button.addEventListener('click', (event) => {
  if (Date.now() - lastClick >= rate) {
    count += event.clientX;
    console.log(count)
    lastClick = Date.now();
  }
});
```

이것을 RxJS로 쓴다면 아래와 같다.

```javascript
const { fromEvent } = rxjs;
const { throttleTime, map, scan } = rxjs.operators;

const button = document.querySelector('button');
fromEvent(button, 'click').pipe(
  throttleTime(1000),
  map(event => event.clientX),
  scan((count, clientX) => count + clientX, 0)
)
.subscribe(count => console.log(count));
```

이 밖에도 `pluck`, `pairwise`, `sample`과 같은 다양한 오퍼레이터를 지원한다.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYxMjQzMzkwNywtMjA1MDg1MzQ4MiwtMT
U0MTI3NTgwMF19
-->