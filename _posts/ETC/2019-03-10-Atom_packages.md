---
layout: post
title: Atom editor packages
category: etc
tags: [atom, emmet, minimap]
comments: true
---

# Atom editor packages

## emmet
- 반복되는 소스코드나 매번 쓰기 귀찮은 태그들을 축약어로 쉽게 작성할 수 있음
- Atom의 플러그인으로 HTML 문법의 자동완성 기능을 수행함
- 태그명을 입력 후 TAB 키를 누르면 자동으로 시작/끝 태그를 생성하고 그 사이로 에디트 할 수 있도록 커서를 자동 위치시킴

### 기본 사용법
- '!' : 타이핑 후 'tab'키를 누르면 html 문서의 기본골격이 만들어짐
- '>' : 내포 혹은 부모 자식관계
- '+' : sibling(동등관계)

```html
<!-- nav>ul>li 후 tab - 내포 관계가 정해진 태그를 의미 -->
<nav>
  <ul>
    <li></li>
  </ul>
</nav>

<!-- div+p+bq 후 tab - 동등 관계 -->
<div></div>
<p></p>
<blockquote></blockquote>
```

## atom 내의 settings
- soft wrap : 자동 줄바꿈 기능
- scroll past end : 에디터가 마지막 줄이 되어도 스크롤을 더 내릴 수 있어 텍스트 입력을 편하게 해 줌

## atom-html-preview
- html/css 적용 파일 미리보기

## open-in-brower
- html파일 작성시 단축키를 통해 자신이 작성한 코드의 결과를 브라우저를 통해 손쉽게 육안으로 확인 가능

## Markdown-preview-enhanced
- 마크다운 문서 프리뷰

## color-picker
- html 태그 이름만 입력해도 자동으로 시작 및 종료 태그를 알아서 붙여주는 플러그

## atom-beatutify
- 정리가 안된 코드들을 깔끔하게 정리해 줌

## pigments
- 작성한 색상코드에 그에 대한 색이 입혀짐

## color-picker
- 포토샵 및 일러스트처럼 색상표에서 색을 직접 뽑아올 수 있어서 편리

## highlight-selected
- 특정 단어를 더블 클릭했을 때 그것과 같은 단어를 표시해 줌

## minimap
- 문서 내에서 이동하고자 하는 곳으로 빠른 이동이 가능

## minimap-cursorline
- 미니맵에서 현재 커서가 있는 코드의 위치 확인

## activate-power-mode
- 코딩을 마치 게임하듯 즐겁게 할 수 있도록 하는 패키지, 시간 안에 적을 때마다 콤보가 올라감

## gpp-complier
- C를 컴파일 시켜주는 Package, F5를 누르면 컴파일, F6을 누르면 디버깅

## linter-gcc
- Error가 나오는 부분을 체크해주는 packages

## autocomplete-clang
- C언어에 대해 Auto-complete 기능을 제공해주는 패키지

## autocomplete-java

## autocomplete-python
