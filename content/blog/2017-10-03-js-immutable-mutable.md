---
title: 자바스크립트 불변과 가변
date: 2017-10-03 22:48:30
categories: [Javascript]
tags: [Javascript]
toc: true
---

참조타입, 원시타입, 불변형, 가변형 등 자료형의 차이에 대해 알아봅시다.

<!-- more -->


#### 참조타입, 원시타입? 불변형, 가변형?

##### 서론

포인트 관련 항목 페이지를 수정하면서 이상한 일이 일어났습니다.

사용자가 원하는 날짜만을 이용하여 필터링을 하던 도중, 필터링한 데이터를 보여주고 

전체데이터를 다시 보여주지 않고 있었습니다.

그 문제는 필터링한 변수와 원본 데이터 변수의 포인터가 서로 같은곳을 바라보고 있어 

원본데이터 자체를 수정하고 있었던 것이었습니다.

이 문제를 해결하기 위하여 더 자세히 알아봅시다.

##### 알아봅시다.

자바스크립트의 변수 타입을 불변형과 가변형으로 나눠 보겠습니다.

원시타입, 참조타입으로 나누기도 하지만, 개념을 따져보면 전부 비슷비슷한 말들입니다.

##### 불변형(Primary Type).

1. 값을 변경할 수 없다.

2. String, Number, Boolean

3. 값을 변경하기 위하여 다음의 과정을 거친다.
	  - 새로운 인스턴스를 생성한다.
	  - 기존의 인스턴스의 포인터를 지운다.
	  - 포인터에 새로운 인스턴스의 주소를 할당한다.

4. null과 undefined는 숫자도 문자도 불리언 값도 아닌 독립적인 값에 속한다.


##### 테스트 해봅시다.

```javascript
let StringExample = new String('맛있는 초꼬렛');

stringExample.replace('맛있는', '맛없는');

console.log(stringExample);
```

다음의 결과값은 무엇이 나올까요?

여러분께서는 **맛없는** 초꼬렛을 예상하셨겠지만,

답은 **맛있는** 초꼬렛입니다. 풀이를 한번 보죠.



![immutable1](http://ddalpange.github.io/images/immutable1.png)

불변객체(String, Number, Boolean) 등은 그 자체가 값이기 때문에 값을 바꿀 수 없습니다.

즉 사용자가 볼때 값이 변하는 것은 새로운 인스턴스(값)을 생성하고 포인터를 재할당하는 것 뿐입니다.

위의 예제에서는 포인터를 재할당하지 않았음으로 stringExample의 값이 바뀌지 않았습니다.

따라서 ___let stringExample2 = stringExample___ 이라는 명령어를 실행할 때.

**객체와 인스턴스의 관계가 1:1로 형성됩니다.**

##### 가변형(Object Type).

1. 값을 변경할 수 있다.

2. Array, List, Map

3. 값을 변경할 때 인스턴스 자체가 변한다.

##### 테스트 해봅시다.

```javascript
let arrayExample = new Array("IU", "YouAndMe");

let arrayExample2 = arrayExample;

arrayExample[1] = "ABCDEFG";

console.log('1 ->', arrayExample);
console.log('2 ->', arrayExample2);
```

다음의 결과값은 무엇이 나올까요?

1 -> IU ABCDEFG

2 -> IU ABCDEFG

![immutable2](http://ddalpange.github.io/images/immutable2.png)

가변객체(Array, List, Map) 등은 값을 변경할 때 기존 변수가 바라보던 인스턴스 자체의 값을 변경합니다.

따라서 ___arrayExample = arrayExample2___ 명령을 실행한다면

두 변수가 같은 인스턴스를 바라보는 2 : 1 의 관계를 형성하게 됩니다.

따라서 하나의 변수에 값을 변경하고자 했을 때 다른 변수의 값도 함께 변화합니다.

**객체와 인스턴스의 관계가 n:1로 형성됩니다.**

#### immutable.js

* 1. 페이스북에서 배포한 자바스크립트 라이브러리입니다.
*
2. 기존의 가변형 객체였던 Map, List 등을 불변형으로 쓸 수 있게 해줍니다.

```javascript
let immutableMap = Immutable.Map({a:10, b:20, c:30});

let immutableMap2 = immutableMap;

immutableMap.set('b', 50);

console.log(immutableMap);
console.log(immutableMap2);
```

다음의 결과값은 무엇이 나올까요 ?

1 -> "10", "20", "30"

2 -> "10", "20", "30"

![immutable3](http://ddalpange.github.io/images/immutable3.png)

객체의 값이 변경될 때마다 새로운 인스턴스를 생성합니다.

**객체와 인스턴스의 관계가 1:1로 형성됩니다.**


#### 정리.

1. Front에서는 사용자에게 보여줄 값과, 실제 데이터값을 분리할 필요가 있다.

2. shouldComponentUpdate같은 중요한 메소드를 사용할 때, 기준이 되는 변수가 함수 A,B를 거쳐왔는지 A,C를 거쳐왔는지 장담할 수 없다.

3. 객체 배열을 Shallow copy 해버린다면 각각의 데이터에 독립성을 보장할 수 없다.

4. Immutable.js, cloneObject 등 Deep copy를 하게된다면 데이터의 독립성을 보장하며, 명시적이며, 유지보수를 쉽게 할 수 있다.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwODQ0MDU5NTMsMTM0NTc4NTYxMF19
-->