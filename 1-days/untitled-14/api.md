# 구글 지도 API

## Maps Javascript API

```markup
<!DOCTYPE html>
<html>
  <head>
    <title>Simple Map</title>
    <script src="https://polyfill.io/v3/polyfill.min.js?features=default"></script>
    <script
      src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&callback=initMap&libraries=&v=weekly"
      defer
    ></script>
    <style type="text/css">
      /* Always set the map height explicitly to define the size of the div
       * element that contains the map. */
      #map {
        height: 100%;
      }

      /* Optional: Makes the sample page fill the window. */
      html, body {
        height: 100%;
        margin: 0;
        padding: 0;
      }
    </style>
    <script>
      let map;

      function initMap() {
        map = new google.maps.Map(document.getElementById("map"), {
          center: { lat: -34.397, lng: 150.644 },
          zoom: 8,
        });
      }
    </script>
  </head>
  <body>
    <div id="map"></div>
  </body>
</html>
```

## Step1 : callback 메서드를 외부로 빼서 ready사용하기

### mepTestStep1.html

```markup
<!DOCTYPE html>
<html>
  <head>
  <meta charset="UTF-8">
    <title>Simple Map</title>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script src="https://polyfill.io/v3/polyfill.min.js?features=default"></script>
    <script
      src="https://maps.googleapis.com/maps/api/js?key=AIzaSyDTqrhOtBTvXfFWDXp3N6TDFYvMF1i49Os&libraries=&v=weekly"
      defer
    ></script>
    <style type="text/css">
      /* Always set the map height explicitly to define the size of the div
       * element that contains the map. */
      #d_map {
        height: 100%;
      }

      /* Optional: Makes the sample page fill the window. */
      /*태그선택자, 요소선택자*/
      html, body {
        height: 100%;
        margin: 0;/*div와 창 사이의 여백*/
        padding: 0;/*map과 div사이의 여백*/
      }
    </style>
    <script>
      var map;//var myCar = new Sonata();가능, map을 생성하기위한 객체 생성 : 재사용을 위한 선언, 이 문서에서는 없어도 실행됨

      function initMap() {//브라우저에서 제공하는 map객체 생성
    	//Map(object=위치, {,}); { }는 안에 열거형 연산자로 값이 n개 올때 사용한다.
        map = new google.maps.Map(document.getElementById("d_map"), {
          center: { lat: 37.47866863504121, lng: 126.87865107049127 },
          zoom: 16,
        });
      }
    </script>
  </head>
  <body>
  	<!-- document가 받는 문서 이름은 mapTestStep1-1.html이고 이것을 객체화 해서 함수를 활용하면 제어가 가능해진다.
  		 html에 접근하려면 브라우저가 DOM트리를 구성완료해야한다. JQuery가 제공하는 ready함수를 재정희해 사용한다.-->
  	<script type="text/javascript">
  		$(document).ready(function(){
  			initMap();  			
  		});
  	</script>
    <div id="d_map"></div><!-- map을 body, 화면에 구현하기위한 박스 지정 -->
  </body>
</html>
```

## Step2 : JQuery를 사용해 마커, 이벤트, 말풍선 구현

### mapTestStep2 : 마커와 말풍선 구현

![](../../.gitbook/assets/7%20%287%29.png)

```markup
<!DOCTYPE html>
<html>
  <head>
  <meta charset="UTF-8">
    <title>Simple Map-Step2[말풍선]</title>
    <script src="https://polyfill.io/v3/polyfill.min.js?features=default"></script>
    <script
      src="https://maps.googleapis.com/maps/api/js?key=AIzaSyDTqrhOtBTvXfFWDXp3N6TDFYvMF1i49Os&callback=initMap&libraries=&v=weekly"
      defer
    ></script>
    <style type="text/css">
      /* Always set the map height explicitly to define the size of the div
       * element that contains the map. */
      #map {
        height: 100%;
      }

      /* Optional: Makes the sample page fill the window. */
      /*태그선택자, 요소선택자*/
      html, body {
        height: 100%;
        margin: 0;/*div와 창 사이의 여백*/
        padding: 0;/*map과 div사이의 여백*/
      }
    </style>
    <script>
      var map;//var myCar = new Sonata();가능, map을 생성하기위한 객체 생성 : 재사용을 위한 선언, 이 문서에서는 없어도 실행됨
      var marker;//marker객체 생성

      function initMap() {//브라우저에서 제공하는 map객체 생성
    	  //구글 제공 함수이므로 앞에 소유주 google의 maps가 제공한다고 붙여야 사용가능하다.
    	  var infoWindow = new google.maps.InfoWindow();//윈도우 창을 띄워주는 함수
    	//Map(object=위치, {,}); { }는 안에 열거형 연산자로 값이 n개 올때 사용한다.
        map = new google.maps.Map(document.getElementById("map"), {
          center: { lat: 37.47866863504121, lng: 126.87865107049127 },
          zoom: 8,
        });
    	marker = new google.maps.Marker({
    		position: new google.maps.LatLng(37.47866863504121, 126.87865107049127)
    		,map: map
    		,title: "현재 내 위치"
    	});
    	infoWindow.open(map, marker);//창이 열리는 부분
      }
    </script>
  </head>
  <body>
    <div id="map"></div><!-- map을 body, 화면에 구현하기위한 박스 지정 -->
  </body>
</html>
```

### mapTestStep2-1 : 마커의 말풍선에 이미지 구현, 링크걸기

![](../../.gitbook/assets/8%20%283%29.png)

```markup
<!DOCTYPE html>
<html>
  <head>
  <meta charset="UTF-8">
    <title>Simple Map-Step2-1 다중마커</title>
    <script type="text/javascript" src="https://www.jeasyui.com/easyui/jquery.min.js"></script>
    <script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyDTqrhOtBTvXfFWDXp3N6TDFYvMF1i49Os"></script>
    <style type="text/css">
      /* Always set the map height explicitly to define the size of the div
       * element that contains the map. */
      #map {
     	width: 100%;
        height: 100%;
      }

      /* Optional: Makes the sample page fill the window. */
      /*태그선택자, 요소선택자*/
      html, body {
        height: 100%;
        margin: 0;/*div와 창 사이의 여백*/
        padding: 0;/*map과 div사이의 여백*/
      }
    </style>
    <script>
      var map;//var myCar = new Sonata();가능, map을 생성하기위한 객체 생성 : 재사용을 위한 선언, 이 문서에서는 없어도 실행됨
      var marker;//marker객체 생성
    </script>
  </head>
  <body>
  <script type="text/javascript">
  	$(document).ready(function(){
  	//Map(object=위치, {,}); { }는 안에 열거형 연산자로 값이 n개 올때 사용한다.
        map = new google.maps.Map(document.getElementById("map"), {
          center: { lat: 37.47866863504121, lng: 126.87865107049127 },
          zoom: 16,
        });
    	var infoWindow = new google.maps.InfoWindow();//윈도우 창을 띄워주는 함수
    	 $.ajax({
	            url:'jsonShopInfo.jsp'
	            ,dataType:'json'
	            ,success:function(data){
	               var result = JSON.stringify(data);
	               var jsonDoc = JSON.parse(result); 
	               for(var i=0;i<jsonDoc.length;i++){
	            	   //마커 세번 생성하기
	            	   marker = new google.maps.Marker({
	            		 id: i
	               		,position: new google.maps.LatLng(jsonDoc[i].lat, jsonDoc[i].lng)
	               		,map: map
	               		,title: jsonDoc[i].loc
	               	  });
	            	   //마커 클릭시 정보창 띄워주기
	            	   //JS에서는 on을 붙여 이벤트를 표시하지만 JQuery에서는 on을 뺀다.
	            	   google.maps.event.addListener(marker,'click',(function(marker,i){//google.map API가 제공해주는 이벤트 리스너
	            		   return function(){//return에 함수 넣기
	            		   		infoWindow.setContent('<b>이름 : </b>'+jsonDoc[i].loc+'<br><a href=http://www.naver.com>네이버</a><br><img src=../../images/gasan.jpg width=100 height=70>');//내용
	            		   		infoWindow.open(map, marker);//창 띄우기
	            	   	   }
	            	   })(marker,i));
	            	   if(marker){
	            		   marker.addListener('click', function(){//마커를 누르면
	            			   map.setZoom(15);
	            			   map.setCenter(this.getPosition());//해당 마커 위치로이동
	            		   });
	            	   }
	               }
	            } /////////////서버와 비동기 통신 후 처리가 완료 되었을때 [내용 다운로드완료]
			});////////////////end of ajax
  	});
  </script>
    <div id="map"></div><!-- map을 body, 화면에 구현하기위한 박스 지정 -->
  </body>
</html>
```

