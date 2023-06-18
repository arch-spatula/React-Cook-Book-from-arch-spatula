# React 입문

## State

State랑 props만 알아도 리액트의 상당부분을 알게되는 것입니다.

state를 사용할 때는 useState 훅을 사용합니다. state를 통해 데이터를 변형하는 이유입니다. 컴포넌트에는 라이프 사이클이 있습니다. 화면의 변화된 값을 업데이트하기 위해서 이렇게 합니다. state에 따라 랜더링 여부를 반영하고 판단합니다. 리액트라 변화를 감지하고 즉시 랜더링하게 만들기 위해서 state를 활용합니다.

state는 리액트에서만 제공하고 존재합니다. 이것은 hook이라고 부릅니다.

```js
import React, { useState } from 'react';

function GrandFather() {
  const [name, setName] = useState('김할아'); // 이것이 state!
  return <Mother grandFatherName={name} />;
}

// .. 중략
```

`name`은 state값입니다. `setName`은 Setter함수입니다. 이 함수로 `name`값을 업데이트할 수 있습니다.

```js
// src/App.js

import React, { useState } from 'react';

function Child(props) {
  return (
    <div>
      <button
        onClick={() => {
          props.setName('박할아');
        }}
      >
        할아버지 이름 바꾸기
      </button>
      <div>{props.grandFatherName}</div>
    </div>
  );
}

function Mother(props) {
  return (
    <Child grandFatherName={props.grandFatherName} setName={props.setName} />
  );
}

function GrandFather() {
  const [name, setName] = useState('김할아');
  return <Mother grandFatherName={name} setName={setName} />;
}

function App() {
  return <GrandFather />;
}

export default App;
```

화면에서 바뀐 값만 반영합니다. 서버랑 통신해야 바뀐 값을 저장할 수 있습니다.

위 코드는 `setter`함수도 `props`로 같이 전달합니다.

컴포넌트의 라이프 사이클과 리랜더링 조건을 알아내도록 합니다.

웹사이트에서 사용자가 상호작용해서 데이터를 변형하는 경우입니다.

```js
import React, { useState } from 'react';

function App() {
  const [name, setName] = useState('춉춉이');
  const onClickHandler = () => {
    setName('루랑이');
  };
  return (
    <div>
      <button onClick={onClickHandler}>{name}</button>
    </div>
  );
}

export default App;
```

버튼을 클릭하면 state가 업데이트 됩니다.

```js
import React, { useState } from 'react';

function App() {
  const [text, setText] = useState('');
  const handleTyping = (event) => {
    setText(event.target.value);
  };
  return (
    <div>
      <input type="text" onChange={handleTyping} value={text} />
    </div>
  );
}

export default App;
```

input에 state를 설정하는 방법입니다.

이벤트 객체는 DOM의 개념입니다.

리액트를 사용할 때는 불변성을 주의해야 합니다. 자바스크립트 데이터 중에 원시형은 불변성이 있고 참조형은 불변성이 없습니다. 메모리의 동작 방식의 문제입니다. 원시형을 비교할 때 엄밀비교하고 동일하면 true를 반환하는 이유는 주소가 동일하기 때문입니다.

참조형은 불변성이 없습니다.

```js
let obj1 = { name: 'kim' };
let obj2 = { name: 'kim' };
console.log(obj1 === obj2); // false
```

각다 다른 주소를 갖고 있기 때문에 동일하게 보여도 주소가 다릅니다.

리액트에서는 데이터의 불변성을 지켜주는 것이 중요합니다. 리랜더링 기준은 state가 변하고 안 변하고가 기준입니다. state 변화 전후를 비교합니다. state의 변화를 알아내기 위해서는 메모리 주소를 비교하는 전략을 활용합니다. 리액트에서 state value만 새로 할당하는 것은 리랜더링이 발생하지 않습니다(`state = "valeu"`). setter함수를 통해 변경해야지만 리랜더링이 발생합니다. setter함수를 통해 변경해야 이전 이후 메모리 차이가 나게 말들 수 있기 때문입니다(`setState("new value")`). 리액트는 내부에서 참조형비교의 `false`가 발생하는지 확인하고 변경여부를 판단하는 방식으로 동작합니다.

직접 수정하지 않습니다. 기존의 값을 복사하고 그 이후의 값을 추가하고 삭제하는 방식으로 구현합니다.

## 컴포넌트와 랜더링

이번에는 이론입니다.

리액트의 모든 UI들은 컴포넌트로 만듭니다. 컴포넌트 기반 라이브러리의 장점을 배웁니다.

컴포넌트는 UI의 최소 단위 선언체입니다. 리액트 컴포넌트가 선언체라는 것은 중요한 개념입니다. 이전까지는 직업 DOM을 조작하는 명령형 프로그래밍이었습니다.

명령형: 어떻게(How)를 중요시여겨서 프로그램의 제어의 흐름과 같은 방법을 제시하고 목표를 명시하지 않는 형태입니다.
선언형: 무엇(What)을 중요시 여겨서 제어의 흐름보다는 원하는 목적을 중요시 여기는 형태입니다.

```js
// Hello, World! 화면에 출력하기
// 순수 javaScript 명령형 코드
const root = document.getElementById('root');
const header = document.createElement('h1');
const headerContent = document.createTextNode('Hello, World!');

header.appendChild(headerContent);
root.appendChild(header);
```

위 코드는 명령형입니다. 처음 작게 프로토타입 정도 만들 때는 적당합니다. 하지만 대규모 앱을 만들면 관리하고 변경하기 상당히 어렵습니다.

```js
// React 코드 (선언적인)
const header = <h1>Hello World</h1>; // jsx
ReactDOM.render(header, document.getElementById('root'));
```

위 코드는 선언형입니다. 번들사이즈가 크기 때문에 바로 사용하지 않습니다. 하지만 어느정도 규모가 있으면 비교적 변경하기 쉬운습니다. 관리하기도 용이합니다.

트리거링(triggering): 랜더링이 발생하게 만드는 것

렌더링(rendering): UI 컴포넌트를 html로 변환시키는 것

커밋(commit): 실제 DOM에 UI를 반형하는 것

1. 렌더링 트리거

   - 첫 리액트 앱을 실행했을 때
   - 래액트 내부의 state가 변경되었을 때
     - 컴포넌트 내부 state가 변경되었을 때
     - 컴포넌트에 새로운 props가 변경되었을 때
     - 부모 컴포넌트가 state, props변경으로 랜더링이 발생할 때

2. 리랜더링

   - 리랜더링 트리거는 state를 변경하면 됩니다. 리랜더링을 유발하는 트리거는 더 많이 있습니다. 하지만 지금은 생략합니다.
   - state변화가 발생하면 리랜더링이 됩니다. 여러 state가 바뀌면 큐 자료구조로 처리합니다. 변경이 가해진 순서대로 처리합니다.

3. 브라우저 렌더링

   - 브라우저의 랜더링과 리액트의 랜더링은 당연히 다른 것입니다. 혼동을 피하기 위해서 브라우저 랜더링을 페인팅이라고 부르기도 합니다. 리액트는 렌더링을 완료하면 DOM에 업데이트 하고 브라우저에 화면을 그립니다.

[브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361)

## Counter App

```js
import React, { useState } from 'react';

function App() {
  const [count, setCount] = useState(0);
  const handleCountUp = () => {
    setCount((count) => ++count);
  };
  const handleCountDown = () => {
    setCount((count) => --count);
  };
  return (
    <div>
      <h1>{count}</h1>
      <button onClick={handleCountUp}>+ 1</button>
      <button onClick={handleCountDown}>- 1</button>
    </div>
  );
}

export default App;
```

이렇게 작성해도 괜찮지만 1번 쓸거면 익명함수가 좋은 해결책이 됩니다.

## 컴포넌트 꾸미기

```js
import './App.css';

const Card = ({ props }) => {
  const style = {
    width: `120px`,
    height: `120px`,
    border: `solid 1px springgreen`,
    borderRadius: `8px`,
    display: `flex`,
    justifyContent: `center`,
    alignItems: `center`,
  };
  const card = props.map((food, idx) => {
    return (
      <div style={style} key={idx}>
        {food}
      </div>
    );
  });
  return <div style={{ display: `flex`, gap: `24px` }}>{card}</div>;
};

function App() {
  const food = ['감자', '고구마', '오이', '가지', '옥수수'];
  return (
    <div
      className="App"
      style={{
        display: `flex`,
        justifyContent: `center`,
        alignItems: `center`,
        height: `100vh`,
      }}
    >
      <Card props={food}></Card>
    </div>
  );
}

export default App;
```

## 반복되는 컴포넌트 처리하기

`map` 고차함수로 동적으로 UI를 만들 수 있습니다.

```js
import './App.css';
import React, { useState } from 'react';

const CustomButton = ({ children, onClick }) => {
  return <button onClick={onClick}>{children}</button>;
};

const User = ({ props, deleteUserHandler }) => {
  const { id, age, name } = props;
  return (
    <div>
      {age}살 - {name}
      <CustomButton onClick={() => deleteUserHandler(id)}>삭제</CustomButton>
    </div>
  );
};

function App() {
  const [users, setUsers] = useState([
    { id: 1, age: 30, name: '송중기' },
    { id: 2, age: 24, name: '송강' },
    { id: 3, age: 21, name: '김유정' },
    { id: 4, age: 29, name: '구교환' },
  ]);
  const [name, setName] = useState('');
  const [age, setAge] = useState('');
  const addUserHandler = () => {
    const newUser = { id: users.length + 1, age, name };
    setUsers((users) => [...users, newUser]);
  };
  const deleteUserHandler = (id) => {
    // const newUserList = users.filter((user) => user.id !== id);
    setUsers((users) => users.filter((user) => user.id !== id));
  };
  return (
    <div className="App">
      {users.map((user, idx) => (
        <User deleteUserHandler={deleteUserHandler} props={user} key={idx} />
      ))}
      <input
        type="text"
        placeholder="이름을 입력해주세요"
        onChange={(event) => {
          setName(event.target.value);
        }}
        value={name}
      />
      <input
        type="text"
        placeholder="나이를 입력해주세요"
        onChange={(event) => {
          setAge(event.target.value);
        }}
        value={age}
      />
      <CustomButton onClick={addUserHandler}>추가</CustomButton>
    </div>
  );
}

export default App;
```

컴포넌트를 분리하는 것으로 스타일링도 디커플링할 수 있습니다. props를 넣고 선언할 때 컬러 같은 스타일 설정하면 됩니다.

key값이 있어야 리액트는 컴포넌트의 차이를 참조할 수 있습니다. key는 props처럼 보이지만 고차함수의 2번째 인자인 idx를 활용하는 것은 자제해야 합니다. 공식 문서에서 제외하도로록 합니다.

하지만 key는 형제관계에서만 고유하면 괜찮습니다. 전체에 모두 고유할 필요은 없습니다.

[리스트와 Key](https://ko.reactjs.org/docs/lists-and-keys.html)

TL;DR

```js
import { nanoid } from 'nanoid';
const createNewTodo = (text) => ({
  completed: false,
  id: nanoid(),
  text
}
```

나노 아이디를 활용합니다.

```js
const User = ({ props, deleteUserHandler }) => {
  const { id, age, name } = props;
  return (
    <div>
      {age < 25 ? (
        <div>
          {age}살 - {name}{' '}
          <CustomButton onClick={() => deleteUserHandler(id)}>
            삭제
          </CustomButton>{' '}
        </div>
      ) : null}
    </div>
  );
};
```

25세 미만만 출력하게 만든 조건부 렌더링입니다. `filter`를 사용해도 큰 문제는 아니지만 배열 메서드를 체이닝하면 시간복잡도를 증가시킵니다. 고차함수 배열 메소드가 하나의 반복문이고 이것을 체이닝(반복문 중첩)하기 때문입니다.

## 컴포넌트 분리하기

재사용성이 높은 컴포넌트는 파일 분리를 권장합니다. 컴포넌트는 파일단위로 연관성 기준으로 분류하기를 권장합니다.

특정한 관심사에 따라 기능을 나누고, 각 기능을 독립적으로 개발한 뒤 이를 조합하는 방식으로 복잡한 소프트웨어를 구성해보자는 아이디어를 관심사의 분리(Separation of concerns, SoC)

```js
// App.js
import './App.css';
import React, { useState } from 'react';
import CustomButton from './components/CustomButton';
import User from './components/User';

function App() {
  const [users, setUsers] = useState([
    { id: 1, age: 30, name: '송중기' },
    { id: 2, age: 24, name: '송강' },
    { id: 3, age: 21, name: '김유정' },
    { id: 4, age: 29, name: '구교환' },
  ]);
  const [name, setName] = useState('');
  const [age, setAge] = useState('');
  const addUserHandler = () => {
    const newUser = { id: users.length + 1, age, name };
    setUsers((users) => [...users, newUser]);
  };
  const deleteUserHandler = (id) => {
    // const newUserList = users.filter((user) => user.id !== id);
    setUsers((users) => users.filter((user) => user.id !== id));
  };
  return (
    <div className="App">
      {users.map((user, idx) => (
        <User deleteUserHandler={deleteUserHandler} props={user} key={idx} />
      ))}
      <input
        type="text"
        placeholder="이름을 입력해주세요"
        onChange={(event) => {
          setName(event.target.value);
        }}
        value={name}
      />
      <input
        type="text"
        placeholder="나이를 입력해주세요"
        onChange={(event) => {
          setAge(event.target.value);
        }}
        value={age}
      />
      <CustomButton onClick={addUserHandler}>추가</CustomButton>
    </div>
  );
}

export default App;
```

```js
// User.js
import CustomButton from './CustomButton';

const User = ({ props, deleteUserHandler }) => {
  const { id, age, name } = props;
  return (
    <div>
      {age < 25 ? (
        <div>
          {age}살 - {name}{' '}
          <CustomButton onClick={() => deleteUserHandler(id)}>
            삭제
          </CustomButton>{' '}
        </div>
      ) : null}
    </div>
  );
};

export default User;
```

```js
// CustomButton.js
const CustomButton = ({ children, onClick }) => {
  return <button onClick={onClick}>{children}</button>;
};

export default CustomButton;
```

## 리액트 배포하기 (vercel)

배포하는 방법은 2가지입니다.

배포를 도와주는 서비스
웹 서버를 직접 구축하고 서버를 통해 배포하는 방법

Vercel를 통해 배포할 수 있습니다. 많은 회사는 Vercel은 거의 사용할 가능성이 없습니다.

Vercel을 활용하면 `git push`로 배포한 사이트를 업데이트할 수 있습니다.

Styled-Component, ReactHook, Redux를 예습하도록 합니다.

[How do I add environment variables to my Vercel project?](https://vercel.com/guides/how-to-add-vercel-environment-variables)

위 아티클은 환경 변수를 설정하는 법을 알려줍니다.

---

# 숙련 주차

[react-homework](react-homework-bk6tik4no-arch-spatula.vercel.app)

배포까지 끝났습니다. 숙련주차 강의 수강 시작했습니다.

리덕스는 다른 자료를 활용해서 추가 공부를 할 것입니다. 강의자료만으로는 당연히 부족합니다. 간단한 체험에 가깝습니다.

https://react-homework-bk6tik4no-arch-spatula.vercel.app/
