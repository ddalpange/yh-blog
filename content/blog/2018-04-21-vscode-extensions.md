---
title: VSCODE 익스텐션
date: 2018-04-21 01:33:28
thumbnail: http://www.itpaper.co.kr/wp-content/uploads/2017/12/0_Tu2sJCmh_CaSOD17.png
banner: http://www.itpaper.co.kr/wp-content/uploads/2017/12/0_Tu2sJCmh_CaSOD17.png
toc: true
tags: [IDE, VSCODE]
categories: [경험]
---

한동안 Webstorm을 쓰다가 무료체험기간 30일이 끝나버려서 다시 VSCODE를 사용하고 있습니다. 유료툴인 Webstorm은 추가 패키지를 안깔아도 각종 인텔리센스와 개발경험을 제공하지만 VSCODE는 그렇지 않습니다. Angular를 개발하면서 필요하다 느꼈던 Extension을 공유합니다.

<!-- more -->

1. Angular Files
해당 위치에 `ng generate someting`를 할 수 있는 패키지입니다.
폴더구조를 고민할때 사용하면 좋습니다.

2. Angular Language Service 
Angular팀에서 내놓은 공식 익스텐션입니다.
`@angular/language-service`를 같이 깔아야 한다는데 안깔았었네요.
역시 설명을 잘 읽어봐야 합니다.

3. Auto Rename Tag
html에서 여는 태그를 변경할때 자동으로 닫는 태그를 변경해줍니다.
비슷한 용도로 Auto Close Tag가 있는데 Generic을 선언해줄때도 태그로 인식해서 닫아버려요
`Observable<any></any>` 이런식으로 말이죠. 사실 *emmet*을 주로 이용하기때문에 필요 없습니다.

4. Beautify
필수 패키지입니다.
html을 복붙할때 자꾸 한칸식 밀리는데 그럴때마다 사용하면 편합니다.

5. Bracket Pair Colorizer
가끔가다 천국의 계단 만드시는분 있는데 이거 없으면 못봐요.

6. Color Picker
디자인도 별로고 속도도 별로지만 별 대안이 없습니다.

7. Debugger Chrome
vscode에서 찍은 중단점을 크롬에서 확인할 수 있어요.
사용법도 간편하고 좋아보이지만 느려서 잘 안써요.

8. Code Runner
가끔가다 검사기로 함수 테스트하는데 이거 있으면 필요없습니다.

9. ESLint
귀찮지만 켜서 나쁠거 없습니다.

10. TSLint
마찬가지에요.

11. expand-region
제일 중요합니다. 웹스톰에 ***Ctrl + w***를 누르면 자동으로 블록이 확장되는데 vscode는 이러한 기능이 없어요.
웹스톰에 비하면 많이 멍청해서 단어 단위를 잘못 읽을때가 많지만 없는것보단 낫습니다.

12. Git History
깃에 대한 히스토리를 볼 수 있어요.

13. Git Lens
편집중인 라인의 마지막 커밋시점을 알 수 있습니다.
쓸만한거같아요.

14. intellisense css class names in HTML
css에 대한 autocomplete를 제공합니다.
emmet할때는 안떠서 아쉬워요.

15. npm
vscode의 명령팔레트에서 npm 명령어를 사용할 수 있습니다.

16. npm intellisense
글로벌에 깔린 패키지들에 대한 인텔리센스를 제공합니다.

17. path intellisense
from 쓸때 경로에 대한 intellisense를 알려줘요

18. TODO Highright
이거 없으면 주석이 하얀색이에요 보기싫습니다.

19. Auto Import
typescript를 사용할때 각종 package를 자동으로 import해줍니다.
비슷한 패키지로 typescript hero가 있는데 이 패키지는 absolute import를 지원하지 않습니다.
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTc0MDQ2OTMxLC0xODUxMzQwNzMsLTM3MD
E1ODMwN119
-->