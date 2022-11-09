---
layout: post
title: '[WEB] Bootstrap'
category: WEB
tags: [web, bootstrap]
comments: true
---

# Bootstrap
> 웹 디자인을 편리하게 만들 목적에서 나온 프레임워크
> 프레임워크란? 정해진 틀에 맞춰서 완성품을 만드는 것
> 즉, 부트스트랩은 적은 노력으로 큰 효과의 웹 디자인을 만들 수 있는 틀이다


## 참고할 만한 사이트

- http://bootstrapk.com/ (한글판)
- http://getbootstrap.com/ (영문판)
- http://maczniak.github.io/bootstrap/index.html  (2.2.2 버전 )


## 추가사항

- http://bootstrapk.com  > 다운로드 > 오른쪽 메뉴 중 > 브라우저와 기기 호환 확인
- http://bootstrapk.com  > 다운로드 > 오른쪽 메뉴 중 > Third party
( 다른 라이브러리와 충돌 나는 경우 - 대표적으로 구글맵 )

# 실습 코드

## Ex00_sample.html

~~~html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 위 3개의 메타 태그는 *반드시* head 태그의 처음에 와야합니다; 어떤 다른 콘텐츠들은 반드시 이 태그들 *다음에* 와야 합니다 -->
    <title>부트스트랩 101 템플릿</title>

    <!-- 부트스트랩 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">

<!--    &lt;!&ndash; IE8 에서 HTML5 요소와 미디어 쿼리를 위한 HTML5 shim 와 Respond.js &ndash;&gt;-->
<!--    &lt;!&ndash; WARNING: Respond.js 는 당신이 file:// 을 통해 페이지를 볼 때는 동작하지 않습니다. &ndash;&gt;-->
<!--    &lt;!&ndash;[if lt IE 9]>-->
<!--    <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>-->
<!--    <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>-->
<!--    <![endif]&ndash;&gt;-->
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">Project name</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <form class="navbar-form navbar-right">
                <div class="form-group">
                    <input type="text" placeholder="Email" class="form-control">
                </div>
                <div class="form-group">
                    <input type="password" placeholder="Password" class="form-control">
                </div>
                <button type="submit" class="btn btn-success">Sign in</button>
            </form>
        </div><!--/.navbar-collapse -->
    </div>
</nav>

<!-- Main jumbotron for a primary marketing message or call to action -->
<div class="jumbotron">
    <div class="container">
        <h1>Hello, world!</h1>
        <p>This is a template for a simple marketing or informational website. It includes a large callout called a jumbotron and three supporting pieces of content. Use it as a starting point to create something more unique.</p>
        <p><a class="btn btn-primary btn-lg" href="#" role="button">Learn more &raquo;</a></p>
    </div>
</div>

<div class="bs-docs-section">
    <h1 id="examples" class="page-header">예제들</h1>

    <p class="lead">부트스트랩의 많은 콤포넌트를 사용한 기본 템플릿을 기반으로 하세요. 우리는 당신이 당신의 개인적인 프로젝트의 필요성에 따라 부트스트랩을 맞춤화하고 적용하기를 권장합니다.</p>

    <h3 id="examples-framework">프레임워크 사용하기</h3>
    <div class="row bs-examples">
        <div class="col-xs-6 col-md-4">
            <a class="thumbnail" href="../examples/starter-template/">
                <img src="../examples/screenshots/starter-template.jpg" alt="Starter template example">
            </a>
            <h4>초보자 템플릿</h4>
            <p>별거 아니지만 기본입니다 : 콘테이너와 함께 합쳐진 CSS 와 자바스크립트.</p>
        </div>
        <div class="col-xs-6 col-md-4">
            <a class="thumbnail" href="../examples/theme/">
                <img src="../examples/screenshots/theme.jpg" alt="Bootstrap theme example">
            </a>
            <h4>부트스트랩 테마e</h4>
            <p>Load the optional Bootstrap theme for a visually enhanced experience.</p>
        </div>
        <div class="clearfix visible-xs"></div>

        <div class="col-xs-6 col-md-4">
            <a class="thumbnail" href="../examples/grid/">
                <img src="../examples/screenshots/grid.jpg" alt="Multiple grids example">
            </a>
            <h4>그리드</h4>
            <p>4단계, 중첩 등의 그리드 레이아웃들의 몇가지 예제.</p>
        </div>
        <div class="col-xs-6 col-md-4">
            <a class="thumbnail" href="../examples/jumbotron/">
                <img src="../examples/screenshots/jumbotron.jpg" alt="Jumbotron example">
            </a>
            <h4>점보트론</h4>
            <p>네비게이션 바와 약간의 기본 그리드 컬럼들과 함께 점보트론을 중심으로 만드세요.</p>
        </div>
        <div class="clearfix visible-xs"></div>

        <div class="col-xs-6 col-md-4">
            <a class="thumbnail" href="../examples/jumbotron-narrow/">
                <img src="../examples/screenshots/jumbotron-narrow.jpg" alt="Narrow jumbotron example">
            </a>
            <h4>좁은 점보트론</h4>
            <p>좁은 콘테이너와 점보트론으로 맞춤화한 페이지를 만드세요.</p>
        </div>
    </div>
</div>

<!-- jQuery (부트스트랩의 자바스크립트 플러그인을 위해 필요합니다) -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
<!-- 모든 컴파일된 플러그인을 포함합니다 (아래), 원하지 않는다면 필요한 각각의 파일을 포함하세요 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
~~~

## Ex01_starter_template.html

~~~html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 위 3개의 메타 태그는 *반드시* head 태그의 처음에 와야합니다; 어떤 다른 콘텐츠들은 반드시 이 태그들 *다음에* 와야 합니다 -->
    <title>Ex01_starter-template</title>

    <!-- 부트스트랩 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">

<!--    &lt;!&ndash; IE8 에서 HTML5 요소와 미디어 쿼리를 위한 HTML5 shim 와 Respond.js &ndash;&gt;-->
<!--    &lt;!&ndash; WARNING: Respond.js 는 당신이 file:// 을 통해 페이지를 볼 때는 동작하지 않습니다. &ndash;&gt;-->
<!--    &lt;!&ndash;[if lt IE 9]>-->
<!--    <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>-->
<!--    <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>-->
<!--    <![endif]&ndash;&gt;-->
</head>
<body>

<!-- #################### -->
<ul class="nav nav-pills">
    <li role="presentation" class="active"><a href="#">홈페이지</a></li>
    <li role="presentation"><a href="#">포트폴리오</a></li>
    <li role="presentation"><a href="#">메시지</a></li>
</ul>
<!-- #################### -->



<!-- jQuery (부트스트랩의 자바스크립트 플러그인을 위해 필요합니다) -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
<!-- 모든 컴파일된 플러그인을 포함합니다 (아래), 원하지 않는다면 필요한 각각의 파일을 포함하세요 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
~~~

## Ex02_button.html

~~~html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title> 기본 샘플 확인 </title>
    
    <!-- 부트스트랩 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    
</head>
<body>
	<!--  ............................................................................  -->   
 <div class="container">  
     
    <h4 class="text-primary"> 버튼 </h4>
      <button type="button">클래스 선택자 적용하지 않는 경우  </button>
      <button type="button" class="btn">class="btn" 만 적용한 경우 </button>
    <hr>
    
    <h4 class="text-primary"> button 태그 사용 </h4>    
      <button type="button" class="btn btn-default">기본 버튼 모양 </button>
      <button type="button" class="btn btn-primary">중요한 버튼</button>
      <button type="button" class="btn btn-success">긍정적 의미의 버튼</button>
      <button type="button" class="btn btn-info">정보제공 버튼</button>
      <button type="button" class="btn btn-warning">주의를 알리는 버튼 </button>
      <button type="button" class="btn btn-danger">위험을 나타내는 버튼</button>
      <button type="button" class="btn btn-link">단순 링크로 처리하는 버튼</button>
      <hr>
    
    <h4 class="text-primary"> input 태그 사용 </h4>
      <input type="button" class="btn btn-default" value="기본 모양 버튼 ">
      <input type="button" class="btn btn-primary" value="중요한 버튼 ">
      <input type="button" class="btn btn-success" value="긍정적 의미의 버튼">
      <input type="button" class="btn btn-info" value="정보제공 버튼">
      <input type="button" class="btn btn-warning" value="주의를 알리는 버튼  ">
      <input type="button" class="btn btn-danger" value="위험을 나타내는 버튼 ">
      <input type="button" class="btn btn-link" value="단순 링크로 처리하는 버튼 ">
      <hr>

    <h4 class="text-primary"> a 태그 사용 </h4>    
      <a href="#" class="btn btn-default" role="button">기본 모양 버튼 </a>
      <a href="#" class="btn btn-primary" role="button">중요한 버튼 </a>
      <a href="#" class="btn btn-success" role="button">긍정적 의미의 버튼 </a>
      <a href="#" class="btn btn-info" role="button">정보제공 버튼 </a>
      <a href="#" class="btn btn-warning" role="button">주의를 알리는 버튼</a>
      <a href="#" class="btn btn-danger" role="button">위험을 나타내는 버튼  </a>
      <a href="#" class="btn btn-link" role="button">단순 링크로 처리하는 버튼 </a>
      <hr>      

      <p>
        <button type="button" class="btn btn-primary">기본 버튼 </button>
        <button type="button" class="btn btn-info">기본 버튼 </button>
      </p>
      <p>
        <button type="button" class="btn btn-primary btn-lg">큰 버튼</button>
        <button type="button" class="btn btn-info btn-lg">큰 버튼</button>
      </p>
      <p>
        <button type="button" class="btn btn-warning btn-sm">작은 버튼</button>
        <button type="button" class="btn btn-info btn-sm">작은 버튼</button>
      </p>
      <p>
        <button type="button" class="btn btn-danger btn-xs">아주 작은 버튼</button>
        <button type="button" class="btn btn-primary btn xs">아주 작은 버튼</button>
      </p>

      <hr>

      <div>
        <button type="button" class="btn-block btn-primary btn-lg">화면 전체 버튼</button>
        <button type="button" class="btn-block btn-info btn-lg">화면 전체 버튼</button>
      </div>


     <hr>
     <div class="label label-primary">위 예문은 부트스트랩 사이트과 양용석 저자의 '부트스트랩으로 디자인하라'에 정리된 내용입니다.</div>
     <div class="alert alert-success">위 예문은 부트스트랩 사이트과 양용석 저자의 '부트스트랩으로 디자인하라'에 정리된 내용입니다.</div>

 </div> 


	<!--  ............................................................................  --> 
	
    <!-- jQuery (부트스트랩의 자바스크립트 플러그인을 위해 필요합니다) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
    <!-- 모든 컴파일된 플러그인을 포함합니다 (아래), 원하지 않는다면 필요한 각각의 파일을 포함하세요 -->
    <script src="js/bootstrap.min.js"></script>
</body>
</html>
~~~


## Ex03_table.html

~~~html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title> 기본 샘플 확인 </title>
    
    <!-- 부트스트랩 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">

  <style type="text/css">
    .table tr:nth-child(even):hover {background-color: #d9edf7}
    /* hover로 색 변경 */
    /*.table tr:nth-child(odd):hover {background-color: #ebcccc}*/
    .table tr:hover {background-color: whitesmoke}
  </style>
   
  
</head>
<body>
	<!--  ............................................................................  -->   

<div>
  <h2  class="text-danger"> 우리 스타 보기</h2>
<!--  <table class="table table-bordered table-striped table-hover">-->
  <table class="table">
    <thead>
        <tr>
          <th>ID</th>
          <th>이름</th>
          <th>직업</th>
          <th>특징</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>203A</td>
          <td>샤이니</td>
          <td>가수</td>
          <td>
            [소속]SM엔터테인먼트 [멤버]온유, 종현, 키, 최민호, 태민	[데뷔]2008년 싱글 앨범 '누난 너무 예뻐 (Replay)'
        </td>
        </tr>
        <tr>
          <td>141B</td>
          <td>소녀시대</td>
          <td>가수</td>
          <td>
             [멤버] 태연, 윤아, 수영, 효연, 유리, 티파니 영, 써니, 서현 [소속사] SM엔터테인먼트 [데뷔]2007년 싱글 앨범 '다시 만난 세계'
          </td>
        </tr>
        <tr>
          <td>2031</td>
          <td>김수현</td>
          <td>배우</td>
          <td>
            탤런트, 영화배우 출생 1988년 2월 16일 (만 30세) 신체 180cm, 65kg  |  AB형
          </td>
        </tr>
        <tr>
          <td>007F</td>
          <td>국카스텐</td>
          <td>가수</td>
          <td>
          [멤버] 하현우(보컬, 기타), 전규호(기타), 김기범(베이스), 이정길(드럼)  [소속사] 인터파크 엔터테인먼트 [데뷔]2008년 싱글 앨범 'Guckkasten'
          </td>
        </tr>
      </tbody>
    </table>

  </div>


	<!--  ............................................................................  --> 
	
    <!-- jQuery (부트스트랩의 자바스크립트 플러그인을 위해 필요합니다) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
    <!-- 모든 컴파일된 플러그인을 포함합니다 (아래), 원하지 않는다면 필요한 각각의 파일을 포함하세요 -->
    <script src="js/bootstrap.min.js"></script>
</body>
</html>
~~~

## Ex04_grid_1.html

~~~html
<!-- (3) 화면을 작게 하여 반응형을 웹을 확인  -->

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title> 기본 샘플 확인 </title>
    
    <!-- 부트스트랩 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    
    	<!-- (2) 선을 그려 확인  -->

        <style>
				.col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6,
				.col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11,  .col-md-12 {
				  border: 1px solid red;
				  padding: 10px;
				}
				.row{
				  margin-bottom: 4px;
				  margin-top: 4px;
				  }
		</style>

</head>
<body>
	<!--  ............................................................................  -->   

  <div class="container">
    <div class="row">
      <!--  (1) 8칸 을 가정하고  -->
      <div class="col-md-1"> 1칸 </div>
      <div class="col-md-1"> 1칸 </div>
      <div class="col-md-1"> 1칸 </div>
      <div class="col-md-1"> 1칸 </div>
      <div class="col-md-1"> 1칸 </div>
      <div class="col-md-1"> 1칸 </div>
      <div class="col-md-1"> 1칸 </div>
      <div class="col-md-1"> 1칸 </div>
    </div>
      <div class="row">
          <div class="col-md-4">4칸</div>
          <div class="col-md-2">2칸</div>
          <div class="col-md-2">2칸</div>
      </div>
      <div class="row">
          <div class="col-md-2">2칸</div>
          <div class="col-md-2">2칸</div>
          <div class="col-md-2">2칸</div>
          <div class="col-md-2">2칸</div>
          <div class="col-md-2">2칸</div>
      </div>


  </div>

	<!--  ............................................................................  --> 
	
    <!-- jQuery (부트스트랩의 자바스크립트 플러그인을 위해 필요합니다) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
    <!-- 모든 컴파일된 플러그인을 포함합니다 (아래), 원하지 않는다면 필요한 각각의 파일을 포함하세요 -->
    <script src="js/bootstrap.min.js"></script>
</body>
</html>
~~~

## Ex04_grid2.html

~~~html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title> 기본 샘플 확인 </title>
    
    <!-- 부트스트랩 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">

     <style>
      @import url(http://fonts.googleapis.com/earlyaccess/nanumgothic.css);
      header{ height: 100px; background-color: rgba(125, 211, 242,0.5); border-radius: 15px; padding: 10px; margin:10px;font-family: 'Nanum Gothic', sans-serif;  text-align: center;}
      footer { text-align: center;}
      ul.nav { background-color:rgba(201, 201, 201,.5); padding: 10px; border-radius: 10px; }
    </style>

</head>
<body>
	<!--  ............................................................................  -->   

  <div class="container">       
   <header>
     <h2> 우리들의 사이트 </h2>
   </header>
   
    <div class="row">
    	<div class="col-md-2 col-md-push-10">
        <ul class="nav">
          <li>주제 1</li>
          <li>주제 2</li>
          <li>주제 3</li>
          <li>주제 4</li>
        </ul>
      </div>
      
     	<div class="col-md-10 col-md-pull-2">
	      <h3>1696년의 죽도(울릉도／다케시마) 도해금지령 </h3>
	       <p> 
		　1696년에 일본 막부는 어민들에게 울릉도에 건너가지 못하도록 명을 내렸습니다. 이를 죽도(울릉도／다케시마) 도해금지령이라고 하는데, 여기에는 독도도 포함됩니다.
		　안용복이 1693년 일본에 끌려가 울릉도와 독도를 우리나라 땅이라고 주장하고부터 일본은 울릉도를 빼앗아 가기 위해 여러 가지 방법을 썼습니다. 당시 막부는 최종 결정을 내리기에 앞서 돗토리번의 영주에게 두 섬이 어느 나라에 속하는지를 물었습니다(1695년 12월).
		　돗토리번의 영주는 조사해 본 결과, 조선에서 마쓰시마(독도)까지는 80~90해리, 마쓰시마(독도)에서 죽도(울릉도／다케시마)까지는 40해리, 그리고 일본 오키섬에서 마쓰시마(독도)까지는 80해리라는 사실을 알아냈습니다. 그래서 돗토리번 영주는 울릉도와 독도, 두 섬이 일본에 속한 섬이 아니라고 막부에 보고하였는데, 이러한 사실이 일본의 여러 기록에 남아 있습니다.
			 </p>
			
			<h3>1870년 일본 외무성의 보고서「조선국 교제시말 내탐서」</h3>
	        <p>1869년 12월 일본은 외무성 관리 3인을 조선에 보냈습니다. 이는 새로 들어선 일본 메이지 정부가 조선과 새로운 관계를 맺기 위해 조선에 관한 조사를 새로 할 필요를 느꼈기 때문입니다. 이때 관리들은 보고서에서 ‘죽도(울릉도／다케시마)와 마쓰시마(독도)가 조선 부속이 된 경위’를 적었습니다. 이 보고서로도 일본이 두 섬을 조선 영토로 인정하고 있었다는 것이 드러납니다. 
			</p>
		</div>
   
    </div>
    <hr>
   <footer>
    여기는 우리 사이트 기본 정보 
   </footer>
	</div>
	<!--  ............................................................................  --> 
	
    <!-- jQuery (부트스트랩의 자바스크립트 플러그인을 위해 필요합니다) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
    <!-- 모든 컴파일된 플러그인을 포함합니다 (아래), 원하지 않는다면 필요한 각각의 파일을 포함하세요 -->
    <script src="js/bootstrap.min.js"></script>
</body>
</html>
~~~

## Ex05_form.html

~~~html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title> 기본 샘플 확인 </title>
    
    <!-- 부트스트랩 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    
</head>
<body>
	<!--  ............................................................................  -->   

 <div class="container">
	 <h4 class="text-primary"> 기본적인 폼</h4>
	 <form role="form">

		 <label for="name">이름</label>
		 <input type="text" id='name1' placeholder="이름">
		 <label for="email">이메일</label>
		 <input type="email" id='email1' placeholder="이메일">
		 <button type="submit" > 확인</button>

	 </form>
	 <hr>
     
	    <h4 class="text-primary"> 부트스트랩의 폼</h4>
	    <form role="form" class="form-inline">
			<div class="form-group">
				<label for="name">이름</label>
				<input class="form-control" type="text" id='name' placeholder="이름">
			</div>
			<div class="form-group">
				<label for="email">이메일</label>
				<input class="form-control" type="email" id='email' placeholder="이메일">
			</div>
	        <div class="form-group">
				<button type="submit" > 확인</button>
			</div>

	    </form>	
	    <hr>
	

    
    
     </div>

	<!--  ............................................................................  --> 
	
    <!-- jQuery (부트스트랩의 자바스크립트 플러그인을 위해 필요합니다) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
    <!-- 모든 컴파일된 플러그인을 포함합니다 (아래), 원하지 않는다면 필요한 각각의 파일을 포함하세요 -->
    <script src="js/bootstrap.min.js"></script>
</body>
</html>
~~~

## Ex04_formSample.html

~~~html
<!doctype html>
<html lang="ko-kr">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>부트스트랩   </title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">

  </head>
<body>
 <div class="container">  
     
    <h4 class="text-primary"> 부트스트랩의 폼 </h4>
        <h5> input type="text"</h5>
            <input type="text" class="form-control">
        <h5> input type="password"</h5>  
            <input type="password" class="form-control">         
        <h5> input type="date" </h5> 
            <input type="date" class="form-control"> 
        <h5>input  type="month"</h5> 
            <input type="month" class="form-control"> 
        <h5> type="week"</h5>
            <input type="week" class="form-control">
        <h5> textarea</h5>     
            <textarea  rows="5" class="form-control"></textarea>

        <h5> input type="checkbox"</h5>         
        <div class="checkbox">
          <label>
            <input type="checkbox" value="">
            여기는 체크박스가 적용되는 곳입니다. 
          </label>
        </div>
        <div class="checkbox">
          <label>
            <input type="checkbox" value="">
            체크박스는 다중 선택이 가능합니다.
          </label>
        </div> 

        <h5> input type="radio"</h5> 
        <div class="radio">
          <label>
            <input type="radio" name="optionsRadios" id="optionsRadios1" value="option1" checked>
            여기는 라디오 속성이 적용되는 곳입니다.
          </label>
        </div>
        <div class="radio">
          <label>
            <input type="radio" name="optionsRadios" id="optionsRadios2" value="option2">
            라디오 속성은 여러 개 중 하나를 선택할 경우 사용합니다.
          </label>
        </div>

        <h5> 인라인 체크 박스 label class="checkbox-inline"</h5>  
        <label class="checkbox-inline">
          <input type="checkbox" id="inlineCheckbox1" value="option1"> 1
        </label>
        <label class="checkbox-inline">
          <input type="checkbox" id="inlineCheckbox2" value="option2"> 2
        </label>
        <label class="checkbox-inline">
          <input type="checkbox" id="inlineCheckbox3" value="option3"> 3
        </label>

        <h5> 인라인 라디오 label class="radio-inline"</h5>  
            <label class="radio-inline">
               <input type="radio" name="optionsRadios" id="optionsRadios1" value="option1" checked>  r1
            </label> 
            <label class="radio-inline">
               <input type="radio" name="optionsRadios" id="optionsRadios1" value="option1" checked>  r2
            </label> 
            <label class="radio-inline">
               <input type="radio" name="optionsRadios" id="optionsRadios1" value="option1" checked>  r3
            </label> 
    

        <h5> select는 기본값과 multiple 적용이 가능합니다.</h5> 
        <select class="form-control">
          <option>1</option>
          <option>2</option>
          <option>3</option>
          <option>4</option>
          <option>5</option>
        </select>
         <br>
        <select multiple class="form-control">
          <option>1</option>
          <option>2</option>
          <option>3</option>
          <option>4</option>
          <option>5</option>
        </select>  
        <h5>폼에 텍스트를 삽입 하기 위해선 p class="form-control-static" 속성을 적용한다. </h5> 
        <form class="form-horizontal" role="form">
          <div class="form-group">
            <label class="col-sm-2 col-lg-2 control-label">이메일</label>
            <div class="col-sm-10 col-lg-10">
              <p class="form-control-static">email@example.com</p>
            </div>
          </div>
          <div class="form-group">
            <label for="Password" class="col-sm-2 col-lg-2 control-label">비밀번호</label>
            <div class="col-sm-10 col-lg-10">
              <input type="password" class="form-control" id="inputPassword" placeholder="Password">
            </div>
          </div>
        </form>
        <h5>form-control-static을 적용하지 않을 경우 텍스트가 label 부분과의 정렬이 틀어진다. </h5> 
        <form class="form-horizontal" role="form">
          <div class="form-group">
            <label class="col-sm-2 col-lg-2 control-label">이메일</label>
            <div class="col-sm-10 col-lg-10">
              <p>email@example.com</p>
            </div>
          </div>

         <h5> input 부분이 disabled 상태일 때</h5>
        <input type="text" class="form-control" disabled placeholder="이 부분은 disabled 상태입니다.">   

        <hr>
        <form class="form-horizontal" role="form">  
        <fieldset>
          <legend>기본정보 </legend>
        <div class="form-group">   
            <label for="Name" class="col-xs-2 col-lg-2 control-label">이름</label>
            <div class="col-xs-10 col-lg-10">
                <input type="text" class="form-control" placeholder="이름"> 
            </div>
        </div>
        <div class="form-group">   
            <label for="email" class="col-xs-2 col-lg-2 control-label">이메일</label>
            <div class="col-xs-10 col-lg-10">
                <input type="email" class="form-control" placeholder="이메일"> 
            </div>
        </div>
        </fieldset>
        <fieldset disabled> 
         <legend>부가정보 </legend>      
        <div class="form-group">  
            <label for="address" class="col-xs-2 col-lg-2 control-label">주소</label>
            <div class="col-xs-10 col-lg-10">
                <input type="text" class="form-control" placeholder="주소"> 
            </div>
          </div> 
        </fieldset>         
        </form> 
       
       <hr>
       <h5> input 값에 다양한 메시지를 담을 수 있다.  </h5>
        <div class="form-group has-success">
          <label class="control-label" for="inputSuccess">Input값이 성공적일(문제없을) 경우</label>
          <input type="text" class="form-control" id="inputSuccess">
        </div>
        <div class="form-group has-warning">
          <label class="control-label" for="inputWarning">Input 값에 문제가 있어 경고를 내보낼 경우 </label>
          <input type="text" class="form-control" id="inputWarning">
        </div>
        <div class="form-group has-error">
          <label class="control-label" for="inputError">Input값에 에러가 있을 때 </label>
          <input type="text" class="form-control" id="inputError">
        </div>

        <hr>
        <h5> input-lg, 기본값, input-sm 일 경우 크기 비교  </h5>
          <input type="text" class="form-control input-lg" placeholder="input-lg">  <br>   
          <input type="text" class="form-control" placeholder="기본값">           <br>       
          <input type="text" class="form-control input-sm" placeholder="input-sm">  


        <hr>
        <h5> 그리드 시스템을 이용해서 컬럼 크기 조절 </h5>
        <div class="row">
          <div class="col-sm-2 col-lg-2">
            <input type="text" class="form-control" placeholder="col-sm-2 col-lg-2">
          </div>
          <div class="col-sm-3  col-lg-3">
            <input type="text" class="form-control" placeholder="col-sm-3  col-lg-3">
          </div>
          <div class="col-sm-4  col-lg-4">
            <input type="text" class="form-control" placeholder="col-sm-4  col-lg-4">
          </div>
        </div>  

        <hr>
        <h5> input 부분에 대한 도움말 </h5>
         <input type="text" class="form-control" placeholder="핸드폰 번호">   
         <span class="help-block"> 이 예문은 양용석 저자의 '부트스트랩으로 디자인하라'에서 참고하였습니다.</span>        
     <div style="height:100px"></div>    
 </div> 
</body>
</html>
~~~

## Ex06_glyphicons.html

~~~html
<!doctype html>
<html lang="ko-kr">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>부트스트랩  CSS</title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <style>
      .red { color: #FF0000}
      .blue { color: #0080FF}
    </style>
 
  </head>
<body>
 <div class="container">  

	<!-- glyphicons_sample 파일에서 확인하며 클래스명을 복사하는 것을 권장  -->
    <hr>
          <span class=" glyphicon glyphicon-globe"> </span> 123-4567-8912
    
    <hr>
    <div class="btn-toolbar">
        <div class="btn-group">
          <button type="button" class="btn btn-default">
            <span class="glyphicon glyphicon-time"> </span>
          </button>
          <button type="button" class="btn btn-default">
            <span class="glyphicon glyphicon-tree-deciduous"> </span>
          </button>
          <button type="button" class="btn btn-default">
            <span class="glyphicon glyphicon-volume-off"> </span>
          </button>
          <button type="button" class="btn btn-default">
            <span class="glyphicon glyphicon-tasks"> </span>
          </button>
        </div>
    </div>   

    <hr>   
          <button type="button" class="btn btn-default btn-lg">
            <span class=""> </span> 설정 
          </button>
          <button type="button" class="btn btn-default">
            <span class=""> </span> 설정 
          </button>    
          <button type="button" class="btn btn-default btn-sm">
            <span class=""> </span> 설정 
          </button>
          <button type="button" class="btn btn-default btn-xs">
            <span class=""> </span> 설정 
          </button>
    <hr>
  
        <button type="button" class="btn btn-default">
            <span class="glyphicon glyphicon-thumbs-up"> </span> 좋아요
        </button>

        <button type="button" class="btn btn-default red">
            <span class="glyphicon glyphicon-thumbs-up"> </span> 좋아요
        </button> 

        <button type="button" class="btn btn-default blue">
            <span class="glyphicon glyphicon-thumbs-up"> </span> 좋아요
        </button> 

   <hr>
       <button type="button" class="btn btn-lg btn-primary">
          <span class="glyphicon glyphicon-euro"> </span>  3,000
       </button>
       <button type="button" class="btn btn-lg btn-warning">   
          <span class="glyphicon glyphicon-question-sign"> </span> Info
       </button>
       <button type="button" class="btn btn-lg btn-danger">    
          <span class="glyphicon glyphicon-facetime-video"> </span> video
       </button>         
 </div> 
</body>
</html>
~~~

## Ex06_glyphiconsSample.html

~~~html
<!--  
이 페이지는  http://bootstrapk.com/components/ 에 있는 내용이므로 사이트에서 확인 하는 것을 권장 합니다.  
-->

<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>부트스트랩  CSS</title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">

  </head>
  
<body>
 <div class="container">  
   <h1>Glyphicons : 특수문자 글꼴 </h1>
   <ul class="bs-glyphicons">
      <li><span class="glyphicon glyphicon-adjust"></span> glyphicon glyphicon-adjust</li>
      <li><span class="glyphicon glyphicon-align-center"></span> glyphicon glyphicon-align-center</li>
      <li><span class="glyphicon glyphicon-align-justify"></span> glyphicon glyphicon-align-justify</li>
      <li><span class="glyphicon glyphicon-align-left"></span> glyphicon glyphicon-align-left</li>
      <li><span class="glyphicon glyphicon-align-right"></span> glyphicon glyphicon-align-right</li>
      <li><span class="glyphicon glyphicon-arrow-down"></span> glyphicon glyphicon-arrow-down</li>
      <li><span class="glyphicon glyphicon-arrow-left"></span> glyphicon glyphicon-arrow-left</li>
      <li><span class="glyphicon glyphicon-arrow-right"></span> glyphicon glyphicon-arrow-right</li>
      <li><span class="glyphicon glyphicon-arrow-up"></span> glyphicon glyphicon-arrow-up</li>
      <li><span class="glyphicon glyphicon-asterisk"></span> glyphicon glyphicon-asterisk</li>
      <li><span class="glyphicon glyphicon-backward"></span> glyphicon glyphicon-backward</li>
      <li><span class="glyphicon glyphicon-ban-circle"></span> glyphicon glyphicon-ban-circle</li>
      <li><span class="glyphicon glyphicon-barcode"></span> glyphicon glyphicon-barcode</li>
      <li><span class="glyphicon glyphicon-bell"></span> glyphicon glyphicon-bell</li>
      <li><span class="glyphicon glyphicon-bold"></span> glyphicon glyphicon-bold</li>
      <li><span class="glyphicon glyphicon-book"></span> glyphicon glyphicon-book</li>
      <li><span class="glyphicon glyphicon-bookmark"></span> glyphicon glyphicon-bookmark</li>
      <li><span class="glyphicon glyphicon-briefcase"></span> glyphicon glyphicon-briefcase</li>
      <li><span class="glyphicon glyphicon-bullhorn"></span> glyphicon glyphicon-bullhorn</li>
      <li><span class="glyphicon glyphicon-calendar"></span> glyphicon glyphicon-calendar</li>
      <li><span class="glyphicon glyphicon-camera"></span> glyphicon glyphicon-camera</li>
      <li><span class="glyphicon glyphicon-certificate"></span> glyphicon glyphicon-certificate</li>
      <li><span class="glyphicon glyphicon-check"></span> glyphicon glyphicon-check</li>
      <li><span class="glyphicon glyphicon-chevron-down"></span> glyphicon glyphicon-chevron-down</li>
      <li><span class="glyphicon glyphicon-chevron-left"></span> glyphicon glyphicon-chevron-left</li>
      <li><span class="glyphicon glyphicon-chevron-right"></span> glyphicon glyphicon-chevron-right</li>
      <li><span class="glyphicon glyphicon-chevron-up"></span> glyphicon glyphicon-chevron-up</li>
      <li><span class="glyphicon glyphicon-circle-arrow-down"></span> glyphicon glyphicon-circle-arrow-down</li>
      <li><span class="glyphicon glyphicon-circle-arrow-left"></span> glyphicon glyphicon-circle-arrow-left</li>
      <li><span class="glyphicon glyphicon-circle-arrow-right"></span> glyphicon glyphicon-circle-arrow-right</li>
      <li><span class="glyphicon glyphicon-circle-arrow-up"></span> glyphicon glyphicon-circle-arrow-up</li>
      <li><span class="glyphicon glyphicon-cloud"></span> glyphicon glyphicon-cloud</li>
      <li><span class="glyphicon glyphicon-cloud-download"></span> glyphicon glyphicon-cloud-download</li>
      <li><span class="glyphicon glyphicon-cloud-upload"></span> glyphicon glyphicon-cloud-upload</li>
      <li><span class="glyphicon glyphicon-cog"></span> glyphicon glyphicon-cog</li>
      <li><span class="glyphicon glyphicon-collapse-down"></span> glyphicon glyphicon-collapse-down</li>
      <li><span class="glyphicon glyphicon-collapse-up"></span> glyphicon glyphicon-collapse-up</li>
      <li><span class="glyphicon glyphicon-comment"></span> glyphicon glyphicon-comment</li>
      <li><span class="glyphicon glyphicon-compressed"></span> glyphicon glyphicon-compressed</li>
      <li><span class="glyphicon glyphicon-copyright-mark"></span> glyphicon glyphicon-copyright-mark</li>
      <li><span class="glyphicon glyphicon-credit-card"></span> glyphicon glyphicon-credit-card</li>
      <li><span class="glyphicon glyphicon-cutlery"></span> glyphicon glyphicon-cutlery</li>
      <li><span class="glyphicon glyphicon-dashboard"></span> glyphicon glyphicon-dashboard</li>
      <li><span class="glyphicon glyphicon-download"></span> glyphicon glyphicon-download</li>
      <li><span class="glyphicon glyphicon-download-alt"></span> glyphicon glyphicon-download-alt</li>
      <li><span class="glyphicon glyphicon-earphone"></span> glyphicon glyphicon-earphone</li>
      <li><span class="glyphicon glyphicon-edit"></span> glyphicon glyphicon-edit</li>
      <li><span class="glyphicon glyphicon-eject"></span> glyphicon glyphicon-eject</li>
      <li><span class="glyphicon glyphicon-envelope"></span> glyphicon glyphicon-envelope</li>
      <li><span class="glyphicon glyphicon-euro"></span> glyphicon glyphicon-euro</li>
      <li><span class="glyphicon glyphicon-exclamation-sign"></span> glyphicon glyphicon-exclamation-sign</li>
      <li><span class="glyphicon glyphicon-expand"></span> glyphicon glyphicon-expand</li>
      <li><span class="glyphicon glyphicon-export"></span> glyphicon glyphicon-export</li>
      <li><span class="glyphicon glyphicon-eye-close"></span> glyphicon glyphicon-eye-close</li>
      <li><span class="glyphicon glyphicon-eye-open"></span> glyphicon glyphicon-eye-open</li>
      <li><span class="glyphicon glyphicon-facetime-video"></span> glyphicon glyphicon-facetime-video</li>
      <li><span class="glyphicon glyphicon-fast-backward"></span> glyphicon glyphicon-fast-backward</li>
      <li><span class="glyphicon glyphicon-fast-forward"></span> glyphicon glyphicon-fast-forward</li>
      <li><span class="glyphicon glyphicon-file"></span> glyphicon glyphicon-file</li>
      <li><span class="glyphicon glyphicon-film"></span> glyphicon glyphicon-film</li>
      <li><span class="glyphicon glyphicon-filter"></span> glyphicon glyphicon-filter</li>
      <li><span class="glyphicon glyphicon-fire"></span> glyphicon glyphicon-fire</li>
      <li><span class="glyphicon glyphicon-flag"></span> glyphicon glyphicon-flag</li>
      <li><span class="glyphicon glyphicon-flash"></span> glyphicon glyphicon-flash</li>
      <li><span class="glyphicon glyphicon-floppy-disk"></span> glyphicon glyphicon-floppy-disk</li>
      <li><span class="glyphicon glyphicon-floppy-open"></span> glyphicon glyphicon-floppy-open</li>
      <li><span class="glyphicon glyphicon-floppy-remove"></span> glyphicon glyphicon-floppy-remove</li>
      <li><span class="glyphicon glyphicon-floppy-save"></span> glyphicon glyphicon-floppy-save</li>
      <li><span class="glyphicon glyphicon-floppy-saved"></span> glyphicon glyphicon-floppy-saved</li>
      <li><span class="glyphicon glyphicon-folder-close"></span> glyphicon glyphicon-folder-close</li>
      <li><span class="glyphicon glyphicon-folder-open"></span> glyphicon glyphicon-folder-open</li>
      <li><span class="glyphicon glyphicon-font"></span> glyphicon glyphicon-font</li>
      <li><span class="glyphicon glyphicon-forward"></span> glyphicon glyphicon-forward</li>
      <li><span class="glyphicon glyphicon-fullscreen"></span> glyphicon glyphicon-fullscreen</li>
      <li><span class="glyphicon glyphicon-gbp"></span> glyphicon glyphicon-gbp</li>
      <li><span class="glyphicon glyphicon-gift"></span> glyphicon glyphicon-gift</li>
      <li><span class="glyphicon glyphicon-glass"></span> glyphicon glyphicon-glass</li>
      <li><span class="glyphicon glyphicon-globe"></span> glyphicon glyphicon-globe</li>
      <li><span class="glyphicon glyphicon-hand-down"></span> glyphicon glyphicon-hand-down</li>
      <li><span class="glyphicon glyphicon-hand-left"></span> glyphicon glyphicon-hand-left</li>
      <li><span class="glyphicon glyphicon-hand-right"></span> glyphicon glyphicon-hand-right</li>
      <li><span class="glyphicon glyphicon-hand-up"></span> glyphicon glyphicon-hand-up</li>
      <li><span class="glyphicon glyphicon-hd-video"></span> glyphicon glyphicon-hd-video</li>
      <li><span class="glyphicon glyphicon-hdd"></span> glyphicon glyphicon-hdd</li>
      <li><span class="glyphicon glyphicon-header"></span> glyphicon glyphicon-header</li>
      <li><span class="glyphicon glyphicon-headphones"></span> glyphicon glyphicon-headphones</li>
      <li><span class="glyphicon glyphicon-heart"></span> glyphicon glyphicon-heart</li>
      <li><span class="glyphicon glyphicon-heart-empty"></span> glyphicon glyphicon-heart-empty</li>
      <li><span class="glyphicon glyphicon-home"></span> glyphicon glyphicon-home</li>
      <li><span class="glyphicon glyphicon-import"></span> glyphicon glyphicon-import</li>
      <li><span class="glyphicon glyphicon-inbox"></span> glyphicon glyphicon-inbox</li>
      <li><span class="glyphicon glyphicon-indent-left"></span> glyphicon glyphicon-indent-left</li>
      <li><span class="glyphicon glyphicon-indent-right"></span> glyphicon glyphicon-indent-right</li>
      <li><span class="glyphicon glyphicon-info-sign"></span> glyphicon glyphicon-info-sign</li>
      <li><span class="glyphicon glyphicon-italic"></span> glyphicon glyphicon-italic</li>
      <li><span class="glyphicon glyphicon-leaf"></span> glyphicon glyphicon-leaf</li>
      <li><span class="glyphicon glyphicon-link"></span> glyphicon glyphicon-link</li>
      <li><span class="glyphicon glyphicon-list"></span> glyphicon glyphicon-list</li>
      <li><span class="glyphicon glyphicon-list-alt"></span> glyphicon glyphicon-list-alt</li>
      <li><span class="glyphicon glyphicon-lock"></span> glyphicon glyphicon-lock</li>
      <li><span class="glyphicon glyphicon-log-in"></span> glyphicon glyphicon-log-in</li>
      <li><span class="glyphicon glyphicon-log-out"></span> glyphicon glyphicon-log-out</li>
      <li><span class="glyphicon glyphicon-magnet"></span> glyphicon glyphicon-magnet</li>
      <li><span class="glyphicon glyphicon-map-marker"></span> glyphicon glyphicon-map-marker</li>
      <li><span class="glyphicon glyphicon-minus"></span> glyphicon glyphicon-minus</li>
      <li><span class="glyphicon glyphicon-minus-sign"></span> glyphicon glyphicon-minus-sign</li>
      <li><span class="glyphicon glyphicon-move"></span> glyphicon glyphicon-move</li>
      <li><span class="glyphicon glyphicon-music"></span> glyphicon glyphicon-music</li>
      <li><span class="glyphicon glyphicon-new-window"></span> glyphicon glyphicon-new-window</li>
      <li><span class="glyphicon glyphicon-off"></span> glyphicon glyphicon-off</li>
      <li><span class="glyphicon glyphicon-ok"></span> glyphicon glyphicon-ok</li>
      <li><span class="glyphicon glyphicon-ok-circle"></span> glyphicon glyphicon-ok-circle</li>
      <li><span class="glyphicon glyphicon-ok-sign"></span> glyphicon glyphicon-ok-sign</li>
      <li><span class="glyphicon glyphicon-open"></span> glyphicon glyphicon-open</li>
      <li><span class="glyphicon glyphicon-paperclip"></span> glyphicon glyphicon-paperclip</li>
      <li><span class="glyphicon glyphicon-pause"></span> glyphicon glyphicon-pause</li>
      <li><span class="glyphicon glyphicon-pencil"></span> glyphicon glyphicon-pencil</li>
      <li><span class="glyphicon glyphicon-phone"></span> glyphicon glyphicon-phone</li>
      <li><span class="glyphicon glyphicon-phone-alt"></span> glyphicon glyphicon-phone-alt</li>
      <li><span class="glyphicon glyphicon-picture"></span> glyphicon glyphicon-picture</li>
      <li><span class="glyphicon glyphicon-plane"></span> glyphicon glyphicon-plane</li>
      <li><span class="glyphicon glyphicon-play"></span> glyphicon glyphicon-play</li>
      <li><span class="glyphicon glyphicon-play-circle"></span> glyphicon glyphicon-play-circle</li>
      <li><span class="glyphicon glyphicon-plus"></span> glyphicon glyphicon-plus</li>
      <li><span class="glyphicon glyphicon-plus-sign"></span> glyphicon glyphicon-plus-sign</li>
      <li><span class="glyphicon glyphicon-print"></span> glyphicon glyphicon-print</li>
      <li><span class="glyphicon glyphicon-pushpin"></span> glyphicon glyphicon-pushpin</li>
      <li><span class="glyphicon glyphicon-qrcode"></span> glyphicon glyphicon-qrcode</li>
      <li><span class="glyphicon glyphicon-question-sign"></span> glyphicon glyphicon-question-sign</li>
      <li><span class="glyphicon glyphicon-random"></span> glyphicon glyphicon-random</li>
      <li><span class="glyphicon glyphicon-record"></span> glyphicon glyphicon-record</li>
      <li><span class="glyphicon glyphicon-refresh"></span> glyphicon glyphicon-refresh</li>
      <li><span class="glyphicon glyphicon-registration-mark"></span> glyphicon glyphicon-registration-mark</li>
      <li><span class="glyphicon glyphicon-remove"></span> glyphicon glyphicon-remove</li>
      <li><span class="glyphicon glyphicon-remove-circle"></span> glyphicon glyphicon-remove-circle</li>
      <li><span class="glyphicon glyphicon-remove-sign"></span> glyphicon glyphicon-remove-sign</li>
      <li><span class="glyphicon glyphicon-repeat"></span> glyphicon glyphicon-repeat</li>
      <li><span class="glyphicon glyphicon-resize-full"></span> glyphicon glyphicon-resize-full</li>
      <li><span class="glyphicon glyphicon-resize-horizontal"></span> glyphicon glyphicon-resize-horizontal</li>
      <li><span class="glyphicon glyphicon-resize-small"></span> glyphicon glyphicon-resize-small</li>
      <li><span class="glyphicon glyphicon-resize-vertical"></span> glyphicon glyphicon-resize-vertical</li>
      <li><span class="glyphicon glyphicon-retweet"></span> glyphicon glyphicon-retweet</li>
      <li><span class="glyphicon glyphicon-road"></span> glyphicon glyphicon-road</li>
      <li><span class="glyphicon glyphicon-save"></span> glyphicon glyphicon-save</li>
      <li><span class="glyphicon glyphicon-saved"></span> glyphicon glyphicon-saved</li>
      <li><span class="glyphicon glyphicon-screenshot"></span> glyphicon glyphicon-screenshot</li>
      <li><span class="glyphicon glyphicon-sd-video"></span> glyphicon glyphicon-sd-video</li>
      <li><span class="glyphicon glyphicon-search"></span> glyphicon glyphicon-search</li>
      <li><span class="glyphicon glyphicon-send"></span> glyphicon glyphicon-send</li>
      <li><span class="glyphicon glyphicon-share"></span> glyphicon glyphicon-share</li>
      <li><span class="glyphicon glyphicon-share-alt"></span> glyphicon glyphicon-share-alt</li>
      <li><span class="glyphicon glyphicon-shopping-cart"></span> glyphicon glyphicon-shopping-cart</li>
      <li><span class="glyphicon glyphicon-signal"></span> glyphicon glyphicon-signal</li>
      <li><span class="glyphicon glyphicon-sort"></span> glyphicon glyphicon-sort</li>
      <li><span class="glyphicon glyphicon-sort-by-alphabet"></span> glyphicon glyphicon-sort-by-alphabet</li>
      <li><span class="glyphicon glyphicon-sort-by-alphabet-alt"></span> glyphicon glyphicon-sort-by-alphabet-alt</li>
      <li><span class="glyphicon glyphicon-sort-by-attributes"></span> glyphicon glyphicon-sort-by-attributes</li>
      <li><span class="glyphicon glyphicon-sort-by-attributes-alt"></span> glyphicon glyphicon-sort-by-attributes-alt</li>
      <li><span class="glyphicon glyphicon-sort-by-order"></span> glyphicon glyphicon-sort-by-order</li>
      <li><span class="glyphicon glyphicon-sort-by-order-alt"></span> glyphicon glyphicon-sort-by-order-alt</li>
      <li><span class="glyphicon glyphicon-sound-5-1"></span> glyphicon glyphicon-sound-5-1</li>
      <li><span class="glyphicon glyphicon-sound-6-1"></span> glyphicon glyphicon-sound-6-1</li>
      <li><span class="glyphicon glyphicon-sound-7-1"></span> glyphicon glyphicon-sound-7-1</li>
      <li><span class="glyphicon glyphicon-sound-dolby"></span> glyphicon glyphicon-sound-dolby</li>
      <li><span class="glyphicon glyphicon-sound-stereo"></span> glyphicon glyphicon-sound-stereo</li>
      <li><span class="glyphicon glyphicon-star"></span> glyphicon glyphicon-star</li>
      <li><span class="glyphicon glyphicon-star-empty"></span> glyphicon glyphicon-star-empty</li>
      <li><span class="glyphicon glyphicon-stats"></span> glyphicon glyphicon-stats</li>
      <li><span class="glyphicon glyphicon-step-backward"></span> glyphicon glyphicon-step-backward</li>
      <li><span class="glyphicon glyphicon-step-forward"></span> glyphicon glyphicon-step-forward</li>
      <li><span class="glyphicon glyphicon-stop"></span> glyphicon glyphicon-stop</li>
      <li><span class="glyphicon glyphicon-subtitles"></span> glyphicon glyphicon-subtitles</li>
      <li><span class="glyphicon glyphicon-tag"></span> glyphicon glyphicon-tag</li>
      <li><span class="glyphicon glyphicon-tags"></span> glyphicon glyphicon-tags</li>
      <li><span class="glyphicon glyphicon-tasks"></span> glyphicon glyphicon-tasks</li>
      <li><span class="glyphicon glyphicon-text-height"></span> glyphicon glyphicon-text-height</li>
      <li><span class="glyphicon glyphicon-text-width"></span> glyphicon glyphicon-text-width</li>
      <li><span class="glyphicon glyphicon-th"></span> glyphicon glyphicon-th</li>
      <li><span class="glyphicon glyphicon-th-large"></span> glyphicon glyphicon-th-large</li>
      <li><span class="glyphicon glyphicon-th-list"></span> glyphicon glyphicon-th-list</li>
      <li><span class="glyphicon glyphicon-thumbs-down"></span> glyphicon glyphicon-thumbs-down</li>
      <li><span class="glyphicon glyphicon-thumbs-up"></span> glyphicon glyphicon-thumbs-up</li>
      <li><span class="glyphicon glyphicon-time"></span> glyphicon glyphicon-time</li>
      <li><span class="glyphicon glyphicon-tint"></span> glyphicon glyphicon-tint</li>
      <li><span class="glyphicon glyphicon-tower"></span> glyphicon glyphicon-tower</li>
      <li><span class="glyphicon glyphicon-transfer"></span> glyphicon glyphicon-transfer</li>
      <li><span class="glyphicon glyphicon-trash"></span> glyphicon glyphicon-trash</li>
      <li><span class="glyphicon glyphicon-tree-conifer"></span> glyphicon glyphicon-tree-conifer</li>
      <li><span class="glyphicon glyphicon-tree-deciduous"></span> glyphicon glyphicon-tree-deciduous</li>
      <li><span class="glyphicon glyphicon-unchecked"></span> glyphicon glyphicon-unchecked</li>
      <li><span class="glyphicon glyphicon-upload"></span> glyphicon glyphicon-upload</li>
      <li><span class="glyphicon glyphicon-usd"></span> glyphicon glyphicon-usd</li>
      <li><span class="glyphicon glyphicon-user"></span> glyphicon glyphicon-user</li>
      <li><span class="glyphicon glyphicon-volume-down"></span> glyphicon glyphicon-volume-down</li>
      <li><span class="glyphicon glyphicon-volume-off"></span> glyphicon glyphicon-volume-off</li>
      <li><span class="glyphicon glyphicon-volume-up"></span> glyphicon glyphicon-volume-up</li>
      <li><span class="glyphicon glyphicon-warning-sign"></span> glyphicon glyphicon-warning-sign</li>
      <li><span class="glyphicon glyphicon-wrench"></span> glyphicon glyphicon-wrench</li>
      <li><span class="glyphicon glyphicon-zoom-in"></span> glyphicon glyphicon-zoom-in</li>
      <li><span class="glyphicon glyphicon-zoom-out"></span> glyphicon glyphicon-zoom-out</li>
    </ul>



 </div> 
</body>
</html>
~~~

## Ex07_panelSample.html

~~~html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>부트스트랩</title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">

    <style>
      body { padding-bottom: 50px;}
    </style>
 
  </head>
<body>

<div class="container"> 

 <h1>패널  </h1>

  <h4>기본 패널 </h4>
    <div class="panel panel-default">
      <div class="panel-body">
        Basic panel example
      </div>
    </div>

<div class="col-sm-4">
  <h4>기본 패널 상단  </h4>
    <div class="panel panel-default">
      <div class="panel-heading">Panel heading without title</div>
      <div class="panel-body">
        Panel content
      </div>
    </div>
</div>
<div class="col-sm-4">
  <h4>기본 패널 상단 h태그 이용   </h4>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Panel title</h3>
      </div>
      <div class="panel-body">
        Panel content
      </div>
    </div>
</div>
<div class="col-sm-4">     
  <h4>기본 패널 하단  </h4>
    <div class="panel panel-default">
      <div class="panel-body">
        Panel content
      </div>
      <div class="panel-footer">Panel footer</div>
    </div>
</div>
  <h4>패널에 색상 적용  </h4>
  <div class="col-sm-4">    
    <div class="panel panel-primary">
        <div class="panel-heading">
        <h3 class="panel-title">Panel title</h3>
      </div>
      <div class="panel-body">
        Panel content
      </div>
    </div>
  </div>
  <div class="col-sm-4">  
    <div class="panel panel-success">
        <div class="panel-heading">
        <h3 class="panel-title">Panel title</h3>
      </div>
      <div class="panel-body">
        Panel content
      </div>
   </div>
  </div>
   <div class="col-sm-4">  
    <div class="panel panel-info">
        <div class="panel-heading">
        <h3 class="panel-title">Panel title</h3>
      </div>
      <div class="panel-body">
        Panel content
      </div>
    </div>
  </div>

  <div class="col-sm-4">  
    <div class="panel panel-warning">
        <div class="panel-heading">
        <h3 class="panel-title">Panel title</h3>
      </div>
      <div class="panel-body">
        Panel content
      </div>
    </div>
   </div> 
  <div class="col-sm-4">      
    <div class="panel panel-danger">
        <div class="panel-heading">
        <h3 class="panel-title">Panel title</h3>
      </div>
      <div class="panel-body">
        Panel content
      </div>
    </div>
  </div>
 <div class="clearfix"></div>

  <h4>패널 내부에 테이블 적용  패널 내부에 panel-body 있는 경우   </h4>
    <div class="panel panel-default">
      <div class="panel-heading">패널 제목 </div>
      <div class="panel-body">
        Panel content
      </div>
       <table class="table">
        <thead>
         <tr>
           <th>번 호 </th>
           <th>제 목</th>
           <th>글쓴이</th>
         </tr>
         </thead>
         <tr>
           <td>1</td>
           <td>테이블 테스트  테이블 테스트 테이블 테스트 </td>
           <td>홍길동</td>
         </tr>
         <tr>
           <td>2</td>
           <td>테이블 테스트  테이블 테스트 테이블 테스트</td>
           <td>임꺽정 </td>
         </tr>
         <tr>
           <td>3</td>
           <td>테이블 테스트  테이블 테스트 테이블 테스트</td>
           <td>성춘향 </td>
         </tr>      
       </table>
    </div>

  <h4>패널 내부에 테이블 적용 패널 내부에 panel-body 없는 경우 </h4>
    <div class="panel panel-default">
      <div class="panel-heading">패널 제목 </div>
       <table class="table">
        <thead>
         <tr>
           <th>번 호 </th>
           <th>제 목</th>
           <th>글쓴이</th>
         </tr>
         </thead>
         <tr>
           <td>1</td>
           <td>테이블 테스트  테이블 테스트 테이블 테스트 </td>
           <td>홍길동</td>
         </tr>
         <tr>
           <td>2</td>
           <td>테이블 테스트  테이블 테스트 테이블 테스트</td>
           <td>임꺽정 </td>
         </tr>
         <tr>
           <td>3</td>
           <td>테이블 테스트  테이블 테스트 테이블 테스트</td>
           <td>성춘향 </td>
         </tr>      
       </table>
    </div>

  <h4>패널 내부에 리스트 그룹 적용  </h4>
    <div class="panel panel-default">
      <div class="panel-heading">패널 제목 </div>
            <div class="panel-body">
              Panel content
            </div>
        <ul class="list-group">
          <li class="list-group-item">기본 목록 </li>
          <li class="list-group-item">기본 목록 2</li>
          <li class="list-group-item">기본 목록 3</li>
        </ul>
    </div>    

</div> <!-- container 끝 -->

    <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
    <!-- Include all compiled plugins (below), or include individual files as needed -->
    <script src="../js/bootstrap.min.js"></script>
    <!-- 이 예문은 양용석 저자의 '부트스트랩으로 디자인하라'에서 참조하였습니다. -->
</body>
</html>
~~~

## Ex07_toggletab.html

~~~html
<!doctype html>
<html lang="ko-kr">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>부트스트랩</title>
    <link href="css/bootstrap.min.css" rel="stylesheet">
    
    <style>
    h2 { margin: 20px 0}
    .tab-content {padding: 10px 0;}
    </style>

  </head>
<body>
 <div class="container">  
    <h2>토글되는 탭 </h2>
    
    <ul class="nav nav-tabs">
      <li class="active"><a href="#home" data-toggle="tab">독도</a></li>
      <li><a href="#tab1" data-toggle="tab">독도이름</a></li>
      <li><a href="#tab2" data-toggle="tab">독도의 지형</a></li>
      <li><a href="#tab3" data-toggle="tab">독도의 우편</a></li>
    </ul>
   
  <!-- 중요: 위의 a href에서 연결되는 이름과 보여줄 id값이 동일해야 한다.  --> 
  <div class="tab-content">
    <div class="tab-pane active" id="home">
      <h3>독도 이야기 1</h3>
			울릉도 동남쪽 87.4㎞[4] 바다 위에 있는 대한민국 영토를 총칭한다. 날씨가 좋을 때 울릉도의 고지대에서 맨눈으로 볼 수 있다. 대한민국 정부의 실효지배 지역 중 한반도 본토에서 가장 멀리 떨어져 있다.[5]
			일본에서는 이 섬이 한국 영토가 아니라 시마네 현 오키 군 오키노시마 정(오키 제도)에 딸린 섬 '다케시마(竹島)'며 대한민국이 강제 점령하고 있으니 돌려받아야 한다고 억지스러운 주장을 한다. 이로 말미암아 벌어지는 외교 분쟁이 유명하다.[6] 반면 대한민국은 독도에 대해서 애초에 영토 분쟁은 없으며, 따라서 분쟁조차 되지 않는다는 입장을 명백히 하고 있으며 실효지배 또한 줄곧 대한민국이 하고 있다.
			북한 역시 한국이 불법 점거하고 있다고 하는데 일본과는 성격이 아주 다르다. 단지 휴전선 남쪽에 있어서 불법 점거 지역에 쏙 들어가버린 것. 즉, 조국이 남북으로 갈라져서 불법 점거 영토가 된 것이지, 근본적으로는 한민족 고유 영토라는 입장은 한국과 같다. 이 때문에 일본 주장에는 남북 모두 고유 영토인데 일본은 먹으려 들지 말라며 한 목소리를 내고 있다.   
    </div>
    <div class="tab-pane" id="tab1">
      <h3>독도 이야기 2</h3>
			한자로는 홀로 독(獨)자를 쓴다. 하지만 독도의 한자표기는 이런 한자 뜻과는 상관없이 그저 한자의 소리를 빌려 쓴 음차로, 진짜 뜻은 돌(石)의 서남 방언인 '독'이다[7]. '돌로 된 섬'이란 소리.[8] 독도는 동도(東島)와 서도(西島)라는 큰 두 암초와 크고 작은 89개 부속도서로 나뉜다. 실제로는 암초 하나로 이루어진 것이 아니라는 소리. 따지고 보면 노래 독도는 우리땅의 첫 소절인 '외로운 섬 하나'는 잘못된 셈이다. 물론 절해고도(絶海孤島)란 점에서 보면 '외로운 섬'이라는 이름도 어울리기는 하다.
  	</div>
	 <div class="tab-pane" id="tab2">
      <h3>독도 이야기 3</h3>
			많은 사람들이 잘 모르는 사실이지만 화산 분화로 형성되었고 지질학적 높이가 2,000m에 이른다. 수백만 년 전 신생대에 동해에서 분출한 화산이 오랜 세월이 지나면서 풍화되어 화산의 모습이 거의 다 사라지고, 나머지 부분은 평균 수심이 깊은 동해에 가려 잘 보이지 않는 것. 마찬가지로 화산섬인 울릉도는 여전히 화산의 모습이 희미하게나마 있다.[11] 독도의 해저 지형
	 </div>
    <div class="tab-pane" id="tab3">
      <h3>독도 이야기 4</h3>
	      독도경비대 및 1가구가 거주하며, 일대에 천연가스, 메탄 하이드레이트 등 자원이 풍부한 것으로 알려져 있다. 행정구역상 주소는 대한민국 경상북도 울릉군 울릉읍 독도리. 시설물로는 RKDD라는 ICAO 코드를 받은 헬리콥터 포트와 # 노무현 정부 시기 만들어진 접안시설, 어민숙소 등이 있다. 접안시설은 확장될 예정이다. RK**는 대한민국의 공항을 뜻하는 국가 코드이다. 일본은 RJ**.
		 독도에도 우체통이 있다. 이 우체통에 투함된 우편물은 2개월에 한 번 독도경비대함이 들어올 때 집배원이 수거한다. 서울 영등포구 기준으로 독도에서 투함한 우편물이 오는 경로는 독도⇨울릉우체국⇨포항우편집중국⇨대전교환센터⇨서서울우편집중국⇨영등포우체국⇨배달이다. 우편번호는 개정 전 6자리 번호가 799-805, 2015년 개정 후 5자리 번호는 40240이다.
      </div>
</div>

</div>
    <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
    <!-- Include all compiled plugins (below), or include individual files as needed -->
    <script src="js/bootstrap.min.js"></script>


</body>
</html>
~~~

## Ex08_carousel.html

~~~html
<!doctype html>
<html lang="ko-kr">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>부트스트랩</title>
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <style>
    h2 { margin: 20px 0} 
    </style>

  </head>
<body>
 <div class="container"> 

    <h2>캐러셀 슬라이드 효과  </h2>
        <div id="carousel-example-generic" class="carousel slide">
            <!-- Indicators -->
            <ol class="carousel-indicators">
              <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
              <li data-target="#carousel-example-generic" data-slide-to="1"></li>
              <li data-target="#carousel-example-generic" data-slide-to="2"></li>
            </ol>
             
             <!-- Carousel items -->
             <div class="carousel-inner">
                <div class="item active">
                   <img src="images/slide1.jpg" alt="First slide">
                </div>
                <div class="item">
                   <img src="images/slide2.jpg" alt="Second slide">
                </div>
                <div class="item">
                   <img src="images/slide3.jpg" alt="Third slide">
                </div>
             </div>
             
            <!-- Controls -->
              <a class="left carousel-control" href="#carousel-example-generic" data-slide="prev">
                <span class="icon-prev"></span>
              </a>
              <a class="right carousel-control" href="#carousel-example-generic" data-slide="next">
                <span class="icon-next"></span>
              </a>
          </div>
  </div>

<!--    케러셀 시작-->

<!-- 케러셀 끝-->
    <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
    <!-- Include all compiled plugins (below), or include individual files as needed -->
    <script src="js/bootstrap.min.js"></script>
    <script>
      $('.carousel').carousel({
          interval:1000
      })
    </script>
</body>
</html>
~~~

## Ex08_dropdown.html

~~~html
<!doctype html>
<html lang="ko-kr">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>부트스트랩</title>
    <link href="css/bootstrap.min.css" rel="stylesheet">
  </head>
<body>
 <div class="container">  
<h1> 드롭다운 </h1>

<hr>
<h4> 네비게이션 바 드롭다운 </h4>
    <nav role="navigation" class="navbar navbar-default">
      <!-- Brand and toggle get grouped for better mobile display -->
      <div class="navbar-header">
        <button class="navbar-toggle" type="button" data-toggle="collapse" data-target=".navbar-ex1-collapse">
          <span class="sr-only">Toggle navigation</span> <!-- sr-xxx : 라벨숨기는 것인데 시각장애인을 위한 screen reader에서 사용하도록  -->
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
        </button>
        <a class="navbar-brand" href="#">로고 </a>
      </div>
     
      <!-- Collect the nav links, forms, and other content for toggling -->
      <div class="collapse navbar-collapse navbar-ex1-collapse">
        <ul class="nav navbar-nav">
          <li class="dropdown">
            <a class="dropdown-toggle" href="#" data-toggle="dropdown">메뉴 1 <b class="caret"></b></a>
            <ul class="dropdown-menu">
              <li><a href="#">서브메뉴 1</a></li>
              <li><a href="#">서브메뉴 2</a></li>
              <li><a href="#">서브메뉴 3</a></li>
            </ul>
          </li>
          <li class="dropdown">
            <a class="dropdown-toggle" href="#"  data-toggle="dropdown">메뉴 2 <b class="caret"></b></a>
            <ul class="dropdown-menu">
              <li><a href="#">서브메뉴 1</a></li>
              <li><a href="#">서브메뉴 2</a></li>
              <li><a href="#">서브메뉴 3</a></li>
            </ul>
		</li>
          <li class="dropdown">
            <a class="dropdown-toggle" href="#" data-toggle="dropdown">메뉴 3 <b class="caret"></b></a>
            <ul class="dropdown-menu">
              <li><a href="#">서브메뉴 1</a></li>
              <li><a href="#">서브메뉴 2</a></li>
              <li><a href="#">서브메뉴 3</a></li>
            </ul>
          </li>
        </ul>
        

      </div><!-- /.navbar-collapse -->
    </nav>



</div>
    <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
    <!-- Include all compiled plugins (below), or include individual files as needed -->
    <script src="js/bootstrap.min.js"></script>

</body>
</html>
~~~

## Ex08_nav_sample.html

~~~html
<!doctype html>
<html lang="ko-kr">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title> 우리 가게  </title>
    <link href="css/bootstrap.min.css" rel="stylesheet">
    
  <!-- 부트스트랩을 이용하여 만들고 나서도 화면을 수정하고자 css를 추가해야 한다  --> 

    <style>
    @import url(http://fonts.googleapis.com/earlyaccess/nanumgothic.css);
    .navbar{ background-color: #fff; border: none; padding-bottom: 10px;  font-family: 'Nanum Gothic', sans-serif; font-weight: 300; font-size: 18px;height: 90px; text-transform: capitalize; border-bottom: 1px solid #AAAAAA}
    .navbar-toggle {position: relative;margin-top: 40px;top: 2px;}
    .navbar-nav { padding-right: 10px;margin-top: 20px; background-color: #fff}
    .navbar-nav li { margin:0 20px; }

      #carousel-example-generic {
        margin-top: 50px;
        width: 100%;
        hegiht: 450px;
      }
      #carousel-example-generic img {
        margin-left: 300px;
        width: 600px;
        height: 400px;
      }


    </style>

    
  </head>
<div>
  <div class="container-fluid">
    <!-- nav bar 부분 -->
    <div class="container">
          <nav class="navbar navbar-default navbar-fixed-top" role="navigation" id="navbar-scroll">
            <div class="container">
            <!-- Brand and toggle get grouped for better mobile display -->
            <div class="navbar-header">
              <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-1-collapse">                
              </button>
              <a class="navbar-brand" href="#"><img src="images/logo.png" alt="9PixelStudio"> </a>
            </div>
           
            <!-- Collect the nav links, forms, and other content for toggling -->
            <div class="collapse navbar-collapse navbar-right navbar-1-collapse">
              <ul class="nav navbar-nav">
                <li><a href="#"> 우리가게 소개 </a></li>  
                <li><a href="#"> 주력상품 </a></li>  
                <li><a href="#"> 소소한 이야기 </a></li>  
                <li><a href="#"> 찾아오시는 길 </a></li>                  
              </ul> 
            </div>
            </div><!-- /.navbar-collapse -->
          </nav>
     </div>   
    <!-- // nav bar 부분 끝 -->

    <!-- // 케러셀 부분 시작 -->
    <div id="carousel-example-generic" class="carousel slide">
      <!-- Indicators -->
      <ol class="carousel-indicators">
        <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
        <li data-target="#carousel-example-generic" data-slide-to="1"></li>
        <li data-target="#carousel-example-generic" data-slide-to="2"></li>
      </ol>

      <!-- Carousel items -->
      <div class="carousel-inner">
        <div class="item active">
          <img src="images/slide1.jpg" alt="First slide">
        </div>
        <div class="item">
          <img src="images/slide2.jpg" alt="Second slide">
        </div>
        <div class="item">
          <img src="images/slide3.jpg" alt="Third slide">
        </div>
      </div>

      <!-- Controls -->
      <a class="left carousel-control" href="#carousel-example-generic" data-slide="prev">
        <span class="icon-prev"></span>
      </a>
      <a class="right carousel-control" href="#carousel-example-generic" data-slide="next">
        <span class="icon-next"></span>
      </a>
    </div>
  </div>
</div>

  <!-- // 케러셀 부분 끝 -->

  <!-- // 썸네일 이미지 추가 -->
  <div class="row">
    <div class="col-md-3 col-xs-6">
      <a href="#" class="thumbnail">
        <img src="images/slide1.jpg" alt="" class="img-circle" />
      </a>
    </div>
    <div class="col-md-3 col-xs-6">
      <a href="#" class="thumbnail">
        <img src="images/slide2.jpg" alt="" class="img-circle" />
      </a>
    </div>
    <div class="col-md-3 col-xs-6">
      <a href="#" class="thumbnail">
        <img src="images/slide3.jpg" alt="" class="img-circle" />
      </a>
    </div>
    <div class="col-md-3 col-xs-6">
      <a href="#" class="thumbnail">
        <img src="images/logo.png" alt="" class="img-circle" />
      </a>
    </div>
  </div>



  </div> 
    <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
    <!-- Include all compiled plugins (below), or include individual files as needed -->
    <script src="js/bootstrap.min.js"></script>
</body>
</html>
~~~

## Ex99_googlemap.html

~~~html
<!DOCTYPE html>
<html>
	<head>
		<meta name="viewport" content="initial-scale=1.0, user-scalable=no">
		<meta charset="euc-kr">
		<title>구글맵 API 활용하기</title>
		<script src="https://maps.googleapis.com/maps/api/js?sensor=false"></script>
		
		<script>
		function initialize() {
		/*
				구글 지도에서 목적지를 찾고 오른쪽 마우스에서 '이곳이 궁금한가요?'에서 좌표값을 구한다
		*/
		var Y_point = 37.486678; // Y 좌표
		var X_point = 127.020709; // X 좌표
		var zoomLevel = 17; // 첫 로딩시 보일 지도의 확대 레벨
		var markerTitle = "목적지"; // 현재 위치 마커에 마우스를 올렸을때 나타나는 이름
		var markerMaxWidth = 300; // 마커를 클릭했을때 나타나는 말풍선의 최대 크기

		var myLatlng = new google.maps.LatLng(Y_point, X_point);
		var mapOptions = {
			zoom: zoomLevel,
			center: myLatlng,
			mapTypeId: google.maps.MapTypeId.ROADMAP
		}
		var map = new google.maps.Map(document.getElementById('map_view'), mapOptions);

		var marker = new google.maps.Marker({
			position: myLatlng,
			map: map,
			title: markerTitle
		});

		var infowindow = new google.maps.InfoWindow(
		{
			content: '여기',
			maxWidth: markerMaxWidth
		}
		);

		google.maps.event.addListener(marker, 'click', function() {
			infowindow.open(map, marker);
		});
		}
			</script>
	</head>

	<body onload="initialize()">
		<div id="map_view" style="width:500px; height:300px;"></div>
	</body>
</html>
~~~