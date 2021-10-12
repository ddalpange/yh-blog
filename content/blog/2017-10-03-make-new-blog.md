---
title: 헥소 블로그 삽질
date: 2017-10-03 23:39:41
thumbnail: http://cdn2.hubspot.net/hub/53/file-23115630-jpg/blog/images/blogging_image.jpg
banner: http://cdn2.hubspot.net/hub/53/file-23115630-jpg/blog/images/blogging_image.jpg
categories: [블로그]
tags: [Hexo, Deploy, Encoding, Url]
---

블로그 배포에 문제가 있었다. 한글 제목의 포스팅이 윈도우에서 빌드 및 배포했을 때는 잘 되지만 맥에서 빌드 및 배포를 했을 때 404 에러가 뜨기 시작했다.

<!-- more -->

원인을 찾아보니 윈도우에서 배포를 했을때는 URL에서 한글 인코딩이 정상적으로 작동하지만
맥에서 배포를 하니 한글 인코딩이 제대로 이루어지지 않고 있었다.

이 말이 무엇이냐 함은
**안녕하세요 김요한입니다.**
라는 문자를 접근할려면 URL에서는 한글 부분만 인코딩을 하게된다. 즉 결과값은 아래와 같이 되는것이다.

> **%EC%95%88%EB%85%95%ED%95%98%EC%84%B8%EC%9A%94+%EA%B9%80%EC%9A%94%ED%95%9C%EC%9E%85%EB%8B%88%EB%8B%A4.**

윈도우에서 배포를 했을 때 포스트 링크를 클릭하면 정상적으로 한글을 인코딩한다.
반면에 맥에서 배포를 했을 때는 아래와 같이 작동한다.

>~~**SOME-POST-LINK/안녕하세요-김요한입니다.**~~
~~즉 인코딩을 하지 않고 URL을 접근한다는 뜻이다.~~
정정한다.
맥(unix)계열의 한글 인코딩과 URL 한글 인코딩 방식이 틀리기 때문에 발생하는 문제이다.

여러 방면으로 검색을 해보았으나 해결하지 못하였다.
어쩔수 없이 포스트의 파일명을 전부 영어로 변경하는 수 밖에 없었다.

![before](/images/hexo/filename-before.png)
![after](/images/hexo/filename-after.png)

-> *헥소에서 URL은 파일명을 따라가고 제목은 title을 따라간다.*

기분도 전환할 겸 테마도 새로 바꾸었으니 새로운 마음으로 포스트를 시작해야겠다 :)

---

참고문서
- http://www.partialrecall.net/2015/10/11/hexo-blog-deploy/
- http://www.convertstring.com/ko/EncodeDecode/UrlEncode