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
