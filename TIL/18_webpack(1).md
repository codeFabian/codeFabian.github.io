---
layout: post
title: Create React and Typescript Project using Webpack(1)
subtitle: "CRA manually"
type: "Webpack"
createDate: 2021-03-08
til: true
text: true
author: "codeFabian"
post-header: false
header-img: ""
order: 18
---

평소에 관심이 있던 웹팩을 공부하고 직접 사용해보았다. 이 글에서는 웹팩에 대한 개념보다는 실제로 프로젝트를 수동으로 만들었던 실습에 대한 글을 적으려고 한다.

<br>

## 빠르게 초기셋팅을 시작해봅시다

---

일단 프로젝트를 만들 것이기 때문에 터미널을 열고 폴더를 만들어 줍니다.

`mkdir webpack_tutorial`

> TMI: 항상 새로운 것을 배울 땐 tutorial 이라고 지정합니다..!!

<br>

## Start Create React App

---

자 만들어진 우리의 프로젝트 폴더에 필요한 폴더들과 파일들을 만들고 시작할 것 입니다.

- **build**: React 앱이 포함 될 HTML 페이지가 들어갈 것이며 이후 빌드된 파일들이 저장될 공간입니다.
- **src**: 익숙한 곳이죠. 소스코드들이 저장될 것 입니다.

이제 방금 만들었던 build 폴더에 HTML페이지를 추가할 것 입니다. 이름은 index.html 로 만들겠습니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
  </head>
  <body>
    <div id="root"></div>
    <script src="bundle.js"></script>
  </body>
</html>
```

이후에 자바스크립트 코드들은 bundle이라고 불리는 파일에 집합되어질 것 입니다.

이번엔 **Package.json** 파일을 루트 폴더에 만들어보겠습니다.

```json
{
  "name": "fabian-app",
  "version": "0.0.1"
}
```

<br>

## React & Typescript 설정

---

리액트와 타입스크립트를 사용할 프로젝트를 만들 것이기 때문에 두가지를 설치해야합니다.

타입스크립트는 런타임 시, 바벨을 통해 자바스크립트로 변환되기 때문에 개발단계에서만 사용할 것입니다.

```jsx
npm install react react-dom
npm install --save-dev typescript
```

타입스크립트를 설치했다면 필요한 것이 있죠? `tsconfig.json` 파일 입니다.
프로젝트를 컴파일하는데 필요한 루트파일과 컴파일러 옵션을 지정하는 파일입니다.

```jsx
{
  "compilerOptions": {
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "allowSyntheticDefaultImports": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react"
  },
  "include": [
    "src"
  ]
}
```

`lib` : 타입체크단계에서 최신버전의 ECMAScript와 브라우저 DOM 버전을 설정하였습니다.

`allowJs` : 자바스크립트 파일을 컴파일 할 수 있도록 설정합니다.

`allowSyntheticDefaultImports` : true로 사용하여 import를 할 수 있도록 설정합니다.
ex) `import React from "react"`

`skipLibCheck` : 선언파일 (\*.d.ts) 의 타입검사를 건너뜁니다.

`esModuleInterop` : Babel 과 호환성을 가능하게 합니다.

`strict` : 타입체킹 옵션을 활성화 합니다.

`forceConsistentCasingInFileNames` : 참조파일에 일관성 없는 대소문자가 있는지 확인합니다. (비활성화)

`moduleResolution` : 모듈 해석 방법을 결정하는 것이라고 합니다. `노드` 와 `클래식` 이 있는데, 런타임 시 Node.js 모듈해석 메커니즘을 사용합니다. (이후 리서치가 필요)

`resolveJsonModule` : .json 파일에 import된 모듈을 허용합니다.

`noEmit` : 컴파일 과정에서 타입스크립트 생성코드를 막을지 여부입니다. Babel이 자바스크립트 코드를 생성할 것 이기 때문에 true 로 설정합니다.

`jsx` : .tsx파일에서 JSX를 지원 여부

`include`: 타입스크립트가 확인할 파일 및 폴더입니다. `src`폴더의 모든 파일을 지정하였습니다.

React 타입을 개발종속성으로 프로젝트에 추가 설치합니다.

`npm install --save-dev @types/react @types/react-dom`

<br>

## Create React Component

---

src 폴더에서 index.tsx 파일과 App.tsx파일을 생성합니다.

```tsx
// index.tsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(<App />, document.getElementById("root"));
```

```tsx
//App.tsx

import React from "react";

const App = () => {
  return <div>Hello World!</div>;
};

export default App;
```

<br>

## Install Babel

---

Babel 은 React와 Typescript 코드를 Javascript로 변환시켜줍니다.
바벨에 필요한 플러그인을 개발종속성으로 설치합니다.

`npm install --save-dev @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript @babel/plugin-transform-runtime @babel/runtime`

---

`@babel/core`: 이름과 같이 바벨의 핵심입니다. transform 과 관련한 기능을 제공합니다.

`@babel/preset-env` : 최신 자바스크립트 기능을 사용할 수 있지만, 지원하지않는 브라우저를 대상으로하는 플러그인 입니다.

`@babel/preset-react` : 리액트 코드를 자바스크립트로 변환 가능하게 합니다.

`@babel/preset-typescript` : 타입스크립트 코드를 자바스크립트로 변환 가능하게 합니다.

`@babel/plugin-transform-runtime && @babel/runtime` : async/await 의 JavaScript 기능을 사용할 수있게 합니다.

---

이제 루트 폴더에 `.babelrc` 파일을 만들어줍니다.

```json
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react",
    "@babel/preset-typescript"
  ],
  "plugins": [
    [
      "@babel/plugin-transform-runtime",
      {
        "regenerator": true
      }
    ]
  ]
}
```

<br>

---

다음글에 이어서 작업을 이어나가도록 하겠습니다.

**현재 우리는 리액트와 타입스크립트를 사용하는 프로젝트를 만들었으며, 자바스크립트로 트랜스파일을 할 수 있도록 설정하는 부분까지 완료하였습니다.**
