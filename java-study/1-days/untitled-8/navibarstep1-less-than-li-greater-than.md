# naviBarStep1 - 시멘틱태그, &lt;li&gt;

## naviBarStep1.html

![&#xBE0C;&#xB77C;&#xC6B0;&#xC800;](../../../.gitbook/assets/1%20%2840%29.png)

```markup
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>li태그를 활용한 내비바 구성하기</title>
<style type="text/css">
	*{margin:0; padding:0;}
	a{text-decoration:none;}/*href의 링크에 생기는 밑줄 제거*/
	li{list-style:none;}
	#main_header{
		height: 60px;
		border-bottom: 1px solid black;
		padding-left: 10px;
		background: #6799FF;
		color: white;
	}
	/*통상 부모태그 사용, 자식태그 가려줌*/
	#main_gnb{
		overflow: hidden;/*넘치는 부분 숨김처리*/
		background: #6B66FF;
	}
	#main_gnb > ul.left > li{width: 120px; float: left;}
	#main_gnb > ul.left{/*id하위 class에 접근하기 > */
		overflow: hidden;
		float: left;
	}
	#main_gnb > ul.right > li{width: 110px; float: right;}
	#main_gnb > ul.right{
		overflow: hidden;
		float: right;
	}
	#main_gnb a{/*tag에 접근할때에는 >필요없음*/
		display: block;
		padding: 10px 20px 10px 20px;
		color: white;
		font-weight: bold;
		border-left: 1px solid #FFFFFF;
		border-right: 1px solid #EAEAEA;7777
	}
	#content{
		width: 960px;
		margin: 0 auto;
		overflow: hidden;
	}
	#content > #main_section{
		width: 750px;
		float: left;
	}
	#main_section > article.main_article{
		margin-botton: 10px;
		padding: 20px;
		border: 1px solid black;
	}
	#content > #main_aside{
		width: 200px;
		float: right;
	}
</style>
</head>
<body>
	<header id="main_header">
		<h1>Java69기</h1>
	</header>
	<nav id="main_gnb">
		<ul class="left">
			<li><a href="#">JAVA</a></li>
			<li><a href="#">Oracle</a></li>
			<li><a href="#">MyBatis</a></li>
			<li><a href="#">HTML</a></li>
			<li><a href="#">JavaScript</a></li>
		</ul>
		<ul class="right">
			<li><a href="#">로그인</a></li>
			<li><a href="#">회원가입</a></li>
		</ul>
	</nav>
<!-- ====================== 본문 ============================ -->
	<div id="content">
		<section id="main_section">
			<article class="main_article">
				<h1>Main News</h1>
			</article>
			<article class="main_article">
				<h1>Main Sports</h1>
			</article>
			<article class="main_article">
				<h1>Main Shopping</h1>
			</article>
		</section>
		<aside>
			<h1>Main Menu</h1>
			<p>aside에 뿌려질 내용 추가</p>
		</aside>
	</div>
<!-- ====================== 본문 ============================ -->
</body>
</html>
```

