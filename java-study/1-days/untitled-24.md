---
description: 2020.11.11 - 61일차
---

# 61 Days - 기상자료개방포털 : ME

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 작업지시서

1. 기상청 기상자료개방포털에서 기온분석 데이터를 2010년 1월 1일 부터 2019년 12월 31일까지 검색하여 excel파일\(xls\)로 다운 받는다. - 기온분석  - 검색조건  - 자료구분 : 일  - 자료형태 : 기본  - 지역/지점 : 서울 경기  - 기간 : 20100101 ~ 20191231
2. 1번에서 수집한 자료를 바탕으로 오라클서버에 추가할 테이블을 설계한다.\(ER-WIN을 활용한다\)  
   - 화면에서 입력받는 날짜 조건은 String이다.   
     DB에서 해당 데이터의 타입을date로 해버리면 조건 타입을 변경해야하는데, 사용자가 많으면 속도가 저하되는 원인이 될 수 있으므로 VARCHAR2로 하자.  
   - CHAR타입은 고정형타입으로,  지정된 공간에 글자수가 다 차지 않더라도 공백공간을 넣어버린다.

     VARCAHAR타입은 가변형타입으로, 남는 공간을 반납하므로 공간사용에 더 효율적이다.  
   - NUMBER\( x , y\) x : 전체자리수, y : 소수점 자리수

3. 2번에서 설계한 대로 scott계정에 테이블을 생성한다.
4. 생성된 테이블에 엑셀로 백업 받은 자료를 붓는다.
5. 수집된 기상정보에서 검색해 보고 싶은 조건들을 정해 본다. - 연별최고, 연별최저기온 평균기온, 계절 - SELECT 를 List, List중 어느것으로 할지 선택한다.    VO는 타입이 결정되어있다, Map은 Object타입으로 변환해야한다.    Object는 CastingException이 일어날 수 있다.    회계프로그램, 은행, 계산 이런 경우에는 VO를 사용한다.   VO와 Map은 둘다 하나의 row만 담는다.   -&gt; List를 사용하는 이유 확장할떄에는 Map이 유리하고, 조인 시에도 Map이 유리하다.    Map은 여러 집합을 구분해 담을 수 있지만, VO로 구현하면 컬럼이 중복관리 되므로 복잡해진다. - 계산이 많이 일어날때 : VO - 여러 테이블 조인할 떄 : Map
6. 검색한 결과를 보여줄 화면을 그린다.
7. 클래스를 설계해 본다.
8. 테스트 시나리오를 작성해 본다.
9. 구현시작
10. 8번에서 작성한 시나리오 대로 테스트 해 본다.
11. 년별 혹은 월별 혹은 계절별 통계자료를 출력할 수 있도록 쿼리문을 작성해 본다.
12. 통계자료가 포함된 화면 설계 및 구현을 다시 진행해 본다.
13. 통계자료를 활용하여 각종 그래프로 출력해 본다.  수집된 자료는 JSON포맷을 활용하여 수집, 관리할 수 있도록 해 본다.
14. 13번에서 수집된 정보는 안드로이드 앱으로도 작성해 볼 예정입니다.

## SQL

### 연별 최저, 최고 기온과 해당 일자 SQL

![](../../.gitbook/assets/1%20%2864%29.png)

```sql
SELECT tt.DATE_TEM, l.LOW_TEM, rr.DATE_TEM, l.HIGH_TEM
  FROM (SELECT MAX(HIGH_TEM) as HIGH_TEM, MIN(LOW_TEM) as LOW_TEM FROM ONDO WHERE DATE_TEM LIKE '%'||'2019'||'%') l
      ,(SELECT t.DATE_TEM as DATE_TEM
         FROM (SELECT DATE_TEM, LOW_TEM FROM ONDO WHERE DATE_TEM LIKE '%'||'2019'||'%') t
        WHERE t.LOW_TEM LIKE (SELECT MIN(LOW_TEM) FROM ONDO WHERE DATE_TEM LIKE '%'||'2019'||'%')) tt
      ,(SELECT r.DATE_TEM as DATE_TEM
         FROM (SELECT DATE_TEM, HIGH_TEM FROM ONDO WHERE DATE_TEM LIKE '%'||'2019'||'%') r
        WHERE r.HIGH_TEM LIKE (SELECT MAX(HIGH_TEM) FROM ONDO WHERE DATE_TEM LIKE '%'||'2019'||'%')) rr
--연별 최저, 기온 조회 sql
```

* FROM절이 반복되는 구문이 많다. ONDO테이블에 5번이나 갔다오는 것을 따로 처리 할 수 없을까? - DECODE와 CASE를 사용해보자
* 참고 : [http://www.gurubee.net/lecture/1028](http://www.gurubee.net/lecture/1028)

## PROCEDURE

```sql
CREATE OR REPLACE PROCEDURE proc_ondo
(u_start IN varchar2, u_end IN varchar2, p_rc OUT sys_refcursor)
IS
BEGIN 
      OPEN p_rc FOR 
      SELECT DATE_TEM, AVG_TEM, LOW_TEM, HIGH_TEM
        FROM ONDO 
       WHERE DATE_TEM BETWEEN TO_DATE(u_start)
                 AND TO_DATE(u_end);
END;

commit;

variable p_rc refcursor;
exec proc_ondo('2010/01/01', '2011/12/31', :p_rc);
print p_rc;
```

* 1-10번 : 프로시저 생성
* 12번 : 적용
* 14-16번 : 테스트

후기 : 마지막 프로젝트를 위한 면담을 진행하고 있다. 나는 마지막 이니까 금요일쯤 하려나.. 마지막 프로젝트의주제를 강의 설명회때부터 생각하고 있는데 딱히 신박한 생각이 안든다. 지하철 빈자리 표시하기?? 흠... 더 생각해봐야지 일상속의 불편함을 찾아봐야겠다. 오늘은 빼빼로데이!

