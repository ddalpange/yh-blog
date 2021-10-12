---
title: 자바스크립트 차트 라이브러리
thumbnail: https://cdn-images-1.medium.com/max/1600/1*d7dvz2Zh28hJbzYXmKsCcQ.png
date: 2018-05-05 08:30:00
toc: true
categories: [Javascript]
tags: [Javascript, Chart]
---

Knowru 회사에서 나의 주요업무는 데이터 시각화다. 여러 차트 라이브러리를 사용해보며 부딪히고 깨진 경험을 공유해보고자 한다.

<!-- more -->

### [ChartJS](https://www.chartjs.org/)
깃허브에서 가장 많은 스타를 받은 차트 라이브러리이다.
사용법이 굉장히 직관적이고 문서화도 잘되어있는 편이다.
Canvas를 이용하여 그리기 때문에 반응형 레이아웃도 문제없으며
세부적인 커스터마이징도 어렵지 않은편이다.

무엇보다 사용자가 많기 때문에 스택 오버플로에서 왠만하면 다 해결할 수 있다.
현재 Knowru 서비스에서 사용중이다.


### [D3.js](https://d3js.org/)
D3또한 굉장히 유명한 데이터 시각화 라이브러리이다.
다만 ChartJS와는 용도가 좀 다른데 D3는 차트라는 단순한 주제를 넘어서
데이터 시각화에 대한 포괄적인 개념을 담아놓은 라이브러리이기 떄문에 사용법이 굉장히 복잡하다.
Table HeatMap 차트를 그리는데 Axis 픽셀 위치 정하다가 빡쳐서 집어던졌다.

### [C3.js](http://c3js.org/)
D3를 베이스로차트에 한 차트 라이브러리이다.
사용법, 디자인 모두 괜찮아보이나 ChartJS에 비교하여 특징이 될만한 장점이 없어서 써보진 않았다.

### [AmCharts](https://www.amcharts.com/)
엄청난 예제를 자랑하는 라이브러리이다.
유료차트 답게 Theming, Zoomable, Time Responsible 등 
부수적인 기능들이 많이 탑재되어 있으며
다양한 차트 커스터마이징을 지원한다.

이에 반해 차트를 그리는 api는 직관적이지 못해 아쉽다.

### [HighCharts](https://www.highcharts.com)
유료인거같던데 안써봐서 모르겠다.

### [TUIChart](https://github.com/nhnent/tui.chart)
네이버에서 제공하는 오픈소스 차트 라이브러리다.
이 차트의 장점은 무려 *IE8*을 지원한다는 것과 다양한 차트타입을 지원한다는 것이다.
Oowa 서비스에서 Table HeatMap을 사용하기 위해 쓰고있다.

단점은 반응형 웹에 매우 취약하단것과 문서가 매우매우매우 빈약하다는 것. 그리고 자잘한 에러들이 많다는 것이다.
반응형은 document.body에 resize이벤트를 걸어 놨지만, 속도가 매우매우 느리니 성능개선을 할 방법을 찾아야 할 것 같다.

외국에서는 쓰질 않으니 스택오버플로 등 검색해도 나올리는 없고 정 궁금하면 개발자한테 메일을 쏘거나 소스를 까봐야한다.

나 자신도 Table HeatMap을 쓰는데 에러가 있어 Pull Request를 보낸 상태이다.

---

차트 용어는 이 [링크](https://wiki.pentaho.com/display/Reporting/Charting+Terminology)를 참고하면 좋다.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAzNDQzNjYxLC0xMzk0NjA4Mzc1LDcyMT
Q0ODA0NSwtNjU1NDEzNzQ1XX0=
-->