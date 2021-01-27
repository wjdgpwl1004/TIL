# 웹팩

## 웹팩
- 모듈 번들러
- 모듈 번들러 : 웹 애플리케이션을 구성하는 자원(HTML, CSS, Javscript, Images 등)을 모두 각각의 모듈로 보고 이를 조합해서 병합된 하나의 결과물을 만드는 도구

### 엔트리
- 웹팩에서 모든 것은 모듈
- 자바스크립트, 스타일시트, 이미지 등 모든 것을 자바스크립트 모듈로 로딩해서 사용하도록 한다.
- 자바스크립트가 로딩하는 모듈이 많아질수록 모듈간의 의존성은 증가한다. 의존성 그래프의 시작점을 웹팩에서는 엔트리(entry)라고 한다.

```js
// webpack.config.js
module.exports = {
  entry: {
    main: "./src/main.js",
  },
}
```

### 아웃풋
- 엔트리에 설정한 자바스크립트 파일을 시작으로 의존되어 있는 모든 모듈을 하나로 묶을 것이다. 번들된 결과물을 처리할 위치는 output에 기록

```js
// webpack.config.js
module.exports = {
  output: {
    filename: "bundle.js",
    path: "./dist",
  },
}
```

### 로더
- 비 자바스크립트 파일을 웹팩이 이해하게끔 변경해야하는데 로더가 그런 역할을 한다.

### 플러그인
- 로더가 파일단위로 처리하는 반면 플러그인은 번들된 결과물을 처리한다.
- 번들된 자바스크립트를 난독화 한다거나 특정 텍스트를 추출하는 용도로 사용할 수 있다.

### 정리
- 의존성 그래프에서 엔트리로 그래프의 시작점을 설정하면 웹팩은 모든 자원을 모듈로 로딩한 후 아웃풋으로 묶어준다. 로더로 각 모듈별로 바벨, 사스변환 등의 처리하고 이 결과를 플러그인이 받아 난독화, 텍스트 추출 등의 추가 작업을 한다. 그리고 코드스플리팅, 트리쉐이킹을 가능하게 하기도 한다. 틈틈히 써보면서 웹팩을 익혀보자!


# 출처
- https://joshua1988.github.io/webpack-guide/webpack/what-is-webpack.html
- https://jeonghwan-kim.github.io/js/2017/05/15/webpack.html
- https://www.zerocho.com/category/Webpack/post/58ad4c9d1136440018ba44e7
