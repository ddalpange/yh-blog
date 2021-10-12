---
title: 자바스크립트 객체 복사하기
date: 2017-10-10 16:26:05
thumbnail: http://cfile2.uf.tistory.com/image/215FD64D56BD8AAD21359E
banner: http://cfile2.uf.tistory.com/image/215FD64D56BD8AAD21359E
categories: [Javascript]
tags: [Javascript, DeepCopy, ShallowCopy]
toc: true
---

#### 시작하기 전에

**A코드**
```js
let a = 1;
let b = a;
b = 2;
console.log(a, b);
```
**B코드**
```js
let a = { p : 1 };
let b = a;
b.p = 2;
console.log(a.p, b.p);
```
A코드와 B코드 두가지의 코드가 있다.
두 코드 모두 `b에 a를 대입하였다.`라고 생각하는가?
혹은
두 코드 모두 `b에 a를 복사하였다`라고 생각하는가?
<!-- more -->
결론부터 말하자면 **틀렸다**.
자바스크립트는 불변형의 데이터를 선언할 때 포인터와 값 모두 생성하지만,
오브젝트(배열)을 생성할 때에는 메모리 절약을 위해 포인터만 새로 할당할 뿐이다.

즉 A코드에서는 `b에 a를 복사하였다.`가 맞는것이고
B코드에서는 `b에 a를 대입하였다.`가 맞는 해석이 된다.
이제 B코드에서 `b에 a를 복사하였다`가 성립하도록 해보자.

#### 1. [Object.assign()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)을 이용

```js
function cloneObject(obj) {
  return Object.assign({}, obj);
}
```

**Object.assign**은 첫번째 인자로 들어오는 객체에 두번째 인자로 들어오는 객체의 프로퍼티들을 차례대로 덮어쓰기하여 반환하는 메소드이다.
여기서 주의할 점은 **Object.assign**은 프로퍼티들에 대한 참조를 덮어쓰기하기 때문에, 오브젝트 안에 오브젝트 또는 배열이 있을 경우 복사가 아닌 참조를 하게된다.
즉 객체를 `얕은 복사(Shallow Copy)`한다.

#### 2. JSON 객체의 메소드를 이용
```js
function cloneObject(obj) {
  return JSON.parse(JSON.stringify(obj));
}
```
**JSON.stringify**는 자바스크립트 오브젝트를 스트링 포멧으로 변환하는 메소드이다.
**JSON.parse**는 스트링 포멧을 자바스크립트 오브젝트로 변환하는 메소드이다.

스트링으로 변환하였다가 다시 오브젝트로 변환하기 때문에 이전 객체에 대한 참조가 없어지지만 JSON 메소드 자체가 성능면에서 다른 방법에 비해 굉장히 느리기 때문에 주의해야한다.
이 방법은 객체를 `깊은 복사(Deep Copy)`한다.

#### 3. 자바스크립트 재귀 사용
```js
function cloneObject(obj) {
    var clone = {};
    for(var i in obj) {
        if(typeof(obj[i])=="object" && obj[i] != null)
            clone[i] = cloneObject(obj[i]);
        else
            clone[i] = obj[i];
    }
    return clone;
}
```

오브젝트의 프로퍼티들을 순회하여 빈 오브젝트에 더한다. 그 과정에서 원본 오브젝트의 프로퍼티가 오브젝트일 경우 재귀적으로 함수를 실행한다.
이 방법은 객체를 `깊은 복사(Deep Copy)`한다.

#### 4. [Immutable.js](https://facebook.github.io/immutable-js/) 사용
```js
import { Map } from 'immutable';

let map = Map({a : 1});
let newMap = map;
newMap.set('a', 2);
console.log(map.get('a'));
console.log(newMap.get('a'));
```
페이스북에서 만든 오픈소스 라이브러리이다.
Immutable을 쓰게된다면 Array, Map 모두 이뮤터블하게 쓸 수 있게된다.
객체의 내부 값을 변경해도 원본 객체의 값은 변화하지 않고 새로운 객체를 배출한다는 뜻이다.
사용법이 비교적 간편하지만 처음 보는 사람일 경우 적응하는데 어려움이 있을 수 있다.

> 여담이지만 이 라이브러리를 사용할 때 로그를 찍을려면 **toJS**메소드를 사용하여 순수 자바스크립트 객체로 변환해야하는데 매번 까먹어서 불편했다 :(

#### 정리하며

사실 유지보수나 신규개발을 하면서 객체의 딥카피가 필요한 경우는 많이 없었다.
대부분 스왈로카피로 해결할 수 있으며 딥카피는 피하는것이 더 빠르고 직관적이기 때문이다.

주의할 점은 단순히 `=`를 통해 변수를 대입하는것과  `얕은 복사`는 엄연히 다르다는 것이다.
`=`와 `얕은 복사`와 `깊은 복사`의 차이만 알아가도 성공한것이 아닐까 ?
이것을 모르는 개발자들이 적지 않아서 조금 놀랐다.