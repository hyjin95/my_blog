# Toad&CMD - procedure

### Toad

![nickname procedure](../../.gitbook/assets/1%20%2814%29.png)

* 프로시저 이름 : proc\_login69
* 사용자가 입력하는 값 : u\_id, u\_pw
* 반환 값 : r\_nickname
* IS : 변수선언
* BEGIN : 실행문 시작
* END; : 실행문 종료
* / : 프로시저 종료

## CMD

![cmd sql &#xB85C;&#xADF8;&#xC778;](../../.gitbook/assets/2%20%2810%29.png)

* sqlplus
* 아이디, 암호 

![&#xC2E4;&#xD589; &#xBC0F; print](../../.gitbook/assets/3%20%2811%29.png)

* variable 변수이름 타입\(크기\); - 반환값을 담을 변수 선언 - 타입은 해당 프로시저의 반환값의 타입과 같아야한다.
* exec 프로시저이름\(IN값, IN값, :선언한 변수\); - exec : 실행예약어
* print 변수이름; - 반환값을 출력한다.

