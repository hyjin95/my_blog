---
description: 2021.01.20 - 106일차
---

# 106 Days - Python, Anacondas\(Jupyter Notebook\) 환경설정

### 설치

{% page-ref page="100-days.md" %}

### 1

![](../../.gitbook/assets/1%20%28111%29.png)



* window &gt; Anaconda Prompt\(anaconda3\) 우클릭 &gt; 관리자 권한으로 실행

![](../../.gitbook/assets/2%20%2886%29.png)

* pip install cx\_oracle  _-_ cx\_oracle 패키지 다운로드
* python - python확인 - quit \|\| ctrl+z : 나가기
* python -m pip install cx\_Oracle  --upgrade - 업데이트있을시 업그레이드 하기

![](../../.gitbook/assets/3%20%2862%29.png)

* [https://www.oracle.com/database/technologies/instant-client/downloads.html](https://www.oracle.com/database/technologies/instant-client/downloads.html) - 다운로드 받은 cx\_oracle을 사용하기위해 운영체제에 맞는 Instant Client for Microsoft Windows 다운
* zip파일의 압축을 풀고 내부의 instantclient\_19\_9폴더를 사용할 드라이브에 배포한다.

![](../../.gitbook/assets/4%20%2844%29.png)

* window &gt; 시스템 환경 변수 편집 &gt; 환경변수 &gt; 시스템 변수 Path &gt; 편 &gt; 새로만들기 - instantclient 폴더의 경로 등록

![](../../.gitbook/assets/5%20%2831%29.png)

* python에서 접근하기위한 환경변수 설정 -기존에 설치 되어있던 오라클 서버의 물리적인 경로 등록
* 시스템 환경변수는 운영체제가 인식하는 것이기때문에 등록 후에 인식을 위해서는 재부팅 해야한다.

