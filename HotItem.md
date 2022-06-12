### 실시간 인기상품

```jsp
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.8.2/jquery.min.js"></script>
<script type="text/javascript" src="http://image.gsshop.com/mobile/main/js/idangerous.swiper.js"></script>
<style type="text/css">
	*{
		margin:10px 10px 10px 10px;
		padding:5px 5px;
	}
	ul{
		list-style:none
	}
	ul.on{
		display:block
	}
	
 
	li{display:none}
	li.on{display:block}
	#timer_search{position:relative; width:400px;}
	.list_wrap{padding-top:20px;}
/* 	.list_wrap.on{border:1px solid #f1f1f1; width:250px; display:inline-block;} */
/* 	.list_wrap.on li{display:block} */
/* 	.list_wrap.on ul:hover{background:#f1f1f1; display: inline-block;} */
/* 	.list_wrap.on li.on{font-weight:bold} */
/* 	.view{display:none; position:absolute; left:10px; top:10px; z-index:99; font-size:16px; font-weight:bold} */
/* 	.view.on{display:block} */
	
	.view{
		margin-top: 7px;
		margin-bottom: 7px;
	}
 
</style>
</head>
<body>
	<div id="timer_search">
		<ul class="list_wrap" id="list_wrap">
			<h1 id="view" class="view">실시간 인기 상품</h1>
			<li class="on">01 리스트 01</li>
			<li>02 리스트 02</li>
			<li>03 리스트 03</li>
			<li>04 리스트 04</li>
			<li>05 리스트 05</li>
		</ul>
	</div>
	
 	<div>
 		놀고싶어라,,   // 밑으로 밀리는 현상을 어떻게 없앨수 있을까...??
 	</div>
	<script>
		var next = document.getElementById("next");
		var view = document.getElementById("view");
		var objF = document.getElementById("list_wrap");
		var obj = document.getElementsByTagName("LI");
		var title = document.getElementById("view");
		var len = obj.length;
		var idx = 0;
		var timer = null;
		
		function list(){
			obj[idx].className = "";
			idx++;
		
			if (idx >= len){
				idx = 0;
			}
			obj[idx].className = "on";

			/*view.innerHTML = "";
			view.innerHTML = obj[idx].innerHTML;*/
		}
		
		
		function start(){
			timer = setInterval (function(){
				list();
			},1000);
		}
		start();
 
		objF.onmouseover=function(){
			
			objF.className="list_wrap on";
			view.className = "view on"
			clearInterval(timer);
 
		};
 
		objF.onmouseout=function(){
			start();
			objF.className="list_wrap";
			view.className = "view";
		};

	</script>
	
