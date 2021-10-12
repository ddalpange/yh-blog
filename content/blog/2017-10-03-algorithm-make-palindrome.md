---
title: 회문 만들기
date: 2017-10-03 22:48:09
thumbnail: https://media2.fdncms.com/portmerc/imager/u/large/18918590/logo.jpg
banner: https://media2.fdncms.com/portmerc/imager/u/large/18918590/logo.jpg
categories: [Algorithm]
tags: [Algorithm, Programming]
toc: true
---

회문(Palindrome)은 앞/뒤 어느쪽으로 읽어도 같은 말이 되는 어구를 의미한다.

<!-- more -->

#### 첫번째 문제

Palindrome(이하 회문)은 앞/뒤 어느쪽으로 읽어도 같은 말이 되는 어구를 의미한다.

예) 191, 4325234, 123321, eye

어떤 수를 받아서 그 수를 뒤집은(reverse) 다음 원래의 수에 더하여 나온 값이 회문이 될 때까지, 뒤집은 수 더하기를 반복하여 회문을 찾는 프로그램을 작성하라.

예) 입력값 195인 경우

```
195 + 591 = 786
786 + 687 = 1473
1473 + 3741 = 5214
5214 + 4125 = 9339
출력 : 195 4 9339
```

##### 참고
회문을 찾을 수 없는 수도 있다.

예) 아직 증명되지는 않았지만 196은 회문을 찾을 수 없는 수이다.

뒤집어 더하는 것을 100번 해도 회문을 찾을 수 없는 경우는 회문이 없다고 가정한다.

예)

```
195 4 9339
196 is not palindrome
```

##### 코드

```html
<!DOCTYPE HTML>
<html>
  <head>
    <title>회문 만들기</title>
    <meta charset="utf-8" />
  </head>
  <body>
    <input id="doInputNumber" /><button onclick="makePalindrome()">Click me</button>
    <div id="result"></div>
    <script>
      window.makeReverseNumber = function(number) {
        return parseInt(new String(number).split("").reverse().join(""));
      }

      window.makePalindrome = function() {
        var defaultNumber = parseInt(document.getElementById("doInputNumber").value);
        var number = defaultNumber;
        var count = 0;

        var result = "";

        while(count < 100) {
          var reverseNumber = window.makeReverseNumber(number);
          var isPalindrome = reverseNumber === number;

          if(isPalindrome) {
            result += "결과 : "+ defaultNumber + " " + count + " " + number
            break;
          }

          var sum = number + reverseNumber;
          result = result + number + " + " + reverseNumber + " = " + sum + "<br/>";

          count ++;
          number = sum;
        }

        if(count == 100) {
          result = defaultNumber + " is not palindrome";
        }

        document.getElementById("result").innerHTML = result;;
      }
    </script>
  </body>
</html>
```



#### 두번째 문제

조소연씨는 다재다능한 사람입니다. 그래서 그녀에게는 친구가 많습니다.
하지만 불행하게도 그녀의 친구들은 다재다능하지 않습니다.
각각의 친구는 2가지 주제에만 관심이 있고 다른 주제로 이야기하는 것을 싫어합니다.
그래서 파티를 개최할 때마다 모두가 즐겁게 파티를 보내려면 어떤 친구를 초대할지가 큰 문제입니다.
조소연씨는 그 동안의 경험으로 초대된 친구 모두가 공통의 흥미 있는 화제가 있을 때 파티를 즐긴다는 것을 알았습니다.

문자열 배열 first, second가 주어집니다.
조소연씨의 index 번째 친구가 흥미 있는 화제는 first[index] 와 second[index] 입니다.
즐거운 파티가 되려면 초대할 수 있는 친구는 최대 몇 명인지 리턴하세요.

정의 : 클래스와 함수 정의
Python : InterestingParty
Method : bestInvitation(first, second)

제약조건
first : 1 부터 50개의 요소를 갖는 배열입니다.
second : first 와 같은 크기의 배열입니다.
first, second 공통 : 각 요소는 1~5개의 문자이며, 각 문자는 영어 소문자입니다.
index 번째 요소 first[index] 와 second[index] 의 내용은 다릅니다.

입력 / 출력 데이터

[1]

```
first = ['fishing', 'gardening', 'swimming', 'fishing']
second = ['hunting', 'fishing', 'fishing', 'biting']
return : 4
```

[2]

```
first = ['variety', 'diversity', 'loquacity', 'courtesy']
second = ['talking', 'speaking', 'discussion', 'meeting']
return : 1
```

[3]

```
first = ['snakes', 'programming', 'cobra', 'monty']
second = ['python', 'python', 'anaconda', 'python']
return : 3
```

[4]

```
first = ['t', 'o', 'p', 'c', 'o', 'd', 'e', 'r', 's', 'i', 'n', 'g', 'l', 'e', 'r', 'o', 'u', 'n', 'd', 'm', 'a', 't', 'c', 'h', 'f', 'o', 'u', 'r', 'n', 'i']
second = ['n', 'e', 'f', 'o', 'u', 'r', 'j', 'a', 'n', 'u', 'a', 'r', 'y', 't', 'w', 'e', 'n', 't', 'y', 't', 'w', 'o', 's', 'a', 't', 'u', 'r', 'd', 'a', 'y']
return : 6
```


##### 코드

```html
<!DOCTYPE HTML>
<html>
<head>
  <title>즐거운 파티</title>
  <meta charset="utf-8" />
</head>
<body>
  <button onclick="calculateBestInvitation();">결과값은 콘솔로보세요.</button>
  <script>
    var first = ["fishing", "gardening", "swimming", "fishing"];
    var second = ["hunting", "fishing", "fishing", "biting"];
    // var first = ['variety', 'diversity', 'loquacity', 'courtesy'];
    // var second = ['talking', 'speaking', 'discussion', 'meeting'];

    var calculateBestInvitation = function () {
      /* topic마다 count를 세서 최대한이면 출려억 */
      var topicCounts = {};
      var mergedArray = first.concat(second);

      mergedArray.map((topic, topic_index)=>{
        if(topicCounts.hasOwnProperty(topic)) {
          topicCounts[topic] ++;
        } else {
          topicCounts[topic] = 1;
        }
      });

      var maximum = 0;
      var topicNames = Object.keys(topicCounts);

      topicNames.map((topicName, topicName_index)=>{
        if(topicCounts[topicName] > maximum) {
          maximum = topicCounts[topicName]
        }
      });
      console.log("return : ", maximum);
    }


  </script>
</body>
</html>
```