---
description: 2020.09.22 - 30일차
---

# 30 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## 복습

### Thread 구현

* 방법1 : extends Thread - 자바는 단일상속만 허용한다.
* 방법2 : implements Runnable - 다중구현이 가능하다.
* Thread클래스의 run메서드를 오버라이딩해서 사용하기 위함이다.
* 서버쪽은 여러 접속을 처리해야하므로 thread가 있어야하고, 접속자의 입장에서도 병원-원무과-약국 순서로 가듯이 순서대로 서버에 접속해야하므로 thread가 필요하다.

### 메서드 오버라이딩

* @Override
* 파라미터와 타입이 같아야한다.
* 기능을 재정의해서 사용한다.

### RUN\( \)

* 여러 접속이 일어날때, 순서대로 진행하게 한다.
* wait\( \) : Thread를 대기시킨다.
* notify\( \) : 대기하던 single thread를 차례가 되어 호출한다.
* notifyAll\( \) : 대기하던 모든 thread를 호출한다.
* sleep\( \) : 일정시간 얼린다. 1000=1 
* run\( \) 호출 : strat\( \);
* run\( \) 종료 : stop\( \);

## JAVA

### 멀티스레드 구현시 주의사항

* dead lock : 교착상태가 일어나 계속 기다리게되는 것 - wait메서드 사용시 발생하지 않도록 주의해야한다.
* synchronzation : 동기화

