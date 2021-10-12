---
title: 임포트와 익스포트
date: 2017-10-03 22:46:44
thumbnail: http://cfile2.uf.tistory.com/image/215FD64D56BD8AAD21359E
banner: http://cfile2.uf.tistory.com/image/215FD64D56BD8AAD21359E
categories: [Javascript]
tags: [Javascript]
toc: true
---

자바스크립트 프로젝에서 상수를 관리하기 위한 방법을 설명하는 글.

<!-- more -->

###  Import, Export

기존 프로젝트 유지보수를 진행하다 보면 "AS20342"과 같은 특정한 코드값들이 있다. 그러한 코드값들은 전부 공통으로 모아 상수로 빼는게 상책인데, import, export를 어떻게 해야할지 감이 오질 않았다. 아래 내용은 공통함수를 뺄때까지의 과정이다.


#### 1. Obejct형태로 Export한다.

```javascript
const obj = {
  name: "ddalpange",
  password: 123456
}

export default obj;
```


-> 나쁘지 않은 방법이다. 하지만 오브젝트를 const로 선언한다면 오브젝트의 프로퍼티 값은 const가 아니기 때문에 obj.name  = "puppy"와 같이 값 자체를 바꿀 수 있다. 엄밀히 따지자면 상수가 아닌 셈이다. (Object.freeze()라는 것을 사용하면 될 수도 ??)

#### 2. Class 형태로 Export한다.

```javascript
class test {
  init() {
    this.obj = { "name" : "ddalpange"}
  };
  getName(keyName) {
    if(this.obj.hasOwnProperty(keyName) {
      this.obj[keyName];
    } else {
      return null;
    }
  }
}
```

-> 이상하다. 그저 변수 하나를 뺴기 위하여 클래스를 선언한다는 것은 값어치가 없는 일이다.

#### 3. const 선언을 할 떄 마다 export를 붙여준다.

```javascript
export const a = "1";
export const b = "2";
export const c = "3";
```
-> 이 방법이 제일 좋은 방법인 것 같다. 하지만 위의 방법으로 Export하여 사용할려면 Import의 방식이 다르다.

```javascript
Import * as test from {SRC}
```

위와 같이 Import하면 test.a, test.b 와 같이 접근할 수 있다.

test라는 오브젝트의 프로퍼티에 접근한다고 생각하면 쉬울 듯 하다 :)

별것 아닌것 같아 보이지만 찾느라 고생을 많이 한 것 같다.

프로젝트 폴더구조가 알아보기 쉽지않고, import, export 경로 찾기도 매우 어려운데

나중에 폴더마다 index.js를 만들어 import하기 쉽게 만들어야겠다. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI0MTU5NTUyNF19
-->