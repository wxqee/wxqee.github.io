---
layout: post
title:  "快速起步cordova手机app工程"
date:   2023-04-25 18:00:00 +0800
categories: 快速起步系列
---
# 快速起步cordova手机app工程

## 安装 Cordova

Cordova命令行运行于 [Node.js](https://nodejs.org/) 并可在[NPM](https://npmjs.org/package/cordova)上使用。跟随 [平台特定指南](https://cordova.apache.org/docs/en/latest/index.html#develop-for-platforms) 安装额外的平台依赖项。打开命令提示符或终端，然后键入 `npm install -g cordova`。

```sh
npm install -g cordova
```

## 创建工程

使用命令行工具创建一个空白的Cordova项目。导航到您希望创建项目并键入的目录 `cordova create <path>`。

创建工程 `MyApp`：

```sh
cordova create MyApp
```

如需一整套选项，请键入`cordova help create` 。

## 添加平台

创建Cordova项目后，导航到项目目录。从项目目录中，您需要添加一个要构建应用程序的平台。

要添加平台，请键入 `cordova platform add <platform name>`。

有关您可以添加的平台的完整列表，请运行 `cordova platform`。

```sh
cd MyApp
cordova platform add browser
```

## 运行应用程序

从命令行中，运行 `cordova run <platform name>`。

```sh
cordova run browser
```

## 常见的后续步骤

- [阅读文档](https://cordova.apache.org/docs/en/latest/guide/overview/)
- [添加插件](https://cordova.apache.org/docs/en/latest/guide/cli/#add-plugins)
- [自定义应用程序图标](https://cordova.apache.org/docs/en/latest/config_ref/images.html)
- [配置您的应用程序](https://cordova.apache.org/docs/en/latest/config_ref/)

## 为 Android 应用程序配置

添加Android平台

```sh
cd MyApp
cordova platform add android
```

运行App

```sh
cordova run android
```

> 若无Android开发环境，会报错：
>
> ```text
> Checking Java JDK and Android SDK versions
> ANDROID_SDK_ROOT=undefined (recommended setting)
> ANDROID_HOME=undefined (DEPRECATED)
> Failed to find 'ANDROID_SDK_ROOT' environment variable. Try setting it manually.
> Failed to find 'android' command in your 'PATH'. Try update your 'PATH' to include path to valid SDK directory.
> ```

### 安装 Gradle

安装 [Gradle](https://gradle.org/install/) 包管理器。手动安装：

下载 [Gradle 8.1.1](https://gradle.org/next-steps/?version=8.1.1&format=bin)，得到 `gradle-8.1.1-bin.zip` 文件。

Linux & macOS 用户直接解压部署

```sh
sudo mkdir /opt/gradle
sudo unzip -d /opt/gradle ~/Downloads/gradle-8.1.1-bin.zip
```

设置环境变量

```sh
export GRADLE_HOME=/opt/gradle/gradle-8.1.1
export PATH=$PATH:$GRADLE_HOME/bin
```

就可以用了。

### Android Studio ([Cordova官方文档](https://cordova.apache.org/docs/en/11.x/guide/platforms/android/index.html#android-studio))

下载并安装 [Android Studio](https://developer.android.com/studio/index.html)。按照链接的Android Developer网站上的说明开始。

> 1. 采用默认的选项安装，不要自定义安装！除非你是Android专家。
> 2. 过程中会提示网络不好，让你配置Proxy。那就配置吧，没有的话应该就玩不下去了，毕竟Android是Google的东西。

首次打开Android Studio将指导您完成安装Android SDK软件包的过程。

在 Android Studio 欢迎界面 > More Actions > **SDK Manager**。

* 或 Preference > Appearance & Behavior > System Settings > **Android SDK**，即 SDK Manager。
* 或 Menu > Tools > **SDK Manager**。

在 SDK Manager 界面上，确认为目标版本的 Android 安装了以下内容：

- **SDK Platform** 标签页里，**Android** 符合您的目标API级别。
- **SDK Tools** 标签页里，**Android SDK Build-Tools** 符合您的目标版本。

#### Android SDK Tools

In Android Studio 3.6 or later, the obsolete Android SDK Tools will need to be intalled. To do this:

1. Open the Android Studio
2. Open the **SDK Manager** (`Tools > SDK Manager`)
3. Navigate to the **SDK Tools** tab
4. Uncheck `Hide Obsolete Packages`
5. Check `Android SDK Tools (Obsolete)`

See Android's documentation on how to [Update your tools with the SDK Manager](https://developer.android.com/studio/intro/update#sdk-manager) for more details.

### 设置环境变量

Cordova的CLI需要特定的环境变量，这样它才能正常运行。如果环境变量缺失，CLI将尝试暂时解析变量。如果缺失的变量无法解析，则必须手动设置。

必须设置以下变量：

- `JAVA_HOME` - JDK安装位置的环境变量
- `ANDROID_SDK_ROOT` - Android SDK安装位置的环境变量

还建议更新 `PATH` 环境变量以包含以下目录。

- `cmdline-tools/latest/bin`
- `emulator`
- `platform-tools`
- `build-tools`
  - This is required for the `apksigner` and `zipalign` tools.

***Note:** 上面的目录通常位于Android SDK ROOT中。*

#### macOS and Linux

On a Mac or Linux, with a text editor, create or modify the `~/.bash_profile` file.

To set an environment variable, add a line that uses `export` like so (substitute the path with your local installation):

```
export ANDROID_SDK_ROOT=/Development/android-sdk/
```

To update your `PATH`, add a line resembling the following (substitute the paths with your local Android SDK installation's location):

```
export PATH=$PATH:$ANDROID_SDK_ROOT/platform-tools/
export PATH=$PATH:$ANDROID_SDK_ROOT/cmdline-tools/latest/bin/
export PATH=$PATH:$ANDROID_SDK_ROOT/emulator/
```

Reload your terminal to see this change reflected or run the following command:

```
source ~/.bash_profile
```

## 开发

在 `www` 目录下开始开发，这里修改 `index.html` 弄点 Hello World 内容就好。

## 发布 APK

默认情况下，`cordova create` 生成一个基于 Web 的骨架应用程序，其起始页是项目的 `www/index.html` 文件。任何初始化都应指定为 `www/js/index.js` 中定义的设备就绪事件处理程序的一部分。

运行以下命令为所有平台构建项目：

```sh
cordova build
```

您可以选择将每个构建的范围限制在特定平台-在这种情况下是“android”：

```sh
cordova build android
```

注意日志，会提示生成了文件 `/Your Project Path/platforms/android/app/build/outputs/apk/debug/app-debug.apk`。把它发给自己测试一下。
