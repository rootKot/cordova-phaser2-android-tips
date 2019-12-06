# cordova-phaser2-android-tips
After hours of headache with setuping cordova let me save some tips for myself, and maybe it will be helpfull also for you :)

# Preparing to install cordova
1. Java Development Kit 8 (JDK)
https://www.oracle.com/java/technologies/jdk8-downloads.html
2. Android Studio
https://developer.android.com/studio?pkg=studio

Note: Be sure that PATHs is setted
Windows - https://evothings.com/doc/build/cordova-install-windows.html

From Android Studio -> Sdk manager installing android versions. I setuped 28v.

# Cordova
"""
npm install -g cordova
cordova create projectDirectory com.companyName.appName AppName
"""

lets move to installed project directory and add android platform
"""
cordova platform add android
"""

After that, we can place our web source into 'www' directory, and try to build.
"""
cordova build android
"""
If you facing the error that the taget is not specified then specifie it in platforms/android/build.gradle

In my case my game on phaser2 was realy bad performance, so I decided to change default WebView to ChromeView.

# cordova-plugin-crosswalk-webview
This is a cordova plugin for solving our issue.
https://github.com/crosswalk-project/cordova-plugin-crosswalk-webview
But as my cordova version is 9 this plugin wont work for me, so I found forked one which is solves my issue.
https://github.com/ardabeyazoglu/cordova-plugin-crosswalk-webview-v3
Lets add it as plugin
"""
cordova plugin add cordova-plugin-crosswalk-webview-v3
"""

After that I had issue something like "the minimum SDK version is setted 16" (actually don't remember the error :) )
So I found all places where minSdkVersion was 16 and changed to 19.

## IMPORTANT (what if performance still sucks?)
For good performace we need add --ignore-gpu-blacklist attibute in our config.xml
"""
<preference name="xwalkCommandLine" value="--ignore-gpu-blacklist" />
"""

## If your configurations not make changes
Try to set performance options directly in /platforms/android/android.json

# Lets lock orientation and make it fullscreen without status bar and buttons!
https://github.com/apache/cordova-plugin-screen-orientation
"""
cordova plugin add cordova-plugin-screen-orientation
"""
https://github.com/apache/cordova-plugin-statusbar
"""
cordova plugin add cordova-plugin-statusbar
"""

and in www/index.html adding <script> tag with the following code
"""
  document.addEventListener("deviceready", onDeviceReady, false);
  document.addEventListener("resume", onResume, false);
  function onDeviceReady() {
        screen.orientation.lock('landscape');
        StatusBar.hide();
  }
  function onResume() {
        StatusBar.hide();
  }
"""
Also do not forget to add cordova.js before your script
  <script type="text/javascript" src="cordova.js"></script>

And for fullscreen adding this preference:
"""
<preference name="Fullscreen" value="true" />
"""

# Finnaly my XML configurations look like this
"""
    <preference name="webView" value="org.crosswalk.engine.XWalkWebViewEngine" />
    <preference name="xwalkVersion" value="23+" />
    <preference name="xwalkLiteVersion" value="xwalk_core_library_canary:17+" />
    <preference name="xwalkCommandLine" value="--ignore-gpu-blacklist" />
    <preference name="android-hardwareAccelerated" value="true" />
    <preference name="xwalkMode" value="embedded" />
    <preference name="xwalkMultipleApk" value="false" />
    <preference name="android-minSdkVersion" value="19" />
    <preference name="Fullscreen" value="true" />
    <preference name="StatusBarOverlaysWebView" value="true" />
    <preference name="Orientation" value="landscape" />
"""

## Conclusion
Mainly my problem was with performance. And only thing that I needed was adding crossover-v3 plugin and set xwalk to --ignore-gpu-blacklist. After that everything magicly works fantastic! 
