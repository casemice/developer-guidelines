# React Native App Setup with Expo

This is a step-by-step guide to customize CRA for Casemice projects. You can review [React Native Expo Boilerplate](https://github.com/casemice/react-native-expo-boilerplate) project to see how your application looks like when all steps followed.

### Tech Stack

#### Core

- [Expo](https://expo.io)
- [React Native](https://reactjs.org)
- [React Navigation](https://github.com/ReactTraining/react-router)
- [Redux](https://redux.js.org)
- [Redux Toolkit](https://redux-toolkit.js.org)
- [Typescript](https://github.com/microsoft/TypeScript)

#### Unit Testing

- [Jest](https://jestjs.io/docs/en/tutorial-react)
- [react-test-renderer](https://tr.reactjs.org/docs/test-renderer.html)

#### Linting

- [ESLint](https://eslint.org)
- [Prettier](https://prettier.io)

## Table of Contents

- [Step 1: Creating a new app](#step-1-creating-a-new-app)
- [Step 2: Update TypeScript Configurations](#step-2-update-typescript-configurations)
- [Step 3: Installing Prettier](#step-3-installing-prettier)
- [Step 4: Create IDE Configurations](#step-4-create-ide-configurations)
- [Step 5: Installing ESLint](#step-5-installing-eslint)
- [Step 6: Setting up our test environment](#step-6-setting-up-our-test-environment)
- [Step 7: Setting up React Navigation](#step-7-setting-up-react-navigation)
- [Step 8: Setting up Redux store](#step-8-setting-up-redux-store)
- [Step 9: Organizing Folder Structure](#step-9-organizing-folder-structure)
- [Step 10: Final Touches](#step-10-final-touches)
- [Step 11: Starting to Development](#step-11-starting-to-development)

## Step 1: Creating a new app

First of all, we will create React Native app with Expo to initialize our codebase with Typescript template.

```sh
expo init -t expo-template-blank-typescript react-native-starter
cd react-native-starter
```

## Step 2: Update TypeScript Configurations

We want to keep type safety as strict as possibble. In order to do that, we update `tsconfig.json` with the settings below.

```jsonc
"strict": true,
"noImplicitAny": true,
"noImplicitReturns": true,
"skipLibCheck": true,
"isolatedModules": false,
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

Finally, we update `package.json` with related format scripts.

```json
"format": "yarn run format:ts",
"format:ts": "prettier --write 'src/**/*.{ts,tsx}'",
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
yarn add eslint eslint-config-airbnb eslint-config-prettier eslint-plugin-eslint-comments eslint-plugin-import eslint-plugin-jest eslint-plugin-jsx-a11y eslint-plugin-prettier eslint-plugin-react eslint-plugin-react-native @typescript-eslint/eslint-plugin @typescript-eslint/parser --dev
```

Then we need to setup ESLint configurations.

`.eslintrc`

```json
{
  "parser": "@typescript-eslint/parser",
  "extends": [
    "airbnb",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended",
    "plugin:eslint-comments/recommended",
    "plugin:import/errors",
    "plugin:import/warnings",
    "plugin:import/typescript",
    "plugin:jest/recommended"
  ],
  "env": {
    "browser": true,
    "jest": true,
    "react-native/react-native": true
  },
  "plugins": [
    "react",
    "react-native",
    "@typescript-eslint",
    "jsx-a11y",
    "import",
    "prettier",
    "jest",
    "eslint-comments"
  ],
  "rules": {
    "@typescript-eslint/indent": "off",
    "@typescript-eslint/explicit-function-return-type": "off",
    "@typescript-eslint/no-use-before-define": "off",
    "react/jsx-filename-extension": [
      1,
      { "extensions": [".js", ".jsx", ".ts", ".tsx"] }
    ],
    "react/prop-types": "off",
    "react/button-has-type": "off",
    "no-use-before-define": "off",
    "import/no-extraneous-dependencies": [
      "error",
      {
        "devDependencies": [
          "storybook/**/*.{ts,tsx,js}",
          "config-overrides.js",
          "src/setupTests.ts",
          "src/components/**/*.stories.tsx",
          "src/styles/**/*.stories.tsx",
          "src/**/*.test.{ts,tsx}"
        ]
      }
    ],
    "react-native/no-unused-styles": "error",
    "react-native/no-inline-styles": "error",
    "react-native/no-color-literals": "error",
    "react/jsx-one-expression-per-line": "off",
    "@typescript-eslint/explicit-member-accessibility": "off",
    "prettier/prettier": ["error"]
  },
  "overrides": [
    {
      "files": ["*.style.ts"],
      "rules": {
        "@typescript-eslint/camelcase": "off"
      }
    },
    {
      "files": ["*.stories.tsx", "*.test.tsx"],
      "rules": {
        "@typescript-eslint/no-explicit-any": "off",
        "react-native/no-color-literals": "off",
        "react-native/no-inline-styles": "off"
      }
    }
  ]
}
```

`.eslintignore`

```ignore
ios
android
build
coverage

# Storybook
storybook/storyLoader.js
```

We need to update `package.json` for ESLint scripts.

```json
"lint": "yarn run lint:ts",
"lint:ts": "tsc && yarn lint:eslint",
"lint:eslint": "eslint 'src/**/*.{ts,tsx}'",
"format:ts": "prettier --write 'src/**/*.{ts,tsx}' && yarn lint:eslint --fix",
```

Finally, we need to enable prettier ESLint integration on VSCode.

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

## Step 6: Setting up our test environment

Jest is a JavaScript based test runner, which allows tests to be run blazing fast and in parallel.

We'll use `jest` with `react-test-renderer`.

```sh
yarn add jest-expo jest react-test-renderer @types/jest @types/react-test-renderer --dev
```

We need to create config file for jest.

`jest.config.js`

```js
module.exports = {
  preset: "jest-expo",
  transformIgnorePatterns: [
    "node_modules/(?!(jest-)?react-native|react-clone-referenced-element|@react-native-community|expo(nent)?|@expo(nent)?/.*|react-navigation|@react-navigation/.*|@unimodules/.*|unimodules|sentry-expo|native-base|@sentry/.*)",
  ],
  moduleFileExtensions: [
    "ts",
    "tsx",
    "js",
    "jsx",
    "json",
    "ios.ts",
    "ios.tsx",
    "android.ts",
    "android.tsx",
  ],
  collectCoverageFrom: [
    "src/**/*.{ts,tsx}",
    "!src/index.tsx",
    "!src/setupTests.ts",
    "!src/components/**/index.{ts,tsx}",
    "!src/**/*.stories.{ts,tsx}",
    "!src/**/*.style.ts",
    "!src/styles/**/*",
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
};
```

We need to add the `coverage` to `.gitignore` and `.prettierignore`.

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

Add the following script into `package.json`

`package.json`

```json
"test": "jest --config ./jest.config.js",
"test:watch": "yarn test --watch",
"coverage": "yarn run test --coverage"
```

Let's add a simple test to verify our setup.

`src/App.test.tsx`

```js
import React from "react";
import renderer from "react-test-renderer";

import App from "../App";

describe("<App />", () => {
  it("Renders as expected", () => {
    const tree = renderer.create(<App />).toJSON();
    expect(tree).toMatchSnapshot();
  });
});
```

Also, verify coverage report with yarn coverage.

When you run yarn coverage, a folder named coverage will be created in the root directory. This folder is auto-generated file. We should add it to `.gitignore`

```ignore
# .gitignore

...
# Test Coverage
coverage
```

## Step 7: Setting up React Navigation

React Navigation is a routing and navigation for our React Native apps. We will start by installing it.

```sh
yarn add @react-navigation/native
```

React Navigation has some dependencies that we need to install as well. Thanks to Expo that they are really easy to install with it.

```sh
expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

We can now start using it. We will setup and stack navigation example for now. To create stack navigation, we need to install it is related package.

```sh
yarn add @react-navigation/stack
```

Finally, we can update App.tsx file to try stack navigation.

```js
import * as React from "react";
import { View, Text } from "react-native";
import { NavigationContainer } from "@react-navigation/native";
import { createStackNavigator } from "@react-navigation/stack";

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text>Home Screen</Text>
    </View>
  );
}

const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```

## Step 8: Setting up redux store

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

## Step 9: Organizing Folder Structure

Our folder structure should look like this;

```
src/
├── App.test.tsx
├── App.tsx
├── __snapshots__
│   └── App.test.tsx.snap
├── components
│   └── Button
│       ├── Button.style.ts
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
├── index.tsx
├── screens
│   ├── Feed
│   │   ├── Feed.style.ts
│   │   ├── Feed.test.tsx
│   │   ├── Feed.tsx
│   │   ├── index.ts
│   │   └── tabs
│   │       ├── Discover
│   │       │   ├── Discover.style.ts
│   │       │   ├── Discover.test.tsx
│   │       │   ├── Discover.tsx
│   │       │   └── index.ts
│   │       └── MostLiked
│   │           ├── MostLiked.test.tsx
│   │           ├── MostLiked.tsx
│   │           └── index.ts
│   ├── Home
│   │   ├── Home.style.ts
│   │   ├── Home.test.tsx
│   │   ├── Home.tsx
│   │   └── index.ts
│   └── index.ts
├── styles
│   ├── Colors.ts
│   ├── Spacing.ts
│   ├── Typography.ts
│   └── index.ts
└── utils
    ├── location.test.ts
    └── location.ts
```

## Step 10 Final Touches

We are ready to develop our application. Just a final step, we need to update our `README.md` to explain what we add a script so far.

```md
This project was set up with following [React Native Expo Setup](https://github.com/casemice/react-native-expo-setup).

## Available Scripts

In the project directory, you can run:

### `yarn start`

Runs the app in the development mode.<br>
Expo will open browser window to control your development.

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

## Learn More

You can learn more in the [Reac Native documentation](https://reactnative.dev/docs/getting-started).

Also check out the [Expo documentation](https://expo.io).
```

## Step 11: Starting to Development

Everything is done! You can start to develop your next awesome React Native application now on 🚀

## Related

- [react-native-expo-boilerplate](https://github.com/casemice/react-native-expo-boilerplate) - React Native Expo Boilerplate
