---
title: 자바스크립트 코딩 인터뷰 정리
categories: [Javascript]
date: 2017-10-26 23:53:22
tags: [Javascript, Interview]
thumbnail: http://cfile2.uf.tistory.com/image/215FD64D56BD8AAD21359E
banner: http://cfile2.uf.tistory.com/image/215FD64D56BD8AAD21359E
toc: true

---

### 자바스크립트 sort 메소드

```js
let array = [20, 1, 3];
array = array.sort();
console.log(array);
```

`위의 코드가 프로그래머가 원하는 결과값을 가져올것이라 생각하는가?`
<!-- more -->
답은 틀렸다. **[1, 3, 20]**이 아닌 **[1, 20, 3]**이라는 결과값이 나온다.
자바스크립트의 sort메소드는 배열을 순회할 떄 배열요소의 **String** 으로 바꾸기 떄문이다.
위의 코드를 정상적으로 작동하도록 바꾸려면 아래와 같이 코드를 작성해야한다.

```js
let array = [20, 1, 3];
array = array.sort((a, b) => {
  return parseInt(a) - parseInt(b);
});
console.log(array);
```

### 자바스크립트 async 테스트
```js
console.log(0);
setTimeout(() => { console.log(1)}, 100);
setTimeout(() => { console.log(2)}, 0);
console.log(3);
```
답은 **0, 2, 3, 1**이 아니라 **0, 3, 2, 1**의 순서로 찍힌다.
자바스크립트는 기본적으로 **비동기**로 작성이 되는데 setTimeout의 시간을 0으로 준다 하더라도 처리하는 시간이 발생하여 코드 아랫 라인이 같이 실행되기 때문이다.

### 변수 호이스팅
```js
var a = 1;
function abc() {
  if(a) {
    var a = 0;
    console.log(a);
  }
  console.log(a);
}
abc();
```

가장 큰 충격을 받았던 문제이다.
당연히 함수 abc의 a가 undefined이기 때문에 외부 스코프에 있는 a를 참조하여  **0, 0**이라는 결과값이 나올줄 알았는데 abc()안의 if문 안에서 **var**키워드로 선언되어있는 지역변수 a가 undefined로 올라가기 때문에 따로 if분기를 타지 않고 **undefined**결과값을 뱉어낸다.
당연히 a가 undefined니 스코프체인을 타서 바깥 a를 찾을줄 알았는데 해당 함수 스코프에서 끝나버리니 당황했다.

정리를 해보니 쉬우면서도 틀린문제들이 많다.
다 안다고 생각하는 것이 제일 무섭다. 열심히 공부하자.