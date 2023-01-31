# code-guide

优秀规范是保证代码质量的最有效途径

## 准备工作

### 演示项目初始化

使用 vite + react 来演示

```bash
npm create vite@latest code-guide
# 选择 react + typescript, ... 项目初始化完成。

cd code-guide
npm install
npm run dev
```

### 安装 vscode 插件 ESLint 和 ESLint，并配置项目工作区

```ts
// .vscode/settings.json
{
  "editor.bracketPairColorization.enabled": true,
  "editor.codeActionsOnSave": {
    "source.fixAll": true, // 保存时，自动格式化
    "source.fixAll.eslint": true, // 保存时，eslint 自动纠正
    "source.organizeImports": false // 关闭vscode 文件头部 import 排序
  },
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.tabSize": 2,
  "typescript.tsdk": "node_modules/typescript/lib",
  "typescript.enablePromptUseWorkspaceTsdk": true
}
```

## [可组装的 JavaScript 和 JSX 检查工具 ESLint](http://eslint.cn/docs/user-guide/getting-started)

### 入门

```bash
# 安装
npm install eslint --save-dev

# 初始化配置，自主选择配置后，根目录生成.eslintrc.cjs
./node_modules/.bin/eslint --init

# 调试 https://eslint.org/docs/latest/user-guide/command-line-interface#--fix
npx eslint --fix src/App.tsx
```

注意： If you are using the new JSX transform from React 17,
extend react/jsx-runtime in your eslint config (add "plugin:react/jsx-runtime" to "extends") to disable the relevant rules.

```ts
{
  extends: [
    ...,
    'plugin:react/jsx-runtime',
  ]
}
```

### [eslint-plugin-react-hooks](https://www.npmjs.com/package/eslint-plugin-react-hooks)

支持校验 hook 语法

```bash
npm install eslint-plugin-react-hooks --save-dev
```

```ts
// .eslintrc.cjs
{
  'extends': [ ..., 'plugin:react-hooks/recommended']
}
```

### [eslint-plugin-import](https://github.com/import-js/eslint-plugin-import/blob/6304ddc70fc187e248aa65c69bc8983c5051ecd3/docs/rules/order.md)

自定义 ts 文件头部 import 导入排序规则

```bash
npm install eslint-plugin-import --save-dev
```

```ts
// .eslintrc.cjs
{
  plugins: [..., 'eslint-plugin-import'],
  rules: {
    // ...
    // eslint-plugin-import 导入排序规则
    'import/order': [
      'error',
      {
        groups: ['builtin', 'external', 'internal', 'parent', 'sibling', 'index', 'object', 'type'],
        pathGroups: [
          {
            pattern: 'react**',
            group: 'builtin',
            position: 'before',
          },
          {
            pattern: 'vite**',
            group: 'builtin',
            position: 'before',
          },
          {
            pattern: '@/**',
            group: 'internal',
            position: 'before',
          },
          {
            pattern: '.scss',
            group: 'type',
            position: 'after',
          },
        ],
        pathGroupsExcludedImportTypes: ['react'],
        'newlines-between': 'never',
        alphabetize: { order: 'asc', caseInsensitive: true },
      },
    ],
  },
}
```

### [eslint-plugin-unused-imports](https://www.npmjs.com/package/eslint-plugin-unused-imports)

无效 import 引用， eslint 提示报错

```bash
npm install eslint-plugin-unused-imports --save-dev
```

```ts
// .eslintrc.cjs
{
  plugins: [ ..., 'unused-imports'],
  rules: {
    // ...
    // unused-imports
    '@typescript-eslint/no-unused-vars': 'off',
    'unused-imports/no-unused-imports': 'error',
    'unused-imports/no-unused-vars': [
      'warn',
      {
        vars: 'all',
        varsIgnorePattern: '^_',
        args: 'after-used',
        argsIgnorePattern: '^_',
      },
    ],
  },
}
```

## [一个“有态度”的代码格式化工具 Prettier](https://www.prettier.cn/)

分享一个我的常用配置

```ts
// .prettierrc.cjs
module.exports = {
  // 一行最多 120 字符
  printWidth: 120,
  // 使用 2 个空格缩进
  tabWidth: 2,
  // 不使用缩进符，而使用空格
  useTabs: false,
  // 行尾需要有分号
  semi: true,
  // 使用单引号
  singleQuote: true,
  // 对象的 key 仅在必要时用引号
  quoteProps: 'as-needed',
  // jsx 不使用单引号，而使用双引号
  jsxSingleQuote: false,
  // 末尾需要有逗号
  trailingComma: 'all',
  // 大括号内的首尾需要空格
  bracketSpacing: true,
  // jsx 标签的反尖括号需要换行
  jsxBracketSameLine: false,
  // 箭头函数，只有一个参数的时候，也需要括号
  arrowParens: 'always',
  // 每个文件格式化的范围是文件的全部内容
  rangeStart: 0,
  rangeEnd: Infinity,
  // 不需要写文件开头的 @prettier
  requirePragma: false,
  // 不需要自动在文件开头插入 @prettier
  insertPragma: false,
  // 使用默认的折行标准
  proseWrap: 'preserve',
  // 根据显示样式决定 html 要不要折行
  htmlWhitespaceSensitivity: 'css',
  // vue 文件中的 script 和 style 内不用缩进
  vueIndentScriptAndStyle: false,
  // 换行符使用 lf
  endOfLine: 'lf',
  // 格式化嵌入的内容
  embeddedLanguageFormatting: 'auto',
};
```

## [git commit 规范 commitlint](https://commitlint.js.org/#/)

增强代码 commit msg 的可读性

### 演示

```bash
git add .
git commit -m 'xxx' # no no no

git commit -m 'feat: 新功能' # yes
```

| 类型     | 描述                                                   |
| -------- | ------------------------------------------------------ |
| build    | 编译相关的修改，例如发布版本、对项目构建或者依赖的改动 |
| feat     | 新特性、新功能                                         |
| fix      | 修复 bug                                               |
| refactor | 代码重构                                               |
| docs     | 文档修改                                               |
| chore    | 其他修改, 比如改变构建流程、或者增加依赖库、工具等     |
| style    | 代码格式修改, 注意不是 css 修改                        |
| revert   | 回滚到上一个版本                                       |
| ci       | 持续集成修改                                           |
| perf     | 优化相关，比如提升性能、体验                           |
| test     | 测试用例修改                                           |

### 配置教程

```bash
# 安装 lint-staged husky
npm install lint-staged husky --save-dev
# 在package.json中添加脚本
npm set-script prepare "husky install"

# 初始化husky,将 git hooks 钩子交由,husky执行
npm run prepare
# 初始化 husky, 会在根目录创建 .husky 文件夹
npx husky add .husky/pre-commit "npx lint-staged"

# 安装 commitlint 相关依赖
npm install @commitlint/cli @commitlint/config-conventional --save-dev
# .husky/commit-msg
npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
```

```ts
// package.json 增加配置
{
  ...,
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "src/**/*.{js,jsx,ts,tsx,json,css,scss}": [
      "prettier --write"
    ]
  }
}
// .commitlintrc.js 配置
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [
      2,
      'always',
      ['build', 'feat', 'fix', 'refactor', 'docs', 'chore', 'style', 'revert', 'ci', 'perf', 'test'],
    ],
    'type-case': [0],
    'type-empty': [0],
    'scope-empty': [0],
    'scope-case': [0],
    'subject-full-stop': [0, 'never'],
    'subject-case': [0, 'never'],
    'header-max-length': [0, 'always', 72],
  },
};
```
