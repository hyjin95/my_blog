---
description: 2021.03.29 - 월요일 2021.03.30 - 화요일
---

# 컴포넌트 제작

## 컴포넌트 제작

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

### 3. props

* 컴포넌트로 사용자 태그를 정의해 패키징하면 다른 사용자들이 사용할 수 있게 된다. 재사용성이 높아지지만 항상 똑같은 결과를 보여줄것이다. 같은 컴포넌트를 사용하더라도 사용자마다 원하는 값을 넣어 표시할 수 있게 해주는 것이 바로 리액트의 props이다.

```javascript
class Subject extends Component {
  render(){
    return (
      <header>
            <h1>{this.props.title}</h1>
            {this.props.sub}
      </header>
    );
  }
}
```

* 리액트 에서 this.props로 변수를 설정 할 수 있다.

```javascript
class App extends Component {
  render() {
    return (
      <div className="App">
        <Subject title="WEB" sub="World Wide Web!"></Subject>
        <Subject title="React" sub="For UI"></Subject>
        <TOC></TOC>
        <Content></Content>
      </div>
    );
  }
}
```

* App 클래스에서는 위와 같이 사용자 정의태그에 props로 지정된 속성을 넣어 출력할 수 있다.

### 4. Component 파일 분리하기

```javascript
import { Component } from 'react';

class TOC extends Component {
    render(){
      return (
        <nav>
            <ul>
                <li><a href="1.html">HTML</a></li>
                <li><a href="2.html">CSS</a></li>
                <li><a href="3.html">JavaScript</a></li>
            </ul>
        </nav>
      );
    }
  }

  export default TOC;
```

* src 디렉토리 밑에 'components'폴더를 만들어 컴포넌트들을 정리하자.
* 위 소스는 TOC.js로 App.js에 정의했던 TOC class를 별도의 파일로 분리한 것이다. 1번 : react로부터 Component를 import해야 Component클래스를 상속받을 수 있다. 17번 : export default 이름; 이 작성되어 있어야 외부에서 TOC.js를 사용할 수 있다.

## 더보기

### React Developer Tools

* chrome [https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi/related?utm\_source=chrome-ntp-icon](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi/related?utm_source=chrome-ntp-icon)

![](../../.gitbook/assets/1%20%28139%29.png)

* 개발자 도구에서 컴포넌트를 확인할 수 있다. 리액트 분석에 용이한 도구
* 위 사진과 같이 개발자도구에서 props를 수정 &gt; enter 시 브라우저에 바로 적용되는 것을 볼 수 있다.

