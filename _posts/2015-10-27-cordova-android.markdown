---
layout: post
title: Cordova-Android Plugin Example / Cordova-Android 插件详解
date: 2015-10-27 00:19
categories: [blog ]
tags: [Cordova, Android, ]
description:
---

<center>**------English (英文)------**</center>


This is sample cordova plugin (currently for android platform only). Tested with Cordova 5.3.3 + Android 5.1.1

You can get the original codes from [here][cd].

How to run example?
-------------------

Create a new cordova project, for example:

	cordova create PluginTest com.shongsu.test PluginTest

Go into the directory you just created.

	cd PluginTest

Add Android platform.

	cordova platform add android

Add this plugin to your cordova project, type:

	cordova plugin add https://github.com/shongsu/cordova-example-plugin.git

  or clone this git and add it locally, for example:

	cordova plugin add file-path-here

Eidt your `www/index.html` file, here is full example:


	<!DOCTYPE html>
    	<html>
        <head>
            <meta charset="utf-8" />
            <title>Cordova - Android Plugin Example</title>

            <script type="text/javascript" src="cordova.js"></script>
            <script type="text/javascript" src="plugins/SamplePlugin.js"></script>

            <script type="text/javascript">
                function onLoad() {
                    document.addEventListener('deviceready', onDeviceReady, false);
                }

                function onDeviceReady() {
            var a = 5;
            var b = 3;
                    window.func_add(a, b, function(result) { alert(a + " + " + b + " = " + result); }, function(err) { alert(err); });
            window.func_sub(a, b, function(result) { alert(a + " - " + b + " = " + result); }, function(err) { alert(err); });
            window.func_factorial(a, function(result) { alert(a + "! = " + result); }, function(err) { alert(err); });		
          }
            </script>
        </head>

        <body onload="onLoad()">
            <h1>Cordova - Android Plugin Example</h1>
        <p>
        1. a+b; <br/>
        2. a-b; <br/>
        3. a!;
        </p>

        </body>
	</html>

Run application.

  In this example, page shuld alert "5 + 3 = 8", "5 - 3 = 2" and "5! = 120".




How it works?
-------------

We have **three files**: native code (.java), javascript code which exposes native code to cordova box (.js) and plugin manifest file (.xml). The structure looks as

        ExamplePlugin
        │  plugin.xml
        │
        ├─src
        │  └─android
        │       ExamplePlugin.java
        │
        └─www
            ExamplePlugin.js
<br />

Let's take a look at plugin files:

`src/android//ExamplePlugin.java` contains native code which executes outside of your cordova box.
<br />
`www/ExamplePlugin.js` connects native code with javascript - declares javascript functions `func_add`, `func_sub` and `func_factorial`.
<br />
`plugin.xml` contains info about plugin itself and paths to java and js files.


ExamplePlugin.java
-----------------

        package com.shongsu.plugin;

        import org.apache.cordova.CordovaPlugin;
        import org.apache.cordova.CallbackContext;
        import org.apache.cordova.PluginResult;

        import org.json.JSONArray;
        import org.json.JSONException;
        import org.json.JSONObject;

        /**
         * This class performs sum called from JavaScript.
         */
        public class ExamplePlugin extends CordovaPlugin {
            @Override
            public boolean execute(String action, JSONArray args, CallbackContext callbackContext) throws JSONException {
                if (action.equals("func_add")) {
                    Integer num1 = args.getInt(0);
                    Integer num2 = args.getInt(1);
                    this.func_add(num1, num2, callbackContext);
                    return true;
                } else if (action.equals("func_sub")) {
                    Integer num1 = args.getInt(0);
                    Integer num2 = args.getInt(1);
                    this.func_sub(num1, num2, callbackContext);
                    return true;
                } else if (action.equals("func_factorial")) {
                    Integer num1 = args.getInt(0);
                    this.func_factorial(num1, callbackContext);
                    return true;
                }

                return false;
            }

            private void func_add(Integer num1, Integer num2, CallbackContext callbackContext) {
                if(num1 != null && num2 != null) {
                    callbackContext.success(num1 + num2);
                } else {
                    callbackContext.error("Expected two integer arguments.");
                }
            }

            private void func_sub(Integer num1, Integer num2, CallbackContext callbackContext) {
                if(num1 != null && num2 != null) {
                    int s = num1 - num2;
                    callbackContext.success(s);
                } else {
                    callbackContext.error("Expected two integer arguments.");
                }
            }

            private void func_factorial(Integer num1, CallbackContext callbackContext) {
                if(num1 != null && num1 >= 0) {
                    int result = 1;
                    if (num1 == 0 || num1 ==1) {
                      result = 1;
                    } else {
                      for(int i=1;i<=num1;i++)
                      {
                        result *= i;
                      }
                    }

                    callbackContext.success(result);
                } else {
                    callbackContext.error("Expected a non-negative integer argument.");
                }
            }

        }



First method `execute` is the same in every plugin, it overrides standard method of CordovaPlugin class and allways has the same parameters:

- Parameter `action` is string containing name of method to execute.
- `args` is array containing variable number of arguments
- `callbackContext` has `.success` and `.error` methods which are called when your function finishes or in case of error.

Note that our plugin class can have many methods, but just one `execute` function. We test `action` argument to see which method we should call.
<br />

Next method `func_add` is our example function: it receives two integers and callbackContext. In case of error it calls `callbackContext.error` with error message string, or in case of success it calls `callbackContext.success` with result.

Same as method `func_sub` and `func_factorial`.

ExamplePlugin.js
---------------

    window.func_add = function(num1, num2, successCallback, errorCallback) {
    	cordova.exec(successCallback, errorCallback, "ExamplePlugin", "func_add", [num1, num2]);
    };

    window.func_sub = function(num1, num2, successCallback, errorCallback) {
    	cordova.exec(successCallback, errorCallback, "ExamplePlugin", "func_sub", [num1, num2]);
    };

    window.func_factorial = function(num1, successCallback, errorCallback) {
    	cordova.exec(successCallback, errorCallback, "ExamplePlugin", "func_factorial", [num1]);
    };

In this file we extend `window` object with function `func_add` (for example) which calls `cordova.exec`. Cordova translates that and calls `execute` method from our class declared in ExamplePlugin.java.

Arguments to `exec` are allways the same: two callback functions (success and error callback), plugin class name, action name and array of arguments.
You can pass any number of arguments - they are processed by execute function as described.

plugin.xml
----------

        <?xml version="1.0" encoding="UTF-8"?>
        <plugin xmlns="http://apache.org/cordova/ns/plugins/1.0"
            id="com.shongsu.plugin.ExamplePlugin"
            version="1.0.0">

            <name>ExamplePlugin</name>

          <description>
            Cordova-Android Plugin Example
          </description>

          <js-module src="www/ExamplePlugin.js" name="ExamplePlugin">
                <clobbers target="ExamplePlugin" />
            </js-module>

          <engines>
            <engine name="cordova" version=">=3.0.0" />
          </engines>

          <!-- android -->
          <platform name="android">
            <config-file target="res/xml/config.xml" parent="/*">
              <feature name="ExamplePlugin">
                  <param name="android-package" value="com.shongsu.plugin.ExamplePlugin"/>
              </feature>
            </config-file>

            <source-file src="src/android/ExamplePlugin.java" target-dir="src/com/shongsu/plugin/ExamplePlugin" />
          </platform>

          <!-- more platforms here -->

        </plugin>


This file tells cordova how to deal with plugin. Package name, class name and (relative) paths to files are defined here. More details about [plugin.xml][h].



For more details refer:

[Cordova Plugin Development Guide][cp] and [Android Plugins][ap]

[h]: https://cordova.apache.org/docs/en/5.1.1/config_ref/index.html
[ap]:http://docs.phonegap.com/en/3.4.0/guide_platforms_android_plugin.md.html#Android%20Plugins
[cp]:http://docs.phonegap.com/en/3.4.0/guide_hybrid_plugins_index.md.html#Plugin%20Development%20Guide
[cd]:https://github.com/ShongSu/CordovaPluginExample
