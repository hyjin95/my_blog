---
description: 2021.08.23  월요일
---

# EXISTS, NOT EXISTS

### EXISTS

```sql
--id를 기준으로 두 테이블에 모두 존재하는 데이터 조
select * from tableA a 
where exists (select * from tableB aa where a.id = aa.id ); 
```

* 두 테이블 간에 where절에 걸리는 값 기준으로 두 테이블에 함께 존재하는 데이터만 가져온다.

### NOT EXISTS

```sql
--id를 기준으로 a테이블에만 있는 데이터 조회
select * from tableA a 
where not exists (select * from tableB aa where a.id = aa.id ); 
```

* exists 명령어와 반대되는 명령어로 where절의 조건을 기준으로 서브쿼리에 작성되는 테이블에 존재하지 않고 A테이블에 존재하는 데이터만 출력한다.

### 사용 예제

```sql
--id를 기준으로 a테이블에만 있는 데이터 삭제
delete * from tableA a 
where not exists (select * from tableB aa where a.id = aa.id ); 
```

* 업무 처리중 서로 참조되는 테이블 A, B테이블에서 A테이블에는 존재하는 자료이지만 B테이블에서 삭제완료된 자료가 발견되어 남은 자료로인해 에러사항이 발생했다.
* 위와 같은 경우, 서로 공유하는\(pk\) 값과 not exists 명령어를 사용해 B테이블에 없는 A테이블 자료를 조회할 수 있다.

