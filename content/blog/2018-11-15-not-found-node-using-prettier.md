---
title: Prettier에서 node를 찾지 못할 때.
date: 2018-11-15 15:35:15
thumbnail: https://www.keycdn.com/img/blog/node-version-manager.png
categories: [Node]
tags: [Prettier, Node, NVM]
---

NVM을 사용할 때 Node의 경로를 찾지 못하는 경우 해결하는 방법을 설명합니다.

<!-- more -->

Webstorm에서 Prettier를 실행할 때
```
/usr/bin/env: node: No such file or directory
```

위와 같은 에러가 나는 경우가 있다.

Webstorm의 Prettier `/usr/bin/node`의 경로로 노드를 돌리는데 nvm같은 경우에는 해당 경로에 node가 없기 때문에 에러가 난다.

이럴 경우 nvm의 노드 경로와 `/usr/bin/node`에 심볼릭 링크를 연결해주면 정상적으로 동작한다.


```shell
sudo ln -s `which node` /usr/bin/node
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUzNDU1NDg1MCwtMTkxODI1MDU3MywtMT
YyMzAwMTQ5Ml19
-->