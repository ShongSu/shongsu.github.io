---
layout: post
title: Update your Cordova-Android apk From CrossWalk to Non-CorssWalk / 升级从拥有Crosswalk的Cordova-Android的apk到没有Crosswalk的apk的解决办法
date: 2017-10-30 22:29
categories: [blog ]
tags: [Cordova, Android, CrossWalk]
description:
---


In our previous app development, we were using [Cordova][co] + [Crosswalk][xw] for a better webView. As of February 2017, Crosswalk is not being developed anymore. And since Android 7.0 added Chromium as their default web view, we don’t need Crosswalk anymore.

### Problem

We get ride of cordova crosswalk plugin `cordova-plugin-crosswalk-webview` and rebuild an update version (increased `android:versionCode` and `versionName`) of our current app, but it keeps showing Application Installation Failed. The error they provided us is `the device already has a newer version of this application`. It makes me confused since we de increased the version but why shows this error. It means we are not able to upgrade our current app to the newer version.

After few hours investigation, I found the reason and a potential solution for it.


### Reason

Let’s take a look at `build.gradle` at module level, there’s a piece of code shows:

```
if (Boolean.valueOf(cdvBuildMultipleApks)) {
        productFlavors {
            armv7 {
                versionCode defaultConfig.versionCode*10 + 2
                ndk {
                    abiFilters "armeabi-v7a", ""
                }
            }
            x86 {
                versionCode defaultConfig.versionCode*10 + 4
                ndk {
                    abiFilters "x86", ""
                }
            }
            all {
                ndk {
                    abiFilters "all", ""
                }
            }
        }
    }
```

The `defaultConfig.versionCode` comes from your android `AndroidManifest.xml` file which is also come from your condova’s `config.xml`

Such as:

`AndroidManifest.xml:  <manifest android:versionCode=“40001" …/>`

`Config.xml: <widget id=“xxx.xxx.xxx” version=“4.0.1" …/>`


Since crosswalk provided tow different CPU architectures: armV7 and x86, when you building and compiling your project, it will call for `build.gradle` to reset your `versionCode`.

For armv7, the final `versionCode` should be `versionCode*10 + 2`, in our example, it becomes to `400012`.

For x86, the final `versionCode` should be `versionCode*10 + 4`, in our example, it becomes to `400014`.

When we remove Corsswalk, there’s no more architectures options when building or generating an apk, so the final `versionCode` is exactly what we set in `AndroidManifest` and `config`, which is `40001` in our example.

Until now we learnt that why `the device already has a newer version of this application` is the versionCode of the app with crosswalk always more than ten times large than the app without crosswalk.

### Solution:

If you are going to update your version from 4.0.1 to 4.0.2, you have to change `android:versionCode=”40002”` to “40002x”, `x` can be any number from `0 to 9`, personally I attached  `0` at the end like `400020`. As long as the new `versionCode` is greater than the old one.






[co]: https://cordova.apache.org/
[xw]: https://github.com/crosswalk-project/crosswalk
