# DECODE 와 CASE

### DECODE

```sql
DECODE(비교값, 조건, true값, false값)
DECODE(값, IF1, THEN1, IF2, THEN2, ...)
```

* 경우의 수가 2개 이상이라면 if문이 필요하지만 DML에서는 if문을 사용할 수 없다. DML에서는 DECODE 함수를 사용한다.
* FROM 절을 제외한 어디든 사용될 수 있다. - SELECT문, WHERE절

### CASE

```sql
CASE 값
    WHEN IF조건1 THEN1
    WHEN IF조건2 THEN2
    ...
ELSE
END
```

* CASE 값 

### 차이점

* 
## 용어

### DML

