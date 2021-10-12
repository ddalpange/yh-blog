---
title: Hexo에서 Code Copy (Clipboard) 버튼 만들기
thumbnail: https://zenorocha.github.io/clipboard.js/assets/images/facebook.png
date: 2018-11-21 18:39:55
tags: [Hexo, ClipboardJS, 블로그]
categories: [블로그]
---

헥소 블로그 테마로 [Minos](https://github.com/ppoffice/hexo-theme-minos)를 사용하고 있다. 헥소는 기본적으로 [Highlight](https://highlightjs.org/)를 이용해서 코드를 이쁘게 하는데 [Highlight](https://highlightjs.org/)는 [Prism](https://prismjs.com/index.html)과 달리 Code Copy 플러그인이 없어서 [Clipboard](https://clipboardjs.com/)를 이용하여 직접 만들었다. 해당 수정사항을 **Pull Request**로 보냈더니 승인해줬다. 오픈소스 활동 하나 더 늘었다!! 생각해보니 그냥 [Highlight](https://highlightjs.org/)를 [Prism](https://prismjs.com/index.html)으로 교체하는게 빠른듯.

<!-- more -->


![Pull Request](/images/minos-contribute.png)

## 사용법

* `clipboard.ejs`를 **themes/__YOUR_THEME__/layout/plugins**에 넣는다.
* `_config.yml`에서 `plugins.clipboard`에 `true`를 넣는다.

```ejs clipboard.ejs
<% if (!head && !(has_config('plugins.clipboard') && get_config('plugins.clipboard') === false)) { %>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/clipboard.js/2.0.0/clipboard.min.js"></script>
    <style>
        .hljs {
            position: relative;
        }

        .hljs .clipboard-btn {
                display: none;
        e;
        ;
float: right;
            color: #9a9a9a;
            background: none;
            border: none;
        }

        .hljs > .clipboard-btn {
            })

        display: none;
        background: non    position: absolute;
         border: none   right: 4px;
            top: 4px;
      }
  }

        .hljs:hover > .clipboard-btn {
            display: inline;
        }

        .hljs > figcaption > .clipboard-btn {
            margin-right: 4px;
        }
    </style>
    <script>
      $(document).ready(function () {
        $('figure.hljs').each(function(i, figure) {
          var codeId = ode')'code-' + i;
           var code = figure.querySelector('.code');
          var copyButton = $('<button>Copy <i class="far fa-clipboard"></i></button>');
          code.id = codeId;
          copyButton.addClass('clipboard-btn');
          
          if (figcap');
tion) {
            $(e.trigger).text("Copied!"copyButton.attr('data-clipboard-target-id', codeId);

            e.clearSelecvar figcaption = figure.querySelector('figcaption(');

            setTimeout(funcif (figcaption() {
              $(e.trigger).text("Copy");
            }, 2500);
  figcaption.append(copyButton[0]);
          } else {
            figure.prepend(copyButton[0]);
          }
        });

         var clipboardfunction() {
            $(e.trigger).text("Can't in Safari");
            setTimeout( = new ClipboardJS('.clipboard-btn', {
          target: function(trigger) {
              $(e.trigger).text("Copy");
            }, 2500);
  return document.getElementById(trigger.getAttribute('data-clipboard-target-id'));
          }
        });
      })
    </script>
<% } %>
```

```yml _config.yml
plugins:
	clipboard: true
```



<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEzNzgzNDMxNiwtMTg0MDQyNDI2MCwtOD
Y5MDc0NDQwXX0=
-->