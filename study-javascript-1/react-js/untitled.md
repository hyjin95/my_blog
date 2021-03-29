---
description: 201.03.29 - 월요일
---

# 컴포넌트 제작

### 1. react 없이 html 작성하기

```markup
<!-- pure.html -->
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

```javascript
//App.js
import './App.css';
import { Component } from 'react';

class Subject extends Component {
  render(){
    return (
      <header>
            <h1>Web</h1>
            World Wide Hansome
      </header>
    );
  }
}

class App extends Component {
  render() {
    return (
      <div className="App">
        <Subject></Subject>
      </div>
    );
  }
}

export default App;
```

* 리액트가 제공하는 Component 클래스를 상속받아 클래스를 만든다. src &gt; App.js
* 이 클래스는 render 메서드를 정의해야한다. function render\( \)가 맞지만 JS최신 버전에서는 class내부의 함수는 function을 생략한다.
* render 메서드의 return은 최상위 태그를 단 하나만 갖도록 한다.
* 위 문법은 유사  JS문법으로 JS문법은 아니다. JS에서 해당 코드를 실행하면 문법오류가 발생한다. JS에서 태그 작성시 "와 역슬래쉬를 사용해 복잡하게 작성해야하는데 이를 페이스북에서 만든 jsx코드로 편하게 작성하면 create-react-app가 JS코드로 컨버팅\(변환\)하여 브라우저에 출력해주는 것이다. &lt;Subject&gt;&lt;/Subject&gt; -&gt; &lt;header&gt; ... &lt;/header&gt;

![&#xACB0;&#xACFC;](../../.gitbook/assets/1%20%28137%29.png)

### 3. 

* 
