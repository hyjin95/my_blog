---
description: 2021.01.11
---

# 100 Days -

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio, Visual Studio Code, Nexacro
* 사용 서버 - WAS : Tomcat

## React.js

### 작업지시서

1. 이클립스에서 boot프로젝트 생성하기 비쥬얼 스튜디오에서 했었던 설정은 이클립스에서 import할 수 없다. 이클립스에서 생성하고 VScode를 사용하는 것은 동기화를 사용하면 가능하다. 이클립스를 먼저 설치 하는것이 좋다.
2. Visual Studio Code에서 spring-boot와 React.js 연동하기 비쥬얼 스튜디오를 설치한 목적은 파이썬 보다 TypeScript 요구사항을 우선했기 때문으로 ECMAScript의 퀵 픽스 기능을 사용하기 위함이다.  React.js로 ECMAScript의 이해와 활용 능력을 키우고 TypeScript를 활용한다. zip의 소스를 배포하고 실행 실습을 진행한다.
3. VSCode에서는 서버 운용이 불편하다. 이클립스를 메인으로 활용하고 VSCode를 서브로 활용한다.

### ECMAScript

* 스크립트 문법중 ECMAScript를 표준이라 한다.
* React.js, Vue.js, TypeScript

### 바벨

* 리액트와 ECMAScript 를 활용하는데 있어서 필요한 외부 라이브러리를 참조하는데 필요하다.
* 리액트의 경우 xxx.jsx 문서를 xxx.js로 변환하는데 사용된다. js로 변환이 되어야 브라우저가 실행할 수 있다.
* js는 표준 JS + TypeScript가 추가될 수 있다.

### 웹 팩

* 바벨과 스크립트를 표준 JS로 생성하는데 사용된다.
* 리액트의 경우 xxx.jsx 문서를 xxx.js로 변환하는데 사용된다. js로 변환이 되어야 브라우저가 실행할 수 있다.
* js는 표준 JS + TypeScript가 추가될 수 있다.

## React.js + spring-boot 설정

### 1. 이클립스 : React.js + spring-boot 설정

* File &gt; New &gt; Other... 
* Spring Starter Project &gt; 이름지정, 배포 확장자 War &gt; Next
* Spring Web, Spring Boot DevTools 체크 &gt; Finish
* 프로젝트 파일에  node\_modules.zip package.json package-lock.json webpack.config.js 추가

### 2. Visual Studio Code : MainPage.jsx

```java
import React from 'react';
import ReactDOM from 'react-dom';

//클래스 생성 및 선언
//가상 돔 구성하기 - 필요한 곳에 미리 렌더링(메모리)했다가 출력이 나갈 떄 결합시킨다.
//React.Component상속 받아 객체를 생성한다.
//안드로이드에서는 class MainActivity 를 만들때 extends AppCompatActivity를 상속 받았으며
//자바도 extends Object하는 등 객체 지향적인 방향으로 가고 있다. 
//코드 블럭을 재사용할 수 있도록 제공한다.
//클래스 내부에는 render함수를 정의해 화면의 출력될 내용을 여기서 만들게 된다.
//안드로이드에서도 life cycle이 제공되고 있다. : onCreate - onStart - onResume - onStop - 상태정보 수정 메서드 추가 위치 - onDestroy 활동을 메서드로 정의했다.
//리액트에서도 상태코드를 관리하고 삽입 시점, 위치에 대한 작업이 필요하다. 클래스 내부어 render처럼 컨텐츠로 들어오게 된다.
class MainPage extends React.Component {
 
    render() {
		//클래스네임은 css사용에 활용될 것이다.
        return <div className="main">메인 페이지</div>;
    }
 
}
//리액트 돔, 즉 가상의 돔이  render라는 콜백함수를 호출하게 된다.
//파라미터로 
//@param1 : 리액트 클래스 이름
//@param2 : 위치 지정
ReactDOM.render(<MainPage/>, document.getElementById('root'));
```

* React를 사용한 jsx문서 작성은 Vscode에서 자동완성 기능을 제공 하는 등의 편의성이 있어 VSCode에서 작성한다.

### 3. 이클립스 : webpack.config.js에 jsx 등록

```javascript
var path = require('path');

module.exports = {
    context: path.resolve(__dirname, 'src/main/jsx'),
    entry: {
         main: './MainPage.jsx'
				,login: './LoginPage.jsx'
    },
    devtool: 'sourcemaps',
    cache: true,
    output: {
        path: __dirname,
        filename: './src/main/webapp/js/react/[name].bundle.js'
    },
    mode: 'none',
    module: {
        rules: [ {
            test: /\.jsx?$/,
            exclude: /(node_modules)/,
            use: {
                loader: 'babel-loader',
                options: {
                    presets: [ '@babel/preset-env', '@babel/preset-react' ]
                }
            }
        }, {
            test: /\.css$/,
            use: [ 'style-loader', 'css-loader' ]
        } ]
    }
};
```

* 등록된 jsx를 표준 JS로 변환시켜주는 웹 팩 파일이다.
* 5-8번에서 jsx문서를 등록한다.
* 11번에서 output\(변환되는\) path가 지정된다.
* 13번에서 접두어 + 파일이름 + 접미어를 붙여 파일 경로를 완성한다.
* 20-25번은 jsx를 만들 때 바벨라이브러리를 참조하는 구문이다.

### 3. 이클립스 : termenal 가동, 웹팩 실행

![](../../.gitbook/assets/d%20%281%29.png)

* 프로젝트 우클릭 &gt; Show in &gt; terminal
* 입력 : node\_modules\.bin\webpack --watch -d
* 위 이미지로 js문서로 변환된 것을 알 수 있다.

