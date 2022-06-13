### 실시간 인기상품
> 수정해서 이제 쓸수 있음.

``` jsp
<!-- 실시간 인기순위 -->
<style type="text/css">

.relative {
    position: relative;
}

.center-align {
    margin: 0 auto;
    width: 1080px;
}

#rank-hot {
    height: 200px;
    border-top: 1px solid #f1f3f6;
    border-bottom: 1px solid #d1d8e4;
}

#rank-hot ul {
    display: inline-block;
    margin: 0;
    padding: 16px 0 0 4px;
    list-style: none;
}

#view{
	display:none;
	position:absolute;
	left:10px;
	top:10px;
	z-index:99;
	font-size:10px;
	font-weight:bold;
	border: 1px solid #d9d9d9;
	border-radius: 4px;
	}

#search-ranking {
    position: relative;
    left: 0;
    width: 200px;
    height: 25px;
}

#search-ranking ul {
    display: none;
    list-style: none;
    padding: 17px;
    margin: 0;
    width: 300px;
    height: 330px;
    border: 1px solid #d9d9d9;
    background: white;
    border-radius:5px;
    position: absolute;
    top: 0;
    right: 0;
    z-index: 100;
    box-shadow: 4px #d9d9d9;
}

#search-ranking:hover ul {
    display: inline-block;
}

#rank-number {
    margin-top: -9px;
    color: #00ab33;
    font-size: 16px;
}

#rank-title {
    letter-spacing: -1px;
    font-size: 14px;
}

</style>
<body>
  <!-- 실시간 인기순위 -->
    <nav class="rank-hot">
      <div class="center-align">
        <div id="search-ranking">
          <div>
            <span id="rank-number">실시간 인기!</span>
          </div>
          <ul>
          <p class="view">실시간 인기순위 &nbsp;</p>
            <c:if test="${sessionScope.hotList eq null }">
              <input type="button" value="Click!" onclick="location.href='./hotItem'">
            </c:if>
        <c:forEach var="hot" items="${sessionScope.hotList }">
              <li>
                <span class="rank-number">${hot.num}.</span>
                <span class="rank-title"><a href="">${hot.item_title}</a></span>&nbsp;<span>${hot.item_readCnt}</span>
              </li>
        </c:forEach>
          </ul>
        </div>
       </div>
    </nav>
 </body>
 
 
 ```xml
 <!-- 실시간 인기상품 -->
	<select id="selectHotItemList" resultType="com.itwillbs.cono.vo.ItemDTO">
		SELECT CAST(@rownum:=@rownum+1  AS UNSIGNED) AS num, item_title, item_readCnt
		FROM item, (SELECT @rownum:=0) TMP
		ORDER BY LENGTH(item_readCnt) DESC, item_readCnt DESC
		LIMIT 1, 10
	</select>
	

