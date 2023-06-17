---
sidebar_position: 1
---

## Redux part 1

리덕스의 장점 중 하나는 전역으로 State를 관리할 수 있습니다. 훨씬더 복잡한 앱을 만들 수 있습니다.

리덕스는 state와 props에 대해서 학습의존성을 갖고 있습니다. 리덕스는 상태관리 라이브러리입니다.

기존 리액트는 prop drilling을 했어야 합니다. 실제 프로그램을 만들 때는 부모에서 자식에게 전달할 수 있는 경우는 많지 않습니다. 전역 state로 만들어서 관리하는 라이브러리가 리덕스입니다. 리덕스를 사용하면 중간에 불필요하게 건너가야하는 컴포넌트를 피할 수 있습니다.

리덕스를 사용하는 다른 장점은 전달하기 위해 불필요하게 전달했어야 합니다. 리덕스는 글로벌 state랑 로컬 state로 구분해서 사용할 수 있습니다.

store는 글로벌 state를 보관합니다.

## Redux part 2

설치하는 방법입니다. 리액트를 설치하고 난뒤에 다음 명령을 하도록 합니다.

```sh
yarn add redux react-redux
```

```sh
# yarn add redux react-redux 과 같은 의미
yarn add redux
yarn add react-redux
```

`react-redux`는 리액트에서 사용가능하게 서로 연결해주는 패키지입니다.

설치하고 후 `config`, `modules` 폴더를 추가합니다.

```txt
├── src/
│   └── redux/
│       ├── config/
│       │   └── configStore.js
│       └── modules/
│           └── ???.js
├── App.js
└── index.js
```

디렉토리 구조는 이렇게 생겼습니다.

configStore.js는 설정 파일입니다. 전역 상태로 받을 수 있게 해주는 설정 파일입니다.

```js
import { createStore } from 'redux';
import { combineReducers } from 'redux';

/*
1. createStore()
리덕스의 가장 핵심이 되는 스토어를 만드는 메소드(함수) 입니다. 
리덕스는 단일 스토어로 모든 상태 트리를 관리한다고 설명해 드렸죠? 
리덕스를 사용할 시 creatorStore를 호출할 일은 한 번밖에 없을 거예요.
*/

/*
2. combineReducers()
리덕스는 action —> dispatch —> reducer 순으로 동작한다고 말씀드렸죠? 
이때 애플리케이션이 복잡해지게 되면 reducer 부분을 여러 개로 나눠야 하는 경우가 발생합니다. 
combineReducers은 여러 개의 독립적인 reducer의 반환 값을 하나의 상태 객체로 만들어줍니다.
*/

const rootReducer = combineReducers({});
const store = createStore(rootReducer);

export default store;
```

modules은 만든 state의 묶음입니다. 모듈을 저장하는 파일입니다.

index.js에서 작성할 코드입니다.

```js
// 원래부터 있던 코드
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import reportWebVitals from './reportWebVitals';

// 우리가 추가할 코드
import store from './redux/config/configStore';
import { Provider } from 'react-redux';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  //App을 Provider로 감싸주고, configStore에서 export default 한 store를 넣어줍니다.
  <Provider store={store}>
    <App />
  </Provider>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

공부하는 방법에 따라다르지만 지금은 사용법을 먼저 공부하고 다음에 원리를 이해한다고 가정하면 내부의 동작원리는 설명은 잠시 보류하겠습니다.

## Redux part 3

이번에 만들어볼 예제는 카운터 app입니다.

```js
// src/modules/counter.js

// 초기 상태값
const initialState = {
  number: 0,
};

// 리듀서
const counter = (state = initialState, action) => {
  switch (action.type) {
    default:
      return state;
  }
};

// 모듈파일에서는 리듀서를 export default 한다.
export default counter;
```

```txt
├── src/
│   └── redux/
│       ├── config/
│       │   └── configStore.js
│       └── modules/
│           └── counter.js
│   ├── App.js
│   └── index.js
```

디렉토리 구조는 이렇게 됩니다.

```js
// 초기 상태값
const initialState = {
  number: 0,
};
```

코드 중이 부분은 `useState(0)`의 인자 `0`을 넣은 것과 유사합니다.

참고로 초깃값은 반드시 객체일 필요가 없습니다. 참조형, 원시형 무관합니다.

```js
// 리듀서
const counter = (state = initialState, action) => {
  switch (action.type) {
    default:
      return state;
  }
};
```

리듀서입니다. 리듀서란 변화를 일으키는 함수입니다.

```js
// 예시 코드

const onClickHandler = () => {
  setNumber(number + 1); // setState를 이용해서 state 변경
};
```

useState만 활용하면 위처럼 코드를 작성해서 state를 업데이트했습니다.

state는 초깃값 할당이 필요합니다. 그리고 action 매개 변수가 필요합니다.

Redux 속에 Store가 있고 Store 속에는 Reducer가 있습니다. Store 상태를 저장하는 곳입니다.

어떤 액션을 실행하는 것을 보고 디스패치(dispatch)라고 합니다. reducer가 자동실행되고 액션에 해당하는 방식으로 데이터를 수정합니다.

모듈과 store를 연결하는 방법입니다.

```js
import { createStore } from 'redux';
import { combineReducers } from 'redux';
import counter from '../modules/counter';

const rootReducer = combineReducers({
  counter: counter,
});
const store = createStore(rootReducer);

export default store;
```

연결여부를 판단할 때는 컴포넌트에서 store를 조회하면 됩니다. redux의 `useSelector` hook을 사용하면 조회할 수 있습니다.

```js
import './App.css';
import React from 'react';
import { useSelector } from 'react-redux';

function App() {
  const countStore = useSelector((state) => state);
  console.log(countStore);
  return <div className="App"></div>;
}

export default App;
```

```js
{
  counter: {
    number: 0,
  }
}
```

console.log를 확인하면 이렇게 피드백 주는 것을 확인할 수 있습니다.

데이터가 흐르는 방향입니다.

modules은 기능의 이름을 참고해서 파일을 생성합니다. modules의 데이터를 configStore.js에 전달합니다. configStore.js에서 호출할 때 redux의 `useSelector` hook으로 접근합니다. state는 모든 모듈의 데이터를 접근할 수 있습니다.

모듈의 구성요소는 initialState, reducer 2가지가 있습니다. 생성하면 store에 연결해야 합니다. `useSelector`로 사용할 컴포넌트에 전달합니다.

## Redux part 4

[리덕스 흐름 도식화](https://user-images.githubusercontent.com/84452145/205887636-7bf7044a-72e3-4cae-ada6-81e2b05a06f5.gif)

1. 사용자는 ui와 어떤 상호작용을 합니다.
2. dispatch에서 action이 일어나게 됩니다.
3. action에 의한 reducer 함수가 실행되기 전에 middleware가 동작합니다.
4. middleware에서 요청한 수행 이후 reducer함수를 실행합니다.
5. reducer의 실행결과 store에 새로운 값을 저장합니다.
6. store의 state에 subscribe하고 있던 ui에 변경된 값을 줍니다.

이 순서에서 3, 4는 나중에 배웁니다.

리덕스에는 dispatch, reducer 같은 다양한 중간단계가 있습니다. 중요한 개념들입니다.

다시 말하지만 setter 함수처럼 값을 업데이트하는 부분은 reducer에서 진행합니다.

글로벌 state는 어디서나 접근할 수 있기 때문에 접근하기 쉬운 만큼 변경도 쉽습니다.

글로벌 state를 변경하는 함수는 reducer함수이고 그리고 이 함수를 실행하는 것을 보고 action입니다. action 객체를 통해 접근하고 실행합니다.

action객체를 사용할 때는 지켜야할 규칙이 있습니다. `{type: 명령}`처럼 생긴 것이 액션 객체입니다. 이 액션객체는 반드시 `type`이라는 명령할 때는 `key`를 가져야 합니다.

```js
const counter = (state = initialState, action) => {
  switch (action.type) {
    default:
      return state;
  }
};
```

그 이유는 이렇게 생긴 reducer를 보면 바로 알 수 있습니다.

`useDispatch` redux hook으로 제어할 수 있습니다.

```js
// App.js
import './App.css';
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';

function App() {
  const dispatch = useDispatch();
  return (
    <div className="App">
      <button
        onClick={() => {
          dispatch({ type: 'plusOne' });
        }}
      >
        +1
      </button>
    </div>
  );
}

export default App;
```

```js
// src/modules/counter.js

// 초기 상태값
const initialState = {
  number: 0,
};

// 리듀서
const counter = (state = initialState, action) => {
  console.log(action, state);
  switch (action.type) {
    case 'plusOne':
      return { number: state.number + 1 };
    default:
      return state;
  }
};

// 모듈파일에서는 리듀서를 export default 한다.
export default counter;
```

이렇게 state를 업데이트할 수 있습니다.

```js
import './App.css';
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';

function App() {
  const dispatch = useDispatch();
  const countStore = useSelector((state) => state.counter.number);
  return (
    <div className="App">
      <h2>{countStore}</h2>
      <button
        onClick={() => {
          dispatch({ type: 'plusOne' });
        }}
      >
        +1
      </button>
    </div>
  );
}

export default App;
```

UI에는 반영을 이렇게 보여줄 수 있습니다.

useState처럼 useSelector가 참조하고 있는 컴포넌트도 모두 리랜더링됩니다.

```js
// src/modules/counter.js

// 초기 상태값
const initialState = {
  number: 0,
};

// 리듀서
const counter = (state = initialState, action) => {
  console.log(action, state);
  switch (action.type) {
    case 'plusOne':
      return { number: state.number + 1 };
    case 'minusOne':
      return { number: state.number - 1 };
    default:
      return state;
  }
};

// 모듈파일에서는 리듀서를 export default 한다.
export default counter;
```

```js
import './App.css';
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';

function App() {
  const dispatch = useDispatch();
  const countStore = useSelector((state) => state.counter.number);
  return (
    <div className="App">
      <h2>{countStore}</h2>
      <button
        onClick={() => {
          dispatch({ type: 'plusOne' });
        }}
      >
        +1
      </button>
      <button
        onClick={() => {
          dispatch({ type: 'minusOne' });
        }}
      >
        -1
      </button>
    </div>
  );
}

export default App;
```

- 액션객체란, 반드시 `type`이란 `key`를 가져야 하는 객체입니다. 또한 reducer로 보낼 `명령`입니다.
- 디스패치란, 액션객체를 리듀서로 보내는 `전달자` 함수입니다.
- 리듀서란, 디스패치를 통해 전달받은 액션객체를 검사하고, 조건이 일치했을 때 `state`를 업데이트하는 함수입니다.
- 디스패치(dispatch)를 사용하기위해서는 `useDispatch()` 라는 훅을 이용해야 합니다.
  - 디스패치는 스토어의 내장함수 중 하나입니다.
  - 우선, 디스패치는 액션을 발생 시키는 것 정도로 이해하시면 됩니다.
  - `dispatch` 라는 함수에는 액션을 파라미터로 전달합니다(예: `dispatch(action)`).
- 액션객체 `type`의 `value`는 상수의 식별자로 작성합니다(예: `{type: "PLUS_ONE"}`). 따라서 위 코드의 카멜케이스에서 어퍼스네이크 케이스로 작성해야 올바릅니다.

## Redux part 5

Action Creator입니다. action 객체를 지금까지 하드코딩을 많이 했습니다. 액션 객체를 여러곳에 만들었지만 만약에 수정해야 한다면 큰일날 수 있습니다. 현실에서는 더욱더 복잡한 프로젝트에서 다룰 것이기 때문에 알아야합니다.

액션객체를 한곳에서 관리할 것입니다. 함수와 action value 상수를 이용해서 만듭니다.

action 객체를 만드는 것이 함수의 기능입니다.

```js
// src/modules/counter.js

// 추가된 코드 👇 - 액션 value를 상수들로 만들어 줍니다. 보통 이렇게 한곳에 모여있습니다.
const PLUS_ONE = 'PLUS_ONE';
const MINUS_ONE = 'MINUS_ONE';

// 추가된 코드 👇 - Action Creator를 만들어 줍니다.
export const plusOne = () => {
  return {
    type: PLUS_ONE,
  };
};

export const minusOne = () => {
  return {
    type: MINUS_ONE,
  };
};

// 초기 상태값
const initialState = {
  number: 0,
};

// 리듀서
const counter = (state = initialState, action) => {
  switch (action.type) {
    case PLUS_ONE: // case에서도 문자열이 아닌, 위에서 선언한 상수를 넣어줍니다.
      return {
        number: state.number + 1,
      };
    case MINUS_ONE: // case에서도 문자열이 아닌, 위에서 선언한 상수를 넣어줍니다.
      return {
        number: state.number - 1,
      };
    default:
      return state;
  }
};

export default counter;
```

컴포넌트에서 사용하는 방법은 간단합니다.

먼저 액션함수는 export되어 있어야 합니다. 반대로 사용할 때는 import하면 됩니다.

```js
// src/App.js

import React from 'react';
import { useDispatch, useSelector } from 'react-redux';

// 사용할 Action creator를 import 합니다.
import { minusOne, plusOne } from './redux/modules/counter';

const App = () => {
  const dispatch = useDispatch();
  const number = useSelector((state) => state.counter.number);

  return (
    <div>
      {number}
      <button
        onClick={() => {
          dispatch(plusOne()); // 액션객체를 Action creator로 변경합니다.
        }}
      >
        + 1
      </button>
      {/* 빼기 버튼 추가 */}
      <button
        onClick={() => {
          dispatch(minusOne()); // 액션객체를 Action creator로 변경합니다.
        }}
      >
        - 1
      </button>
    </div>
  );
};

export default App;
```

위 코드를 보면 이전 `dispatch`의 인자로 함수를 넣었습니다. 당연히 함수의 반환값인 객체를 대입하게 된 것입니다.

Action creator를 사용하면 상당히 큰 장점이 있습니다.

1. 오타확인하기 좋습니다. 자동완성으로 사용했던 액션의 이름을 볼 수 있습니다.
2. 유지보수하기도 좋습니다. 하나의 추상화로 전역으로 수정하기 용이합니다.
3. 문서의 역할도 합니다. 어떤 액션을 수행하게 될지 알 수 있습니다.

## Redux part 6

Payload입니다. 액션객체에 담아 보내는 데이터를 보고 Payload라고 합니다. 주로 사용자가 조금더 복잡한 액션을 취할 때 사용합니다. 예를 들어 이전 카운터는 1단위로 더하고 빼고를 제어했지만 이제는 사용자가 단위를 정할 수 있게 해줍니다.

```js
// payload가 추가된 액션객체

{type: "ADD_NUMBER", payload: 10} // type뿐만 아니라 payload라는 key와 value를 같이 담는다.
```

리덕스는 유연한 라이브러리라 `payload`이외 용어로도 보낼 수 있습니다. 그래서 `type` 이외는 원하는 키를 사용해도 됩니다. `payload`를 사용하는 이유는 컨벤션 때문입니다.

나중에 혼자 한번에 진행해보도록 합니다.

```js
import { createStore } from 'redux';
import { combineReducers } from 'redux';
import counter from '../modules/counter';

const rootReducer = combineReducers({
  counter: counter,
});
const store = createStore(rootReducer);

export default store;
```

```js
// src/redux/modules/counter.js

// Action Value
const ADD_NUMBER = 'ADD_NUMBER';
const SUBTRACT_NUMBER = 'SUBTRACT_NUMBER';
// Action Creator
export const addNumber = (payload) => {
  return { type: ADD_NUMBER, payload };
};

export const subtractNumber = (payload) => {
  return { type: SUBTRACT_NUMBER, payload };
};

// Initial State
const initialState = {
  number: 0,
};

// Reducer
const counter = (state = initialState, action) => {
  switch (action.type) {
    case ADD_NUMBER:
      return { number: state.number + action.payload };
    case SUBTRACT_NUMBER:
      return { number: state.number - action.payload };
    default:
      return state;
  }
};

// export default reducer
export default counter;
```

```js
import React, { useState } from 'react';
import { addNumber, subtractNumber } from './redux/modules/counter';
import { useSelector, useDispatch } from 'react-redux';

function App() {
  const [num, setNum] = useState(0);
  const globalNumber = useSelector((state) => state.counter.number);
  const dispatch = useDispatch();
  const handleChangeText = (event) => {
    const { value } = event.target;
    setNum(+value);
  };
  const onClickAddNumberHandler = () => {
    dispatch(addNumber(num));
  };
  const onClickSubtractNumberHandler = () => {
    dispatch(subtractNumber(num));
  };
  return (
    <div className="App">
      <h2>{globalNumber}</h2>
      <button onClick={onClickSubtractNumberHandler}>-</button>
      <input
        type="number"
        onChange={(event) => handleChangeText(event)}
        value={num}
      />
      <button onClick={onClickAddNumberHandler}>+</button>
    </div>
  );
}

export default App;
```

리덕스를 사용하기 위해서는 모든 구성요소를 만들어야 합니다. 구성요소를 만드는 패턴이 존재합니다. 이 강의는 Ducks 패턴으로 작성한 것입니다.

1. Reducer 함수를 `export default` 합니다.

2. Action creator 함수들을 `export` 합니다.

3. Action type은 `app/reducer/ACTION_TYPE` 형태로 작성합니다.

리덕스 작성 패턴의 고전이라고 많이 알고 있습니다.

[덕스 패턴 소개 리포](https://github.com/erikras/ducks-modular-redux)

이외 flux 패턴이라는 것도 존재합니다.

## 리액트 라우터

react-router-dom입니다. 버전별로 문법차이가 큽니다.

여러 페이지를 만들 수 있습니다.

```sh
yarn add react-router-dom
```

위처럼 리액트 설치이후 추가하면 됩니다.

```txt
├── src/
│   ├── pages/
│       ├── Home.jsx
│       ├── About.jsx
│       ├── Contact.jsx
│       └── Works.jsx
│   ├── shared/
│       └── Router.js
│   ├── App.js
│   └── index.js
```

url을 입력하면 페이지 컴포넌트를 접근할 수 있게 해줍니다.

라우터 파일은 주로 따로 분리해서 작성합니다.

```js
// shared/Router.js
import React from 'react';
// 1. react-router-dom을 사용하기 위해서 아래 API들을 import 합니다.
import { BrowserRouter, Route, Routes } from 'react-router-dom';

// 2. Router 라는 함수를 만들고 아래와 같이 작성합니다.
//BrowserRouter를 Router로 감싸는 이유는,
//SPA의 장점인 브라우저가 깜빡이지 않고 다른 페이지로 이동할 수 있게 만들어줍니다!
const Router = () => {
  return (
    <BrowserRouter>
      <Routes></Routes>
    </BrowserRouter>
  );
};

export default Router;
```

위 코드로 작성하는 것이 출발입니다.

```js
import React from 'react';
// 1. react-router-dom을 사용하기 위해서 아래 API들을 import 합니다.
import { BrowserRouter, Route, Routes } from 'react-router-dom';
import Home from '../pages/Home';
import Contact from '../pages/Contact';
import About from '../pages/About';
import Works from '../pages/Works';

// 2. Router 라는 함수를 만들고 아래와 같이 작성합니다.
//BrowserRouter를 Router로 감싸는 이유는,
//SPA의 장점인 브라우저가 깜빡이지 않고 다른 페이지로 이동할 수 있게 만들어줍니다!
const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="about" element={<About />} />
        <Route path="contact" element={<Contact />} />
        <Route path="works" element={<Works />} />
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

라우팅할 때마다 접근할 페이지를 이렇게 맵핑할 수 있습니다.

```js
import './App.css';
import Router from './shared/Router';

function App() {
  return <Router />;
}

export default App;
```

모든 라우트는 App을 통해 거처가야 합니다. 개념적으로 라우트는 앱에 포함된 개념입니다.

react-router-dom가 제공하는 hook들이 존재합니다.

`useNavigate`는 어떤 페이지로 이동시킬 때 사용합니다.

[https://reactrouter.com/en/6.4.4/hooks/use-navigate](useNavigate - 공식 문서링크)

```js
// src/pages/home.js
import React from 'react';
import { useNavigate } from 'react-router-dom';

const Home = () => {
  const navigate = useNavigate();

  return (
    <button
      onClick={() => {
        navigate('/works');
      }}
    >
      works로 이동
    </button>
  );
};

export default Home;
```

`useLocation`현재 url 정보를 얻을 수 있습니다. 조건부 랜더링에 활용할 수 있습니다.

[https://reactrouter.com/en/6.4.4/hooks/use-location](useLocation - 공식 문서링크)

```js
// src/pages/works.js
import React from 'react';
import { useLocation } from 'react-router-dom';

const Works = () => {
  const location = useLocation();
  console.log('location :>> ', location);
  return (
    <div>
      <div>{`현재 페이지 : ${location.pathname.slice(1)}`}</div>
    </div>
  );
};

export default Works;
```

`<Link></Link>`는 JSX에서 a태그랑 동일한 기능을 지원해줍니다.

[<Link> - 공식 문서링크](https://reactrouter.com/en/6.4.4/components/link)

```js
import { Link, useLocation } from 'react-router-dom';

const Works = () => {
  const location = useLocation();
  console.log('location :>> ', location);
  return (
    <div>
      <div>{`현재 페이지 : ${location.pathname.slice(1)}`}</div>
      <Link to="/contact">contact 페이지로 이동하기</Link>
    </div>
  );
};

export default Works;
```

`useParams`은 URL의 query를 접근할 수 있습니다.

[useParams - 공식 문서링크](https://reactrouter.com/en/6.4.4/hooks/use-params)

```js
import * as React from 'react';
import { Routes, Route, useParams } from 'react-router-dom';

function ProfilePage() {
  // Get the userId param from the URL.
  let { userId } = useParams();
  // ...
}

function App() {
  return (
    <Routes>
      <Route path="users">
        <Route path=":userId" element={<ProfilePage />} />
        <Route path="me" element={...} />
      </Route>
    </Routes>
  );
}
```

어떤 컴포넌트들은 어떤 자식 엘리먼트가 들어올지 미리 예상할 수 없는 경우가 있습니다. 범용적으로 사용하는 컴포넌트들이 있습니다. Sidebar, Header, Footer, Dialog같은 컴포넌트들이 존재합니다. 이런 컴포넌트들을 라우팅과 무관하게 접근할 수 있게 해줍니다.

컴포지션은 합성이라는 의미를 가졌습니다.

```js
// src/shared/Layout.js

import React from 'react';

const HeaderStyles = {
  width: '100%',
  background: 'black',
  height: '50px',
  display: 'flex',
  alignItems: 'center',
  paddingLeft: '20px',
  color: 'white',
  fontWeight: '600',
};
const FooterStyles = {
  width: '100%',
  height: '50px',
  display: 'flex',
  background: 'black',
  color: 'white',
  alignItems: 'center',
  justifyContent: 'center',
  fontSize: '12px',
};

const layoutStyles = {
  display: 'flex',
  flexDirection: 'column',
  justifyContent: 'center',
  alignItems: 'center',
  minHeight: '90vh',
};

function Header() {
  return (
    <div style={{ ...HeaderStyles }}>
      <span>Sparta Coding Club - Let's learn React</span>
    </div>
  );
}

function Footer() {
  return (
    <div style={{ ...FooterStyles }}>
      <span>copyright @SCC</span>
    </div>
  );
}

function Layout({ children }) {
  return (
    <div>
      <Header />
      <div style={{ ...layoutStyles }}>{children}</div>
      <Footer />
    </div>
  );
}

export default Layout;
```

`children props`를 활용해서 적용하는 개념입니다. 어떤 자식 태그가 들어와도 범용적으로 적용할 수 있게 됩니다.

```js
import React from 'react';
// 1. react-router-dom을 사용하기 위해서 아래 API들을 import 합니다.
import { BrowserRouter, Route, Routes } from 'react-router-dom';
import Home from '../pages/Home';
import Contact from '../pages/Contact';
import About from '../pages/About';
import Works from '../pages/Works';
import Layout from './Layout';

// 2. Router 라는 함수를 만들고 아래와 같이 작성합니다.
//BrowserRouter를 Router로 감싸는 이유는,
//SPA의 장점인 브라우저가 깜빡이지 않고 다른 페이지로 이동할 수 있게 만들어줍니다!
const Router = () => {
  return (
    <BrowserRouter>
      <Layout>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="about" element={<About />} />
          <Route path="contact" element={<Contact />} />
          <Route path="works" element={<Works />} />
        </Routes>
      </Layout>
    </BrowserRouter>
  );
};

export default Router;
```

`Layout`컴포넌트가 감싸야 공통 컴포넌트를 적용할 수 있습니다.

이제 동적 라우팅입니다. url에 특정 데이터를 넣어 동적으로 이동하는 것입니다. 주로 커머스에서 레이아웃은 동일합니다. 같은 컴포넌트의 같은 레이아웃을 활용합니다. 하지만 Content만 다릅니다. url에 따라 동적으로 데이터를 연결하고 제공합니다.

```js
import React from 'react';
import { BrowserRouter, Route, Routes } from 'react-router-dom';
import Home from '../pages/Home';
import Contact from '../pages/Contact';
import About from '../pages/About';
import Works from '../pages/Works';
import Layout from './Layout';

const Router = () => {
  return (
    <BrowserRouter>
      <Layout>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="about" element={<About />} />
          <Route path="contact" element={<Contact />} />
          <Route path="works" element={<Works />} />
          <Route path="works/:id" element={<Works />} />
        </Routes>
      </Layout>
    </BrowserRouter>
  );
};

export default Router;
```

여기서 id가 동적인 부분에 해당합니다. `useParams` hook으로 접근할 수 있습니다.

```url
http://localhost:3000/works/100
```

이렇게 입력해도 정상작동하는 것을 확인할 수 있습니다.

동적 라우팅의 url path의 데이터를 가져오는 방법입니다. 동적 라우팅을 하면 페이지에 동일하게 랜더링합니다. 컴포넌트는 모두 같지 content만 달라야 합니다.

```js
// src/pages/Works.js

import React from 'react';
import { Link, useParams } from 'react-router-dom';

const data = [
  { id: 1, todo: '리액트 배우기' },
  { id: 2, todo: '노드 배우기' },
  { id: 3, todo: '자바스크립트 배우기' },
  { id: 4, todo: '파이어 베이스 배우기' },
  { id: 5, todo: '넥스트 배우기' },
  { id: 6, todo: 'HTTP 프로토콜 배우기' },
];

function Works() {
  return (
    <div>
      {data.map((work) => {
        return (
          <div key={work.id}>
            <div>할일: {work.id}</div>
            <Link to={`/works/${work.id}`}>
              <span style={{ cursor: 'pointer' }}>➡️ Go to: {work.todo}</span>
            </Link>
          </div>
        );
      })}
    </div>
  );
}

export default Works;
```

커머스가 홈에서 상세페이지 접근하기 전에 중간 상품 목록페이지랑 유사합니다. `<Link>`태그를 활용해서 상세페이지를 접근하게 해줍니다.

```js
// src/pages/Work.js

import React from 'react';
import { useParams } from 'react-router-dom';

const data = [
  { id: 1, todo: '리액트 배우기' },
  { id: 2, todo: '노드 배우기' },
  { id: 3, todo: '자바스크립트 배우기' },
  { id: 4, todo: '파이어 베이스 배우기' },
  { id: 5, todo: '넥스트 배우기' },
  { id: 6, todo: 'HTTP 프로토콜 배우기' },
];

function Work() {
  const param = useParams();

  const work = data.find((work) => work.id === parseInt(param.id));

  return <div>{work.todo}</div>;
}

export default Work;
```

접근하게 될 상세 페이지입니다.

고유한 id를 활용하는 것이 전략입니다. 각 url의 고유한 id를 활용하는 전략입니다.

리액트 라우터 DOM으로 동적 라우팅 설정이 가능합니다. id는 각 컴포넌트에서 조회할 수 있습니다.

```js
import React from 'react';
import { BrowserRouter, Route, Routes } from 'react-router-dom';
import Home from '../pages/Home';
import Contact from '../pages/Contact';
import About from '../pages/About';
import Works from '../pages/Works';
import Work from '../pages/Work';
import Layout from './Layout';

const Router = () => {
  return (
    <BrowserRouter>
      <Layout>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="about" element={<About />} />
          <Route path="contact" element={<Contact />} />
          <Route path="works" element={<Works />} />
          <Route path="works/:id" element={<Work />} />
        </Routes>
      </Layout>
    </BrowserRouter>
  );
};

export default Router;
```

이렇게 접근가능하도록 Route 설정을 하면 끝납니다.

# 예습 키워드

redux-toolkit, JSON server, axios, thunk, optimizing custom hook
