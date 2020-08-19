This article will show you how you can run your application from the command line and package it for distribution for the different platforms!
## Contents
* [**Prerequisites**](#setting-up-android_home) 
  * [Setting the ANDROID_HOME variable](#setting-up-android_home)
* [**Running**](#running-the-project)
  * [Running on Desktop](#running-the-desktop-project)
  * [Running on Android](#running-the-android-project)
  * [Running on iOS](#running-the-ios-project)
  * [Running HTML](#running-the-html-project)
* [**Packaging**](#packaging-the-project) 
* [**Debugging/Problems**](#debugging-and-common-problems) 
  * [Gradle tasks failing](#gradle-tasks-are-failing)
  * [Common problems](#common-problems)
  * [Debugging projects](#debugging-projects)
* [**Tweaking**](#tweaking)

***

## Setting up ANDROID_HOME
The ANDROID_HOME environment variable needs to be pointing to a valid android SDK before you do any command line wizardry.

Windows: `set ANDROID_HOME=C:/Path/To/Your/Android/Sdk`

Linux, Mac OS X: `export ANDROID_HOME=/Path/To/Your/Android/Sdk`

Alternatively you can create a file called "local.properties" with the following content: `sdk.dir /Path/To/Your/Android/Sdk`

## Running the project
Gradle let's you easily run a project from the commandline. Just use the gradlew command specify your target platform and the run command for that platform.

[**Desktop**](#running-the-desktop-project) - [**Android**](#running-the-android-project) - [**iOS**](#running-the-ios-project) - [**HTML**](#running-the-html-project) 

### Running the desktop project
`gradlew desktop:run`

This compiles your core and desktop project, and runs the desktop starter. The working directory is the project's assets folder!

### Running the Android project
`gradlew android:installDebug android:run`

This task will create a debug APK of your application, install it on the first connected emulator or device and start the main activity. The process is split into two tasks because the Android Gradle plugin lets you create multiple flavors of your app (e.g. debug as above, release, ...). You can find more information on the [Android Gradle Plugin site](http://tools.android.com/tech-docs/new-build-system/user-guide).

### Running the iOS project
Before you run on an iOS simulator or device, make sure you have [activated your free RoboVM key](https://github.com/libgdx/libgdx/wiki/Setting-up-your-Development-Environment-(Eclipse,-Intellij-IDEA,-NetBeans)#setup-for-ios-development). If you run on a device, you need to [provision](http://docs.robovm.com/getting-started/provisioning.html) it to be able to deploy to it!

`gradlew ios:launchIPhoneSimulator`

`gradlew ios:launchIPadSimulator`

`gradlew ios:launchIOSDevice`

The first two commands will launch your app on an iPhone or iPad simualtor, the last command will launch your ios project on a connected iDevice, provided it is provisioned. Please refer to Apple's docs on how to provision a device. Note that the first time you run your iOS project, the compilation will take a long time. Compilation time will decrease significantly on subsequent runs!

### Running the HTML project
`gradlew html:superDev`

This will start your application in [GWT Super Dev Mode](http://www.badlogicgames.com/wordpress/?p=3073), which compiles your Java code to Javascript, and allows you to debug your Java code directly in the browser. If you see the message `Next, visit: http://localhost:9876` in your shell, open a browser and navigate to the address. Drag the "Dev Mode On" bookmarklet to your browser bookmarks bar. Next open [http://localhost:8080/index.html](http://localhost:8080/index.html). This is your application running in the browser! If you change any of your Java code in the core project, just click the bookmarklet, then click "Compile". The changes will take effect in a few seconds. If you modify your assets, you have to restart the server with the above command.

## Packaging the project
Every platform has a different kind of distribution format. How a libGDX app can be distributed via gradle is detailed [here]().

## Debugging and common problems
### Gradle tasks are failing
If when you invoke gradle, the build or refresh fails to get more information, run the same command again and add the --debug arguments to the command.
e.g
```./gradlew tasks --debug```
This will provide you with a stacktrace and give you a better idea of why gradle is failing.

### Common problems
(Confirmed) AVG - When running gradlew desktop:dist antivirus will cause this to fail. Add an exception to your antivirus to allow access.
### Debugging Projects
Start Gradle with the --debug-jvm parameter, and attach your debugger to the right port (5005 typically).
```./gradlew desktop:run --debug-jvm```

## Tweaking

You may be like me and wish to have the output jar from the dist task. Gradle seems to name it as the name of the directory and a version number, which is in my case, desktop-1.0. You may also wish to have each build have a unique version/build date of some kind, do as follows:

In the root project build.gradle, add:

    def getDate() {
        def date = new Date()
        def formattedDate = date.format('yyyyMMddHHmmss')
        return formattedDate
    }

(at top level)

Within allprojects{}, add: 'version = "0.1-build-" + getDate()'

Within the desktop target add this within task dist(type: Jar) { }...

'baseName = "myproject"'

I'm sure there are better ways, but it took me a while to get together that info and then get it to work/figure out where to put it. You will now have a jar file in your desktop/libs, named something like 'project-0.1-build-20150120033412.jar' which makes distribution a lot easier and more trackable, less conflicts etc.