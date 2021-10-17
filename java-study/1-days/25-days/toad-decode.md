# Toad - Decode, Sign

### Discribe화면

![](../../../.gitbook/assets/1-3.png)

* edit 테이블명 -> 테이블명 드래그 -> F4

## Decode & Sign 예제문제

![](../../../.gitbook/assets/2-1.png)

* lec_id : 과목번호, lec_time : 시수,  lec_point : 학점
* 1-1 강의 시간과 학점이 같은 강좌의 갯수\
  \- WHERE 문이 아닌 Decode함수를 사용할 것\
  \- MAX함수를 사용한 것은 로우의 갯수가 맞지않아 오류가 발생하기때문이다. 의미없다.\
  \- Decode(비교값, 조건, 비교값=조건이면 반환할 값, 비교값!=조건이면 반환할 값)
* 1-2 시수가 크면 '실험과목' 학점이 크면 '기타과목' 같으면 '실험과목'을 반환하는 쿼리\
  \- Decode + Sign함수를 사용할 것\
  \- Decode(sign(표현식), 반환값a, ' a반환시 출력문', 반환값b, ' b반환시 출력문', ......)
