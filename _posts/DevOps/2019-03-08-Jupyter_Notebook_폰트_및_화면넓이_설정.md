---
layout: post
title: '[Jupyter Notebook] 폰트 및 화면넓이 설정'
category: DevOps
tags: [jupyter notebook]
comments: true
---

# jupyter notebook 설정
## Jupyter Notebook 사용가능한 폰트 확인

~~~python
import matplotlib as mpl
set(sorted([f.name for f in mpl.font_manager.fontManager.ttflist]))
~~~

## jupyter notebook 폰트 설정
~/.jupyter/custom/custom.css 파일 수정을 통해 jupyter notebook에서 실행할 폰트를 지정할 수 있다.
만약 ~/.jupyter 내부에 custom 폴더 및 custom.css 파일이 없다면 콘솔에서 다음 명령어를 입력하면 ~/.jupyter/ 내부에 custom/custom.css이 생성된다. 만약 생성되지 않으면 폴더와 파일을 직접 만든다.
<br>

```shell
$ jupyter notebook --generate-config
```

~~~shell
// .jupyter 폴더 내부에 custom 폴더 생성
~/.jupyter > mkdir custom
~/.jupyter > cd custom
~/.jupyter/custom > vi custom.css
~~~

<br>
코딩을 용이하게 하기 위해서는 글자간 간격이 일정한 monospaced fonts를 설치하는 것이 좋다.
개인적으로 주로 사용하는 monospace 폰트는 NanumGothicCoding, D2Coding, Consolas 등이 있다.


font를 적용하기 위해서는 custom.css파일에 아래 설정을 넣어주면 된다.
아래 스타일은 예시일 뿐이고 개인의 취향에 맞게 스타일을 변경하면 된다.
<br>

```css
/* 폰트 설정 */
#notebook,
.CodeMirror pre,
.output pre {
  font-family: NanumGothicCoding, Consolas;
  font-size: 1.9rem !important;
  line-height: 140% !important;
}

.dataframe,
table {
  font-family: NanumGothicCoding, Consolas;
  font-size: 1.5rem !important;
}

.input {
flex-direction: column !important;
}

.prompt_container {
flex-direction: row !important;
}
```

<br>
## jupyter notebook 화면 넓이 설정
가로 화면이 좁은 경우에 jupyter notebook의 여백을 제거하여 더 넓은 화면으로 이용할 수 있다. 개인적으로 코딩을 위해 모니터를 수직으로 배치하는 경우에는 아래 코드를 추가해 사용하고 있다.
<br>

```css
/* 모니터 수직배치할 경우 화면 넓게 쓰기 */
@media (max-width: 1100px) {
  #notebook-container {
    padding: 0 !important;
  }

  .container {
    width: auto;
  }
}
```

<br>
## Scroll Past End 기능
Atom 에디터에는 스크롤을 내릴 때 커서 최하단에서 추가로 더 스크롤할 수 있는 Scroll Past End 기능이 있다. 이러한 기능으로 엔터를 연발하지 않고 항상 눈높이에서 코딩을 할 수 있는 장점이 있다. 그런데 Jupyter Notebook에는 이러한 기능이 없어 불편하지만 css를 활용한다면 Scroll Past End 기능을 구현할 수 있다.
<br>

```css
/* Scroll Past End 기능 */
.end_space {
  height: 75vh;
}
```


## 전체 코드

~~~css
#notebook,
.CodeMirror pre,
.output pre {
  font-family: NanumGothicCoding, Consolas;
  font-size: 1.9rem !important;
  line-height: 140% !important;
}

.dataframe,
table {
  font-family: NanumGothicCoding, Consolas;
  font-size: 1.5rem !important;
}

.input {
flex-direction: column !important;
}

.prompt_container {
flex-direction: row !important;
}

.prompt {
  text-align: left !important;
  min-width: 0 !important;
}

.output_prompt {
  font-size: 0px;
}

@media (max-width: 1100px) {
  #notebook-container {
    padding: 0 !important;
  }

  .container {
    width: auto;
  }
}

.end_space {
  height: 75vh;
}
~~~


