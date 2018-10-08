# ReactTutorial
이제부터 정말 간단한 React 튜토리얼을 시작해보겠습니다.

우선 [노드 튜토리얼](https://github.com/Reminsce/NodeTutorial/blob/master/README.md#nodetutorial)과 [리액트 공식 튜토리얼](https://brunch.co.kr/@hee072794/72) 을 해보지 않았다면 먼저 하고 오셔야합니다.  

## React란
React는 싱글페이지 웹페이지를 제작할 때 최적화되어있습니다. 페이스북에서 만든 프레임워크로, 데이터가 변하기 시작하면 완전히 새로고침되던 기존의 웹과는 달리 데이터가 변한 부분만 새로고침되면 더 효율적이라는 생각으로 만들어지게됩니다.  

## React 서버와 Node 서버 합치기..!
기존에 프로젝트 디렉토리에 있던 파일은 다 지우고
`npm init` 명령어를 칩시다.

`package.json`을 다음과 같이 수정합니다.
```
{
  "name": "reacttutorial",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/Reminsce/ReactTutorial.git"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@babel/cli": "^7.0.0",
    "express": "^4.16.3",
    "mysql2": "^1.6.1",
    "npm": "^6.4.1",
    "sequelize": "^4.39.0"
  },
  "devDependencies": {
    "@babel/core": "^7.1.2",
    "@babel/preset-env": "^7.1.0",
    "@babel/preset-react": "^7.0.0",
    "babel-core": "^7.0.0-bridge.0",
    "babel-loader": "^8.0.4",
    "html-loader": "^0.5.5",
    "html-webpack-plugin": "^3.2.0",
    "react": "^16.5.2",
    "react-dom": "^16.5.2",
    "webpack": "^4.20.2",
    "webpack-cli": "^3.1.2",
    "webpack-dev-middleware": "^3.4.0",
    "webpack-dev-server": "^3.1.9"
  }
}
```
그리고 `npm install`.


설치가 완료됐으면 폴더를 미리 만들어둡시다.  
```
mkdir -p public src/server src/client
```
  
  
`babel` 설정을 위한 `.babelrc` 파일을 만들고 다음과 같이 입력합니다.
```
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}

```

`babel`은 `ES6` 문법을 사용하기 위함이며, `() => {}` 같은 문장을 `function(){}` 으로 translate 해줍니다.  
  
  
다음은 `webpack.config.js`파일을 만들고 다음과 같이 작성합니다.
```
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  module: {
    rules: [
    {
      test: /\.js$/,
      exclude: /node_modules/,
      use: {
        loader: 'babel-loader'
      }
    }, {
      test: /\.html$/,
      use: [
      {
        loader: 'html-loader'
      }
      ]
    }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: __dirname + '/public/index.html',
      filename: './index.html'
    })
  ]
}
```
  
  
`public/index.html` 파일을 만들고 다음과 같이 작성합니다.
```
<html lang="en">
  <head>
    <meta charset="utf-8">
  </head>
  <body>
    <div id="root">
    </div>
  </body>
</html>
```

`src/server/server.js` 파일을 다음과 같이 작성합니다.
```
import express from 'express';
const app = express();

import webpack from 'webpack';
import middleware from 'webpack-dev-middleware';
import webpackConfig from '../../webpack.config';
const compiler = webpack(webpackConfig);

const PORT = process.env.PORT || 8080;

app.use(middleware(compiler, {
  
}));

app.get('/api/users', function(req, res) {
  res.send('Hi I am DongKyoo!');
});

app.listen(PORT, '0.0.0.0',  function() {
  console.log("DongKyoo's server is starting!" + PORT);
});
```
이 부분이 제일 중요합니다. 기존에 `npm start`를 통해 알 수 없는 자동화 툴로 `React`가 켜졌다면 그 작업을 수동으로 해주는 부분입니다.  
  
몰랐을 수도 있지만 `babel`과 `webpack`설정 또한 자동으로 이루어지고 있었습니다.  
  
다음은 클라이언트(React) 소스입니다. `src/client/user.js`를 만들고 다음과 같이 작성합니다.
```
import React, { Component } from "react";
import ReactDOM from "react-dom";

class User extends Component {
    render() {
      return (<div>hi</div>)
    }
}

const wrapper = document.getElementById("root");
ReactDOM.render(<User />, wrapper);

export default User;
```

마지막으로 `webpack`은 자동으로 `src/index.js`를 찾도록 설정되어 있어서 `src/index.js`를 만들고 다음과 같이 수정합니다.

```
import User from "./client/user"; 
```

`babel`을 통해 번역된 js파일을 node로 실행하기 위해 다음 명령어로 `babel-node`를 설치합니다.
```
npm install -g @babel/node
```
`-g` 옵션은 global의 약자로, 프로젝트에 한정된 모듈이 아니라 컴퓨터 전체에 영향을 미치는 모듈이라는 뜻입니다.  
  
  
설치가 되었으면 다음 명령어로 서버를 실행합니다.
```
babel-node src/server/server.js
```
  
  
그럼 다음과 같이 실행됩니다.  
![run](https://user-images.githubusercontent.com/10896116/46278039-02f74980-c5a0-11e8-83bc-06b52705163b.PNG)
  
  
저는 원격 서버라 url이 `35.232.22.98`이지만 여러분들은 `localhost:8080`으로 접속하면 될 것입니다.  
  
`React 렌더링 된 모습`  
![result1](https://user-images.githubusercontent.com/10896116/46278037-025eb300-c5a0-11e8-89b1-4b0c97cdfd6a.PNG)  
  
`Node.js 서버에서 넘어온 메세지`  
![result2](https://user-images.githubusercontent.com/10896116/46278038-02f74980-c5a0-11e8-937b-965ca6e8a3bd.PNG)  

## React 기본
본 튜토리얼에서 `src` 폴더 내의 `client` 디렉토리가 `React` 를 담당합니다.  
`user.js` 파일을 다음과 같이 수정합니다.
```
import React, { Component } from "react";
import ReactDOM from "react-dom";

class User extends Component {
  constructor(props) {
    super(props);
    
    this.state = {
      users: [
        {
          name: '이동규'
        },
        {
          name: '전유정'
        },
        {
          name: '김아정'
        },
        {
          name: '남혜미'
        }
      ]
    };
  }
  
  render() {
    const users = this.state.users.map(user => {
      return (<li>{ user.name }</li>);
    });
    return (<div><ul>hi</ul></div>)
  }
}

const wrapper = document.getElementById("root");
ReactDOM.render(<User />, wrapper);

export default User;
```
찬찬히 뜯어보겠습니다.  
  
`User`의 생성자에서 this.state 변수를 초기화시켜줍니다. 여기서 `this`의 범위는 `User` 클래스입니다. `User`클래스에 state라는 전역변수가 생겼다고 이해하시면 편합니다.  
  
`render`함수는 `<User/>`가 호출됨과 동시에 실행되는 함수로서, 그 결과는 다음과 같습니다.  

`<User/>` -> `<div><ul>{users}</ul></div>`  
  
이제 `{users}`라는 변수에 값을 채워넣을 차례입니다. 중괄호는 html 소스 내에서 변수를 표현하는 표현식입니다.  
즉
```
const users = this.state.users.map(user => {
  return (
    <li>{user.name}</li>
  );
});
```
에서 생성된 배열이 `{user}`에 들어가게 되는 것입니다.  
결론부터 말하자면 `{user}`의 결과값은 아래와 같습니다.
```
<li>이동규</li>
<li>전유정</li>
<li>김아정</li>
<li>남혜미</li>
```

지금부터 왜 그렇게 나오는지 알아보겠습니다.  
우선 `map` 함수에 대해서 알아야 합니다.  
`map`함수는 자바스크립트 배열에 있는 함수로, `for`문처럼 loop를 돌면서 return되는 객체들을 모아 새로운 배열을 만들어주는 함수입니다.
간단한 예시로 설명하겠습니다.
```
var a = [1,2,3];
var b = a.map(num => {
    return num + 1;
});
// b = [2,3,4]
```
이해가 되시나요? a를 반복하며 각 element가 `num`안에 들어갑니다. 즉 num은 1, 2, 3을 각각 갖게 됩니다.  
리턴 값으로는 `num + 1`을 리턴하게 되니 `b`는 결국 `[2,3,4]` 를 갖게 됩니다.

```
var a = [1,2,3];
var b = a.map(n => {
    if (n % 2 == 1)
        return n;
});
// b = [1, 3]
```
위 소스의 결과를 이해하셨다면 `map`함수에 대해 대략적으로 이해하신 겁니다.  
  
다시 원래 소스로 돌아와
```
const users = this.state.users.map(user => {
    // user = {name: '이동규'} But it is different for each loop! It could be {name: '전유정'} too.
  return (
    <li>{user.name}</li>
  );
});
// users = [(<li>이동규</li>), (<li>전유정</li>), (<li>김아정</li>), (<li>남혜미</li>)]
```
이 소스를 다시 보신다면 그 결과를 예측할 수 있습니다.  
  
이렇게 만들어진 users를
```
render() {
    const users = this.state.users.map(user => {
      return (
        <li>{user.name}</li>
      );
    });
    return (<div><ul>{users}</ul></div>);
}
```
render 함수에서 return하면 유정하우스 멤버의 리스트가 출력되게됩니다.

  
약간의 수정을 더해서 `constructor`의 `this.state`를 다음과 같이 수정합니다.
```
this.state = {
  users: [
    {
      name: '이동규',
      playgrounds: ['계룡', '조와']
    },
    {
      name: '전유정',
      playgrounds: ['논산', '딸기밭']
    },
    {
      name: '김아정',
      playgrounds: ['TK']
    },
    {
      name: '남혜미',
      playgrounds: ['인천', '국제공항']
    }
  ]
};
```

뿐만아니라 `render`함수 또한 다음과 같이 수정합니다.

```
render() {
    const users = this.state.users.map(user => {
      const playgrounds = user.playgrounds.map(playground => {
        return (
          <li>
            {playground}
          </li>
        );
      });
      
      return (
        <span>
          <li>{user.name}</li>
          <ol>
            {playgrounds}
          </ol>
        </span>
      );
    });
    return (<div><ul>{users}</ul></div>);
}
```
달라진 users를 봅시다. `this.users.map`의 인자함수(user => {}) 내에서 또 다른 map함수가 보입니다.  
```
// user = {name: '이동규', playgrounds: ['계룡', '조와']}
const playgrounds = user.playgrounds.map(playground => {
    // playground = '계룡' 이는 매 루프마다 바뀌며
    // 다음 루프에서는 playground = '조와'
    return (
      <li>
        {playground}
      </li>
    );
});
// playgrounds = [(<li>계룡</li>), (<li>조와</li>)]
```

따라서 users는
```
const users = this.state.users.map(user => {
  const playgrounds = user.playgrounds.map(playground => {
    return (
      <li>
        {playground}
      </li>
    );
  });
  // playgrounds = [(<li>계룡</li>), (<li>조와</li>)]
  
  return (
    <span>
      <li>{user.name}</li>
      <ol>
        {playgrounds}
      </ol>
    </span>
  );
  // return = <span><li>이동규</li><ol><li>계룡</li><li>조와</li></ol></span>
});
// users = [<span><li>이동규</li><ol><li>계룡</li><li>조와</li></ol></span>, <span><li>전유정</li><ol><li>논산</li><li>딸기밭</li></ol></span>
...]
```

다음과 같은 결과가 화면에 나온다면 성공한 것입니다!
![결과1](https://user-images.githubusercontent.com/10896116/46149239-d5ab5280-c2a4-11e8-9c32-f22b8fe2cc5f.PNG)
  
  
## 서버 연동
이번 파트부터는 서버와 MySQL연동이 나옵니다. [MySQL 튜토리얼](https://github.com/Reminsce/MySqlTutorial#mysqltutorial)을 먼저 하고 와주세요.
대부분의 웹페이지는 `서버`와 연동됩니다.  
`서버`는 말그대로 `서비스를 제공하는 자`입니다.  
우리가 단순히 네이년 페이지에 접속하는 행위도 사실은 네이버 서버에 접속하는 것입니다.
  
서버에 대한 개념이 처음이시라면 아래 그림이 아리송할 것입니다.
![네이년](https://user-images.githubusercontent.com/10896116/46149957-6cc4da00-c2a6-11e8-8340-f764518940d2.PNG)
  
사용자는 말그대로 네이년을 사용하는 평범한 사람 또는 PC가 될 것입니다.  
사용자가 www.naver.com이라고 주소창에 입력한 뒤 엔터를 치는 순간 네이년 서버에 접속을 시도하게 됩니다.  
사용자의 접속을 인지한 서버는 네이버 페이지를 응답(respond)하게 됩니다.  
이는 우리가 위에서 localhost:3000에 접속했을 때 React 페이지가 로딩되며 유정하우스 멤버가 나온것과 동일합니다.  
하지만 우리는 아직 DB를 사용한 적이 없습니다.  
말하자면 다음과 같은 상태입니다.

![사진1](https://user-images.githubusercontent.com/10896116/46149959-6cc4da00-c2a6-11e8-8c14-cc0155cd431a.PNG)
다음은 우리가 구현하고자 하는 궁극적인 형태입니다.  
![db1](https://user-images.githubusercontent.com/10896116/46284506-0c3de180-c5b3-11e8-972a-cf16b3128de4.PNG)

조금 더 구체적으로 하자면 다음과 같을 것입니다.  
![db2](https://user-images.githubusercontent.com/10896116/46284618-78b8e080-c5b3-11e8-98a7-5c58836b37a4.PNG)
다음과 같이 표현한 이유는 사용자 입장에서는 그냥 `서버`에 접속하는 것이지만 우리는 내부적으로 `클라이언트(React)`와 `서버(Node)`를 구분하여 개발할 것이기 때문입니다.  

`React`는 단순히 우리에게 화면을 보여주는 역할만 담당합니다.  
`DB연동`과 같은 비즈니스 로직은 `Node`에서 담당합니다.  
  
  
### axios
`React`는 `Node`로부터 데이터를 제공받고, 제공 받은 데이터를 화면에 보여줍니다.  

`React`와 `Node`는 `http 통신`으로 의사소통을 합니다.  

하지만 React는 http 통신을 할 능력이 없습니다. 그래서 `axios`라는 모듈의 도움을 필요로 합니다.  

아래 명령어로 `axios`를 설치합니다.
```
npm install --save axios
```
설치가 완료되었다면  
`src/client/user.js`파일을 다음과 같이 수정합니다.
```
import React from 'react';
import ReactDOM from 'react-dom';
import axios from 'axios';

class ShowUser extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
    };
  }
  
  componentDidMount() {
    axios.get('/api/users').then(response => {
      console.log(response);
    });
  }
  
  render() {
//     const users = this.state.users.map(user => {
//       const playgrounds = user.playgrounds.map(playground => {
//         return (
//           <li>
//             {playground}
//           </li>
//         );
//       });
      
//       return (
//         <span>
//           <li>{user.name}</li>
//           <ol>
//             {playgrounds}
//           </ol>
//         </span>
//       );
//     });
//     return (<div><ul>{users}</ul></div>);
    return <div></div>;
  }
}

ReactDOM.render(
  <ShowUser/>,
  document.getElementById('root')
);
```
새로운 `componentDidMount` 함수가 눈에 띕니다.  
이 함수는 컴포넌트가 화면에 안정적으로 렌더링 되었을 때(이를 mount라고 표현합니다) 리액트가 자동으로 호출하는 함수입니다.  
네트워크 처리는 `componentDidMount`에서 처리하는 것이 리액트 권장사항입니다!  
  
### Node
`src/server/server.js` 파일을 다음과 같이 수정합니다.
```
app.get('/api/users', function(req, res) {
  let users = [
    {
      name: '이동규',
      playgrounds: ['계룡', '조와']
    },
    {
      name: '전유정',
      playgrounds: ['논산', '딸기밭']
    },
    {
      name: '김아정',
      playgrounds: ['TK']
    },
    {
      name: '남혜미',
      playgrounds: ['인천', '국제공항']
    }
  ];
  res.send(users);
});
```
  
#### Tip
`주소`는 `호스트네임`과 `포트`로 이루어져있습니다.  
`localhost` = 호스트네임, `3002`는 포트입니다.  
이 둘이 온전히 조합되어야 하나의 주소가 됩니다. 즉 `localhost:3000`과 `localhost:3002`는 완전히 다른 서버인 것입니다.
  
네이버를 예로 들겠습니다.  
`www.naver.com`이 호스트네임, `80`이 포트입니다. 80은 디폴트 값으로 아무것도 입력하지 않을 때 적용됩니다.  
테스트 하고 싶다면 지금 `네이버`에 접속해보세요.  
그리고 주소 맨 끝에 `www.naver.com:80` 이와 같이 80번 포트를 적어주고 다시 접속해보세요.  
마찬가지로 80을 다른 수로 바꾼 뒤 접속해보세요.
  
  
원래 소스로 돌아와서
```
app.get('/api/users', function(req, res) {

// axios.get('/api/users')
```
눈치 빠른 사람이라면 `노드서버`소스의 `app.get`과 `리액트서버`소스의 `axios.get`이 같은 `get`임을 알 수 있습니다.  
그렇다면 `노드서버`소스가 `app.post`로 바뀐다면 `리액트서버` 소스도 `axios.post`로 바뀌어야 합니다.
```
app.post('/api/users', function(req, res) {

// axios.post('/api/users')
```
약간의 설명을 하자면, `app.post('/api/users', function(req, res){`는 `/api/users` url로는 `post`로만 접근 할 수 있어! 라고 정의내린 것이고, 이에 맞게 `axios`는 `post`방식으로 `/api/users`에 접근 한 것입니다.  
당연하겠지만 서로 맞지 않은 메소드로 접근은 불가능합니다.


이에 대해 조금 더 알고 싶다면 [다음](http://gyrfalcon.tistory.com/entry/HTTP-%EC%9D%91%EB%8B%B5-%EC%BD%94%EB%93%9C-%EC%A2%85%EB%A5%98-HTTP-%EB%A9%94%EC%86%8C%EB%93%9C-%EC%A2%85%EB%A5%98)을 참조하세요.

#### 결과
수정된 소스가 적용된 React 페이지에 접속해보면 아무것도 없는 흰 화면만 뜰것입니다.  
하지만 (크롬 기준) `F12`나 `개발자도구`를 켜보면 오른쪽에 로그가 찍힌것을 확인할 수 있습니다.  
```
axios.get('/api/users').then(response => {
  console.log(response);
});
```
`console.log('')` 함수 덕분에 생긴 로그입니다.  

![로그](https://user-images.githubusercontent.com/10896116/46197773-a7ca1a80-c345-11e8-8fa3-63deb682266f.PNG)

화살표를 눌러 내용을 확인하다보면 `data` 속성에 다음과 같이 우리가 원하는 정보가 들어있음을 확인할 수 있습니다.  
```
data:
Array(4)
0:{name: "이동규", playgrounds: Array(2)}
1:{name: "전유정", playgrounds: Array(2)}
2:{name: "김아정", playgrounds: Array(1)}
3:{name: "남혜미", playgrounds: Array(2)}
length:4
__proto__:Array(0)
```

이로써 `axios` 를 통해 React가 Node와 의사소통하는 것을 확인하였습니다.  
하지만 아직 Node가 데이터를 저장하고, 수정하는 부분이 없습니다. 이는 `DB`를 통해 해보도록 하겠습니다.  

## DB 연동
우선 MySQL을 `노드 서버`에서 사용하기 위한 설치가 필요합니다.
```
npm install --save mysql
```
`src/server/db` 디렉토리를 만들고, `db_info.js`를 생성합니다.
```
// db_info.js
module.exports = (function() {
  return {
    local: {
      host: 'localhost',
      port: '3306',
      user: 'root',
      password: 'your password',
      database: 'TUTORIAL'
    },
    real: {
      host: '',
      port: '',
      user: '',
      password: '',
      database: 'TUTORIAL'
    },
    dev: {
      host: '',
      port: '',
      user: '',
      password: '',
      database: 'TUTORIAL'
    }
  }
})();
```
`Git`을 쓰고 있는 여러분들도 잘 알다시피 소스코드가 `로컬 소스`, master 브랜치에 해당되는 `운영 소스`, develop 브랜치에 해당되는 `개발 소스`로 나뉘듯이  
데이터베이스도 그에 맞게 `로컬 DB`, `개발 DB`, `운영 DB`로 나뉘게 됩니다.  
각각에 상응하는 설정을 적어주면 되며, 현재 저희는 `로컬 DB` 밖에 없으므로 위와 같이 설정해줍니다.  
  
마찬가지로 `db`폴더 내에 `db_con.js` 파일을 만들고 다음과 같이 작성합니다.
```
// db_con.js
const mysql = require('mysql');
const configuration = require('./db_info');
const local = configuration.local;

module.exports = function() {
  return {
    init() {
      return mysql.createConnection({
        host: local.host,
        port: local.port,
        user: local.user,
        password: local.password,
        database: local.database
      });
    },
    
    test_open(con) {
      con.connect(function(err) {
        if (err) {
          console.error('mysql connection error : ' + err);
        } else {
          console.info('mysql is connected successfully');
        }
      });
    }
  }
};
```

이제 `server.js`로 돌아와 맨 윗부분을 다음과 같이 수정합니다.
```
import express from 'express';
const app = express();

import webpack from 'webpack';
import middleware from 'webpack-dev-middleware';
import webpackConfig from '../../webpack.config';
const compiler = webpack(webpackConfig);

const mysql_connector = require('./db/db_cons')();
const conn = mysql_connector.init();
mysql_connector.test_open(conn);
```
  
그럼 다음과 같이 성공적으로 연결될 것입니다.
```
mysql is connected successfully
```

혹시 `ACCESS DENIED` 관련 에러가 발생하면 비밀번호를 확인해주시면 됩니다.  
여기까지는 [이곳](https://cheese10yun.github.io/mysql-node/)을 참조하여(?) 사실 거의 베껴왔습니다.  

## 노드에 MySQL 쿼리문 적어보기
`노드서버 - server.js`를 다음과 같이 수정합니다.
```
app.get('/api/users', function(req, res) {
  conn.query('SELECT * FROM tbl_users', function(err, result) {
    if (err) throw err;
    console.log(result);
  });
  
  let users = [
  ...
```
인터넷을 통해 `localhost:8080/api/users`에 접속해봅시다.  
크롬 페이지보다는 `노드 서버`를 실행시켰던 `cmd(또는 터미널)` 창에 아래 사진과 같은 로그가 뜰 것입니다.  

![db](https://user-images.githubusercontent.com/10896116/46241095-e36bef80-c3ec-11e8-99aa-7b7253805dbd.PNG)
  
  
#### 소소한 Tip
이미 눈치 챘을 수도 있지만 주소창에 url을 입력하고 엔터를 치는 행위는 `get`방식 요청입니다.  
즉 `app.get('/api/users')` 는 `axios.get('localhost:8080/api/users')`로 접근 가능하지만 브라우저 주소창에 `localhost:8080/api/users`를 입력하고 엔터치는 방법으로도 접근할 수 있습니다.  
  
어찌됐든 mysql과 연결된 `conn`을 통해 원하는 쿼리를 날리면 `result`를 통해 결과값이 넘어옵니다.  
로그를 보면 다소 이상한 형태라고 생각할 수 있는데 사실 그냥 `json`형태 입니다.  

즉
```
[{
    id: '20161234',
    name: 'aaaaajeong'
},{
    id: '20161818',
    name: 'yujeong'
},{
    id: '20162489',
    name: 'dongkyoo'
},{
    id: '20169876',
    name: 'ham'
}]
```
인 것입니다. 하지만 이는 클라이언트에서 원하는 형태가 아닙니다.  
클라이언트는 `name`과 `client`를 원하고 있으므로 우리는 다음과 같은 형태를 목표로 합니다.  
```
[
    {
      name: '이동규',
      playgrounds: ['계룡', '조와']
    },
    {
      name: '전유정',
      playgrounds: ['논산', '딸기밭']
    },
    {
      name: '김아정',
      playgrounds: ['TK']
    },
    {
      name: '남혜미',
      playgrounds: ['인천', '국제공항']
    }
];
```
조금 더 개선된 버전은 다음과 같습니다.
```
[
    {
      id: '20162489',
      name: '이동규',
      playgrounds: ['계룡', '조와']
    },
    {
      id: '20161818',
      name: '전유정',
      playgrounds: ['논산', '딸기밭']
    },
    {
      id: '20161234',
      name: '김아정',
      playgrounds: ['TK']
    },
    {
      id: '20169876',
      name: '남혜미',
      playgrounds: ['인천', '국제공항']
    }
];
```

`src/server/server.js`를 다음과 같이 수정합니다.
```
conn.query('SELECT u.id, u.name, p.playground FROM tbl_users u LEFT JOIN tbl_user_playgrounds p ON p.user_id = u.id', function(err, result) {
    if (err) throw err;
    console.log(result);
  });
```

쿼리문을 좋기보게 쓰면 다음과 같습니다.
```
SELECT
    u.id, u.name, p.playground
FROM tbl_users u
LEFT JOIN tbl_user_playgrounds p
ON p.user_id = u.id
```
우선 `tbl_users u`는 `tbl_users`의 별명을 `u`라고 지정한 것입니다.  
마찬가지로 `tbl_user_playgrounds p`는 `tbl_user_playgrounds`의 별명을 `p`라고 지정한 것입니다.  

### JOIN
`Join`은 `A테이블`과 `B테이블`이 있을 때 이 둘을 엮어서 하나의 테이블처럼 생각할 수 있는 기능이다.  
- JOIN = 양 테이블이 ON 조건과 모두 일치하는 경우
- LEFT JOIN = 왼쪽 테이블은 무조건 출력되고 오른쪽 테이블만 ON 조건과 일치하는 경우
  
말로는 어려우니 예를 들어 설명하겠습니다. 참고로 현재 tbl_user_playgrounds에는 아무 데이터도 없는 상황입니다.
```
// JOIN
SELECT
    u.id, u.name, p.playground
FROM tbl_users u
JOIN tbl_user_playgrounds p
ON p.user_id = u.id
// Return Null
```
에서
```
SELECT
    u.id, u.name, p.playground
FROM tbl_users u
JOIN tbl_user_playgrounds p
```
까지는 `tbl_users`와 `tbl_user_playgrounds`를 묶었다. 라는 의미입니다.  
하지만 중요한건 그 다음입니다.
`ON` 조건에 맞는 ROW끼리 묶이게 되는데
`tbl_users`의 id와 `tbl_user_playgrounds`의 user_id가 일치하는 row는 묶는다! 라는 의미가 됩니다.  
하지만 `tbl_users`의 `id`인 `20162489`와 `tbl_user_playgrounds`의 `user_id`가 `20162489`인 row가 없기 때문에 `20162489`는 결과값에서 제외됩니다.  
마찬가지로 `tbl_users`의 `id`인 `20161818`과 `tbl_user_playgrounds`의 `user_id`가 `20161818`인 row가 없기 때문에 `20161818` 또한 결과값에서 제외됩니다.  
결국 어떠한 값도 매칭되지 못하고 쿼리 결과값은 `Null`이 되게 됩니다.
```
// LEFT JOIN
SELECT
    u.id, u.name, p.playground
FROM tbl_users u
LEFT JOIN tbl_user_playgrounds p
ON p.user_id = u.id
// Return [{u.id='20162489', u.name='dongkyoo', p.playground=null}, {...}, ...]
```
하지만 `Left JOIN`은 왼쪽(쿼리를 일렬로 적었을 때 기준, 즉 tbl_users)의 결과는 무조건 출력한 뒤 오른쪽의 결과만 `ON` 조건에 맞춰서 붙여줍니다.  
즉 `[{u.id='20162489', u.name='dongkyoo', p.playground=null}, {...}, ...]` 과 같은 형태로 값이 리턴되게 됩니다.  
  
조금 더 확실히 해보기 위해서 `tbl_user_playgrounds`에 값을 넣어보겠습니다.  

![db2](https://user-images.githubusercontent.com/10896116/46241421-adcb0480-c3f4-11e8-8ca0-aeabfbd8b7bb.PNG)
  
`JOIN 결과`  
![db3](https://user-images.githubusercontent.com/10896116/46241438-fb477180-c3f4-11e8-8c12-296bd104b6bb.PNG)
  
`LEFT JOIN 결과`  
![db4](https://user-images.githubusercontent.com/10896116/46241439-fb477180-c3f4-11e8-8018-91a9e9c529e1.PNG)

차이가 눈에 보이시나요?

`tbl_user_playgrounds` 를 다음과 같은 결과가 나오도록 값을 `INSERT` 해보세요.  
  
![db](https://user-images.githubusercontent.com/10896116/46389829-589e3400-c70f-11e8-8616-8ba21164acf8.PNG)


`server.js`를 다음과 같이 수정합니다.
```
app.get('/api/users', function(req, res) {
  conn.query('SELECT u.id, u.name, p.playground FROM tbl_users u LEFT JOIN tbl_user_playgrounds p ON p.user_id = u.id', function(err, result) {
    if (err) throw err;
    res.send(result);
  });
});
```

`/api/users`에 접속하면 다음과 같은 결과가 화면에 나옵니다.  
  
![result](https://user-images.githubusercontent.com/10896116/46389861-9602c180-c70f-11e8-90fe-5235e8df676d.PNG)

```
[{"id":"20162489","name":"dongkyoo","playground":"Gaeryong"},
{"id":"20161818","name":"yujeong","playground":"Nonsan"},
{"id":"20161818","name":"yujeong","playground":"Strawberry farm"},
{"id":"20161234","name":"ham","playground":"TK"},
{"id":"20169876","name":"aaaaajeong","playground":"Incheon"},
{"id":"20169876","name":"aaaaajeong","playground":"International Airport"}]
```
하지만 이는 우리가 원하는 형태가 아닙니다.  
  
우리가 원하는 형태는 다음과 같습니다.  
```
[{"id":"20162489","name":"dongkyoo","playgrounds":["Gaeryong"]},
{"id":"20161818","name":"yujeong","playgrounds":["Nonsan", "Strawberry farm"]},
{"id":"20161234","name":"ham","playgrounds":["TK"]},
{"id":"20169876","name":"aaaaajeong","playgrounds":["Incheon", "International Airport"]}]
```
  
차이를 아시겠나요? `id`를 기준으로 `playgrounds`가 배열로서 묶여서 표현되었습니다.  
  
## Sequelize
와 같이 `가공된 형태`의 데이터를 만들어주는 것을 `orm(Object Relational Mapping)`이라고 합니다. 그리고 지금부터 `Node.js`의 강력한 `ORM` 모듈 `Sequelize`를 소개합니다.  
  
### 설치
다음 명령어로 sequelize 와 mysql2를 설치합니다
```
npm install --save sequelize
npm install --save mysql2
```
`sequelize`는 `mysql`이 아닌 `mysql2`를 지원합니다. 2라고 해서 다른게 아니라 그냥 이름만 다른 mysql이니 mysql2를 또 공부할 필요는 없습니다.  
  
설치가 완료되었다면 `server.js`를 다음과 같이 수정합니다.

### 
```
import express from 'express';
const app = express();

import webpack from 'webpack';
import middleware from 'webpack-dev-middleware';
import webpackConfig from '../../webpack.config';
const compiler = webpack(webpackConfig);

// Sequelize
import Sequelize from 'sequelize';

// DB 접속
const sequelize = new Sequelize('TUTORIAL', 'root', 'your password', {
  host: 'localhost',
  dialect: 'mysql'
});

// 테이블 정의
const User = sequelize.define('user', {
  id: {
    type: Sequelize.STRING,
    primaryKey: true
  },
  name: Sequelize.STRING
}, {
  createdAt: false,
  updatedAt: false,
  tableName: 'tbl_users'
});
```

`server.js`의 `api/users`부분도 다음과 같이 수정합니다.
```
app.get('/api/users', function(req, res) {
  User.findAll().then((reuslt) => {
    res.send(reuslt);
  });
});
```

그리곤 `api/users`에 접속하면 다음과 같은 결과가 나올 것입니다.  
  
![result2](https://user-images.githubusercontent.com/10896116/46390454-e29bcc00-c712-11e8-9bc8-16a8e2f89fda.PNG)
  
이어서 `server.js`를 다음과 같이 수정합니다.  
```
// tbl_user_playgrounds 테이블 정의
const Playground = sequelize.define('playground', {
  user_id: Sequelize.STRING,
  playground: Sequelize.STRING
}, {
  createdAt: false,
  updatedAt: false,
  tableName: 'tbl_user_playgrounds'
});

User.hasMany(Playground, {
  foreignKey: 'user_id',
  sourceKey: 'id'
});
```
`Sequealize`에서는 테이블 정의 시 데이터가 만들어진 시점을 저장하는 `createdAt`과 데이터가 마지막으로 수정된 시점을 저장하는 `updatedAt` 컬럼을 자동으로 생성합니다.  
  
하지만 우리는 그것을 원하지 않으므로 `define`함수의 마지막 `option` 파라미터에 `{createdAt: false, updatedAt: false}`를 넘김으로서 이를 막습니다.  
  
```
User.hasMany(Playground, {
  foreignKey: 'user_id',
  sourceKey: 'id'
});
```
이는 `User`와 `Playground` 사이의 `1:N` 관계를 정의합니다.  
  
`foreignKey: 'user_id'`는 Playground의 `user_id`가 `Foreign Key`로 잡혀있고, 그 연결대상은 `sourceKey: 'id'`  
  
즉 User의 `id`다 라는 뜻입니다.  
  
한 가지 알아둬야 할 것은 `User.someFunction(Playground);` 형태의 함수가 있다면 `source`는 `User`, `target`은 `Playground`입니다.  

현재까지의 모든 `server.js` 소스입니다.
```
import express from 'express';
const app = express();

import webpack from 'webpack';
import middleware from 'webpack-dev-middleware';
import webpackConfig from '../../webpack.config';
const compiler = webpack(webpackConfig);

import Sequelize from 'sequelize';
const sequelize = new Sequelize('TUTORIAL', 'root', 'your password', {
  host: 'localhost',
  dialect: 'mysql'
});

const User = sequelize.define('user', {
  id: {
    type: Sequelize.STRING,
    primaryKey: true
  },
  name: Sequelize.STRING
}, {
  createdAt: false,
  updatedAt: false,
  tableName: 'tbl_users'
});

const Playground = sequelize.define('playground', {
  user_id: Sequelize.STRING,
  playground: Sequelize.STRING
}, {
  createdAt: false,
  updatedAt: false,
  tableName: 'tbl_user_playgrounds'
});

User.hasMany(Playground, {
  foreignKey: 'user_id',
  sourceKey: 'id'
});

const PORT = process.env.PORT || 8080;

app.use(middleware(compiler, {
  
}));

app.get('/api/users', function(req, res) {
  User.findAll().then((reuslt) => {
    res.send(reuslt);
  });
});

app.listen(PORT, '0.0.0.0',  function() {
  console.log("http://35.232.22.98 server is starting!" + PORT);
});
```

그리고 다시 `api/users`로 접속해보아도 결과는 바뀌지 않습니다.  
  
우리는 분명 `Users`와 `Playground`의 관계를 정의내렸는데도 말이죠  
  
### Association
이를 해결하기 위해 `findAll` 함수를 다음과 같이 수정합시다.  
```
User.findAll({
    include: [{
      model: Playground,
      where: {user_id: Sequelize.col('user.id')}, // WHERE playground.user_id = user.id
      attributes: ['playground']  // Playground의 columns 중에서 playground column만 뽑아옴
    }]
  }).then((reuslt) => {
    res.send(reuslt);
  });
```

그리고 `api/users`에 접속하면 결과가 나오지 않을 것입니다.  
  
### 
충격적이게도 `Sequelize`는 모든 테이블에 `Primary Key`가 필요하다고 합니다.  
  
하지만 `tbl_user_playgrounds`에는 `Primary Key`가 지정되어 있지 않습니다.  
  
`mysql`을 통해 다음과 같이 `id` 컬럼과 `Primary Key`를 추가해줍시다.  
![query](https://user-images.githubusercontent.com/10896116/46391670-11b53c00-c719-11e8-8e04-89eb2ef5c9d5.PNG)

`ALTER TABLE tbl_user_playgrounds` = `tbl_user_playgrounds`를 수정하겠다.  
  
`ADD COLUMN` = `column`을 수정하겠다. (이후는 column의 정보)  
  
`id INT PRIMARY KEY AUTO_INCREMENT` = 이름은 `id`, 자료형은 `INT`, `Primary Key`이며, `AUTO_INCREMENT` 자동으로 증가한다.  
자동으로 증가한다는 뜻은 `tbl_user_playgrounds`에 row를 insert 할 때 id에 null 값을 주면 현재 테이블의 id의 최댓값에서 1 더한 값이 자동으로 들어간다는 뜻입니다.  
  
`FIRST`는 `column`이 추가될 위치를 명시하는데 `맨 앞`을 뜻하는 키워드입니다.  
즉 `user_id|playground` 였던 테이블이 `id|user_id|playground`로 바뀌게 됩니다.  
  
![result3](https://user-images.githubusercontent.com/10896116/46391821-b172ca00-c719-11e8-993c-416b3f046af2.PNG)
  
  
위와 같이 `id`에 순차적으로 값이 들어갔음을 볼 수 있습니다.  
  
db를 수정했으면 `sequelize`또한 수정해주어야 합니다.
  
`Playground`를 다음과 같이 수정합니다.
```
const Playground = sequelize.define('playground', {
  id: {
    type: Sequelize.INTEGER,
    primaryKey: true,
    autoincrement: true
  },
  user_id: Sequelize.STRING,
  playground: Sequelize.STRING
}, {
  createdAt: false,
  updatedAt: false,
  tableName: 'tbl_user_playgrounds'
});
```

현재까지의 모든 `server.js` 소스입니다.
```
import express from 'express';
const app = express();

import webpack from 'webpack';
import middleware from 'webpack-dev-middleware';
import webpackConfig from '../../webpack.config';
const compiler = webpack(webpackConfig);

import Sequelize from 'sequelize';
const sequelize = new Sequelize('TUTORIAL', 'root', 'your password', {
  host: 'localhost',
  dialect: 'mysql'
});

const User = sequelize.define('user', {
  id: {
    type: Sequelize.STRING,
    primaryKey: true
  },
  name: Sequelize.STRING
}, {
  createdAt: false,
  updatedAt: false,
  tableName: 'tbl_users'
});

const Playground = sequelize.define('playground', {
  id: {
    type: Sequelize.INTEGER,
    primaryKey: true,
    autoincrement: true
  },
  user_id: Sequelize.STRING,
  playground: Sequelize.STRING
}, {
  createdAt: false,
  updatedAt: false,
  tableName: 'tbl_user_playgrounds'
});

User.hasMany(Playground, {
  foreignKey: 'user_id',
  sourceKey: 'id'
});

const PORT = process.env.PORT || 8080;

app.use(middleware(compiler, {
}));

app.get('/api/users', function(req, res) {
  User.findAll({
    include: [{
      model: Playground,
      where: {user_id: Sequelize.col('user.id')},
      attributes: ['playground']
    }]
  }).then((reuslt) => {
    res.send(reuslt);
  });
});

app.listen(PORT, '0.0.0.0',  function() {
  console.log("http://35.232.22.98 server is starting!" + PORT);
});
```

그리고 결과는 다음과 같습니다.  
  
![result4](https://user-images.githubusercontent.com/10896116/46392126-1b3fa380-c71b-11e8-8bbf-758ac7a07314.PNG)  

우리가 완벽히 원하던 모양은 아니지만 `id`에 맞게 `playgrounds`가 배열의 형태로 묶여진 모습을 볼 수 있습니다.  

원래 생각했던 데이터의 꼴은
```
[{
    name: 'dongkyoo',
    playgrounds; ['Gaeryong', 'Seoul']
}]
```
이였는데 실제 데이터는
```
[{
    name: 'dongkyoo',
    playgrounds:[{
        playground: 'Gaeryong'
    },{
        playground: 'Seoul'
    }]
}]
```
이므로 `playgrounds`의 각 element에 `playground`라는 `key`값이 생겼음을 알 수 있습니다.  
  
따라서 `React`의 소스도 조금 변경될 필요가 있습니다.  
  
`src/client/user.js` 파일 내 `render`함수의 `playgrounds` 부분을 다음과 같이 수정합니다.
```
render() {
    const users = this.state.users.map(user => {
        const playgrounds = user.playgrounds.map(playground => {
            return (
              <li>{playground.playground}</li>          // 기존 : <li>{playground}</li>
            )
          });
        ...
```
  
그리고는 `localhost:8080`에 접속하면 다음과 같이 화면에 뜨게 됩니다.  
  
![default](https://user-images.githubusercontent.com/10896116/46578239-8ba93600-ca35-11e8-89d4-c0bf25589416.PNG)
  
제가 데이터를 잘못 넣어서 아정이가 인천에서 놀게되었지만 어찌됐든 잘 뜹니다.  
  
이로서 React 튜토리얼을 마칩니다.

## Summary
튜토리얼을 작성하다보니 다소 길어지게 된 점 미안하게 생각합니다.  
  
일반적인 React 튜토리얼이라기보다는 프로젝트 생성부터 서버 개설, DB 연동까지 하나의 웹어플리케이션 개발 프로세스를 보이고 싶었습니다.  
  
1. [프로젝트 생성](https://github.com/Reminsce/ReactTutorial#react-%EC%84%9C%EB%B2%84%EC%99%80-node-%EC%84%9C%EB%B2%84-%ED%95%A9%EC%B9%98%EA%B8%B0)
2. 디자인  
이 부분은 기술적인 부분은 아니지만 굉장히 중요합니다. 우리가 어떤 기능을 만들지, 완성된 모습을 그림으로 그릴 수 있다는 것은 분명한 목표가 생기는 일이기 때문입니다. 목표가 분명해야 설계도, 개발도 시작 할 수 있습니다.
3. [데이터 설계](https://github.com/Reminsce/ReactTutorial#%EB%85%B8%EB%93%9C%EC%97%90-mysql-%EC%BF%BC%EB%A6%AC%EB%AC%B8-%EC%A0%81%EC%96%B4%EB%B3%B4%EA%B8%B0)  
어떤 데이터가 어떤 꼴로 제공될 때 본 기능을 만들어 낼 수 있는지 설계하는 것은 매우 중요합니다. 가령 예를 들면 `게시글 작성`이라는 기능을 구현한다면 "음~ 게시글에는 제목, 내용, 작성자, 작성 시간"이 필요하니까 데이터는 `article: {title:'hi', content: 'bye', author: 'dongkyoo', createdAt: '2018-10-07'}` 이런 모양이 좋겠네~ 라고 생각 할 수 있어야 합니다.
4. 표현  
데이터 설계 및 DB 연동을 통해 원하는 꼴의 데이터를 뽑아 냈다면 이를 `React`로 표현하기만 하면 됩니다.

  
  
아무리 큰 프로젝트라도 본 프로세스는 동일하게 적용됩니다.  
어떤 작업을 더 중요시여기냐에 따라 약간의 변형은 있겠지만 결코 생략되진 않습니다.  
본 튜토리얼을 참고하여 내가 어느 단계에 있는지 다음에 무엇을 해야할 지 알게 된다면  
여러분은 뭐든지 만들어 낼 수 있습니다!
