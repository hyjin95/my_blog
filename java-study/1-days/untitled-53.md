# Untitled



## Python : CSV 읽어오기

### CSV파일에서 데이터 읽어오기

* csv.reader\( \) : 읽기 csv.writer\( \) : 쓰기

```python
import csv
```

* CSV 모듈 불러오기

```python
f = open('ta_seoul', 'r', encoding='cp949')
```

* csv파일을 open함수로 열어서 f\(fileHandler\)에 저장한다.

```python
data = csv.reader(f, delimiter=',')
```

* f를 reader함수에 넣어 data라는 csv reader객체를 생성한다.

```python
pring(data)
```

* 출력

```python
f.close()
```

* open했던 파일을 닫아준다.

