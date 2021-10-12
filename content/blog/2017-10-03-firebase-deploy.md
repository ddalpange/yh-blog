---
title: Firebase 배포
date: 2017-10-03 22:45:54
thumbnail: https://firebase.google.com/images/homepage/news/500_blog_firebase_2x.png?hl=ko
banner: https://firebase.google.com/images/homepage/news/500_blog_firebase_2x.png?hl=ko
categories: [Firebase]
tags: [Javascript, Angular, Firebase, Ionic]
toc: true
---

Firebase 호스팅은 개발자를 위한 프로덕션 등급 웹 콘텐츠 호스팅 서비스입니다. Firebase 호스팅을 사용하면 한 번의 명령으로 웹 앱과 정적 콘텐츠를 글로벌 콘텐츠 전송 네트워크(CDN)에 빠르고 손쉽게 배포할 수 있습니다.

<!-- more -->

### 주요기능

제목 | 설명
--- | ---
보안 연결을 통해 제공	 | 오늘날의 웹에서 가장 중요한 과제는 바로 보안입니다. Firebase 호스팅은 별도의 구성 없이 SSL을 기본적으로 제공하여 콘텐츠를 항상 안전하게 전송합니다.
빠른 콘텐츠 전송 | 업로드하는 각 파일이 전 세계 CDN 에지의 SSD에 캐싱되므로 사용자가 어디에 있든 빠르게 콘텐츠를 전달합니다.
신속한 배포	 | Firebase CLI가 불과 몇 초만에 앱을 궤도에 올려 드립니다. 명령줄 도구로 빌드 프로세스에 배포 타겟을 손쉽게 추가할 수 있습니다.
클릭 한 번으로 롤백	 | 빠른 배포도 좋지만 실수를 빠르게 되돌릴 수 있다면 더 좋을 것입니다. Firebase 호스팅은 완벽한 버전 관리 및 릴리스 관리를 제공하며 클릭 한 번으로 롤백이 가능합니다.


### 작동원리

Firebase 호스팅은 최신형 웹 개발자를 위해 개발되었습니다. Angular 등의 프런트 엔드 JavaScript 프레임워크 및 Jekyll 등의 정적 생성기 도구가 부상하면서 정적 사이트의 기능이 점점 커지고 있습니다. 간단한 앱 방문 페이지를 배포하든 복잡한 미래형 웹 앱을 배포하든, Firebase 호스팅은 정적 웹사이트를 배포하고 관리하는 데 특화된 인프라, 기능, 도구를 제공합니다.

Firebase 호스팅은 프로젝트에 firebaseapp.com 도메인의 하위 도메인을 제공합니다. Firebase CLI를 사용하여 컴퓨터의 로컬 디렉토리에 있는 파일을 호스팅 서버에 배포할 수 있습니다. 이러한 파일은 글로벌 CDN 중 사용자와 가장 가까운 에지 서버로부터 SSL 연결을 통해 제공됩니다.

Firebase 호스팅은 정적 콘텐츠 호스팅뿐 아니라 정교한 미래형 웹 앱을 개발하는 데 필요한 간단한 구성 옵션을 제공합니다. 개발자는 손쉽게 클라이언트측 라우팅 URL을 수정하거나 맞춤 헤더를 설정할 수 있습니다.

사이트를 프로덕션 단계로 운영할 준비가 끝났으면 자체 도메인 이름을 Firebase 호스팅에 연결할 수 있습니다. 도메인의 SSL 인증서가 자동으로 발급되므로 모든 콘텐츠가 안전하게 제공됩니다.


### 시작

```bash
$ npm install -g firebase-tools
```

Firebase CLI를 글로벌환경에 인스톨해주세요.

```bash
$ firebase login
? Allow Firebase to collect anonymous CLI usage information? Yes

Visit this URL on any device to log in:
https://accounts.google.com/o/oauth2/auth?client_id=563584335869-fgrhgmd47bqnekij5i8b5pr03ho849e6.apps.googleusercontent
.com&scope=email%20openid%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloudplatformprojects.readonly%20https%3A%2F%2Fwww
.googleapis.com%2Fauth%2Ffirebase%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloud-platform&response_type=code&state=27
2873884&redirect_uri=http%3A%2F%2Flocalhost%3A9005

Waiting for authentication...

+  Success! Logged in as ddalpange@gmail.com
```

Allow Firebase to collect anonymous CLI usage information?
파이어베이스가 당신의 CLI 사용정보를 익명으로 수집하는데 동의하시겠습니까? 라는 의미입니다.
Y를 입력하고 나면 웹페이지 창으로 구글 로그인이 뜰겁니다. 로그인해주세요.

![로그인 성공 화면](/images/firebaseLoginSuccessful.png)


```bash
$ firbase init
```

파이어베이스 프로젝트를 초기화합니다. 이 명령어를 실행하고 나면 firebase.json이라는 파일이 생길거에요.

```javascript
{
  "hosting": {
    "public": "www", // 업로드될 폴더의 경로입니다.
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ]
  }
}
```

저같은 경우는 아이오닉 프로젝트를 호스팅할것이기 때문에 "www" 폴더를 썼습니다 보통이면 "public"폴더를 써야겠죠 ?.


```bash
$ ionic build
```
서비스를 빌드한 후 



```bash
$ firebase deploy
```

![de1](http://ddalpange.github.io/images/deploy-error.png)

어떤 파이어베이스 프로젝트를 배포할 것인지 정해주지 않아서 나는 에러입니다.

```bash
$ firebase use --add
```

파이어베이스 프로젝트를 추가해주신 후 다시 실행해주세요.

```bash
$ firebase deploy
```

![de2](http://ddalpange.github.io/images/deploy-complete.png)

완료되었습니다!

https://cacaotalk-d32be.firebaseapp.com/#/friend/list

https://YOUR_PROJECT_ID.firebase.app.com 으로 들어가보세요 ~

### 정리

별도의 웹서버를 올리기 귀찮을 때 파이어베이스 호스팅 서비스는 그 대안이 될 수 있습니다. 별도의 웹서버, 호스팅 관리 서비스를 이용하지 않고 단순한 설정만으로 무료로 쓸 수 있는 웹서버가 생긴다는 것은 굉장한 장점이죠.

그러나 파이어베이스 호스팅서비스를 실제 서비스로 운용하기에는 아직 미숙한점이 많습니다. 그 대표적인 예로 버전관리의 부재이죠. (배포기록과, 해당 배포버전으로 돌아갈 수 있는 롤백기능을 제공하긴 합니다. 하지만 그 두 기능만으론 부족합니다.)




