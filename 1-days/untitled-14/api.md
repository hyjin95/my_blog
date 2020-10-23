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



