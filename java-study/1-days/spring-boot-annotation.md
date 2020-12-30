# Spring-boot : annotation

### @Controller

* 컨트롤 계층 클래스를 선언시 사용

### @Service

* 모델 계층 클래스를 선언시 사용
* 자바에서 xxxLogic과 xxxDao클래스에 해당

### @Autowired

* 의존성 주입시 작성
* setter메서드의 역할을 수행
* required 속성 - true : default, 주입 객체가 없는 경우 예외가 발생한다. - false : 주입 객체가 없어도 예외가 발생하지 않는다.

### @RequestParam

* 메서드의 파라미터에 작성된다.
* 사용자가 화면에 입력한 값, ajax에서 넘어오는 값을 자동으로 받아준다.

### @RequestParam : 첨부파일처리용

* MultipartFile 
* MultipartFile\[ \]
* 파일을 받을 수 있는 클래스

### ModelMap

* 메서드의 파라미터에 작성된다.
* forward scope로 사용가능하다.
* UI지원을 위한 별도의 클래스가 제공된다.
* 
### Model

* ModelMap과같이 UI지원을 위한 클래스이다.
* 
