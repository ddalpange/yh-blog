---
title: 자바스크립트 클로져
date: 2017-10-03 22:44:53
thumbnail: http://cfile2.uf.tistory.com/image/215FD64D56BD8AAD21359E
banner: http://cfile2.uf.tistory.com/image/215FD64D56BD8AAD21359E
categories: [Javascript]
tags: [Javascript, Closure]
toc: true
---

자바스크립트의 주요 개념중 하나인 클로져에 대해 자세히 알아보자.

<!-- more -->

**클로져**

클로져는 독립적인 (자유) 변수 (지역적으로 사용되지만, 둘러싼 범위 안에서 정의된 변수)를 참조하는 함수들이다. 
다른 말로 해석하면, 
이 함수들은 그들이 생성된 환경을 **기억**한다.

**한가지 의문**

클로져는 지역적으로 선언되었지만, 해당 함수가 선언된 블럭 안에 있는 독립된 변수들을 참조할 수 있는 함수라고 하였다. 
위 문장에서만 보면 자바스크립트의 모든 함수는 해당 블럭의 변수를 참조할 수 있음으로 자바스크립트의 모든 함수는 클로져라 할 수 있다. 
하지만 과연 자바스크립트의 모든 함수는 클로져일까 ??


```javascript
function init() {
  var name = "Mozilla";
  
  function displayName() { // displayName()은 내부 함수인, 클로져다
    alert(name); // 부모 함수에서 선언된 변수를 사용한다
  }

  displayName();
}

init();
```
`displayName`은 자신이 선언된 환경을 기억하였다. 그렇다면 `displayName`은 클로져라고 부를 수 있는가 ?


```javascript
function makeFunc() {
  var name = "Mozilla";

  function displayName() {
    alert(name);
  }

  return displayName;
}

var myFunc = makeFunc();
myFunc();
```
`myFunc` 를 실행했을 때 `makeFunc`에 대한 참조가 없음에도 불구하고 `makeFunc`의 지역변수인 `name` 변수에 정상적으로 접근할 수 있다. 이상하지 않은가?

`displayName`은 자신이 선언된 스코프에서 벗어나 global 환경에서 초기화되었다.
스코프 탐색은 실행스택과는 관련이 없는 `makeFunc`를 거쳐갔으며 `displayName`의 외부 스코프는 global이 아닌 `makeFunc`의 스코프이다. 

`myFunc`가 글로벌 환경에서 초기화 되더라도 리턴된 `dispalyName`의 스코프체인은 `displayName` -> `makeFunc` -> `global` 순으로 형성된다. 

즉 초기화되는 위치와 관계없이 해당 함수가 **선언**된 곳에서 스코프를 형성한다는 뜻이다.

`myFunc`에 null을 할당하지 않으면 가비지콜렉터가 `makeFunc`의 메모리를 해제하지 않기 때문에 클로져를 사용한다면 별도로 꼭 null을 할당해줘야한다.

```javascript
function count() {
    var i;
    for (i = 1; i < 10; i ++) {
        setTimeout(function timer() {
            console.log(i);
        }, i*100);
    }
}
count();
```

1 ~ 10까지 1씩 증가하여 출력하는 코드를 원했지만 결과는 기대와 달리 10이 9번 출력된다. 
0.1초동안 i는 10이 되었기 때문이며 클로져함수 timer에서 외부 스코프인 `count`의 변수인 `i`에 직접 접근하여 출력하였기 때문이다. 
해당 코드를 원하는 결과값으로 바꾸기 위해 어떻게 해야할까 ?

**내부 스코프를 하나 더 추가하는 방식.**

```javascript
function count() {
    var i;
    for (i = 1; i < 10; i += 1) {
        (function(countingNumber) {
            setTimeout(function timer() {
                console.log(countingNumber);
            }, i * 100);
        })(i);
    }
}
count();
```

**블록 스코프를 이용하는 방식**

```javascript
function count() {
    'use strict';
    for (let i = 1; i < 10; i += 1) {
        setTimeout(function timer() {
            console.log(i);
        }, i * 100);
    }
}
count();
```

**느낀 점**
클로져는 재미있는 개념이다. 클로져란 개념을 이해하기 위해서 11개 정도의 글을 정독하였는데, 사람마다 이해하는 클로져의 개념이 다 똑같은것 같지는 않다.
최대한 다양한 관점에서 클로져를 바라보고 글을 쓸려고 노력하였는데 결국 나랑 비슷한 사람의 글을 거의 베끼다시피 한것 같다.
언제나 느끼지만 글쓰기는 참 어렵다.


**참고**

1. http://blog.javarouka.me/2012/01/javascripts-closure.html
2. http://meetup.toast.com/posts/86
3. http://unikys.tistory.com/309
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg1Njg2ODQ5Nl19
-->