# Toad - t_orderbasket, decode\&sum

## t_orderbasket 

### table

![](<../../../.gitbook/assets/3 (13).png>)

### 문제

![](<../../../.gitbook/assets/1 (17).png>)

* t_orderbasket테이블에서 분석용 함수를 사용하지 않고 각 날짜별로 총 몇개의 물건이 얼만큼 팔렸으며, 매출액은 얼마인지 알고싶다고 한다. 빠진 물건은 다시 주문을 채워 넣어야한다.\
  \- 입금금액, 매출액
* t_orderbasket이 판매실적이라고 했을때,
* 위의 sql을 활용하면 테이블을 복제하여 두번 나타낼 수 있다.
* 위 3줄은 날짜를 출력한다(중복되는 날짜는 하나만 표시) 네 번째 줄에는 날짜 대신 총계 라고 출력해야한다.어떤 경우에는 날짜를, 어떤 경우에는 총계를 나타내야 하므로 경우의 수가 2가지 이다.\
  \= if문 -> DML에서는 decode을 사용한다.

## DECODE & SUM

### 문제

![](<../../../.gitbook/assets/2 (13).png>)

* 부서별 연봉 합계를 출력하시오.
* SUM(DECODE( )) 함수를 이용한다.
* 알리아스명을 주어 표시한다.

### 결과

![](<../../../.gitbook/assets/4 (12).png>)
