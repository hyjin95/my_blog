# Toad - 서브쿼리 & seq & 중복제거 & 아우터조인

## 설정

### seq 설정

![](../../.gitbook/assets/1%20%287%29.png)

* 접속자 user 
* 시퀀스 컬럼이름
* 시작 값
* 최소 값
* 최대 값
* 증가 값
* 반복
* 캐시 옵션
* 정렬

### oracle data excel로 꺼내오기

![](../../.gitbook/assets/4%20%287%29.png)

## 쿼리문

### seq - nextval & currval

![](../../.gitbook/assets/2%20%286%29.png)

* 자바단에서 예외가 발생하더라도 무조건 새로운 번호가 채번되므로 중복 에러는 발생하지 않는다.
* 안전하다.
* 특히, 동시 접속자가 있는 경우에는 더욱 그렇다.

### WHERE - 서브쿼리

![](../../.gitbook/assets/3%20%286%29.png)

### 중복제거

![](../../.gitbook/assets/5%20%285%29.png)

* 부서집합의 deptno가 사원집합에 내려와서 외래키가 되었을때 distinct함수를 이용해 중복 제거를 해보면, 외래키는 자동으로 인덱스를 만들어주지 않으므로 중복이 허용됨을 알 수 있다.

### 인덱스 & pk

![](../../.gitbook/assets/6%20%282%29.png)

* b.deptno에서 가져오면 pk, index를 갖는다.
* a.deptno는 외래키이므로 a에서 가져오는 것 보다 b에서 가져오는 것이 빠르다.

### 아우터 조인

![](../../.gitbook/assets/7%20%281%29.png)

* a.ename컬럼이 null이더라도 b.deptno가 있으면 표시한다.



