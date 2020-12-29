# React Single Page App Setup

This is a step-by-step guide to customize CRA for Casemice projects. You can review [React Boilerplate](https://github.com/casemice/react-boilerplate) project to see how your application looks like when all steps followed.

### Tech Stack

#### Core

- [React](https://reactjs.org)
- [React Router](https://github.com/ReactTraining/react-router)
- [Redux](https://redux.js.org)
- [Redux Toolkit](https://redux-toolkit.js.org)
- [Typescript](https://github.com/microsoft/TypeScript)

#### Unit Testing

- [Enzyme](https://enzymejs.github.io/enzyme/)

#### Linting

- [ESLint](https://eslint.org)
- [Prettier](https://prettier.io)
- [Stylelint](https://stylelint.io)

## Table of Contents

- [Step 1: Creating a new app](#step-1-creating-a-new-app)
- [Step 2: Update TypeScript Configurations](#step-2-update-typescript-configurations)
- [Step 3: Installing Prettier](#step-3-installing-prettier)
- [Step 4: Create IDE Configurations](#step-4-create-ide-configurations)
- [Step 5: Installing ESLint](#step-5-installing-eslint)
- [Step 6: Enabling Sass](#step-6-enabling-sass)
- [Step 7: Installing stylelint](#step-7-installing-stylelint)
- [Step 8: Setting up our test environment](#step-8-setting-up-our-test-environment)
- [Step 9: Enabling hot reloading](#step-9-enabling-hot-reloading)
- [Step 10: Adding Storybook](#step-10-adding-storybook)
- [Step 11: Adding React Router](#step-11-adding-react-router)
- [Step 12: Setting up Redux store](#step-12-setting-up-redux-store)
- [Step 13: Enabling code-splitting](#step-13-enabling-code-splitting)
- [Step 14: Organizing Folder Structure](#step-14-organizing-folder-structure)
- [Step 15: Final Touches](#step-15-final-touches)
- [Step 16: Starting to Development](#step-16-starting-to-development)

## Step 1: Creating a new app

First of all, we will create React app with CRA to initialize our codebase with Typescript template.

```sh
npx create-react-app cra-starter --template typescript
cd cra-starter
yarn start
```

## Step 2: Update TypeScript Configurations

We want to keep type safety as strict as possibble. In order to do that, we update `tsconfig.json` with the settings below.

```jsonc
"noImplicitAny": true,
"noImplicitReturns": true,
```

## Step 3: Installing Prettier

Prettier is a code formatter tool to make your code more readable. We want to format our code automatically. So, we need to install Prettier.

```sh
yarn add prettier --dev
```

`.prettierrc`

```jsonc
{
  "printWidth": 100,
  "singleQuote": true,
  "trailingComma": "all"
}
```

`.prettierignore`

```ignore
build
```

Finally, we update `package.json` with related format scripts.

```json
"format:ts": "prettier --write 'src/**/*.{ts,tsx}'",
"format": "yarn run format:ts",
"format:check": "prettier -c 'src/**/*.{ts,tsx}'"
```

## Step 4: Create IDE Configurations

VS Code looks at the settings in the file with .vscode extension for project-based editor settings. Here we activate the formatting feature on save file.

`.vscode/settings.json`

```json
{
  "editor.formatOnSave": true
}
```

## Step 5: Installing ESLint

ESLint is a comprehensive code analysis tool. We want to have consistency in our codebase and also want to catch mistakes. So, we need to install ESLint.

```sh
yarn add eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin --dev
```

`.eslintrc`

```jsonc
{
  "root": true,
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint"],
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {}
}
```

`.eslintignore`

```ignore
public
build
react-app-env.d.ts
node_modules
dist
```

`.vscode/settings.json`

```json
{
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ]
}
```

We need to update `package.json` for ESLint scripts.

```json
"lint": "yarn run lint:ts",
"lint:ts": "tsc && yarn lint:eslint",
"lint:eslint": "eslint 'src/**/*.{ts,tsx}'",
"format:ts": "prettier --write 'src/**/*.{ts,tsx}' && yarn lint:eslint --fix",
```

## Step 6: Enabling Sass

SASS is a tool that allows you to write more advanced CSS. CRA comes with Sass support out of the box. In order to enable it, we only add `node-sass` to our project.

```sh
yarn add node-sass --dev
```

## Step 7: Installing stylelint

We also want a linter for our sass files. We need to install `stylelint`.

```sh
yarn add stylelint stylelint-config-prettier stylelint-prettier @atolye15/stylelint-config --dev
```

`.stylelintrc`

```jsonc
{
  "extends": ["@atolye15/stylelint-config", "stylelint-prettier/recommended"]
}
```

Finally, we need to update `package.json` and `.vscode/settings.json`

`package.json`

```json
"lint:css": "stylelint --syntax scss \"src/**/*.scss\"",
"lint": "yarn run lint:ts && yarn run lint:css",
"format:css": "stylelint --fix --syntax scss \"src/**/*.scss\"",
"format": "yarn run format:ts && yarn run format:css"
```

## Step 8: Setting up our test environment

### Jest

Jest is a JavaScript based test runner, which allows tests to be run blazing fast and in parallel.

### Enzyme

Enzyme is a JavaScript Testing utility for React that makes it easier to test your React Components’ output.

We'll use `jest` with `enzyme`.

```sh
yarn add enzyme enzyme-adapter-react-16 react-test-renderer --dev
yarn add @types/enzyme @types/enzyme-adapter-react-16 --dev
```

> Note: We do not need to install Jest, as CRA is bundled with Jest out of the box.

Also we need to install `enzyme-to-json` for simpler snapshosts.

```sh
yarn add enzyme-to-json --dev
```

We update our `package.json` for jest configuration.

```json
"scripts": {
  "coverage": "yarn run test --coverage"
},
"jest": {
  "snapshotSerializers": [
    "enzyme-to-json/serializer"
  ],
  "collectCoverageFrom": [
    "src/**/*.{ts,tsx}",
    "!src/index.tsx",
    "!src/setupTests.ts",
    "!src/components/**/index.{ts,tsx}",
    "!src/components/**/*.stories.{ts,tsx}"
  ],
  "coverageThreshold": {
    "global": {
      "branches": 80,
      "functions": 80,
      "lines": 80,
      "statements": 80
    }
  }
}
```

And finally, we need to add the `coverage` to `.gitignore` and `.prettierignore`.

`.eslintignore`

```ignore
# ...
coverage
```

`.prettierignore`

```ignore
# ...
coverage
```

Before the testing, we need to add our setup file to initialize enzyme.

`src/setupTests.ts`

```ts
import { configure } from "enzyme";
import Adapter from "enzyme-adapter-react-16";

configure({ adapter: new Adapter() });
```

With this config, we are able to run tests with snapshots and create coverage. Let's add a simple test to verify our setup.

`src/App.test.tsx`

```tsx
import React from "react";
import { shallow } from "enzyme";
import App from "./App";

it("runs correctly", () => {
  const wrapper = shallow(<App />);

  expect(wrapper).toMatchSnapshot();
});
```

Also, verify coverage report with `yarn coverage`.

## Step 9: Enabling hot reloading

We want to take advantage of hot reloading and don't want to lose React's current state. In order to do that we can use react hot loader. Since, we use CRA and don't want to eject it, we need to use `customize-cra` package.

```sh
yarn add react-app-rewired customize-cra @hot-loader/react-dom --dev
```

After the installation we need to update `package.json` scripts to use `react-app-rewired`

```json
"start": "react-app-rewired start",
"build": "react-app-rewired build",
"test": "react-app-rewired test",
"eject": "react-app-rewired eject"
```

Now, we can install `react-hot-loader`.

```sh
yarn add react-hot-loader
```

Also we need to update hot reloader config.

`src/index.tsx`

```tsx
import { setConfig } from "react-hot-loader";

setConfig({
  ignoreSFC: true,
  pureRender: true,
});
```

In order to update babel config for hot loader, we need to create a `config-overrides.js` file on the root.

```ts
// eslint-disable-next-line @typescript-eslint/no-var-requires
const { override, addBabelPlugin, addWebpackAlias } = require("customize-cra");

module.exports = override(
  addBabelPlugin("react-hot-loader/babel"),
  addWebpackAlias({
    "react-dom": "@hot-loader/react-dom",
  })
);
```

Lastly, we need to use hot HOC.

`src/App.tsx`

```tsx
import React from "react";
import { hot } from "react-hot-loader/root";

export default hot(App);
```

## Step 10: Adding Storybook

We need to initialize the Storybook on our project.

```sh
npx -p @storybook/cli sb init --type react
```

Since we use `typescript`, we can change the file extensions (`main` and `preview`) to `.ts` in `.storybook` folder.

Let's create a story for our Button component.

`src/components/Button/Button.stories.tsx`

```tsx
import React from "react";
import { storiesOf } from "@storybook/react";

import Button from "./Button";

storiesOf("Button", module)
  .add("Primary", () => <Button primary>Primary Button</Button>)
  .add("Secondary", () => <Button secondary>Secondary Button</Button>);
```

Run storybook

```
yarn storybook
```

## Step 11: Adding React Router

As usual, we want to use `react-router` for routing.

```
yarn add react-router-dom
yarn add @types/react-router-dom --dev
```

Then, we need to encapsulate our root component with `BrowserRouter`.

```tsx
// src/index.tsx

import React, { FunctionComponent } from "react";
import ReactDOM from "react-dom";
import { setConfig } from "react-hot-loader";
import { BrowserRouter } from "react-router-dom";

import App from "./App";

setConfig({
  ignoreSFC: true,
  pureRender: true,
});

const Root: FunctionComponent = () => (
  <BrowserRouter>
    <App />
  </BrowserRouter>
);

ReactDOM.render(<Root />, document.getElementById("root"));
```

```tsx
// src/routes/Routes.tsx

import React, { FunctionComponent } from "react";
import { Switch, Route } from "react-router-dom";

import Home from "./Home";
import Feed from "./Feed";

const Routes: FunctionComponent = () => (
  <Switch>
    <Route path="/" exact component={Home} />
    <Route path="/feed" exact component={Feed} />
  </Switch>
);

export default Routes;
```

```tsx
// src/App.tsx

import React, { FunctionComponent, Fragment } from "react";
import { hot } from "react-hot-loader";

import Routes from "./routes";

const App: FunctionComponent = () => (
  <Fragment>
    <header>Header</header>
    <Routes />
    <footer>Footer</footer>
  </Fragment>
);

export default hot(module)(App);
```

## Step 12: Setting up redux store

Redux Toolkit is a toolset for efficient Redux development. We are going to setup it and it has little bit different from regular Redux. You may need to check out it's offical [documents](https://redux-toolkit.js.org).

Let's install them and implement loading state for our app

```sh
yarn add react-redux @reduxjs/toolkit
yarn add @types/react-redux --dev
```

In your Loading component folder, create `LoaderSlice.ts` file to create our loading state, action and dispatchers

```js
import { createSlice } from "@reduxjs/toolkit";
import { AppDispatch } from "../../store";

export type LoadingState = { isLoading: boolean };
export type LoadingSliceState = { loading: LoadingState };

export const loadingSlice = createSlice({
  name: "loading",
  initialState: {
    isLoading: true,
  },
  reducers: {
    startLoading: (state) => {
      state.isLoading = true;
    },
    stopLoading: (state) => {
      state.isLoading = false;
    },
  },
});

export const { startLoading, stopLoading } = loadingSlice.actions;

export const stopLoadingAsync: () => void = () => (dispatch: AppDispatch) => {
  setTimeout(() => {
    dispatch(stopLoading());
  }, 1000);
};

export const loadingState = (state: LoadingSliceState) =>
  state.loading.isLoading;

export default loadingSlice.reducer;
```

Now lets create a `store.ts` file in `/src`

```js
import { configureStore } from "@reduxjs/toolkit";
import loadingReducer from "./components/Loading/LoadingSlice";

const store = configureStore({
  reducer: {
    loading: loadingReducer,
  },
});

export default store;

export type AppDispatch = typeof store.dispatch;
```

We need to wrap our application with store provider in `/index.ts` as well

```js
import store from "./store";

const Root: FunctionComponent = () => (
  <BrowserRouter>
    <Provider store={store}>
      <App />
    </Provider>
  </BrowserRouter>
);
```

We can use it on `Home` page

```js
import React, { FunctionComponent } from "react";
import { useSelector, useDispatch } from "react-redux";

// Components
import Loading from "../../components/Loading";

// State
import {
  loadingState,
  stopLoadingAsync,
} from "../../components/Loading/LoadingSlice";

const Home: FunctionComponent = () => {
  const loading = useSelector(loadingState);
  const dispatch = useDispatch();

  React.useEffect(() => {
    // This going to dispatch stopLoadingAsync reducer that delays proccess 1000ms
    dispatch(stopLoadingAsync());
  }, []);

  return (
    <div className="container">{loading ? <Loading /> : <h1>Home</h1>}</div>
  );
};

export default Home;
```

## Step 13: Enabling code-splitting

We want to make route based code-splitting in order to prevent a huge bundled asset. When we done with this, only relevant assets will be loaded by our application. Let's install `react-loadable`.

```
yarn add react-loadable
yarn add @types/react-loadable --dev
```

Now, let's convert our routes to dynamically loaded.

```tsx
// src/routes/Home/index.ts

import Loadable from "react-loadable";

import Loading from "../../components/Loading";

const LoadableHome = Loadable({
  loader: () => import("./Home"),
  loading: Loading,
});

export default LoadableHome;
```

```tsx
// src/routes/Feed/index.ts

import Loadable from "react-loadable";

import Loading from "../../components/Loading";

const LoadableFeed = Loadable({
  loader: () => import("./Feed"),
  loading: Loading,
});

export default LoadableFeed;
```

## Step 14: Organizing Folder Structure

Our folder structure should look like this;

```
src/
├── App.test.tsx
├── App.tsx
├── __snapshots__
│   └── App.test.tsx.snap
├── components
│   └── Button
│       ├── Button.scss
│       ├── Button.stories.tsx
│       ├── Button.test.tsx
│       ├── Button.tsx
│       ├── __snapshots__
│       │   └── Button.test.tsx.snap
│       └── index.ts
├── containers
│   └── Like
│       ├── Like.tsx
│       └── index.ts
├── fonts
├── img
├── index.tsx
├── react-app-env.d.ts
├── routes
│   ├── Feed
│   │   ├── Feed.scss
│   │   ├── Feed.test.tsx
│   │   ├── Feed.tsx
│   │   ├── index.ts
│   │   └── tabs
│   │       ├── Discover
│   │       │   ├── Discover.scss
│   │       │   ├── Discover.test.tsx
│   │       │   ├── Discover.tsx
│   │       │   └── index.ts
│   │       └── MostLiked
│   │           ├── MostLiked.test.tsx
│   │           ├── MostLiked.tsx
│   │           └── index.ts
│   ├── Home
│   │   ├── Home.scss
│   │   ├── Home.test.tsx
│   │   ├── Home.tsx
│   │   └── index.ts
│   └── index.ts
├── setupTests.ts
├── styles
│   └── index.scss
└── utils
    ├── location.test.ts
    └── location.ts
```

## Step 15 Final Touches

We are ready to develop our application. Just a final step, we need to update our `README.md` to explain what we add a script so far.

```md
This project was set up with following [React Single Page App Setup](https://github.com/casemice/react-spa-setup).

## Available Scripts

In the project directory, you can run:

### `yarn start`

Runs the app in the development mode.<br>
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will hot reload if you make edits.<br>
You will also see any lint errors in the console.

### `yarn test`

Launches the test runner in the interactive watch mode.<br>
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `yarn coverage`

Launches coverage reporter. You can view the details from `coverage` folder.

### `yarn lint`

Runs ESLint and StyleLint. If any lint errors found, then it exits with code 1.

### `yarn format`

Formats TypeScript and Sass files.

### `yarn format:check`

Checkes if any formatting error has been made.

### `yarn storybook`

Launches Storybook application.

### `yarn build`

Builds the app for production to the `build` folder.<br>
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br>
Your app is ready to be deployed!

### `yarn deploy`

Deploys app to given platform according to instuctions in `deploy.sh`

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).
```

## Step 16: Starting to Development

Everything is done! You can start to develop your next awesome React application now on 🚀

## Related

- [react-boilerplate](https://github.com/casemice/react-boilerplate) - React Single Page App Boilerplate
