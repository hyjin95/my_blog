---
description: 2021.04.08 목요일
---

# JSP

## class

### Locale : 다국어 처리

* java.lang.Locale
* 특정 지리적, 정치적, 문화적 지역을 나타내는 클래스 사용자의 지역 환경에 따라 결정되는 지역적 문화\(언어, 날짜, 시간 등\) 정보를 담고있다.
* 웹 페이지에서 여러 언어를 제공할때 사용된다. 단순 문자뿐 아니라 숫자, 날짜, 시간 등을 표현하는데 사용된다.
* Locale의 객체 생성  
  request.getLocal\( \);

  request 내장객체를 이용해 현제 웹 브라우저에 미리 정의된 언어, 국가정보를 가져오게 된다.

* 사용자 Locale 환경 감지 메서드 - getDefault\( \) : return static Locale, 기본 Locale 환경의 현재 값 - getCountry\( \) : return String, 현재 Locale 환경의 국가/지역 코드\(대문자\)를로 가져온다. - getDisplayCountry\( \) : return String, 현재 Locale 환경의 국가 이름을 가져온다. - getLanguage\( \) : return String, 현재 Locale 환경의 언어 코드\(소문자\)를 가져온다. - getDisplayLanguage\( \) : return String, 현재 Locale 환경의 언어 이름을 가져온다.
* String sso\_user\_locale = request.getLocale\( \).toString\( \).toLowerCase\( \); 사용자의 Locale환경을 소문자 문자열로 가져온다. 

