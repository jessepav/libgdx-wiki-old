[Google Play Game Services](https://developers.google.com/games/services/) offers cross platform social leaderboards, achievements, and much more (realtime multiplayer, cloud saves, anti-piracy...)

## Android Integration example

A [Super Jumper](https://github.com/libgdx/libgdx-demo-superjumper) based example that makes use of Leaderboards and Achievements is available to download from [Google Play](https://play.google.com/store/apps/details?id=com.theinvader360.tutorial.libgdx.gameservices).

The project is freely available on [GitHub](https://github.com/TheInvader360/libgdx-gameservices-tutorial), and a companion tutorial is available [here](http://theinvader360.blogspot.co.uk/2013/10/google-play-game-services-tutorial-example.html).

Another in-depth LibGDX-based tutorial for adding Google Play Game Services can be found [here](http://fortheloss.org/tutorial-set-up-google-services-with-libgdx/).

Latest tutorial using Android Studio can be found [here](https://chandruscm.wordpress.com/2015/12/30/how-to-setup-google-play-game-services-in-libgdx-using-android-studio/)

### Intellij and Android Studio Setup

1. Install Google Play Service and Google Play Repository using and Android SDK

2. Download BaseGameUtils sample project [here](https://github.com/playgameservices/android-basic-samples) and place it in your project root folder.

3. Edit settings.gradle 
```
include 'desktop', 'android', 'ios', 'html', 'core', "BaseGameUtils"
```
4. Edit root build.gradle and add the below as android dependency: 
```
compile project(":BaseGameUtils")
```

## iOS integration

There are two ways (called `Backend`) to integrate Google Play Games Services with iOS depending whether your are using the open source, community supported [Mobidevelop's RoboVM and its Robopods](https://github.com/MobiDevelop/robovm) or [Intel's Multi-OS Engine](https://software.intel.com/en-us/node/633261) 

### Mobidevelop's RoboVM and its Robopods

Please read the specific page for more information on [Mobidevelop's RoboVM and its Robopods](https://github.com/MobiDevelop/robovm)

In the meantime, you can use the following [Simple LIBDX Google Play Games Services integration for iOS to get you started](https://github.com/julienvillegas/libgdx-GPGS)

note: Until early 2016, LibGDX iOS integration was achieved using [RoboVM.com](robovm.com). This has been deprecated. Be careful when you check examples on the Internet as older examples may be based on this version.
The easy way to find out which version is being referred to:
* Supported version will have `com.mobidevelop.robovm` in the buid.gradle file
* Deprecated examples will have `org.robovm:robovm` in the buid.gradle file   

### Intel's Multi-OS Engine

Checkout the specific page for more information on [Intel's Multi-OS Engine](https://software.intel.com/en-us/node/633261). 

In the meantime, you can check out the following sample to get you started:

[Splinter Sweets](https://github.com/reime005/splintersweets) is a Kotlin based example that makes use of Leaderboards. It is available on Android and iOS (Gamecenter and Google Play Services integration).


