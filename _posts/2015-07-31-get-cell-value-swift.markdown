---
layout: post
title: Get Value from Cells of TableView / TableView点击获取当前Cell内容
date: 2015-07-31 18:12
categories: [blog ]
tags: [iOS, Swift, ]
description:
---


<center>**------English (英文)------**</center>


......


<center>**------中文 (Chinese)------**</center>


首先，在一个TableViewController里面有很多个Cell，我们希望在点击每个条目之后，跳转到第二个视图，在第二个视图上显示所点击的条目的索引和内容。

第二个视图的代码如下：

```
class ViewController: UIViewController {
```


```
var rowIndex = 0     //定义当前条目的索引
var rowValue = ""    //定义当前条目的内容值
```

```
override func viewDidLoad() {
    super.viewDidLoad()
    //分别显示出当前条目的索引和内容值
    label1.text = "rowIndex is \(rowIndex)"
    label2.text = "rowValue is \(rowValue)"
}
```

```
@IBOutlet weak var label1: UILabel!
```

```
@IBOutlet weak var label2: UILabel!
```

```
override func didReceiveMemoryWarning() {
    super.didReceiveMemoryWarning()
}
```


```
}
```


首先我们可以在`TableViewController.swift`文件中定义以下函数代码：


```
override func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
```


```
    // 其中“viewcontroller2”为第二个视图在storyboard中的唯一识别ID
    var viewController = self.storyboard?.instantiateViewControllerWithIdentifier("viewcontroller2") as! ViewController   
    // indexPath.row即为当前行的索引值，从0开始
    viewController.rowIndex = indexPath.row  
    // 加载第二个视图viewController
    self.navigationController?.pushViewController(viewController, animated: true)
}
```


以上代码很容易即可实现对当前条目索引值的获取。那么我们要想获得条目的内容，改怎么办呢？

方法也十分简单，我们只需要把下面三行代码加入到`didSelectRowAtIndexPath`函数中即可。


```
// 定义cell是UITableVeiwCell类型，返回索引为indexPath的Cell
var cell = tableView.cellForRowAtIndexPath(indexPath)
// 转化cell为UILabel类型，并赋值给viewController视图中的label2
viewController.label2 = cell?.textLabel
// 将所得到的label2的text内容赋值给rowValue变量
viewController.rowValue = viewController.label2.text!
```


这样我们就能够在第二个视图中同时获取被点击Cell的索引和内容值了。
