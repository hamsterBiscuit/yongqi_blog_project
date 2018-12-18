---
title: react-native 配置 mobx
tags:
  - mobx RN
categories:
  - RN
---

## 环境

``` bash
    "mobx": "^5.8.0",
    "mobx-react": "^5.4.3",
    "react": "^16.6.3",
    "react-native": "^0.57.8",
    "@babel/core": "^7.2.2",
```

这个东西坑了我好久，还是配置的少，在此写下，希望能帮助其他遇到相同问题的人解决

## install

第一步就是安装 mobx 和 mobx-react

``` bash
    yarn add mobx mobx-react
    # or with npm
    # npm install --save mobx mobx-react
```

<!-- more -->

然后开启装饰器模式，目前装饰器模式还没有被浏览器正式支持，所以借助 babel 插件

``` bash
    yarn add @babel/plugin-proposal-decorators @babel/plugin-proposal-class-properties --dev
    # or with npm
    # npm install --save-dev @babel/plugin-proposal-decorators @babel/plugin-proposal-class-properties
```
``` json
    // .babelrc
    "plugins": [
    	["@babel/plugin-proposal-decorators", { "legacy": true }],
    	["@babel/plugin-proposal-class-properties", { "loose": true }]
    ]
```

设置好这些就可以愉快的使用 mobx 的装饰器了- -！

然后我发现，所有的绑定事件失效了。翻了翻 issue ，找到这个解决方式。

issue 地址：[https://github.com/facebook/react-native/issues/22157](https://github.com/facebook/react-native/issues/22157)

下面是解决办法
``` bash
    yarn add @babel/plugin-transform-flow-strip-types --dev
    # or with npm
    # npm install --save-dev @babel/plugin-transform-flow-strip-types
```
``` json
    // .babelrc
    "plugins": [
    		...
        "@babel/plugin-transform-flow-strip-types",
    		...
      ]
```

是的，加个 babel 插件就可以，好了，我们可以愉快的使用 mobx 了，

等下，我们试试 android

![](http://img.i6miao.cn/WechatIMG32.jpeg)

看来还没完，继续查资料，发现了如下方案

[https://github.com/react-community/jsc-android-buildscripts#how-to-use-it-with-my-react-native-app](https://github.com/react-community/jsc-android-buildscripts#how-to-use-it-with-my-react-native-app)

下面是项目改动
``` bash
    yarn add jsc-android --dev
    # or with npm
    # npm install --save-dev jsc-android
```
然后修改 android 下的文件
``` java
    // android/build.gradle
    allprojects {
        repositories {
            mavenLocal()
            jcenter()
            maven {
                // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
                url "$rootDir/../node_modules/react-native/android"
            }
    +       maven {
    +           // Local Maven repo containing AARs with JSC library built for Android
    +           url "$rootDir/../node_modules/jsc-android/dist"
    +       }
        }
    }
```
``` java
    // android/app/build.gradle
    }

    +configurations.all {
    +    resolutionStrategy {
    +        force 'org.webkit:android-jsc:r236355'
    +    }
    +}

    dependencies {
        compile fileTree(dir: "libs", include: ["*.jar"])
```
在打开 android 就正常了，本文也就结束了。
