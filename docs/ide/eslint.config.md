### Alway use eslint for Javascript code to improvement quanlity code. Don't make your code is a bull shit. Don't code for you, code for everybody.

### Some config for eslint. Step for install eslint
- Run ``` npm install --save eslint ```
- Create a .eslintrc.yml in root project.
- Create config eslint.
- Create file .eslintignore to ignore file you don't need check.

### Some config for ReactJS with Typescript Language.
```
root: true

parser: '@typescript-eslint/parser'

extends:
  - prettier
  - eslint:recommended
  - plugin:react/recommended

plugins:
  - prettier

env:
  browser: true
  node: true

parserOptions:
  ecmaVersion: 2017
  sourceType: module
  ecmaFeatures:
    jsx: true
    globalReturn: true

rules:
  prettier/prettier:
    - error
    - no-unused-vars: 'warn'
      no-console: warn
      no-debbuger: warn
      printWidth: 80
      trailingComma: all
      semi: false
  jsx-quotes:
    - warn
    - prefer-double
```

### File ignore.
```
build/*
node_modules
logs
run
/**/*.md
/**/*.d.ts
```
