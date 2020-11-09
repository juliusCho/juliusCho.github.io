---
layout: post
title:  "Jira/Confluence & Notion 연동에 대해"
author: julius
categories: [ Jekyll, tutorial ]
beforetoc: "** 해당 포스팅은 제가 찾은 정보들을 바탕으로 작성된 것이며, 정보가 빈약하거나 오류가 있을 수 있습니다. 댓글로 지적 & 수정요청 해주시면 너무나 감사하겠습니다!"
toc: true
---
`Jira/Confluence와 Notion의 연동은 글 작성일 기준 현 시점에서는 불가능한 것으로 판명되었습니다.`

저처럼 회사에서는 Jira/Confluence를 사용하며, 얼마전부터 무료 퍼스널 플랜을 발표한 후부터 개인적인 할일이나 계획 등을 정리하는 용도로 Notion을 사용하는 분들이 꽤 있으리라 생각됩니다(혹은 팀 플랜으로 Notion도 같이 사용하는 회사도 많지요). 그러다 분명 Notion에 정리한 자료를 Jira나 Confluence로, 혹은 그 반대로 옮기거나 연동하고픈 생각을 분명 한번쯤은 해보셨을것 같은데요.

검색 결과 결론부터 말씀드리면..
>"Jira/Confluence와 Notion의 연동은 지금으로썬 불가능하다"

라고 합니다..ㅠㅠ

Jira/Confluence에서 기본적인 Markdown 문법 연동을 지원은 하지만 많은 부분에서 깨지거나 컨버전이 지원되지 않는다고 합니다.
Notion 또한 Markdown export 기능만 있을 뿐, 외부와 연동 가능한 API는 전혀 없는 상태입니다.

덕분에 Markdown을 Confluence 페이지로 변환시키는 매크로 오픈소스나
AsciiDoc과 같은 문법을 사용한 대체용 문서관리 툴들이 많이 나오고 있는 상황입니다.

#### Markdown Converting OpenSource/Tools
* Confluence 플러그인들 - https://marketplace.atlassian.com/search?application=confluence&hosting=server&q=markdown&query=markdown
* Pandoc - https://pandoc.org/getting-started.html
* markdown2confluence - https://github.com/justmiles/go-markdown2confluence
* Markdown to Confluence Converter - https://github.com/RittmanMead/md_to_conf

#### AsciiDoc
* AsciiDoctor = https://asciidoctor.org/
* Antora - https://antora.org/

---
그런데.. 고작 몇개 포스트 연동하겠다고 또 새로운 툴을 설치하고 하기 너무 귀찮죠??
그래서 제가 가끔 쓰는 방식을 적어보려고 합니다.

1. 


## Special formatting

As well as bold and italics, you can also use some other special formatting in Markdown when the need arises, for example:

+ ~~strike through~~
+ ==highlight==
+ \*escaped characters\*


## Writing code blocks

There are two types of code elements which can be inserted in Markdown, the first is inline, and the other is block. Inline code is formatted by wrapping any word or words in back-ticks, `like this`. Larger snippets of code can be displayed across multiple lines using triple back ticks:

```
.my-link {
    text-decoration: underline;
}
```¡

If you want to get really fancy, you can even add syntax highlighting using Rouge.


![walking]({{ site.baseurl }}/assets/images/8.jpg)

## Reference lists

The quick brown jumped over the lazy.

Another way to insert links in markdown is using reference lists. You might want to use this style of linking to cite reference material in a Wikipedia-style. All of the links are listed at the end of the document, so you can maintain full separation between content and its source or reference.

## Full HTML

Perhaps the best part of Markdown is that you're never limited to just Markdown. You can write HTML directly in the Markdown editor and it will just work as HTML usually does. No limits! Here's a standard YouTube embed code as an example:

<p><iframe style="width:100%;" height="315" src="https://www.youtube.com/embed/Cniqsc9QfDo?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe></p>
