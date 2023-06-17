---
sidebar_position: 3
---

# 컴포넌트

리액트에서 컴포넌트는 레고랑 비슷합니다. 앱은 레고를 조립하는 방식으로 구현합니다.

누군가 컴포넌트를 만들라고 하먄 html을 `return`하는 부분만 조작하면 됩니다.

컴포넌트는 자바스크립트를 사용해야 할 때가 있습니다.

컴포넌트는 재사용할 수 있습니다. 재사용하려면 `export`, `import`가 필요합니다. 이 부분은 자바스크립트 문법에 해당합니다. `return`은 html처럼 생긴 JSX를 작성하는 영역입니다. 그 위는 자바스크립트로 로직을 처리합니다.

컴포넌트를 만들 때는 항상 파스칼케이스라는 것으 잊지말도록 합니다. 폴더는 카멜케이스로 작성합니다. 이름으로 컴포넌트, 폴더 등을 읽으면서 바로 추론할 수 있어야 합니다.

```jsx
import './App.css';

function App() {
  const handleClick = () => {
    alert('클릭!');
  };
  return (
    <div
      className="App"
      style={{
        height: '100vh',
        display: ' flex',
        flexDirection: 'column',
        justifyContent: 'center',
        alignItems: 'center',
      }}
    >
      <p>이것은 내가 만든 App 컴포넌트입니다.</p>
      <button onClick={handleClick}>클릭!</button>
    </div>
  );
}

export default App;
```

이렇게 html처럼 생긴 자바스크립트로 화면을 구현할 수 있습니다.

컴포넌트를 부모 자식관계로 만들 수 있습니다. html은 부모자식 관계를 들었을 것입니다. 포함관계로 속해있으면 자식에 해당합니다.

화면에 보여지게 하는 것을 보고 랜더링이라고 부릅니다. return에 작성하는 코드들을 보고 JSX라고 합니다.

```jsx
import './App.css';

const Child = () => {
  return (
    <div>
      <span>Child</span>
    </div>
  );
};

const Parent = () => {
  return (
    <div>
      <h2>Parent</h2>
      <Child></Child>
    </div>
  );
};

function App() {
  return (
    <div>
      <h1>Grand Parent</h1>
      <Parent></Parent>
    </div>
  );
}

export default App;
```

부모자식, 자매, 손주, 증조 관계를 만들 수 있습니다.
