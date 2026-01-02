---
layout: post
title: Write Android Component for React Native / 编写React Native自定义Android组件
date: 2017-11-01 10:36
categories: [blog ]
tags: [React Native, Mobile, Android]
description:
---

之前写过一篇[如何在React Native下用Objective-C编写iOS组件][oc]的博文，本篇文章简单介绍一下如何编写Android的React Native组件。

本篇以React Native官方文档[Native Modules][nm]为基础，从零开始介绍如果编写基于React Native的Android的[Toast][tt]组件。

## 1 - 准备工作

  * Android Studio
  * React Native CLI

```
npm install -g react-native-cli
```


## 2 - 创建React Native Bridge Module - ToastDemo

文件结构如下：

```
--Project Folder
```
   |--package.json
   |--android  //这个文件夹内包含我们所需要的Java的原生代码
   |--index.js   
```

```

接下来我们分别来编写、生成这些文件。

#### Package.json

创建一个新的文件夹叫`react-native-toast-demo`，它也是你的模块的名称。下面我们进行初始化该项目：

```
cd react-native-toast-demo
npm init
```
根据命令行提示一路回车键使用默认值即可，当然你也可以进行初始化赋值或之后到文件中修改都可以。



```
~$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (react-native-toast-demo)
version: (1.0.0)
description: Toast Demo for React Native Android
entry point: (index.js)
test command:
git repository:
keywords: react native, android, toast, demo
author: shongsu
license: (ISC) MIT
About to write to ~/react-native-toast-demo/package.json:

{
  "name": "react-native-toast-demo",
  "version": "1.0.0",
  "description": "Toast Demo for React Native Android",
  "main": "index.js",
  "scripts": {
```
"test": "echo \"Error: no test specified\" && exit 1"
```
},
"keywords": [
```
"react",
"native",
"android",
"toast",
"demo"
```
  ],
  "author": "shongsu",
  "license": "MIT"
}


Is this ok? (yes) yes
```

到现在为止，我们的`package.json`文件就已经生成成功了。

#### Android

接下来到了创建Android项目的时候了，首先建立一个简单的Android项目结构目录。结构大体如下：

```
--react-native-toast-demo/android
```
  |--build.gradle
  |--src
```
|--main
    |--AndroidManifest.xml
    |--java
         |--com
             |--shongsu
                   |--toastdemo
                        |--DemoModule.java
                        |--DemoPackage.java
```
```

```

  * build.gradle

  ```
apply plugin: 'com.android.library'

android {
```
  compileSdkVersion 23
  buildToolsVersion "25.0.0"
```

```
  defaultConfig {
```
minSdkVersion 16
targetSdkVersion 23
versionCode 1
versionName "1.0"
```
  }
```
}

dependencies {
```
  compile "com.facebook.react:react-native:+"
```
}
  ```

  * AndoridManifest.xml

  ```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
```
package="com.shongsu.toastdemo">
```
</manifest>
  ```

  * DemoModule.java

  ```
package com.shongsu.toastdemo;

import android.widget.Toast;

import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactMethod;

public class DemoModule extends ReactContextBaseJavaModule {

```
public DemoModule(ReactApplicationContext reactContext) {
  super(reactContext);
}
```

```
@Override
public String getName() {
  return "DemoModule";
}
```

```
@ReactMethod
public void alert(String message) {
  Toast.makeText(getReactApplicationContext(), message, Toast.LENGTH_LONG).show();
}
```
}
  ```
  DemoModule继承了ReactContextBaseJavaModule类并实现了JavaScript所需要的方法。
  在这里，`getName()`返回的字符串必须和你的类名相同

  * DemoPackage.java

  DemoPackage.java是用来注册模块的。

  ```
package com.shongsu.toastdemo;

import com.facebook.react.ReactPackage;
import com.facebook.react.bridge.JavaScriptModule;
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.uimanager.ViewManager;
import com.shongsu.toastdemo.DemoModule;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class DemoPackage implements ReactPackage {

```
public List<Class<? extends JavaScriptModule>> createJSModules() {
  return Collections.emptyList();
}
```

```
@Override
public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
  return Collections.emptyList();
}
```

```
@Override
public List<NativeModule> createNativeModules(
```
ReactApplicationContext reactContext) {
```
  List<NativeModule> modules = new ArrayList<>();
```

```
  modules.add(new DemoModule(reactContext));
```

```
  return modules;
}
```

}
  ```

#### Index.js

接下来我们来创建`index.js`文件，该文件是用来将React Native的模块封装成JavaScript的模块的。

```
import {NativeModules} from 'react-native';
module.exports = NativeModules.DemoModule;
```

至此，我们完成了这个React Native的模块。


### Example

创建一个React Native项目
```
react-native init Example
```

由于所有的Node模块都必须安装在`{React Native project}\node_modules\`目录下，所以我们需要将插件的相对路径写入React Native项目的`package.json`中，以保证项目能够正确找到插件文件。

添加后的`Example/package.json`代码如下：

```
"dependencies": {
```
```
"react": "16.0.0-beta.5",
"react-native": "0.49.5",
"react-native-toast-demo":"../"
```
},
```

```

然后通过`npm`安装模块：
```
cd Example
npm install
```

接下来链接依赖项：
```
react-native link
```


在`App.js`中使用我们的自定义组件：

```
import React, { Component } from 'react';
import {
```
Platform,
StyleSheet,
Text,
View,
Button
```
} from 'react-native';
import DemoModule from 'react-native-toast-demo';

const onButtonPress = () => {
```
DemoModule.alert('Hello World');
```
};

export default class App extends Component<{}> {
```
render() {
  return (
```
<View style={styles.container}>
  <Button title='Click' onPress={onButtonPress}/>
</View>
```
  );
}
```
}

const styles = StyleSheet.create({
```
container: {
  flex: 1,
  justifyContent: 'center',
  alignItems: 'center',
  backgroundColor: '#F5FCFF',
}
```
});
```

然后运行项目即可。

项目代码的[Github链接][code]。



[oc]:http://shongsu.github.io/blog/oc-component-for-react-native.html
[nm]:https://facebook.github.io/react-native/releases/0.21/docs/native-modules-android.html
[tt]:https://developer.android.com/reference/android/widget/Toast.html
[code]: https://github.com/ShongSu/react-native-toast-demo
