---
layout: post
title: Write Objective-C Component for React Native / 编写React Native自定义组件 Objective-C for iOS
date: 2017-02-06 21:57
categories: [blog ]
tags: [React Native, Mobile]
description:
---

最近公司项目准备往React Native上转移，简单学习了一下，个人对React Native的强大感到佩服。
众所周知，React Native是一个极其优秀的移动开发框架，让开发者们使用JavaScript和ReactJS就能开发出优秀的APP。

和PhoneGap／Cordova类似，我们需要依赖于第三方的插件（Plugin），在RN中，我们称之为组件（Component）。除了寻求第三方的优秀组件之外，很多时候我们需要自定义我们自己所需求的组件。本文就简单的介绍一下如何用Objective-C编写iOS组件。


## 1 - 创建新项目

当前RN的最新版本：

    react-native-cli: 2.0.1
    react-native: 0.41.2

根据RN的官方文档[od]，通过命令行创建并初始化一个RN项目。

	 react-native init CustomComponent

初始化完毕后，运行下面命令，可以在iOS模拟器上看到默认的界面。

  	cd CustomComponent
  	react-native run-ios

［img 1］


打开Xcode工程文件，用你的开发者账户Sign一下，否则在Xcode上编译的时候有可能报错。（另外工程的Targets下面会自动生成tvOS和相关的测试目标，也要Sign一下，当然也可以直接删除多余的目标）。

然后找到你的目标工程，这里是CustomComponent，找到Build Settings，查找`Header Search Paths`，添加如下两项内容，`$(SRCROOT)/../react-native/React` 和 `$(SRCROOT)/../../React` 。 否则的话，编译器找不到React Native的关联文件，将会导致`"'RCTBridgeModule.h' file not found"`的报错。

 ［img 2］

## 2 - 编写一个简单的iOS组件模块

打开Xcode项目，在项目文件夹下添加Objective-C文件，这里我们命名为`MyObjcClass`。具体步骤是：在CustomComponent文件夹上右击，选择`New File…` , 选择`Cocoa Touch Class`，Class命名为`MyObjcClass`，SubClass暂且选择`NSObject`，后面我们会做修改，语言选择Objective-C。完成后将会生成`MyObjcClass.h`和`MyObjcClass.m`两个文件。

[img 3]


打开`MyObjcClass.h`，写入以下代码，

    #import <React/RCTBridgeModule.h>

    @interface MyObjcClass : NSObject <RCTBridgeModule>

    @end

这里我们可以看到，导入的`RCTBridgeModule.h`使得我们连接原生代码和JS。

在.m文件中，我们使用`macros`来连接代码，`MyObjcClass.m`代码如下，

    #import "MyObjcClass.h"

    @implementation MyObjcClass

    // The React Native bridge needs to know our module
    RCT_EXPORT_MODULE()

    - (NSDictionary *)constantsToExport {
      // 返回一个常量字符串
      return @{@"greeting": @"Welcome to Custom Component Demo!"};
    }

    RCT_EXPORT_METHOD(squareMe:(NSString *)number:(RCTResponseSenderBlock)callback) {
      // 平方函数，返回一个整型的平方数
      int num = [number intValue];
      callback(@[[NSNull null], [NSNumber numberWithInt:(num*num)]]);
    }

    - (NSInteger)squareMe:(NSString *)number {
      int num = [number intValue];
      return num*num;
    }

    @end

首先我们需要调用`macro`告诉连接模块这个类要对我们的JS代码可见，也就是暴露API给JS。

第一个函数是一个怎样使用常量的例子，这个函数名称是固定的，所以在你的项目中不要更改名称。这里返回一个字典数据结构，包含了一个简单的字符串键值对。

第二个`macro`是导出宏，用来导出我们的自定义函数`squareMe`，这里接受一个数值字符串，和一个回调。需要注意的是，这些函数调用是异步的，所以我们不能直接返回一个数值。回调是一个包涵两个对象的数组，第一个对象是错误对象，此时它是`nil`，第二个对象是平方计算后的结果。

到目前为止，我们已经完成了所有关于Objective-C自定义的iOS组件的编写，下面就可以在JS APP中使用它们了。


## 3 - 在JS应用中使用

这里我们预期实现两个功能：

1 从Objective-C类中提取一个常量

2 调用iOS组件中的函数，并显示调用结果。

因此，我们需要两个简单的`TextView`和一个`TextInput`。另外我们使用ES6语法，确保你的`index.ios.js`代码如下。

    // 1
    import React, { Component } from 'react';
    import {
      AppRegistry,
      StyleSheet,
      Text,
      TextInput,
      View
    } from 'react-native';

    // 2
    var MyObjcClass = require('NativeModules').MyObjcClass;

    export default class CustomComponent extends Component {
       // 3
       constructor(props) {
        super(props);
        this.state = {
          number: 0
        };
      }

      //4
      render() {
        return (
          <View style={styles.container}>
          <Text style={styles.welcome}>
          {MyObjcClass.greeting}
          </Text>
          <TextInput style={styles.input} onChangeText={(text) => this.squareMe(text)}/>
    			<Text style={styles.result}> square is </Text>
          <Text style={styles.result}>
          {this.state.number}
          </Text>
          </View>
        );
      }

       // 5
       squareMe(number) {
        if (number == '') {
          return;
        }
        MyObjcClass.squareMe(number, (error, result) => {
          if (error) {
            console.error(error);
          } else {
            this.setState({number: result});
          }
        })
      }

    }

    // 6
    var styles = StyleSheet.create({
      container: {
        flex: 1,
        justifyContent: 'center',
        backgroundColor: '#F5FCFF',
      },
      welcome: {
        fontSize: 20,
        textAlign: 'center',
        margin: 20,
      },
      input: {
        width: 100,
        height: 40,
        borderColor: 'red',
        borderWidth: 1,
        alignSelf: 'center'
      },
      result: {
        textAlign: 'center',
        color: '#333333',
        fontSize: 30,
        fontWeight: 'bold',
        margin: 20,
      },
    });

    AppRegistry.registerComponent('CustomComponent', () => CustomComponent);


具体代码说明如下：

1. 提取所需要的组件，以方便后面的使用。

2. 这行代码导入Objective-C iOS组件到JS代码。

3. 构造函数，初始化结果域为0。

4. `render`函数创建界面，第一个`TextView`应该显示一个常量字符串，因为调用了`{MyObjcClass.greeting}`。`TextInput`域内容在发生变化时将调用`squareMe`函数，最后一个`TextView`将显示调用函数得到的结果（初始为0）。

5. 这个函数在我们键入数据时调用，仅仅是检查数据是否发生了变化，并再次调用`MyObjcClass`。这里我们期望一个回调，在没有错误的情况下，将会更新我们的`state`变量，从而更新界面上的计算结果。

6. 一些样式，让UI看起来好看那么一点。

最后运行起来的程序如下：

［img 4］

如果你的程序已经在运行了，这里你需要重新运行一下，以确保加载了我们自定义的Objective-C类。
示例代码已上传的我的Github[gh]供参考。



Refecesse:
How To Easily Make Your Objective-C Code Work With React Native. [ht]
React Native Official Docs. [od]



[ht]: https://devdactic.com/objective-c-code-react-native/
[od]: https://facebook.github.io/react-native/docs/getting-started.html
[gh]: https://github.com/ShongSu/React-Native-CustomComponent
