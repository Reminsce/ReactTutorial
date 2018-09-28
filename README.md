# ReactTutorial
이제부터 정말 간단한 React 튜토리얼을 시작해보겠습니다.

우선 [노드 튜토리얼](https://github.com/Reminsce/NodeTutorial/blob/master/README.md#nodetutorial)과 [리액트 공식 튜토리얼](https://brunch.co.kr/@hee072794/72) 을 해보지 않았다면 먼저 하고 오셔야합니다.  

## React란
React는 싱글페이지 웹페이지를 제작할 때 최적화되어있습니다. 페이스북에서 만든 프레임워크로, 데이터가 변하기 시작하면 완전히 새로고침되던 기존의 웹과는 달리 데이터가 변한 부분만 새로고침되면 더 효율적이라는 생각으로 만들어지게됩니다.  

## 프로젝트 생성
node.js와 npm은 노드 튜토리얼에서 모두 설치한 상태이기 때문에 따로 설명하진 않겠습니다.  
아래 명령어를 통해 자동으로 react 앱을 만들어주는 노드 모듈을 설치합니다.  
```
npm install -g create-react-app
```
`-g` 옵션은 global의 약자로, 프로젝트에 한정된 모듈이 아니라 컴퓨터 전체에 영향을 미치는 모듈이라는 뜻입니다.  
아래 명령어를 통해 first-tutorial이라는 폴더와 함께 react 프로젝트가 생성됩니다.  
```
create-react-app first-tutorial
```

그럼 first-tutorial 폴더 안에 `node_module`, `public`, `src` 등등 여러 폴더와 파일들이 생성되어있을 것입니다.  
`src` 폴더 내의 파일을 모두 삭제합시다. (src 폴더는 삭제하면 안된대여)  
바로 `src` 폴더에 `index.js`를 만듭시다.  

```
import React from 'react';
import ReactDOM from 'react-dom';

class ShowUser extends React.Component {
  render() {
    return (<div>HI We Are YuJeong House!</div>);
  }
}

ReactDOM.render(
  <ShowUser/>,
  document.getElementById('root')
);
```
위 코드를 `index.js`에 작성합시다.  
cmd 창에서 `first-tutorial`로 이동한 뒤 `npm start` 명령어를 칩시다.  

```
Compiled successfully!

You can now view first-tutorial in the browser.

  http://localhost:3000/

Note that the development build is not optimized.
To create a production build, use npm run build.
```
위 메세지와 함께 인터넷을 통해 `localhost:3000`으로 접속하면 유정하우스임을 환영하는 메세지가 화면에 뜹니다.  
ShowUser 클래스를 다음과 같이 수정합니다.  
```
class ShowUser extends React.Component {
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
      return (
        <li>{user.name}</li>
      );
    });
    return (<div><ul>{users}</ul></div>);
  }
}
```
찬찬히 뜯어보겠습니다.  
  
`ShowUser`의 생성자에서 this.state 변수를 초기화시켜줍니다. 여기서 `this`의 범위는 `ShowUser` 클래스입니다. 즉 state라는 전역변수가 생겼다고 이해하시면 편합니다.  
  
`render`함수는 `<ShowUser/>`가 호출됨과 동시에 실행되는 함수로서, 그 결과는 다음과 같습니다.  
`<ShowUser/>` -> `<div><ul>{users}</ul></div>`  
  
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
  
다시 원래대로 돌아와
```
const users = this.state.users.map(user => {
    // user = {name: '이동규'} But it is different by loop! It could be {name: '전유정'} too.
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
렌더 함수에서 return하면 유정하우스 멤버의 리스트가 출력되게됩니다.

  
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
![사진2](https://user-images.githubusercontent.com/10896116/46149961-6d5d7080-c2a6-11e8-8432-dd5f3e68f19c.PNG)

`React 서버`는 단순히 우리에게 화면을 보여주는 역할만 담당합니다.  
`DB연동`과 같은 비즈니스 로직은 `Node 서버`에서 담당합니다.  
  
따라서 현재 우리에게 필요한 것은 `Node 서버`와 `데이터 베이스`입니다.

앞서 진행했던 노드 튜토리얼에 이어서 진행하도록 하겠습니다.  
노드 튜토리얼과 리액트 튜토리얼과의 파일명이 겹칠 수 있으므로 앞으로는 파일명 앞에 튜토리얼명을 언급하겠습니다.  
`NodeTutorial - index.js`를 다음과 같이 수정해주세요.
```
const express = require('express');
const app = express();

app.get('/', function(req, res) {
  return res.send('hi');
});

app.get('/users', function(req, res) {
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
  return res.send(users);
});

app.listen(3002, function() {
  console.log('server is openned in 3002.');
});
```
이미 알고 있겠지만 `let users`는 리액트 튜토리얼에 있던 변수입니다.

`React Tutorial - index.js`는 다음과 같이 수정해주세요.
```
class ShowUser extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
        };
    }
    ...
```
지금부터 `this.state`의 값을 서버에서 로딩한 값으로 채워넣을 것입니다.

### axios
React는 http 통신을 할 능력이 없습니다. 그래서 `axios`라는 모듈의 도움을 필요로 합니다.  
아래 명령어로 `axios`를 설치합니다.
```
npm install --save axios
```
설치가 완료되었다면  
`리액트 튜토리얼 - index.js`파일을 다음과 같이 수정합니다.
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
    axios.get('localhost:3002/users').then(response => {
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
`axios.get`은 아주 조금 뒤에 다루도록 하고, `localhost:3002/users`에 대해서 먼저 보도록 하겠습니다.  
`localhost:3002`는 방금 우리가 열었던 `노드서버`의 주소입니다.  

#### Tip
`주소`는 `호스트네임`과 `포트`로 이루어져있습니다.  
`localhost` = 호스트네임, `3002`는 포트입니다.  
이 둘이 온전히 조합되어야 하나의 주소가 됩니다. 즉 `localhost:3000`과 `localhost:3002`는 완전히 다른 서버인 것입니다.
  
네이버를 예로 들겠습니다.  
`www.naver.com`이 호스트네임, `80`이 포트입니다. 80은 디폴트 값으로 아무것도 입력하지 않을 때 적용됩니다.  
테스트 하고 싶다면 지금 `네이버`에 접속해보세요.  
그리고 주소 맨 끝에 `www.naver.com:80` 이와 같이 80번 포트를 적어주고 다시 접속해보세요.  
마찬가지로 80을 다른 수로 바꾼 뒤 접속해보세요.
  
  
`localhost:3002` 뒤에 있는 `/users`는 `노드 튜토리얼 - index.js`에서 호출한 다음 함수 덕분에 생긴 주소 입니다.
```
app.get('/users', function(req, res) {

// axios.get('localhost:3002/users')
```
눈치 빠른 사람이라면 `노드서버`소스의 `app.get`과 `리액트서버`소스의 `axios.get`이 같은 `get`임을 알 수 있습니다.  
그렇다면 `노드서버`소스가 `app.post`로 바뀐다면 `리액트서버` 소스도 `axios.post`로 바뀌어야 합니다.
```
app.post('/users', function(req, res) {

// axios.post('localhost:3002/users')
```
약간의 설명을 하자면, `app.post('/users', function(req, res){`는 `/users` url로는 `post`로만 접근 할 수 있어! 라고 정의내린 것이고, 이에 맞게 `axios`는 `post`방식으로 `/users`에 접근 한 것입니다.  
당연하겠지만 서로 맞지 않은 메소드로 접근은 불가능합니다.


이에 대해 조금 더 알고 싶다면 [다음](http://gyrfalcon.tistory.com/entry/HTTP-%EC%9D%91%EB%8B%B5-%EC%BD%94%EB%93%9C-%EC%A2%85%EB%A5%98-HTTP-%EB%A9%94%EC%86%8C%EB%93%9C-%EC%A2%85%EB%A5%98)을 참조하세요.

#### 결과
수정된 소스가 적용된 React 페이지에 접속해보면 아무것도 없는 흰 화면만 뜰것입니다.  
하지만 (크롬 기준) `F12`나 `개발자도구`를 켜보면 오른쪽에 로그가 찍힌것을 확인할 수 있습니다.  
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
는 사실 안될 수도 있습니다. ㅋㅋ
아래에 적어둔 `cors` 파트까지 따라해보고 다시 시도해보십시오. 될겁니다!

### Nodemon
작업에 앞서 `노드서버`의 소스를 변경할 때마다 `node index.js` 명령어를 통해 서버를 껐다켜줘야만 변경된 소스가 적용됩니다.  
이는 매우 귀찮은 일이므로 소스가 변경되면 자동으로 `노드서버`를 재시작해주는 툴을 깔겠습니다.

```
npm install nodemon -g
```
위 명령어로 `Nodemon`을 설치합니다.  
사용법 또한 간단합니다.  
```
// 기존 = node index.js
nodemon index.js
```
위 명령어로 `노드 서버`를 실행시켜주면 끝입니다!

### Cors
작업을 하면서 알게된 내용인데 웹에는 Cors(Cross Origin Resource Sharing)를 제한하는 것을 권장하고 있다고 합니다.  
Cors란게 뭐냐면 `localhost:3002` 에서 `localhost:3002/something`으로 접속하는건 전혀 문제되지 않지만 `localhost:3000`과 같은 외부 서버에서 `localhost:3002`에 접속하게 되면 문제가 생깁니다.  
쉽게말해서 클라이언트(ex. PC)에서 서버에 접근하는것은 상관 없지만 서버에서 서버로 접근하는 것은 문제가된다는 뜻입니다.  
우리는 `React서버`에서 `Node서버`로 연결하고 있으니 딱 우리의 경우가 문제가 되는 경우입니다.
  
우선 아래 명령어로 cors를 설치합니다.
```
npm install --save cors
```

`노드 튜토리얼 - index.js`의 윗부분을 다음과 같이 수정합니다.
```
const express = require('express');
const cors = require('cors');
const app = express();

app.use(cors());
```
이로써 `리액트 서버`에서 `노드 서버`로 접근이 가능해졌습니다.  
  
## DB 연동
우선 MySQL을 `노드 서버`에서 사용하기 위한 설치가 필요합니다.
```
npm install --save mysql
```
`노드서버`에 `db` 디렉토리를 만들고, `db_info.js`를 생성합니다.
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

이제 `index.js`로 돌아와 맨 윗부분을 다음과 같이 수정합니다.
```
const express = require('express');
const cors = require('cors');
const app = express();
const mysql_connector = require('./db/db_con')();
const conn = mysql_connector.init();
mysql_connector.test_open(conn);
```
  
그럼 다음과 같이 성공적으로 연결될 것입니다.
```
mysql is connected successfully
```

혹시 `ACCESS DENIED` 관련 에러가 발생하면 비밀번호를 확인해주시면 됩니다.  
여기까지는 [이곳](https://cheese10yun.github.io/mysql-node/)을 참조하여(?) 사실 거의 베껴왔습니다.  

