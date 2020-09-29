---
description: 2020.09.28 - 33일차
---

# 33 Days - Chat-level4, Tomato-level3 1대1대화, 대화명변경, 반복문 탈출

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## 복습

### level3 - data이동

1. TomatoClient -&gt; ActionPerformed  - 대화상대 선택 -&gt; oos.writeObject\( \);
2. TomatoServerThread -&gt; Thread부여 및 Vector에 데이터 유지 - 듣기 : 생성자 -&gt; ois.readObject\( \); - 들은 내용 말하기 : run메서드 -&gt;  broadCasting, send 메서드 -&gt; oos.wrtieObject\( \);
3. TomatoClientThread -&gt; client화면에 응답 - 듣 : run메서드 -&gt; ois.readObject\( \);

### 반복문 탈출

* if : return;
* for : break;
* switch, while : 라벨

## 1 : 1 대화 구현

