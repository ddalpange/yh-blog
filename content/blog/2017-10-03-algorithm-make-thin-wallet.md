---
title: 가장 얇은 지갑 만들기
date: 2017-10-03 22:47:24
thumbnail: http://cfile212.uf.daum.net/image/2126BB4F53BCF73D197D26
banner: http://cfile212.uf.daum.net/image/2126BB4F53BCF73D197D26
categories: [Algorithm]
tags: [Algorithm, Programming]
toc: true
---

1만원, 7만원, 11만원, 17만원권의 지폐가 있다. 원하는 액수를 입력하면, 가장 얇은 지갑을 만들 수 있도록, 지폐의 갯수를 최소화 한 구성을 보여주는 프로그램을 작성하시오.

<!-- more -->


## 예시
입력값 150000 인 경우 가장 좋은 구성은 7만원 2장 1만원 1장으로 총 3장이다.

## 입력 / 출력 

1. 입력 : 프로그램의 첫번쨰 인자로 숫자를 받는다.
  * 입력값에 오류는 없다고 가정한다. 즉, "135000원" 같이 구성 불가능한 입력값은 없다.
  별도로 오류처리 해 줄 필요 없음.
2. 출력 : 예) 1만원 X장, 7만원 X장, 11만원 X장, 17만원 X장

## 코드

```html
<!DOCTYPE HTML>
<html>
<head>
  <title>가장 얇은 지갑 만들기</title>
  <meta charset="utf-8" />
</head>
<body>
  <input id="money"/><button id="submit" onClick="createMinimumWallet()">Click me!!</button>
  <div id="result"></div>
  <script>
    function createMinimumWallet() {
      /* 지폐를 최대한 쓸 수 있는 갯수를 구하여 0부터 순회를 돌아 값을 구하는 로직. */
      /* 50억 넣을 경우 삑난다 다른 로직 필요함. */

      var min = 99999;
      var result = null;
      var defaultMoney = document.getElementById("money").value;

      var seventeenMax = defaultMoney / 170000;
      for(var seventeenCount = 0; seventeenCount <= seventeenMax ; seventeenCount++) {
        var money = defaultMoney - seventeenCount * 170000;
        var elevenMax = money / 110000;

        for(var elevenCount = 0; elevenCount <= elevenMax; elevenCount++) {
          money = defaultMoney - (seventeenCount * 170000) - (elevenCount * 110000);
          var sevenMax = money / 70000;

          for(var sevenCount = 0 ; sevenCount <= sevenMax; sevenCount++) {
            money = defaultMoney - (seventeenCount * 170000) - (elevenCount * 110000) - (sevenCount * 70000);
            var oneCount = money / 10000;
            var sum =  seventeenCount + elevenCount + sevenCount + oneCount;
;
            if(min > sum) {
              min = sum;
              result = '결과: '+ "17만원권, "+ seventeenCount+ "장 11만원권, "+ elevenCount+ "장 7만원권, "+ sevenCount+ "장 1만원권, "+ oneCount+"장 총 " + sum + "장" ;
            }

          }
        }
      }
      console.log('입력하신 값', defaultMoney);
      console.log(result);
      document.getElementById("result").innerHTML = result;
    }
  </script>
</body>
</html>
```
