# Git Hooks

## 安装 husky

方式 1: 使用 npm 安装 husky

```bash
# 使用 npm 安装 husky
npm install husky --save-dev
```

![](./使用npm安装husky的方式.png)

方式 2: 使用 yarn 安装 husky

```bash
# 使用 yarn 安装 husky
yarn add husky -D
```

![](./使用yarn安装husky的方式.png)

**如上：在同等条件下，使用 yarn 无法安装成功，且无法报出 git 版本过低的问题。所以 husky 的安装必须使用 npm，如果使用 yarn 是无法生效的。**

## 安装 prettier 和 lint-staged

```bash
# 安装 prettier 和 lint-staged
yarn add lint-staged prettier pretty-quick -D
```

在 package.json 中添加如下配置项：

```json
// package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "pretty-quick --staged && lint-staged"
    }
  }
}
```

在项目根目录下新建 .prettier.js 并填入以下内容：

```js
module.exports = {
  semi: false, // 使用分号 默认true
  singleQuote: true, // 使用单引号 默认false jsx中无效
  bracketSpacing: false, // jsx标签闭合位置 默认false false为换行闭合 true为当前行闭合
  // tab缩进大小,默认为2
  tabWidth: 2,
  // 使用tab缩进，默认false
  useTabs: true,
  jsxSingleQuote: true, // jsx中使用单引号
};
```

## commitlint

安装 commitlint 依赖：

```bash
yarn add @commitlint/cli @commitlint/config-conventional -D
```

在 husky hooks 中追加属性：

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "pretty-quick --staged && lint-staged",
      "commit-msg": "commitlint -e $HUSKY_GIT_PARAMS"
    }
  }
}
```

在项目根路径下新建 commitlint.config.js

```js
module.exports = {
  extends: ["@commitlint/config-conventional"],
  rules: {
    "type-enum": [
      2,
      "always",
      ["upd", "feat", "fix", "refactor", "docs", "chore", "style", "revert"],
    ],
    "type-case": [0],
    "type-empty": [0],
    "scope-empty": [0],
    "scope-case": [0],
    "subject-full-stop": [0, "never"],
    "subject-case": [0, "never"],
    "header-max-length": [0, "always", 72],
  },
};
```

然后新建提交即可看到效果。
