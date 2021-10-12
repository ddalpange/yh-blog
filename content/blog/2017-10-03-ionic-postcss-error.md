---
title: 아이오닉 postCss 에러
date: 2017-10-03 22:46:29
thumbnail: https://ionicframework.com/img/ionic-meta.jpg
banner: https://ionicframework.com/img/ionic-meta.jpg
categories: [Javascript]
tags: [Ionic, Javascript, Angular, Ionic]
toc: true
---

[메모만들기 2](https://ddalpange.github.io/2017/07/11/%EA%B0%84%EB%8B%A8%ED%95%9C-%EB%A9%94%EB%AA%A8%EC%9E%A5-%EB%A7%8C%EB%93%A4%EA%B8%B0-2/) 포스트가 잘 되는지 확인할려고 처음부터 다시 하던 도중, ionic의 기본 css가 적용되지 않는 문제가 발생하였다.

<!-- more -->

![문제 화면](/images/ionic/postCssError.png)

흐음 .. 처음 실행하는 것이니 웹팩의 설정에 잘못된게 있을리 없다. 그래서 콘솔창을 유심히 살펴보았다.

![문제 콘솔](/images/ionic/postCssConsole.png)

> Your current PostCSS version is 5.2.17, but autoprefixer uses 6.0.8. Perhaps this is the source of the error below.

당신의 postcss 버전은 5.2.17이다. 하지만 autoprefixer는 6.0.8을 요구하는데, 이건 아마 당신의 소스에 문제를 발생시킬 것이다. 가만히 있던 postcss가 문제일리는 없고, ionic팀에서 버전업데이트를 할 때 예상치 못한 에러가 발생한듯 하다. 그래서 검색을 해서 해결법을 찾았다.


https://github.com/ionic-team/ionic/issues/12441


프로젝트의 @ionic/app-scripts를 1.3.7 버전으로 다운그레이드 하면 된다.

변경 전
```javascript
{
  "name": "ddalpange-memo",
  "version": "0.0.1",
  "author": "Ionic Framework",
  "homepage": "http://ionicframework.com/",
  "private": true,
  "scripts": {
    ....
  },
  "dependencies": {
    ....
  },
  "devDependencies": {
    "@ionic/app-scripts": "2.0.2",
    "@ionic/cli-plugin-ionic-angular": "1.3.2",
    "typescript": "2.4.2"
  },
  "description": "An Ionic project"
}
```

변경 후
```javascript
{
  "name": "ddalpange-memo",
  "version": "0.0.1",
  "author": "Ionic Framework",
  "homepage": "http://ionicframework.com/",
  "private": true,
  "scripts": {
    ....
  },
  "dependencies": {
    ....
  },
  "devDependencies": {
    "@ionic/app-scripts": "1.3.7",
    "@ionic/cli-plugin-ionic-angular": "1.3.2",
    "typescript": "2.4.2"
  },
  "description": "An Ionic project"
}
```

package.json (루트폴더에 위치)에  "@ionic/app-scripts": "1.3.7" 버전으로 수정한 후 패키지를 지우고, 다시 깐다.

```bash
$ npm uninstall @ionic/app-scripts
$ npm install @ionic/app-scripts
```

실행한다.

```bash
$ ionic serve
```

![해결 완료](/images/ionic/postCssComplete.png)

해결 완료. 나중에 ionic팀에서 이 문제를 해결해 준다면 다시 업그레이드 하도록 한다.
