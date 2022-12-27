# team 7 프로젝트 셋업 방법

## 1. CRA 프로젝트 만들기 & git 원격 저장소에 올리기

- `npx create-react-app project-name`
- GitHub에서 저장소 만들기
- `git init` `git remote add origin repo-url`
- 첫 번째 커밋 올리기

## 2. Jest test setup

### 2.1 `src/__test__` 폴더 생성

    - App 컴포넌트의 `h1` 렌더링 여부 확인하는 테스트 코드

```js
function App() {
  return (
    <div className="App">
      <h1>app</h1>
    </div>
  );
}

export default App;
```

```js
// DOM.test.js
import "@testing-library/jest-dom"; // CRA의 setupTest.js 파일 삭제 시 추가
import { render, screen } from "@testing-library/react";
import App from "../App";

describe("App 컴포넌트 렌더링 테스트", () => {
  test("<App /> 렌더링이 되나요?", async () => {
    render(<App />);

    const headingEl = screen.getByRole("heading", makeOptions(1, "app"));
    expect(headingEl).toBeInTheDocument();
  });
});

const makeOptions = (level, name) => {
  return { level, name };
};
```

### 2.2 package.json의 `test` script 수정

- Jest Unexpected Token Error 방지를 위해

```json
//package.json
  "scripts": {
    "test": "react-scripts test --transformIgnorePatterns \"node_modules/(?!@toolz/allow-react)/\" --env=jsdom --watchAll",
  },
```

### 2.3 필요 시 유닛 테스트 코드 추가

## 3. ESLint & Prettier 설정

### 3.1 패키지 설지

`npm install eslint --save-dev`
`npm install prettier --save-dev`
`npm install eslint-config-prettier --save-dev` // ESLint의 포맷팅 셋팅을 꺼주는 역할

```js
// .eslintrc
{
  "extends": ["react-app", "eslint:recommended"],
  "rules": {
    "no-var": "error",
    "no-multiple-empty-lines": "error",
    "no-console": ["warn", { "allow": ["warn", "error", "info"] }],
    "eqeqeq": "error",
    "no-unused-vars": "warn",
    "no-undef": "warn"
  }
}
```

```js
// .prettierrc.js
module.exports = {
  singleQuote: true,
  semi: true,
  useTabs: false,
  tabWidth: 2,
  trailingComma: "all",
  printWidth: 80,
};
```

### 3.2 script 추가

- `.gitignore` 파일에 .eslintcache 추가

```json
//package.json
  "scripts": {
    "format": "prettier --write --cache .",
    "lint": "eslint --cache ."
  },
```

## 4. Husky를 사용한 #2 자동화

### 4.1 husky 설치(최초 프로젝트 셋업 시에만 시행)

`npm install husky --save-dev` (`git init`이 되어있어야 함)
`npx husky install` // husky에 등록된 hook을 .git에 적용시키기 위한 스크립트 - 이후 저장소를 클론 받는 사람들은 `npm i` 이후 자동으로 `postinstall` 스크립트가 실행됨

```json
// package.json
  "scripts": {
    "postinstall": "husky install"
  },

```

### 4.2 git hooks 추가하기

- 커밋 이전에 코드 포맷팅
  `npx husky add .husky/pre-commit "npm run format"`
- 푸시 이전에 린터&테스트 실행
  - `npx husky add .husky/pre-push "npm run lint"`
  - `npx husky add .husky/pre-push "npm run test"`
    - `./husky/pre-push`의 커맨드 수정: `npm run test -- --watchAll=false`

## 5. Github Actions를 사용한 #3 자동화

- develop branch에 Pull Request가 발생하면 해당 브랜치에서 자동으로 테스트 실행

```yml
name: test PR

on:
  pull_request:
    branches:
      - develop

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          ref: "develop"
      - run: npm ci
      - run: npm run test -- --watchAll=false
```
