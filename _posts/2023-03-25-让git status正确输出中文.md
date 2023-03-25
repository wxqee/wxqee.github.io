---
layout: post
title:  "让git status正确输出中文"
date:   2023-03-25 19:07:00 +0800
categories: DevOps
---

为了解决这样的问题：

![image-20230325190810829](/assets/image-20230325190810829.png)

只要一条命令：

```sh
git config --global core.quotepath off
```

![image-20230325190919082](/assets/image-20230325190919082.png)

拿下。

