# JavaScript

### 문제 : \["1", "2", "3"\].map\(parseInt\)의 결과는?

1. \[1, 2, 3\]
2. \[0, 1, 2\]
3. 그 외

* 답  : 그 외, \[1, NaN, NaN\]
* parseInt\(val, radix\) map\(element, index, array\)

```javascript
["1", "2", "3"].map(c => parseInt(c))
```

* 1번 답 값을 얻으려면 위와같이 작성한다. map 값이 하나씩 변환된다.
* 함수를 \( \) 없이 작성해버리면 파라미터를 전부 가져와버려 NaN이 발생한다.

### 문제 : \[ \] == \[ \]는 false, true ?

* 답 : false

```javascript
var a = []
var b = []
a == b ?
```

* 위와 같은 문제이므로 false
*  만약 var b = a라는 전제라면 true이다.

