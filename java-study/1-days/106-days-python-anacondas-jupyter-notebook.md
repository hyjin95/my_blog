---
description: 2021.01.20 - 107일차
---

# 107 Days - Python, Anacondas(Jupyter Notebook) : Oracle 환경설정

### 설치

{% content-ref url="100-days.md" %}
[100-days.md](100-days.md)
{% endcontent-ref %}

## Python : Anaconda에 Oracle환경설정

![](<../../.gitbook/assets/1 (111).png>)



* window > Anaconda Prompt(anaconda3) 우클릭 > 관리자 권한으로 실행

![](<../../.gitbook/assets/2 (86).png>)

* pip install cx_oracle \
  _- _cx_oracle 패키지 다운로드
* python\
  \- python확인\
  \- quit || ctrl+z : 나가기
* python -m pip install cx_Oracle  --upgrade\
  \- 업데이트있을시 업그레이드 하기

![](<../../.gitbook/assets/3 (63).png>)

* [https://www.oracle.com/database/technologies/instant-client/downloads.html](https://www.oracle.com/database/technologies/instant-client/downloads.html)\
  \- 다운로드 받은 cx_oracle을 사용하기위해 운영체제에 맞는 Instant Client for Microsoft Windows 다운
* zip파일의 압축을 풀고 내부의 instantclient\_19\_9폴더를 사용할 드라이브에 배포한다.

![](<../../.gitbook/assets/4 (44).png>)

* window > 시스템 환경 변수 편집 > 환경변수 > 시스템 변수 Path > 편 > 새로만들기\
  \- instantclient 폴더의 경로 등록

![](<../../.gitbook/assets/5 (31).png>)

* python에서 접근하기위한 환경변수 설정\
  \-기존에 설치 되어있던 오라클 서버의 물리적인 경로 등록
* 시스템 환경변수는 운영체제가 인식하는 것이기때문에 등록 후에 인식을 위해서는 재부팅 해야한다.

![](<../../.gitbook/assets/1 (112).png>)

![](<../../.gitbook/assets/2 (87).png>)

* 재부팅 후 다시 anaconda prompt에서 pip list명령어를 입력하면 cs_Oracle이 성공적으로 등록된 것을 확인 할 수 있다.

![](<../../.gitbook/assets/3 (62).png>)

* 파이썬 언어를 지정해 cx_oracle을 import하고 연결을 실행해 SQL문을 실행해보면 정상적으로 DB에 접근해 값을 가져오는 것을 확인할 수 있다.\
  \- import : 파이썬이 인식할 수 있는 외부 오라클 드라이버를 가져온다.\
  \- makedsn : 이 함수가 자바의 connection역할을 수행한다.\
  \- db : 연결시 계정정보, 만들어둔 DB정보를 가져와 연결한다.\
  \- cursor : 커서 생성\
  \- cursor.execute(SQL) : 요청\
  \- 연결된 Oracle정보가 띄워진다.\
  \- fetch : 커서의 위치에서 한개 로우를 읽어 메모리에 올려준다.\
  \- 조회 결과\
  \- 자원반납

후기 : 2주도 안남았다니 ㅠㅠㅠ
