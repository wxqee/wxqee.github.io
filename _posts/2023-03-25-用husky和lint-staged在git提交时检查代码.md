---
layout: post
title:  "用husky和lint-staged在git提交时检查代码"
date:   2023-03-25 18:57:00 +0800
categories: DevOps
---

## 需求背景

日常工作中，我们常常用Git管理代码，在Git Repo的云平台创建PR进行分之合并。如果只在PR级别才在CI/CD Pipeline来检查代码，那么很容易出现问题导致PR一次一次又一次的修改。当然，本地手动执行检查和格式化代码也行，但是这些都是基于已修改文件的很难做到每次都记住去手动执行一下。那么，这时候自动化的去做就显得很方便很Cool。

所以就有了如下期望：

1. 当 `git commit` 执行中进行代码检查和代码格式化

   1. 这里就是 git commit 时，触发 husky 的 .husky/pre-commit 脚本

   2. 脚本调用 npm run precommit
   3. precommit 调用 lint-staged
   4. lint-staged 调用 package.json 配置的 prettier 以及 eslint

2. 若代码检查失败，则终止 `git commit`，修改以后再继续。
3. 若代码检查成功，则提交成功。

## 操作前

* 需已安装并配置 [**prettier**](https://www.npmjs.com/package/prettier) 以及 [**eslint**](https://www.npmjs.com/package/eslint)。

  > 若没有安装，为了测试，可以简化安装：
  >
  > ```shell
  > npm install --save-dev eslint prettier
  > ```

* 了解执行流程

  ```mermaid
  graph TD
  	Z(git add) --> A(git commit -m 'test')
    A -->|githook call| B(sh .husky/pre-commit)
    B -->|call| C(npm run precommit)
    C -->|call| D(precommit calls prettier)
    C -->|call| E(precommit calls eslint)
    D & E --> F{All Good?}
    F -->|Yes| G(commit success)
    F -->|No| H(manually update code)
    H --> Z
  ```

  ## 实际操作

  1. 安装 [**husky**](https://www.npmjs.com/package/husky) 用来配置 **git** `pre-commit` 脚本更加丝滑。

     ```shell
     # 安装npm依赖
     npm install --save-dev husky
     
     # 配置 package.json > postinstall 并使 husky 安装好
     npm set-script postinstall "husky install"
     # if not work, then use:
     # npx npe scripts.postinstall "husky install"
     npm run postinstall
     
     # 配置 package.json > precommit 同时让 .husky/pre-commit 在用户提交代码时触发 precommit
     npx husky add .husky/pre-commit "npm run precommit"
     echo "_" >> .husky/.gitignore
     git add .husky/pre-commit .husky/.gitignore
     
     npm set-script precommit "lint-staged"
     ```

     > ⚠️ 若 `npm set-script postinstall "husky install"` 对你的npm无效？
     >
     > 那么试试`npx npe scripts.postinstall "husky install"`这个命令。版本不一样，设置文件的方式也不一样。

  2. 安装 [**lint-staged**](https://www.npmjs.com/package/lint-staged) 用来为 git 已缓存（待提交）的文件依次执行一系列操作。

     ```shell
     # 安装npm依赖
     npm install --save-dev lint-staged
     ```

  3. 配置 **package.json** 文件最终长这样(其他部分已被略去)：

     ```json
     {
         "scripts": {
             "postinstall": "husky install",
             "precommit": "lint-staged"
         },
         "lint-staged": {
             "**/*.{css,html,js,json,md,xml,yaml,yml}": [
                 "prettier --write"
             ],
             "**/*.js": [
                 "eslint"
             ]
         }
     }
     ```

     > 因为 **lint-staged** 只针对 git cached files 有效，所以只关注代码库管理的文件，这么配置很安全。

  4. 试一下成果。

     ```shell
     git commit -m 'let me try try if husky + lint-staged works!'
     ```

  ## The End

  