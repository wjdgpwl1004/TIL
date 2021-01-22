# 코드스플리팅과 트리쉐이킹

## 코드 스플리팅
- 코드를 분리시켜서 필요한 코드만 불러와서 사용, 페이지의 로딩 속도 개선 가능
- 자바스크립트 청크로 애플리케이션을 분할하고, 청크를 필요로 하는 애플리케이션의 경로에만 이 청크들을 배분하여 성능을 개선하는 기술

### 자바스크립트 함수 비동기 로딩
- 자바스크립트 함수는 import() 함수를 통해서 분리 가능
- import() 함수 형태로 메서드 안에서 사용하게 되면 필요할 때 해당 스크립트를 불러와서 사용할 수 있게 됨
- import를 함수로 사용하면 Promise를 반환. 이 문법은 dynamic import 문법으로 현재는 웹팩에서도 지원

```js
// src/notify.js

export default function notify() {
  console.log('notify');
}

// src/App.js

import React from 'react';

function App() {
  const onClick = () => {
    import('./notify').then(res => res.default());
  };
  
  return (
    <div>
      <p onClick={onClick}>Hello World</p>
    </div>
  );
}

export default App;
```

### React.lazy와 Suspense를 통한 컴포넌트 코드 스플리팅
- React.lazy와 Suspense를 통해서는 state를 따로 선언하지 않고 코드 스플리팅을 할 수 있다.
-  React.lazy는 컴포넌트를 렌더링하는 시점에서 비동기적으로 로딩할 수 있게 해 주는 유틸 함수
- Suspense는 리액트 내장 컴포넌트로 코드 스플리팅 된 컴포넌트를 로딩하도록 발동시킬 수 있고, 로딩이 끝나지 않았을 때 보여줄 UI를 설정해 줄 수 있다. fallback 이라는 props를 통해 로딩 중에 보여줄 JSX 문법을 지정할 수 있다.
- 라우트 기반 분할로 코드 분할을 결정하는 것이 가장 많이 쓰이는 방법


```js
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```

### Webpack: Entry Point
- Entry Point는 웹팩이 앱에서 번들링하려는 모듈의 진입 파일
-  entry 프로퍼티를 작성하면 웹팩에서 자동으로 index와 another를 다른 chunk로 관리를 해서 로딩

```js
// webpack.config.js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js',
    another: './src/another-module.js',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

## 트리 쉐이킹
- 사용하지 않는 코드를 제거하는 것
- Webpack4부터 제공
- 쓰지않는 코드들을 DOM트리로부터 제거하는 기능

```js
import { unique, implode, explode } from "array-utils";
```
- 위와 같이 유틸의 일부만 가져오는 방법이 있다.

### 트리쉐이킹 제약조건
- 소스코드가 ES modules로 빌드 되어있어야 한다. (JS에는 주로 commonJS, ES modules 모듈 시스템 방식이 주를 이룬다.)
- 코드간의 의존성 sideEffects (package.json의 sideEffects속성이 false이어야 함.)
    - 만약 import 한 코드를 제외하고 전부 제거해도 괜찮으려면 각 코드간에 의존성 관리에 문제가 없어야 한다.



# 출처
- https://ui.toast.com/weekly-pick/ko_20180716
- https://devowen.com/342
- https://brunch.co.kr/@swimjiy/24
- https://blog-posting.github.io/2020-05-23/webpack-tree-shaking
