---
description: 2021.04.12 - 월요일
---

# 삼항연산자 : 물음표?와 콜론:

### 삼항연산자

* JS에서 세 개의 피연산자를 사용할 수 있는 연산자 보통 if명령문의 단축 형태로 사용된다.
* 형식 \( 결과 = 조건 A? 참이면 B : 거짓이면 C \) 조건 A가 참이면 B를 거짓이면 C를 반환한다.

### if문과의 비교

```javascript
// 삼항연산자
var res = (A === true) ? B : C;

// if 문
var res = null;
if (A === true) {
    res =  B;
} else {
    res =  C;
}
```

* if문과 삼항연산자의 사용 단순한 조건 비교 후 결과를 얻는 연산이라면 삼항연산자가 적합하겠지만 결과값이 두개보다 많거나 추가 로직이 필요한 조건문의 경우에는 if문이 가독성, 효율성면에서 더 낫다.

### 이중 삼항연산자

```javascript
// 주의: 조건 연산은 우측부터 그룹핑 됩니다.
var cond1 = true,
    cond2 = false,
    result = (cond1) ? (cond2 ? "true true" : "true false") : (cond2 ? "false true" : "false false") ;

console.log(result); // logs "true false"
```

* 삼항연산자를 중첩해서 사용하면 우측 조건부터 처리된다.

