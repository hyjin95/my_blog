---
description: 2021.04.12 - 월요일
---

# form 태그

### target 속성

| 속성값 | 설명 |
| :---: | :---: |
| \_blank | 새로운 윈도우나 tab |
| \_self | 기본값, 링크가 위치한 현재 프레임 |
| \_parent | 현재 프레임의 부모 프레임 |
| \_top | 현재 윈도우 전체 |
| 프레임 이름 | 사용자가 명시한 프레임 |

* form data를 서버에 제출한 뒤 받는 응답이 열릴 위치를 명시한다.

### JS에서의 사용

```javascript
form.target = '_self';
```

* form = form태그의 name값

