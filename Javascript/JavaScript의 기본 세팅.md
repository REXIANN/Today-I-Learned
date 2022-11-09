# JavaScript의 기본 세팅

출처: 공부
태그: 자바스크립트

리척님이 table-adapter 초기 세팅을 보고 베꼈다 헤헤

**yarn berry**를 사용하는 듯 하니 나도 한번 찾아보자.

## Folder

- .yarn/releases
- lib
- src

## File

- .editorconfig
- .eslintrc.js
- .gitignore
- .prettierignore
- .yarnrc.yml
- README.md
- jest.config.js
- package.json
- prettier.config.js
- setUpTests.js
- tsconfig.json
- yarn.lock

```jsx
{
  "name": "table-adapter",
  "version": "0.1.0",
  "license": "MIT",
  "packageManager": "yarn@3.1.0",
  "engines": {
    "node": ">= 16"
  },
  "directories": {
    "lib": "lib",
    "src": "src"
  },
  "files": [
    "lib"
  ],
  "scripts": {
    "build": "tsc && tscpaths -p tsconfig.json -s ./src -o lib",
    "eslint": "eslint src",
    "prettier": "prettier --write src",
    "storybook": "start-storybook -s public -p 6006 --no-manager-cache ",
    "test": "jest --passWithNoTests",
    "test:watch": "jest --watch",
    "watch": "rm -rf lib && tsc-watch --onSuccess 'tscpaths -p tsconfig.json -s ./src -o lib'"
  },
  "dependencies": {
    "react-table": "7.7.0"
  },
  "devDependencies": {
    "@babel/cli": "7.16.0",
    "@babel/core": "7.16.0",
    "@storybook/addon-actions": "6.3.12",
    "@storybook/addon-essentials": "6.3.12",
    "@storybook/addon-viewport": "6.3.12",
    "@storybook/react": "6.3.12",
    "@testing-library/jest-dom": "5.15.1",
    "@testing-library/react": "12.1.2",
    "@testing-library/user-event": "13.5.0",
    "@types/jest": "27.0.3",
    "@types/node": "16.11.10",
    "@types/react": "17.0.37",
    "@types/react-dom": "17.0.11",
    "@types/react-table": "7.7.8",
    "@types/react-window": "1.8.5",
    "@typescript-eslint/eslint-plugin": "5.4.0",
    "@typescript-eslint/parser": "5.4.0",
    "babel-plugin-module-resolver": "4.1.0",
    "eslint": "8.3.0",
    "eslint-plugin-import": "2.25.3",
    "eslint-plugin-jsx-a11y": "6.5.1",
    "jest": "27.3.1",
    "prettier": "2.4.1",
    "react": "16.8.6",
    "react-dom": "16.8.6",
    "ts-jest": "27.0.7",
    "tsc-watch": "4.5.0",
    "tsconfig-paths-webpack-plugin": "3.5.2",
    "tscpaths": "0.0.9",
    "typescript": "4.5.2"
  },
  "peerDependencies": {
    "react": "^16.8.3 || ^17.0.0-0"
  }
}
```