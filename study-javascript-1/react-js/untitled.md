---
description: 201.03.29 - 월요일
---

# 컴포넌트 제작

### 1. react 없이 html 작성하기

```markup
<html>
    <body>
        <header>
            <h1>Web</h1>
            World Wide Hansome
        </header>
        
        <nav>
            <ul>
                <li><a href="1.html">HTML</a></li>
                <li><a href="2.html">CSS</a></li>
                <li><a href="3.html">JavaScript</a></li>
            </ul>
        </nav>

        <article>
            <h2>HTML</h2>
            HTML is HyperText Markup Language.
        </article>
    </body>
</html>
```

* public dir에 html을 작성해 'npm run start'명령어로 실행한다. localhost:port번호/파일명.html
* 실무에서는 이 코드가 천만줄이 넘을 수도 있다 알아보기 쉽게 하기위해 사용자 정의 태그로 묶어서 감춰두는 역할을 해주는게 react이다.

### 2. 컴포넌트 만들기

