---
layout: post
title:  "开发者用 Scratch Orgs 方式开发"
date:   2023-04-29 22:11:00 +0800
categories: 教程Salesforce系列
---

# 开发者用 Scratch Orgs 方式开发

步骤：

1. 开启 **Dev Hub**

   1. `SF > Settings > Dev Hub`, 打开 `Dev Hubs` 开关。

      > 一旦打开，不能关闭！

2. 登陆 **Dev Hub Org**（VSCode）

   1. vscode  `CMD+SHIFT+P > SFDX: Authorize a Dev Hub`，用登陆主Org帐号。
   1. 或 terminal `sfdx auth:web:login -d -a myhuborg`

3. 创建默认 **Scratch Org**（VSCode）

   1. vscode `CMD+SHIFT+P > SFDX: Create a Default Scratch Org...`，给个Org名字 `dev1`，给个回收天数 `7`，完。

      > 相当于运行 `sfdx force:org:create -f config/project-scratch-def.json --setalias dev1 --durationdays 7 --setdefaultusername --json --loglevel fatal`

   2. 或 terminal `sfdx force:org:create -s -f config/project-scratch-def.json -a dev1`

4. 查看 Scratch Org

   1. vscode ` CMD+SHIFT+P > SFDX: Open Default Org`
   2. 或 terminal `sfdx force:org:open`

5. 推送代码

   1. vscode `CMD + SHIFT + P > SFDX: Push Source to Default Org`

   2. 或 terminal `sfdx force:source:push`

      > Scratch Org 该命令第一次会把代码库的代码都上传布署，后续会把变更的代码上传布署。

## 参考资料

* [Salesforce - Create Scratch Orgs](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_scratch_orgs_create.htm)
* [Salesforce - Build Your Own Scratch Org Definition File](
