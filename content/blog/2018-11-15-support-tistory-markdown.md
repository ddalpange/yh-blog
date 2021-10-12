---
title: 티스토리에서 마크다운 지원하기
date: 2018-11-15 15:33:47
thumbnail: https://t1.daumcdn.net/cfile/tistory/23711A3856372C9617
categories: [블로그]
tags: [Markdown, Tistory]
---

원래 블로그를 hexo로 운영하고 있었는데 아래와 같은 이유때문에 티스토리로 이전했다.

<!-- more -->


* 이미지를 넣기 매우매우 귀찮다 (일일이 asset폴더에 저장한 후, 경로를 써주어야한다)
* hexo 개발환경을 갖추어야한다. (개발환경이 안맞춰진 PC에서는 빌드 및 배포를 할수가 없다)
* 잡다한 에러가 매우매우 많다.
* 빌드할때마다 nvm으로 노드 버전을 바꾸어줘야한다.
* 생각보다 검색 최적화가 이루어지지 않는다. (지킬을 쓸때는 꽤 많이 들어오는 편이었는데. 이상하게 hexo같은 경우는 유입이 거의 없었다.)


마크다운을 포기하고 싶지 않았기 때문에. 티스토리에서 마크다운을 지원하게 할 수 있도록 바꾸고싶었다. 구글에 검색해보니 다른 사람들이 작업한 방법은 

1. CDN으로 github-markdown.css 를 내려받는다.
2. 별도의 마크다운 편집기를 사용하여 글을 작성한 후. html을 export한다.

위 두 과정이었다.
내가 헥소로 넘어온 이유는 이미지를 넣기 귀찮아서인데, 별도의 마크다운 편집기를 사용해야한다면 티스토리를 사용할 이유가 없다. 직접 코드를 작성하기 시작했다. 

1. CDN이어야한다. 별도로 js,css파일을 업로드하기 귀찮다.
2. 티스토리편집기의 장점과, 마크다운의 장점을 골고루 혼용할 수 있어야한다.

그래서 아래와 같이 코드를 작성하여 기존 티스토리 편집기에서 마크다운을 지원하도록 바꾸었다.
테마 html 편집하기에서 아래 코드를 붙여넣기하면 된다.



```htmlmarkup
<!-- css -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/github-markdown-css/2.10.0/github-markdown.css" />
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/themes/prism.min.css" />
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/plugins/toolbar/prism-toolbar.min.css" />
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/plugins/line-numbers/prism-line-numbers.min.css" />
<!-- js -->
<script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/prism.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/plugins/toolbar/prism-toolbar.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/plugins/line-numbers/prism-line-numbers.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/plugins/copy-to-clipboard/prism-copy-to-clipboard.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/components/prism-markup.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/components/prism-c.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/components/prism-typescript.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/components/prism-scss.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/components/prism-sass.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/components/prism-css.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/components/prism-javascript.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/components/prism-json.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/components/prism-python.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.15.0/components/prism-jsx.min.js"></script>
<script>
try {
  var article = $(".tt_article_useless_p_margin");
  article[0].className += " markdown-body";
  var childs = article.children();
  var results = [];

  for(var i = 0; i < childs.length; i++) {
    var child = childs[i];

    if (child.tagName === "P") {
      if (child.children[0] && child.children[0].tagName === "BR") {
        results.push("");

      } else {
        results.push(child.innerText)

      }
    } else {
      results.push(child);

    }

    child.remove();

  }

  var string = "";

  for (var i = 0; i < results.length; i++) {
    var result = results[i];

    if (typeof result === "string") {
      string += result + "\n";

    }  else {
      article.append(marked(string));
      string = "";
      article.append(result);

    }
  }

  if (string) {
    article.append(marked(string));
  }
} catch (e) {
  console.error(e);
}
</script>

```

대충 만든 코드를 정리하고 **TOC**를 적용해보자.
**TOC**는 Table of Contents의 줄임말로 목차라고 생각하면 된다.

markedJS에서 렌더러를 통해 마크다운을 엘리먼트로 변환하는 중간 과정에 hook을 걸 수 있다.
h1 ~ h6 엘리먼트에 훅을 걸고 본문의 맨 앞에 **TOC**를 삽입하면 된다.


```javascript

var toc = [];
var article = $(".tt_article_useless_p_margin");

prepareTOC(toc);
convert(article);
insertTOC(article, toc);

function insertTOC (article, toc) {
  var result = "목차\n========\n";
  var firstLebel = toc[0] ? toc[0].level : 1;

  for(var i = 0; i < toc.length; i++) {
    var link = toc[i];
    var tabs = "";

    for(var j = firstLebel; j < link.level; j++) {
      tabs += "\t";
    }

    tabs += "*";
    result += tabs + " ["+ link.text + "](#" + link.anchor + ")\n";
  }
  
  result += "\n\n"
    
   if (toc.length > 2) {
     article.prepend(marked(result));	
   }
}



function convert(article) {

  article[0].className += " markdown-body";
  
  var childs = article.children();
  var results = [];

  for(var i = 0; i < childs.length; i++) {

    var child = childs[i];
    if (child.tagName === "P") {
      if (child.children[0] && child.children[0].tagName === "BR") {
        results.push("");
      } else {
        results.push(child.innerText)
      }
    } else {
      results.push(child);
    }

    child.remove();
  }

  var string = "";

  for (var i = 0; i < results.length; i++) {
    var result = results[i];
    if (typeof result === "string") {
      string += result + "\n";
    } else {
      article.append(marked(string));
      string = "";
      article.append(result);
    }
  }

  if (string) {
    article.append(marked(string));
  }
}



function prepareTOC(toc) {

  var renderer = new marked.Renderer();

  renderer.heading = function(text, level, raw) {
    var anchor = Math.random() * 100
    toc.push({
      anchor: anchor,
      level: level,
      text: text
    });
    return '<h'
      + level
      + ' id="'
      + anchor
      + '">'
      + text
      + '</h'
      + level
      + '>\n';
  };

  marked.setOptions({
    renderer: renderer
  });
}

```


[MarkedJS](https://github.com/markedjs/marked)와 [PrismJS](https://github.com/PrismJS/prism)를 사용하였다.


기존 티스토리 편집기에서 마크다운을 붙여넣다보니 다양한 문제들이 있다.


1. 포스트 리스트에서 포스트 요약 내용을 마크다운으로 변환할 수 없다. 개행문자를 없앤채로 서버에서 내려오기 때문에 어디서 개행이 되었는지 알 수가 없기 때문.
2. 복붙하는 과정에서 편집기에서는 이상이 없으나, HTML보기를 체크하면 이상한 태그나 CSS가 먹어져 있는 경우가 많다.
3. 마크다운을 고려하지 않은 테마가 있기 때문에, github-markdown.css를 적용하더라도 눈으로 보고 css를 고쳐주어야한다.
4. 글을 작성할 때 마크다운 하이라이팅을 사용할 수 없다.

1번의 경우 고객센터를 통해 수정을 요구한 상태인데, 적용이 될지 안될지 모르겠다.

좀 더 사용해보고 문제를 픽스한 후, 오픈소스로 배포할 예정이다

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzIxMDMzODUyLC0yNTU1ODc5NzUsLTI1MD
A3NTgyMF19
-->