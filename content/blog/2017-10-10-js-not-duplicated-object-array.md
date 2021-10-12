---
title: 자바스크립트 오브젝트 배열 중복 삭제하기
date: 2017-10-10 15:10:12
thumbnail: http://cfile2.uf.tistory.com/image/215FD64D56BD8AAD21359E
banner: http://cfile2.uf.tistory.com/image/215FD64D56BD8AAD21359E
categories: [Javascript]
tags: [Javascript, ES6, Underscore]
toc: true
---

React, Vue, Angular 등 프론트엔드 프레임웍을 쓰면 Data에 따라 UI가 그려지기 때문에 어느정도 앱이 완성궤도에 올라오면 비지니스 로직 핸들링과 돔에 대한 퍼포먼스를 개선하는 성능최적화 작업이 대부분이다.
<!-- more -->
가상 돔을 쓰는 프레임웍이면 데이터의 흐름 및 변화에 따라 자동으로 렌더링을 돌기 때문에 구조를 왠만큼 꼬거나 뎁스가 3~4뎁스를 넘기는게 아니라면 쾌적한 성능을 보장한다.

비지니스 로직데이터를 핸들링하는 일을 개선하다보면 필히 오브젝트 배열의 특정 키값을 기준으로 중복을 제거해야할 일이 생기는데 그에 대한 방법을 포스팅해본다.


### 언더스코어의 uniq 메소드를 이용하는 방법
```js
var data = [{'name': 'Amir', 'surname': 'Rahnama'}, {'name': 'Amir', 'surname': 'Stevens'}];
var non_duplidated_data = _.uniq(data, 'name'); 
```
[언더스코어](http://underscorejs.org/)를 사용한다면 위와 같이 구현할 수 있다.

개인적으로 언더스코어 라이브러리를 사용해 본적이 없고, 구현하는데 시간이 많이 걸리는 코드가 아닌 이상 라이브러리를 사용하지 않는 편이기 때문에 사용한 적이 없는 방법이다.

### ES6 문법 사용
```js
function getUniqueObjectArray(array, key) {
  return array.filter((item, i) => {
    return array.findIndex((item2, j) => {
      return item.key === item2.key;
    }) === i;
  });
}
```

코드가 단순하고 쉬우며 직관적이지만 **ES6**에서 지원하는 메소드이기 때문에 바벨로 트랜스파일을 해야하며, 경우에 따라 익스플로러를 지원해야할 경우 폴리필을 적용해야한다.

프로젝트의 환경에 따라 위의 코드를 적용할지 말지 결정해야한다.

### ES5 문법 사용
```js
function getUniqueObjectArray(array, key) {
  var tempArray = [];
  var resultArray = [];
  for(var i = 0; i < array.length; i++) {
    var item = array[i]
    if(temArray.include(item[key])) {
      continue;
    } else {
      resultArray.push(item);
      tempArray.push(item[key]);
    }
  }
  return resultArray;
}
```

가장 단순한 방법이다. 양이 제법 길긴 하지만 해당 펑션을 처음 보더라도 코드의 가독성이 어렵지 않은 편이기에 충분히 이해할수 있다.
ES6를 사용하지 않았기 때문에 비교적 크로스 브라우징 문제에 안전하다고 볼 수 있다 :)

### 정리하며
위의 나온 세 방법만이 정답은 아니다. 
문법적으로 또는 이론적으로 이해가 어려운 방법들은 제외했으며 **JSON.stringify**같은 메소드가 들어가는 방법은 성능에 좋지 않을것 같아 마찬가지로 제외하였다.
스택오버플로를 찾아보면 굉장히 다양한 방법이 있다. 참고하여 자신만의 메소드를 만들면 좋을것 같다.

---
참고문서
- https://stackoverflow.com/questions/36032179/remove-duplicates-in-an-object-array-javascript
- https://stackoverflow.com/questions/2218999/remove-duplicates-from-an-array-of-objects-in-javascript
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk5MDUzMTU1LC0xMDkzMDI3Mjg2LDE5OT
YxNTE4NzRdfQ==
-->