---
title: 블로깅을 편하게 하기 위한 발악 (hexo, travis, stackedit)
thumbnail: https://workablehr.s3.amazonaws.com/uploads/account/logo/11901/large_Mascot-fullcolor-png.png
date: 2018-11-16 18:39:55
categories: [블로그]
tags: [Hexo, Travis, Github, StackEdit]
---

티스토리를 커스터마이징 하여 사용하다가 혈압이 올라서 다시 헥소로 포스팅을 하기로 했다.

<!-- more -->

티스토리는 자체 에디터를 사용하는데 html 결과물이 나오는 형태가 일관성이 없어서 테마를 바꿀때도 또 바꾸어줘야하고 심지어 페이지를 작성하는 에디터와 포스트를 작성하는 에디터가 달라서 포멧이 다르다. Gallery 등 다양한 툴을 붙여서 커스터마이징 하고 싶었는데 혈압올라서 그만뒀다

그래서 티스토리의 단점을 커버하기보다 헥소의 단점을 커버하는게 더 낫다고 생각하여 변경하였다.

헥소의 문제점은 두가지다.
1. `hexo deploy -g` 를 매번 해줘야한다.
2. 포스트만 따로 관리할수 있는 어드민이 없다

그래서 이 두가지 문제를 해결하는 방안을 포스팅하려한다.
헥소 블로그를 구성하는 방법은 따로 기술하지 않았다.

## 1. [Travis CI](https://travis-ci.org/)를 사용하여 자동 빌드 및 푸쉬하기
![Travis CI screenshot](https://cdn.travis-ci.org/images/landing-page/laptop-f308ed79defa4f49c5f01af29a60084d.png)

앱의 빌드 및 배포를 도와주는 CI 툴이다. 완전 무료이고 사용법도 굉장히 쉽다. 헥소에서 트래비스를 적용하기 위한 과정은 다음과 같다.

1. https://github.com/settings/tokens 에서 **repo**만 체크한 후 토큰을 발급받는다. 다른 권한은 필요하지 않다.
2. 헥소 저장소에 **.travis.yml**를 추가하고 **_config.yml**의 deploy 부분을 수정한다.
3. [Github Travis](https://github.com/marketplace/travis-ci)에 들어간 후 hexo blog 레파지토리를 등록해준다..
4. travis에서 해당 레파지토리 settings에 들어간 후 environment variables에 **__GITHUB_TOKEN__**라는 이름으로 깃허브에서 발급받은 토큰을 등록한다.

```yml .travis.yml
language: node_js
node_js:
	- "9"

branches:
	only:
		- master

install:
	- npm install

before_script:
	- git config --global user.name 'YOURE_USERNAME'
	- git config --global user.email 'YOURE_EMAIL'
	- sed -i "s/__GITHUB_TOKEN__/${__GITHUB_TOKEN__}/" _config.yml

script:
	- hexo clean
	- hexo generate
	- hexo deploy
```

```yml _config.yml
deploy:
	type: git
	repo: https://__GITHUB_TOKEN__@github.com/YOURE_USERNAME/YOURE_USERNAME.github.io
	branch: master
	message: "travis auto deploy"
```

## 2. Stackedit으로 markdown 파일들을 관리하기.

![Stackedit](https://super-monitoring.com/blog/wp-content/uploads/2017/05/stackedit1.png)

[Stackedit](https://stackedit.io)은 온라인 마크다운 에디터다. 오프라인 글쓰기도 지원한다.
GitLab, Github, Google Drive 등 왠만한 클라우드 환경은 모두 지원해준다.
Github workspace를 등록해주면 자동으로 동기화를 진행한다.

* repo: `https://github.com/username/blog.git`
* branch: `master`
* path: `source/_posts`

이제 Stackedit으로 마크다운 파일들을 관리할 수 있다 !!

![개쩔어](https://media.giphy.com/media/d2Z9QYzA2aidiWn6/giphy.gif)
