# eslint-prettier-angry

ESLint と Prettier を共存させた上で怒られたい

eslint, react, typescript の config が許す style 設定を破る

- eslint
- react
  - https://github.com/prettier/eslint-config-prettier/blob/master/react.js
- typescript
  - https://github.com/prettier/eslint-config-prettier/blob/master/%40typescript-eslint.js

## using plugin config

when

```js
module.exports = {
  env: {
    browser: true,
    es2020: true,
  },
  extends: [
    "./config.js",
    "plugin:prettier/recommended",
    "prettier/@typescript-eslint",
    "prettier/react",
  ],
  parser: "@typescript-eslint/parser",
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 11,
    sourceType: "module",
  },
  plugins: ["react", "@typescript-eslint"],
};
```

then

```
> npx eslint src/**

/Users/ideyuta/Documents/100_projects/toybox/eslint-prettier-angry/src/app.ts
  5:15  error  Replace `·:` with `:·`              prettier/prettier
  7:6   error  Replace `{a:0}` with `·{·a:·0·};⏎`  prettier/prettier

/Users/ideyuta/Documents/100_projects/toybox/eslint-prettier-angry/src/index.jsx
  8:15  error  Replace `⏎······{foo}⏎······` with `{foo}`  prettier/prettier

✖ 3 problems (3 errors, 0 warnings)
  3 errors and 0 warnings potentially fixable with the `--fix` option.
```

## without plugin config

when

```js
module.exports = {
  env: {
    browser: true,
    es2020: true,
  },
  extends: [
    "./config.js",
    // "plugin:prettier/recommended",
    // "prettier/@typescript-eslint",
    // "prettier/react",
  ],
  parser: "@typescript-eslint/parser",
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 11,
    sourceType: "module",
  },
  plugins: ["react", "@typescript-eslint"],
};
```

then

```
> npx eslint src/**

/Users/ideyuta/Documents/100_projects/toybox/eslint-prettier-angry/src/app.ts
  1:1   error  Rule 'space-return-throw-case' was removed and replaced by: keyword-spacing  space-return-throw-case
  5:16  error  Expected a space after the ':'                                               @typescript-eslint/type-annotation-spacing
  5:16  error  Unexpected space before the ':'                                              @typescript-eslint/type-annotation-spacing

/Users/ideyuta/Documents/100_projects/toybox/eslint-prettier-angry/src/index.jsx
  1:1   error  Rule 'space-return-throw-case' was removed and replaced by: keyword-spacing  space-return-throw-case
  8:10  error  Missing parentheses around multilines JSX                                    react/jsx-wrap-multilines

✖ 5 problems (5 errors, 0 warnings)
  3 errors and 0 warnings potentially fixable with the `--fix` option.
```

## ここからわかること

* plugin:prettier/recommended は default で ESLint 組み込みルールのスタイルに関するものをoffにしている
* "prettier/**" 系はきちんと style ルールをOFFにしている