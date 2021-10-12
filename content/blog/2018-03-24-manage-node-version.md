---
title: NVM(Node Version Manager)을 통한 NodeJS 버전 관리
tags: [Node, NVM, N]
categories: [Node]
date: 2018-03-24 18:39:55
thumbnail: http://seanamarasinghe.com/wp-content/uploads/2016/02/nvm-1050x360.jpg
toc: true
---

[Node 릴리즈 노트](https://nodejs.org/en/download/releases/)를 보면 약 3일 정도에 마이너 버전이 하나씩 올라갈 정도로 매우 매우 빠르다.

작년(2017) 3월 21일 기준으로 LTS는 **6.10.1**인데 올해(2018년) 3월 24일 기준으로 LTS는 **8.10.0**이다.

STABLE은 **9.9.0**이니 말 다했다.

<!-- more -->

안본 사이 노드 버전의 격차가 너무 벌어져서 생각없이 업데이트를 때렸다가는 `node-sass`, `node-gyp`같이 예민한 아이들이 피토를 하뿜뿜! 뿜어내며 퇴근을 방해한다.

![흔한 에러](/images/common/node-gyp-error.png)

그러므로 Node의 버전을 올릴때 또는 오래된 노드 프로젝트와 새로 만든 노드 프로젝트를 유연하게 빌드 할수 있을려면

노드 버전을 명령어를 통해 바꿀 수 있어야한다.

이와 같은 라이브러리론 [N](https://github.com/tj/n)과 [NVM](https://github.com/creationix/nvm)이 있다.

윈도우라고 겁먹을 필요 없다. [NVM-Window](https://github.com/coreybutler/nvm-windows)도 있다.

어떻게 쓰는지는 따로 포스팅하지 않겠다. 깃허브에 설명이 너무 잘 되어있으니

`npm install`을 휘갈길때 나는 에러는 내 경험상 노드의 버전이 안맞아서 나는 경우가 450%였다.

사실 버전 관리에 대한건 아무 생각이 없었는데, 백엔드 개발자분이 pipenv, virtualenv, pyenv 등을 사용햐면서 유연하게 개발환경을 설정하는 것을 보고 알게되었다.

좀 더 빨리 알았다면 좋았을텐데 :(
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYzNzM3MDkxNSwtMTcwMDM2NjY2MCw2ND
Y3NjE5MjRdfQ==
-->