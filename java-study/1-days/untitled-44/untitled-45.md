# Untitled

## board\_master\_t

### 테이블 + pk 생성

```sql
create table board_master_t
(bm_no number(5) constraints bm_no_pk primary key
,bm_writer varchar2(30) not null
,bm_title varchar2(100) not null
,bm_content varchar2(4000)
,bm_email varchar2(50)
,bm_hit number(5) default 0
,bm_date varchar2(30)
,bm_group number(5) default 0
,bm_pos number(5) default 0
,bm_step number(5) default 0
,bm_pw varchar2(10)
);
```

* pk가 있으므로 index가 자동으로 생성된다.

### 제약조건 확인하기

![](../../../.gitbook/assets/3%20%2852%29.png)

## board\_sub\_t

### 테이블 생성

![](../../../.gitbook/assets/22%20%286%29.png)

```sql
create table board_sub_t
(bm_no number(5)
,bs_seq number(5)
,bs_file varchar2(100)
,bs_size number(9,2)
);
```

### index 생

![](../../../.gitbook/assets/23%20%281%29.png)

```sql
create unique index scott.bmno_pk on scott.board_sub_t(bm_no, bs_seq);
```

* pk가 없어 index를 따로 생성한다.

### FK 제약조건 생성

![](../../../.gitbook/assets/24%20%281%29.png)

```sql
--제약조건 안전장치 설정, 제약조건 넣기
alter table scott.board_sub_t add(constraint fk_bmno
foreign key(bm_no)
references scott.board_master_t(bm_no)
enable validate);
```

* FK 제약조건 설정하기

