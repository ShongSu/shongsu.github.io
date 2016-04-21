---
layout: post
title: Example of iOS VoIP Notification / iOS VoIP Notification实例  
date: 2016-04-20 22:31
post-link:
---

这篇文章翻译整理自Pierre-Marc Airoldi的[iOS 8 VoIP Notifications][pi]和[M Penades][mp]关于PushKit实现的回答，加入了我自己的文件设定，以及代码的更新，以方便读者更简单便捷的实现PushKit的学习。


在iOS8中，苹果引进了一种可以用在VoIP应用中的新的消息推送，它可以在收到呼叫请求的时候唤醒你的应。通过这种新的推送方式，开发者将不需要再去设置`keep alive handler`来保持程序的后台场链接。新的推送将会在后台唤醒程序而不再需要程序一直运行在后台。总之，它将会减少数据流量的使用并且提高电池寿命。

这一切听起来非常棒，但是在看过官方的文档之后，我们并不能了解怎么去实现它。文档中说到：

`In iOS 8 and later, voice-over-IP (VoIP) apps register for UIRemoteNotificationTypeVoIP remote notifications instead of using this method.`


问题在于`UIRemoteNotificationTypeVoIP`并不存在。它不存在的原因在于Apple引入了新的推送框架`PushKit`来专门处理这一类别的推送。下面让我们看看如何将该框架运用到我们的应用中。


## 配置

在开始之前，我们需要做些准备。首先，我们需要一个VoIP Services certificate。我们需要到iOS开发者中心找到Certificates, Identifier & Profiles部分，可以看到如下截图。

<center><img src="/images/voip/image1.png" alt="" width="80%" /></center>

<center><img src="/images/voip/image2.png" alt="" width="80%" /></center>

为了能够接收到消息推送我们需要创建一个Identifier。可以通过导航到Identifiers部分，然后单击加号按钮。

我们还需要指定一个bundle id给你的应用程序，本例中，我们将使用`com.pengyu.voiptest`。先不用担心App Services这部分，我们将在后面补充。然后点击Continue，在下一个界面点击Register，完成。

<center><img src="/images/voip/image3.png" alt="" width="80%" /></center>


目前为止我们已经设置好了Identifier部分，然而我们仍需设置消息推送。打开Certificates界面，然后单击加号按钮，并选择VoIP Services Certificate如下：
image4


我们将会被要求选择一个app id，这里我们下拉菜单中选择我们之前刚刚创建的那个id，点击Continue。下一步要求并告诉我们如何生成一个Certificate Signing Request。

如何生成CSR文件：

打开Keychain Access，在Keychain Access下拉菜单中选择Keychain Access > Certificate Assistant > Request a Certificate from a Certificate Authority.

在证书信息窗口，在User Email Address输入你的邮件地址，在Common Name那里输入一个你的私钥的名字，保持CA Email Address空白。
在"Request is"那里, 选择"Saved to disk" 选项，然后点击Continue，保存CSR到本地文件夹。


下一步就是上传刚刚保存在本地的CSR，然后生存我们所需要的证书。一旦证书成功生产，结果将如下图所示：

<center><img src="/images/voip/image6.png" alt="" width="80%" /></center>

下载该证书并打开它`voip_services.cer`，这将会在Keychain Access应用中看到这个证书。目前我们完成了设置，下一步我们可以开始真正的工作了。找到你的certificate并点击证书旁边的小箭头，同时得到certificate和对应的key。
同时选中certificate和key，右击，选择Export 2 items…。 ［image7］
选择p12格式并命名文件为 certificate.p12。

将 `voip_services.cer` 和 `certificate.p12`文件保存在同一个文件夹下，方便我们一会儿生成模拟服务器端消息的推送。

最后，在Apple开发者中心找到Provisioning Profiles->Distribution 点击右上角加号，新建一个Ad-Hoc distribution profile。

<center><img src="/images/voip/image11.png" alt="" width="80%" /></center>


根据向导，选择你的APP ID，开发者账号，以及将要用于真机测试的设备的UDID，创建完成后下载该文件并将它拖拽到Xcode中，以便后面的使用。



## 实现：

打开Xcode并创建新的Single View Application iOS app。项目名称叫做voiptest并确保它的bundle identifier是`com.pengyu.voiptest`。bundle identifier非常重要，因为我们需要确保它的名称与我们的证书相同才可以发送推送。

添加PushkKit framework到我们的工程，General-> Linked Frameworks and Libraries.
在Build Settings -> Code Signing那里，provisioning profile选择刚才创建的AdHoc的profile，在Code Signing Identity选择Distribution模式。

在编写代码之前的最后一件事就是打开应用的Background Modes。我们可以在项目Target下的capabilities标签下找到。如下图所示：


在这个标签下有很多不同的选项，找到Background Modes并启用它。我们需要选定VoIP所需要的所有选项：
Audio and Airplay
Voice over IP
Remote notifications


现在我们开始向项目中添加代码。打开AppDelegate.swift，导入PushKit并注册通知。

  import UIKit
  import PushKit


  @UIApplicationMain
  class AppDelegate: UIResponder, UIApplicationDelegate {

      var window: UIWindow?
      let voipRegistry = PKPushRegistry(queue: dispatch_get_main_queue())


      func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
        // Override point for customization after application launch.
        //Enable all notification type. VoIP Notifications don't present a UI but we will use this to show local nofications later
        let notificationSettings = UIUserNotificationSettings(forTypes: [UIUserNotificationType.Alert, UIUserNotificationType.Badge, UIUserNotificationType.Sound] , categories: nil)

        //register the notification settings
        application.registerUserNotificationSettings(notificationSettings)

        //output what state the app is in. This will be used to see when the app is started in the background
        NSLog("app launched with state \(application.applicationState.stringValue)")
        return true
      }

      func applicationWillTerminate(application: UIApplication) {
        // Called when the application is about to terminate. Save data if appropriate. See also applicationDidEnterBackground:.
        //output to see when we terminate the app
        NSLog("app terminated")
      }

    }


这里我们使用一些Log输出来查看我们的应用是否正常工作。这将使我们在控制台观测到程序在后台运行时收到通知的反应。

我们使用了`registerUserNotificationSettings`方法，我们还需要实现它的delegate callback
 `application(application: UIApplication, didRegisterUserNotificationSettings notificationSettings: UIUserNotificationSettings)`.

在这个回调中我们会在用户同意接收推送的情况下注册VoIP通知推送。


extension AppDelegate {

    func application(application: UIApplication, didRegisterUserNotificationSettings notificationSettings: UIUserNotificationSettings) {

        //register for voip notifications
        NSLog("didRegisterUserNotificationSettings called")
        voipRegistry.desiredPushTypes = Set([PKPushTypeVoIP])
        voipRegistry.delegate = self;
    }
  }

我们刚刚在`voipRegistry`对象声明中启用了VoIP推送。初始化`PKPushRegistry`将使用一个GCD队列来决定它的delegates从哪里回调。这里我们使用主队列，因为在这个简单的测试中并没有什么关系。要想实现`voipRegistry`对象的作用，我们需要设置它的delegate为 `.self`。`voipRegistry`的delegate是一个具有三个方法的`PKPushRegistryDelegate`类型，其中两个是必须的，下面我们添加这些方法到我们的项目中。



  extension AppDelegate: PKPushRegistryDelegate {


    func pushRegistry(registry: PKPushRegistry!, didUpdatePushCredentials credentials: PKPushCredentials!, forType type: String!) {
        NSLog("didUpdatePushCredentials called")

        //print out the VoIP token. We will use this to test the nofications.
        NSLog("voip token: \(credentials.token)")
    }

    func pushRegistry(registry: PKPushRegistry!, didReceiveIncomingPushWithPayload payload: PKPushPayload!, forType type: String!) {

        NSLog("didReceiveIncomingPushWithPayload called")


        let payloadDict = payload.dictionaryPayload["aps"] as? Dictionary<String, String>
        let message = payloadDict?["alert"]

        //present a local notifcation to visually see when we are recieving a VoIP Notification
        if UIApplication.sharedApplication().applicationState == UIApplicationState.Background {
            NSLog("incoming notificaiton from background")

            let localNotification = UILocalNotification();
            localNotification.alertBody = message
            localNotification.applicationIconBadgeNumber = 1;
            localNotification.soundName = UILocalNotificationDefaultSoundName;

            UIApplication.sharedApplication().presentLocalNotificationNow(localNotification);
        }

        else {
            NSLog("incoming notificaiton from frontend")


            dispatch_async(dispatch_get_main_queue(), { () -> Void in


                let alertController = UIAlertController(title: "Title", message: "This is UIAlertController default", preferredStyle: UIAlertControllerStyle.Alert)
                let cancelAction = UIAlertAction(title: "Cancel", style: UIAlertActionStyle.Cancel, handler: nil)
                let okAction = UIAlertAction(title: "OK", style: UIAlertActionStyle.Default, handler: nil)
                alertController.addAction(cancelAction)
                alertController.addAction(okAction)

                UIApplication.sharedApplication().keyWindow?.rootViewController?.presentViewController(alertController, animated: true, completion: nil)

            })
        }

        NSLog("incoming voip notfication: \(payload.dictionaryPayload)")
    }

    func pushRegistry(registry: PKPushRegistry!, didInvalidatePushTokenForType type: String!) {
        NSLog("didInvalidatePushTokenForType called")
        NSLog("token invalidated")
    }
  }


我们不需要添加额外的代码到这些方法中去，但对于我们的测试，我们将使用`UILocalNotification`或者`UIAlertController`来显示收到了VoIP推送通知。我们还打印出`token`因为我们后面需要用它来测试我们的功能。好了，现在我们的程序将会在后台收到消息是重新打开应用程序了。

  extension UIApplicationState {

    //help to output a string instead of an enum number
    var stringValue : String {
        get {
            switch(self) {
            case .Active:
                return "Active"
            case .Inactive:
                return "Inactive"
            case .Background:
                return "Background"
            }
        }
    }
  }


## 测试

编写完代码之后，打包（Archive）项目并导出ipa文件，然后安装到测试设备上（可以通过iTunes安装）。

运行App，我们可以在devices viewer（Xcode > Window > Devices）中查看设备的Log：在左侧选择正确的设备并点击底部的log部分。
进入log之后可以使用 `Command＋F` 查找关键字app launched with state或voip token，应当得到类似如下的输出：

<center><img src="/images/voip/image12.png" alt="" width="80%" /></center>


复制你得到的token并将其保存在剪贴板上，后面会用到。

现在回到你存储证书的那个文件夹，应该包含有下面的文件：

voip_services.cer
certificate.p12

1 - 打开终端，通过certificate文件创建pem文件。

  #openssl x509 -in voip_services.cer -inform der -out PushVoipCert.pem

2 - 通过导出的private key文创建另一个pem文件，其过程中需要你创建一个密码用于验证。

  #openssl pkcs12 -nocerts -out PushVoipKey.pem -in certificate.p12

3 - 将两个文件和成为一个:

  #cat PushVoipCert.pem PushVoipKey.pem > ck.pem




下载Simplepush的[php脚本文件][sc]用于发送推送消息。修改`$deviceToken`为你的Token，`$passphrase`为刚刚生成PushVoipKey.pem是所设置的密码。例如：

  $deviceToken = 'b949d3784df30c2cf5c16aa24b494cb82c78acda986113b39e1b89b0a1f0d4bc';
  $passphrase = 'password';

如果你使用的是Simplepush的代码，还要修改下面这一行：
将`ssl://gateway.sandbox.push.apple.com:2195`改成`ssl://gateway.push.apple.com:2195`。
这是因为我们需要使用Apple的产品状态服务器地址而不是开发状态服务器地址。

这里贴一下simplepush.php的脚本代码：

  <?php

  // Put your device token here (without spaces):
  $deviceToken = 'b949d3784df30c2cf5c16aa24b494cb82c78acda986113b39e1b89b0a1f0d4bc';

  // Put your private key's passphrase here:
  $passphrase = 'password';

  // Put your alert message here:
  $message = 'My first push notification!';

  ////////////////////////////////////////////////////////////////////////////////

  $ctx = stream_context_create();
  stream_context_set_option($ctx, 'ssl', 'local_cert', 'ck.pem');
  stream_context_set_option($ctx, 'ssl', 'passphrase', $passphrase);

  // Open a connection to the APNS server
  $fp = stream_socket_client(
  	'ssl://gateway.push.apple.com:2195', $err,
  	$errstr, 60, STREAM_CLIENT_CONNECT|STREAM_CLIENT_PERSISTENT, $ctx);

  if (!$fp)
  	exit("Failed to connect: $err $errstr" . PHP_EOL);

  echo 'Connected to APNS' . PHP_EOL;

  // Create the payload body
  $body['aps'] = array(
  	'alert' => $message,
  	'sound' => 'default'
  	);

  // Encode the payload as JSON
  $payload = json_encode($body);

  // Build the binary notification
  $msg = chr(0) . pack('n', 32) . pack('H*', $deviceToken) . pack('n', strlen($payload)) . $payload;

  // Send it to the server
  $result = fwrite($fp, $msg, strlen($msg));

  if (!$result)
  	echo 'Message not delivered' . PHP_EOL;
  else
  	echo 'Message successfully delivered' . PHP_EOL;

  // Close the connection to the server
  fclose($fp);

  ?>


执行脚本文件

  #php simplepush.php

<center><img src="/images/voip/image14.png" alt="" width="80%" /></center>

如果一切顺利，你将会在设备端收到相应UILocalNotification或UIAlertAction的推送消息，并且能够在Log中看到相应函数调用的输出。例如：

<center><img src="/images/voip/image13.png" alt="" width="80%" /></center>

P.S. 文中截图会略有不同，截自voiptest和coolpush两个项目，只是项目名称不同，不影响整体的理解。


[pi]:http://pierremarcairoldi.com/ios-8-voip-notifications/
[mp]:http://stackoverflow.com/a/28562124/5500154
[sc]:http://d1xzuxjlafny7l.cloudfront.net/downloads/SimplePush.zip
