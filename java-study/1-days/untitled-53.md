# Untitled



## Python : CSV 읽어오기

### CSV파일에서 데이터 읽어오기

* csv.reader\( \) : 읽기 csv.writer\( \) : 쓰기

### Python

```python
import csv
```

* CSV 모듈 불러오기

```python
f = open('ta_seoul.csv')
```

* csv파일을 open함수로 열어서 f\(fileHandler\)에 저장한다.

```python
data = csv.reader(f)
```

* f를 reader함수에 넣어 data라는 csv reader객체를 생성한다.

```python
header = next(data)
```

* header는 cursor와 비슷한 역할을 한다.

```python
for row in data:
    print(row)
```

* 출력
* '{', '}'를 사용하지않고 들여쓰기로 구분한다.

```python
f.close()
```

* open했던 파일을 닫아준다.

### 출력 Data

![](../../.gitbook/assets/1%20%28113%29.png)

* 조회 결과가 대괄호로 묶여있다. 리스트 자료구조이다.
* 각 데이터가 작은 따옴표로 되어있어 문자열 데이터임을 알 수 있다. float형태로 전환해야 비교가 가능해진다.
* 기간을 기준으로 뽑은 자료는 전쟁과 같은 특수상황의 기간동안에는 누락된 정보가 있을 수 있다.

### 예제 : 서울이 가장 더웠던 날 + 기온

* Data를 읽어온다. f = open\('파일명'\)
* 순차적으로 최고 기온을 확인한다.
* 최고 기온이 높았던 날짜의 data를 저장한다.
* 최종 데이터를 출력한다.

![](../../.gitbook/assets/2%20%2888%29.png)

* header를 출력해보면 제일 마지막 data가 최고기온임을 알 수 있다.

![](../../.gitbook/assets/3%20%2864%29.png)

* float을 사용해 네번째 row값을 문자가 아닌 숫자형태로 변환된 것을 확인할 수 있다.

