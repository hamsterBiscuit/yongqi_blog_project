---
title: eslint 配置 git commit 前置钩子
tags:
  - eslint git RN
categories:
  - RN
---

> 配置下来，应该不挑技术栈，这里使用的是 RN, 包管理工具使用 yarn，使用 npm 或者其他技术栈也是一样的。

- **install**

``` bash
yarn add eslint --dev
or yarn add global eslint --dev

```

- **use**

``` bash
eslint --init
```

<!-- more -->

会出现交互式命令，帮组你创建自己的 eslint 规则
这里根据自己的情况自行选择，后期也是可以通过编辑 config 文件修改规则，不再这里赘述

然后安装 husky 来实现 git commit 钩子做 eslint 校验

``` bash
yarn add husky --dev
```

使用

``` json
// package.json
...
"scripts": {
  ...
  "eslint": "./node_modules/.bin/eslint --ext .js ./"
}
...
"husky": {
  "hooks": {
    "pre-commit": "npm run eslint"
  }
}
...
```

husky 可以不进可以实现 git commit 前置钩子，push前置后置钩子也可以实现，具体用法跟本例差不多，官方 github 有详细介绍。有需求请自行查看。
