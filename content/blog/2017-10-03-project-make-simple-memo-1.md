---
title: 간단한 메모장 만들기 1
date: 2017-10-03 22:48:48
thumbnail: /images/memo/memoBanner.png
banner: /images/memo/memoBanner.png
categories: [SimpleMemo]
tags: [Javascript, Angular, Ionic, SimpleMemo]
toc: true

---

파이어베이스, 아이오닉을 이용하여 간단한 하이브리드 어플리케이션을 만들어봅시다.

<!-- more -->

## 간단한 메모장을 만듭시다.

> 파이어베이스, 아이오닉을 이용하여 간단한 하이브리드 어플리케이션을 만들어봅시다. 파이어베이스의 database, hosting(deploy), auth 등의 서비스와 ionic에서 지원하는 컴포넌트를 적극적으로 사용하여 만들겁니다. Angular의 기본 이론보다, Ionic과 Firebase의 기능에 대해서 중점을 두시면 될 듯합니다.

## 사용기술
* 파이어베이스
* ~~위지윅 에디터~~
* Angular2
* Ionic Framework

포스트 1에서는 간단한 설치와, 로컬서버로 앱을 시작해보는 정도로 끝내겠습니다.


## 개발환경 세팅

### Node.js

[NodeJS](https://nodejs.org/en/) 공식 사이트에서 LTS(Long-term support) 버전을 받아주세요.
> Node.js®는 Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임입니다. Node.js는 이벤트 기반, 논 블로킹 I/O 모델을 사용해 가볍고 효율적입니다. Node.js의 패키지 생태계인 npm은 세계에서 가장 큰 오픈 소스 라이브러리 생태계이기도 합니다.

### Editor

[VSCode](https://code.visualstudio.com/)를 깔아주세요. 따로 쓰는 IDE가 있으신 분들은 건너 뛰셔도 됩니다. 툴을 까신 다음 아래 두개의 익스텐션을 다운받아주세요.

* Auto Close Tag
    `html`에서 태그를 자동으로 닫아주는 패키지입니다    
* Auto Import
    `typescript`를 사용할 때 자동으로 임포트를 해주는 패키지입니다.

### Install Global Node Packages

```bash
npm install -g ionic cordova
```
`ionic`과 `cordova`를 global 환경에 인스톨하여주세요. 좀 오래 걸립니다.
> npm은 node package manager의 준말입니다.

## 프로젝트를 만드세요

프로젝트를 생성할 폴더에서 shell 또는 cmd를 키신 후 다음의 명령어를 입력합니다.
```bash
ionic start memo blank
```
memo는 프로젝트의 이름입니다. 빈 프로젝트를 만들거라서 blank 옵션을 줍니다.

> ionic <명령어> <이름> <옵션> 순서가 되겠네요.

```
memo
├─hooks
├─resources
│  ├─android
│  │  ├─icon
│  │  └─splash
│  └─ios
│      ├─icon
│      └─splash
├─src
│  ├─app
│  ├─assets
│  │  └─icon
│  ├─pages
│  │  └─home
│  └─theme
└─www
```
지금까지 잘 따라오셨다면 위와 같은 폴더구조의 프로젝트가 나올겁니다.


```bash
cd memo
ionic serve
```
명령어를 입력하여주세요.

![최초 실행 모습](/images/memo1.png)

---

참고 링크
- [해당 포스트에 작성된 모든 코드는 여기에 있습니다!](https://github.com/ddalpange/simple-memo)
- [해당 프로젝트는 여기서 볼 수 있습니다 !!](https://memo-28314.firebaseapp.com)
