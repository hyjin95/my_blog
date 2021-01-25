---
description: 2021.01.25 -109일차
---

# 109 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio, Visual Studio Code, Nexacro
* 사용 서버 - WAS : Tomcat

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
* 최고 기온이 높았던 날짜의 data를 저장한다. 최고 기온을 저장할 변수 : max\_temp = -999 __기온이 가장 높은 __날짜를 저장할 변수 : max\_data = ' '
* 최종 데이터를 출력한다.

```python
if max_temp < row[4]:
   max date = row[0]
   max_temp = row[4]
```

* 출력 중 현재 최고 기온값보다 현재 행의 기온값이 더 크다면 두 변수를 업데이트한다.

![](../../.gitbook/assets/2%20%2888%29.png)

* header를 출력해보면 제일 마지막 data가 최고기온임을 알 수 있다.

![](../../.gitbook/assets/3%20%2866%29.png)

* float을 사용해 네번째 row값을 문자가 아닌 숫자형태로 변환된 것을 확인할 수 있다. 문자열을 실수형으로 형전환 처리했다.
* 하단을 확인하면 data가 없는 부분에서 값이 없기때문에 오류가 발생하는 것을 볼 수 있다.

![](../../.gitbook/assets/4%20%2846%29.png)

* if문을 사용해 null값은 -999로 치환해 출력한다.

![](../../.gitbook/assets/1%20%28114%29.png)

* if 문을 사용해 row를 읽으면서 최고 기온 data로 변수를 업데이트한다.
* 출력문은 변수를 출력한다.

## Python : Matplotlib을 사용한 Chart

### Chart그리

![](../../.gitbook/assets/2%20%2889%29.png)

* matplotlib를 import한다.
* plt.plot에 배열 하나만 작성하면 y축을 지정한다.
* plt.show\( \)로 차트를 그린다.

![](../../.gitbook/assets/3%20%2864%29.png)

* plt.plot에 배열을 두개 작성하면 각각 x, y축을 지정한다.

![](../../.gitbook/assets/4%20%2845%29.png)

* title함수를 사용해 차트에 제목을 지정할 수 있다.

![](../../.gitbook/assets/5%20%2832%29.png)

* color속성을 사용해 그래프에 색상을 추가할 수 있다.
* label속성을 사용해 그래프 자료에 이름을 지정할 수 있다.
* legend\( \)함수를 사용해 범례를 표시할 수 있다.

![](../../.gitbook/assets/6%20%2822%29.png)

* linestyle속성을 사용해 그래프 선의 스타일을 정할 수 있다.

