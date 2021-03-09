---
layout: post
title: Create React and Typescript Project using Webpack(2)
subtitle: "CRA manually"
type: "Webpack"
createDate: 2021-03-09
til: true
text: true
author: "codeFabian"
post-header: false
header-img: ""
order: 19
---

<br>

지난 글에서는 **CRA** (Create React App) 을 직접 해보기 위해, 프로젝트에 리액트와 타입스크립트 그리고 바벨에 대한 설정을 마쳤습니다.

이번에는 **코드스타일과 웹팩을 추가**하며 마무리를 해보도록 하겠습니다.

<br>

## Adding Linting

---

**ESLint** 를 사용하기 위해서 우리는 ESLint를 설치하도록 하겠습니다. **개발 시에 테스트를 위해서 필요하기 때문에 배포 시에는 필요없기 때문에 Dev Dependencies 로 설치**합니다.

`npm install --save-dev eslint eslint-plugin-react eslint-plugin-react-hooks @typescript-eslint/parser @typescript-eslint/eslint-plugin`

```
 eslint : eslint 라이브러리를 사용하기 위함입니다.
eslint-plugin-react : React 코드에서의 코드스타일 규칙을 가져옵니다.
eslint-plugin-react-hooks: 훅스 코드스타일을 포함하고 있습니다.
@typescript-eslint/parser: 타입스크립트 코드스타일을 적용을 가능하게 합니다.
@typescript-eslint/eslint-plugin: 타입스크립트 코드스타일을 포함합니다.
```

다음과 같이 파일을 만들어줍니다. **`.eslintrc.json`** 파일입니다.

```json
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 2018,
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint", "react-hooks"],
  "extends": [
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "react/prop-types": "off"
  },
  "settings": {
    "react": {
      "pragma": "React",
      "version": "detect"
    }
  }
}
```

<br>

## Adding Prettier

---

다음으로 ESLint와 함께 사용할 때 잘 어울리는 **Prettier**를 통해 자동으로 코드를 수정할 수 있도록 추가시킵니다.

`npm install --save-dev prettier eslint-config-prettier eslint-plugin-prettier`

`**.prettierrc**` 파일에 다음과 같이 작성합니다.

```json
{
  "printWidth": 100,
  "singleQuote": true,
  "semi": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "endOfLine": "auto"
}
```

또한 조금 전에 만들었던 **.eslintrc.json** 파일에 다음과 같이 **Prettier**를 추가 합니다.

```json
{
  ...,
  "extends": [
    ...,
    "plugin:prettier/recommended"
  ],
  "rules": {
    ...,
    "prettier/prettier": [
      "error",
        {
          "endOfLine": "auto"
        }
      ]
    }
  }
}
```

여기까지 프로젝트에 자동으로 코드스타일을 수정하고 규칙을 정하도록 설정하였습니다.

이제 드디어 **웹팩**을 추가시켜보도록 하겠습니다.

<br>

## Install Webpack

---

먼저 웹팩이 무엇인지를 아는게 중요하지만 이번에는 개념에 대한 글보단 실습의 경험을 위주로 적도록 하겠습니다.

일단 설명보다는 설레는 마음으로 설치부터 하도록 하겠습니다.

`npm install --save-dev webpack webpack-cli @types/webpack`

그리고 **webpack dev server**를 설치하도록 하겠습니다.

**webpack dev server 는 빠른 실시간 리로딩 기능을 갖춘 개발서버**라고 합니다.

- 디스크에 저장되지 않는 메모리 컴파일을 사용하기 때문에 컴파일 속도가 빠르다.
- devServer 옵션을 통해 사용이 가능하다.

`npm install --save-dev webpack-dev-server @types/webpack-dev-server`

그리고 Babel 이 리액트와 타입스크립트 코드를 트랜스파일 할 수 있도록 babel-loader를 설치해줍니다.

`npm install --save-dev babel-loader`

<br>

## Setting Webpack

---

웹팩 파일은 자바스크립트가 기본이지만 `ts-node`를 통해서 타입스크립트를 사용할 수 있도록 할 수 있습니다.

`npm install --save-dev ts-node`

자 이제 웹팩파일을 설정해보겠습니다. 파일이름은 `webpack.config.ts` 입니다.

```tsx
import path from "path";
import webpack from "webpack";

const config: webpack.Configuration = {
  entry: "./src/index.tsx",
  module: {
    rules: [
      {
        test: /\.(ts|js)x?$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: [
              "@babel/preset-env",
              "@babel/preset-react",
              "@babel/preset-typescript",
            ],
          },
        },
      },
    ],
  },
  resolve: {
    extensions: [".tsx", ".ts", ".js"],
  },
  output: {
    path: path.resolve(__dirname, "build"),
    filename: "bundle.js",
  },
  devServer: {
    contentBase: path.join(__dirname, "build"),
    compress: true,
    port: 4000,
  },
};

export default config;
```

`entry`: 해당영역은 웹팩이 모듈을 묶어 줄 위치를 찾습니다. 해당 엔트리를 통해서 필요한 모듈을 로딩하고 하나의 파일로 묶게됩니다.

`module`: babel-loader플러그인을 사용하였고, ts파일과 js 파일들을 트랜스파일링 해줄 것 입니다.

`output`: dependency graph에 따라 번들링 된 파일들을 저장할 path와 파일명 입니다. 우리는 build폴더에 bundle.js에 번들링 된 파일들이 저장될 것 입니다.

마지막으로 `fork-ts-checker-webpack-plugin` 를 통해 타입체크를 할 수 있도록 설정하겠습니다.

`npm install --save-dev fork-ts-checker-webpack-plugin @types/fork-ts-checker-webpack-plugin`

그리고 **webpack.config.ts** 파일에 추가시킵니다.

```tsx
...
import ForkTsCheckerWebpackPlugin from 'fork-ts-checker-webpack-plugin';

const config: webpack.Configuration = {
  ...,
  plugins: [
    new ForkTsCheckerWebpackPlugin({
      async: false,
      eslint: {
        files: "./src/**/*",
      },
    }),
  ],
};
```

`async`: false 를 통해서 웹팩이 타입체킹이 완료될때까지 기다리도록 설정합니다.

`eslint`: src 의 파일들을 설정함으로 웹팩이 linting 에러를 알려줄 수 있습니다.

이렇게 webpack 설정을 마치도록 합니다.

<br>

## npm scripts

---

**package.json**에 다음과 같이 작성합니다.

```json
...,
  "scripts": {
    "start": "webpack serve --open",
    "build": "webpack --mode production"
  },
  ...
```

그리고 설레는 마음으로 서버를 열어줍니다. `npm start`

<img width="474" alt="스크린샷 2021-03-09 오후 2 34 22" src="https://user-images.githubusercontent.com/46562138/110424076-8c9e9000-80e5-11eb-9db6-fef39b5d8934.png">

<br>

### 마치며

---

지금까지 웹팩을 이용하여 CRA를 수동으로 해보는 실습을 했습니다.

이번이 처음으로 웹팩을 사용해본 경험이기 때문에 부족한 부분과 아직 모르는부분이 많습니다!

앞으로도 사용해보면서 새롭게 알게되는 부분을 적고, 잘못된 부분은 수정하겠습니다.
