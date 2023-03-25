---
layout: post
title:  "申请一个30天的体验版本的 Salesforce Consumer Goods Cloud Org"
date:   2023-03-25 21:56:00 +0800
categories: Salesforce
---

## 启程

* 从 Consumer Goods Free Trial 网站[注册](https://www.salesforce.com/form/industries/consumer-goods/consumer-goods-free-trial/)30天适用版授权Org。
  * 得到账号邮件。
  * 找个本本记录账号邮件中的“**登陆URL**”、“**Username**”以及“**Password**”。
* 从 Org > Setup > Installed Packages 查看 "Consumer Goods Cloud" 和 "Consumer Goods Cloud Mobile Sync" 的版本号。
  * 得到版本号："**236**"。
* [如果是国内 Andriod 用户] 从 m.apkpure.com 网站[下载](https://m.apkpure.com/consumer-goods-cloud/com.salesforce.industries.offlineapp) 对应版本的APK安装包。
  * 得到APK："**Consumer Goods Cloud_v236.0003.00_apkpure.com.apk**"。
* [其他] [参考FAQ](#faq-download-mobie-app)文档。
* 在手机安装APK文件，不要打开APK。
* 从 Org > "Consumer Goods" App > "Enable Offline Mobile App" 网页，按照说明一步一步完成。
  * 检查是否完成了两个Rep账号的Locale和Language设置？
  * 检查是否完成了APK安装？
  * 检查是否复制了某个Rep账号的Username？
  * 检查是否打开了QR code？
  * 检查是否用手机摄像头扫描了QR code，并自动唤醒了 Consumer Goods Cloud 离线手机应用？
  * 检查是否可以用刚才选择的Rep账号的Username以及默认密码test1234成功登陆，并看见开始加载资源界面？
    * 可能会遇到提示修改密码，可以修改。
    * 可能会遇到同步资源错误提示，可以[参考FAQ](#faq-sync-issue)解决。
* 恭喜你，完成了安装步骤！
* 接下来继续完成 "Enable Offline Mobile App" 网页的其他步骤。

## 学习资料

* Trailmix - [Consumer Goods Cloud](https://trailhead.salesforce.com/en/users/strailhead/trailmixes/consumer-goods-cloud)
* Trail - [Use Consumer Goods Cloud Offline Mobile App](https://trailhead.salesforce.com/en/content/learn/trails/use-consumer-goods-cloud-offline-mobile-app)
* [Consumer Goods Cloud / Partner Pocket Guide](https://quip.com/sfz0Atx4JASz) -- 包含很多额外资源和视频
* [Consumer Goods Cloud Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.retail_api.meta/retail_api)

## 常见问题

### <a name="faq-download-mobie-app">FAQ: 国内手机如何安装离线手机APP (Consumer Goods Cloud Offline Mobile App)？</a>

**Android手机**：在 APKPure 网站找到并下载 [Consumer Goods Cloud 脱壳的 APK](https://m.apkpure.com/consumer-goods-cloud/com.salesforce.industries.offlineapp) 并安装即可，注意版本号要跟Org > Setup > Installed Package 中与其相关的版本号保持一致，注意要配置手机允许这个“**未知来源**”的APK安装包。

**iOS手机**：只能用国外的账号从App Store下载安装，至于国内账号的用户我是真没办法帮上忙，问了一圈也没人有办法。

### <a name="faq-sync-issue">FAQ: 离线手机APP同步资源最后一步提示错误</a>

截止2022年7月20日，离线手机APP，版本236，在同步资源时大概率会在最后一步提示错误。从 Org > "Sync Management" app > "Device Events Log" tab 中，可以看到相关的错误日志。

有一个解决办法：进入 Org > "Sync Management" app > "Remove Request" tab 页面，这里此时应该是空的。点击 New 去创建一条记录，把能选的都给选上，保存。在离线手机APP上，退出并重新打开APP。此时同步资源不会再出现错误，全部都是绿色，并且APP里边的数据也充实起来，登陆的Rep用户的Avatar以及Full Name也会更新出来。