In order to get started you must setup your development environment. LibGDX supports the most popular Integrated Development Environment (IDE). Pick the one you are most familiar with:
* [Eclipse](https://github.com/libgdx/libgdx/wiki/Setting-up-your-Development-Environment-(Eclipse,-Intellij-IDEA,-NetBeans)#setting-up-eclipse)
* [Android Studio](https://github.com/libgdx/libgdx/wiki/Setting-up-your-Development-Environment-(Eclipse,-Intellij-IDEA,-NetBeans)#setting-up-android-studio)
* [Intellij IDEA](https://github.com/libgdx/libgdx/wiki/Setting-up-your-Development-Environment-(Eclipse,-Intellij-IDEA,-NetBeans)#setting-up-intellij-idea)
* [NetBeans](https://github.com/libgdx/libgdx/wiki/Setting-up-your-Development-Environment-(Eclipse,-Intellij-IDEA,-NetBeans)#setting-up-netbeans)


### Setting up Eclipse 
Please note that Eclipse has been [deprecated for Android development by Google](https://androiddevelopers.googleblog.com/2015/06/an-update-on-eclipse-android-developer.html). 
To develop your application via Eclipse, you need to install the following pieces of software.

  * [Java Development Kit 7+ (JDK) (6 will not work!)](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
  * [Eclipse](http://www.eclipse.org/downloads/), the "Eclipse IDE for Java Developers" is usually sufficient.
  ** [Eclipse for Android Developers](http://www.eclipse.org/downloads/packages/eclipse-android-developers-includes-incubating-components/neonr), Contains everything necessary to run described here.
  * [Android SDK](http://developer.android.com/sdk/index.html), you only need the SDK (available at the bottom of the page in the 'command line tools' section), not the whole Android Studio package which is a customized version of Intellij bundled with the Android SDK. Install the latest stable platform via the [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html). If you install the ADT Plugin as described below, then you can use the Android SDK Manager that comes with the plugin instead, to simplify this process. 
  * [Android Development Tools for Eclipse](http://developer.android.com/tools/sdk/eclipse-adt.html), aka ADT Plugin. Use this update site: https://dl-ssl.google.com/android/eclipse/
  * [Eclipse Integration Gradle](https://github.com/spring-projects/eclipse-integration-gradle/), use this update site: http://dist.springsource.com/milestone/TOOLS/gradle or if you feel brave  http://dist.springsource.com/snapshot/TOOLS/gradle/nightly (for Eclipse 4.4) or http://dist.springsource.com/release/TOOLS/gradle (for Eclipse < 4.4)

#### To additionally target iOS

  * A Mac with Mac OS X
  * The latest Xcode 7 from the Mac OS X App Store for free
  * An [Apple developer account](https://developer.apple.com/membercenter/index.action) if you want to test on device or deploy to the App Store. Please direct all hatred towards Apple.
  * The latest release of [MobiDevelop's RoboVM plugin](http://robovm.mobidevelop.com). (Microsoft's legacy RoboVM has been replaced by a version supported by LibGDX's core contributors)

Once all of these tools are installed, proceed to [[creating your project|Project Setup Gradle]]

### Setting up Intellij IDEA
To develop your application via Intellij IDEA, you need to install the following pieces of software.

  * [Java Development Kit 7+ (JDK) (6 will not work!)](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
  * [Intellij IDEA](http://www.jetbrains.com/idea/download/), the Community edition is sufficient.
  * [Android SDK](http://developer.android.com/sdk/index.html), you only need the SDK (available at the bottom of the page in the 'command line tools' section), not the whole Android Studio package which is a customized version of Intellij bundled with the Android SDK. Install the latest stable platform via the [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html). You also have to create an environment variable called ANDROID_HOME, which points at your Android SDK installation directory!

#### To additionally target iOS

  * A Mac with Mac OS X 
  * An [Apple developer account](https://developer.apple.com/membercenter/index.action) if you want to test on device or deploy to the App Store. Please direct all hatred towards Apple.
  * The latest XCode, which you can get from the Mac OS X App Store for free
  * The latest release of [MobiDevelop's RoboVM plugin](http://robovm.mobidevelop.com). (Microsoft's legacy RoboVM has been replaced by a version supported by LibGDX's core contributors). 

##### Setting up Intel Multi-OS Engine
As an alternative to MobiDevelop's RoboVM fork, libGDX also provides a backend for Intel's Multi-OS Engine. You can learn how to install it [here](https://multi-os-engine.org/start/).

Once all of these tools are installed, proceed to [[creating your project|Project Setup Gradle]]

### Setting up Android Studio
To develop your application via Android Studio, you need to install the following pieces of software.

  * [Java Development Kit 7+ (JDK) (6 will not work!)](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
  * [Android Studio](https://developer.android.com/sdk/index.html) already comes packaged with the Android SDK so contrary to Eclipse or Intellij IDEA you do not need to install this component.

#### To additionally target iOS

  * A Mac with Mac OS X 
  * An [Apple developer account](https://developer.apple.com/membercenter/index.action) if you want to test on device or deploy to the App Store. Please direct all hatred towards Apple.
  * The latest XCode, which you can get from the Mac OS X App Store for free
  * The latest release of [MobiDevelop's RoboVM plugin](http://robovm.mobidevelop.com). (Microsoft's legacy RoboVM has been replaced by a version supported by LibGDX's core contributors). Use the Intellij IDEA's release, Android Studio is based on it.

##### Setting up Intel Multi-OS Engine
As an alternative to MobiDevelop's RoboVM fork, libGDX also provides a backend for Intel's Multi-OS Engine. You can download it [here](https://registrationcenter.intel.com/en/forms/?productid=2586). During installation you can install a plugin for IntelliJ IDEA or Android Studio. Intel MOE currently doesn't support Eclipse.

Once all of these tools are installed, proceed to [[creating your project|Project Setup Gradle]]

### Setting up NetBeans
To develop your application via NetBeans, you need to install the following pieces of software.

  * [Java Development Kit 7+ (JDK) (6 will not work!)](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
  * [NetBeans 7.3+](https://netbeans.org/downloads/), the "Java SE" is sufficient
  * [Android SDK](http://developer.android.com/sdk/index.html), you only need the SDK (available at the bottom of the page in the 'command line tools' section), not the whole Android Studio package which is a customized version of Intellij bundled with the Android SDK. Install the latest stable platform via the [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html). You also have to create an environment variable called ANDROID_HOME, which points at your Android SDK installation directory!
  * [NBAndroid](http://www.nbandroid.org), use this update center: http://nbandroid.org/updates/updates.xml (if you have NetBeans 8.1 or higher, use http://nbandroid.org/release81/updates/updates.xml ).
  * [Gradle Support for NetBeans](https://github.com/kelemen/netbeans-gradle-project), use the NetBeans IDE Update Center.

To additionally target iOS

  * A Mac, iOS Development does not work on Windows/Linux thanks to Apple.
  * The latest XCode, which you can get from the Mac OS X App Store for free

Once all of these tools are installed, proceed to [[creating your project|Project Setup Gradle]]