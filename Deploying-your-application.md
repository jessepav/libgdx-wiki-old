###  ###

The mechanism to deploy your game differs between platforms. This article aims to articulate, what is necessary to deploy to each platform that libGDX officially supports:

* [Deploy to Windows/Linux/Mac](#deploy-to-windowslinuxmac-os-x)
* [Deploy to Android](#deploy-to-android)
* [Deploy to iOS](#deploy-to-ios)
* [Deploy Web](#deploy-to-htmljs)

# <a id="Deploy_to_Windows/Linux/Mac"></a>Deploy to Windows/Linux/Mac OS X ##
The easiest way to deploy to Windows/Linux/Mac is to create a runnable JAR in Eclipse, which includes all of your code, the code of libgdx and any extensions, and your assets. 

### As JAR file (via gradle)
`gradlew desktop:dist`

This will create a runnable JAR file located in the `desktop/build/libs/` folder. It contains all necessary code as well as all your art assets from the android/assets folder and can be run either by double clicking or on the command line via `java -jar jar-file-name.jar`. Your audience must have a JVM installed for this to work. The JAR will work on Windows, Linux and Mac OS X!

### As JAR file (via Eclipse)
To create a runnable JAR:

  1. Right click your desktop project, select "Export"
  2. In the next dialog, select "Java -> Runnable Jar", click "Next"
  3. In the next dialog, specify the launch configuration you use to start the game on the desktop from within Eclipse, and select the output JAR file name

### Alternative (modern) ways of deployment
Distributing java applications as JAR file can be very unhandy and prone to issues, as not every user can be expected to have the right JRE (or even any JRE) installed. Other ways of deployment are for example:

* A very convenient way to distribute java application is to just bundle an JRE. See this [[entry|Bundling a JRE]] on how to do this. (**This is the recommended way to distribute an application!**)
* Via electron, HTML5 applications can be deployed to desktop. See [here](https://medium.com/@bschulte19e/how-to-deploy-a-libgdx-game-with-electron-3f1b37f0c26e).
* Games can also be deployed as an [Applet](https://github.com/libgdx/libgdx/wiki/Deploying-as-an-Applet) (outdated, not recommended!)

# <a id="Deploy_to_Android"></a>Deploy to Android ##
### Via Gradle 
`gradlew android:assembleRelease`

This will create an unsigned APK file in the `android/build/outputs/apk` folder. Before you can install or publish this APK, you must [sign it](http://developer.android.com/tools/publishing/app-signing.html). The APK build by the above command is already in release mode, you only need to follow the steps for keytool and jarsigner. You can install this APK file on any Android device that allows [installation from unknown sources](http://developer.android.com/distribute/open.html#unknown-sources). 

### Via Eclipse
Deploying to Android takes some extra steps. This tutorial is done using Eclipse and the Android ADT plugin.
- Right click your Android project and select "Export"
- Select "Android -> Export Android Application"
- Make sure the project you want to export is your project, select next
- Select or create your keystore. If you are new to Android development, a keystore is used to sign your applications **and is required for putting your application on the market.** More information is available at: http://developer.android.com/tools/publishing/app-signing.html
- Select your alias to sign with and enter the password. If you don't have an alias or want to create a new alias, select create new key and select next.
- Now select the destination of where you want your app being exported to. This will export an .apk file in the selected directory capable of being uploaded to Google Play  and alternative app stores.
- Select finish

# <a id="Deploy_to_iOS"></a>Deploy to iOS ##
*This section assumes you're familiar with the basic deployment steps for iOS apps.*

### Via gradle

**Prerequisites:**
In order to upload the IPA to the app store, it must be signed with your distribution signature and linked to your provisioning profile. 
You can follow Apple's guide on [app store distribution](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html) to create provisioning profiles and certificates.
Once you have done that, you must define them in your root build.gradle file, in your IOS Project

```
project(":ios") {
    apply plugin: "java"
    apply plugin: "robovm"

    dependencies {
        // ...
    }

    robovm {
        iosSignIdentity = "[Signing identity name]"
        iosProvisioningProfile = "[provisioning profile name]"
        iosSkipSigning = false
        archs = "thumbv7:arm64"
    }
 }
```

- Your provisioning profile name is available in your developer portal (where you created your provisioning profile).
- Your Signing identity name is available in your keychain, under "My Certificates"

**Packaging:**
To create your IPA, run 

`gradlew ios:createIPA`
 
This will create an IPA in the `ios/build/robovm` folder that you distribute to the Apple App Store. 
To upload your app you will need to use the application loader within XCode (Xode->Open Developer Tool->Application loader) 

Note: as of iOS 11 instead of simply adding your icons into your data folder within your iOS project you need to include an asset Catalog.
If you do not include one, you can still submit your app but later you receive the following message:
> Dear developer,

> We have discovered one or more issues with your recent delivery for "". To process your delivery, the following issues must be corrected: Missing Info.plist value - A value for the Info.plist key CFBundleIconName is missing in the bundle ''. Apps that provide icons in the asset catalog must also provide this Info.plist key. For more information see http://help.apple.com/xcode/mac/current/#/dev10510b1f7. Once these issues have been corrected, you can then redeliver the corrected binary. Regards, The App Store team

To fix this, follow these [instructions to include an asset catalog](https://github.com/MobiVM/robovm/wiki/Howto-Create-an-Asset-Catalog-for-XCode-9-Appstore-Submission%3F)

### Via Eclipse
**Prerequisites:**
 * An OSX machine with Xcode installed (or the equivalent Hackintosh) and an Apple developer license account all established and paid for
 * Your app is configured in `itunesconnect.apple.com` with a version number that matches your `ios/robovm.properties` file
 * You have a matching distribution certificate set up at `developer.apple.com` per usual

On your build machine, open Xcode, go to `Preferences -> Accounts`, provide your login and choose *View Details*. Tap the "Recycle" button at bottom left. This will download all necessary certificates to a place that the RoboVM Eclipse plugin expects to find them. Click *Done* and exit Xcode.

> *Insert more here about tweaking the files: info.plist.xml, robovm.properties and robovm.xml.*

**Packaging:**
Go into Eclipse, right click on your `projectname-ios` and choose `RoboVM Tools -> Package for App-Store/Adhoc Distribution`.  In the resulting dialog, pick a directory where you wish the IPA and associated files to be placed, choose your signing identity (usually your company, not your machine's identifier), and the provisioning profile for this app (from `developer.apple.com`).

This will generate the `projectname.IPA` file. Now you are able to use the `Application Loader` to locate your IPA and submit it to iTunesConnect. It will appear under the Prerelease tab for your app, per usual. Then you can distribute to your TestFlight testers etc., per the usual Apple procedure.

### Additional guides

Deploying to iOS is relatively straight forward, see [here](https://medium.com/@bschulte19e/deploying-your-libgdx-game-to-ios-in-2020-4ddce8fff26c) if you're having difficulties. Take a look at [this post](https://medium.com/dev-genius/deploying-your-libgdx-game-to-ios-testflight-163cada0696b), if you want to deploy your iOS application to TestFlight.

# <a id="Deploy_to_HTML/JS"></a>Deploy Web
### Via Gradle
`gradlew html:dist`

This will compile your app to Javascript and place the resulting Javascript, HTML and asset files in the `html/build/dist/` folder. The contents of this folder have to be served up by a web server, e.g. Apache or Nginx. Just treat the contents like you'd treat any other static HTML/Javascript site. There is no Java or Java Applets involved!

When running the result, you might encounter errors like `Couldn't find Type for class ...`. To fix this, please see our wiki page [Reflection](https://github.com/libgdx/libgdx/wiki/Reflection) and include the needed classes/packages.

With Python installed, you can test your distribution by executing the following in the `html/build/dist` folder:

**Python 2.x**

`python -m SimpleHTTPServer`

**Python 3.x**

`python -m http.server 8000`

You can then open a browser to [http://localhost:8000](http://localhost:8000) and see your project in action.

With Node.js `npm install http-server -g` then `http-server html/build/dist` and browse at <http://localhost:8080>. [docs](https://github.com/indexzero/http-server)

With PHP you may type `php -S localhost:8000` and browse at <http://localhost:8080>. [docs](http://php.net/manual/en/features.commandline.webserver.php)

### Deploy to HTML/JS (via Eclipse)
Deploying to HTML/JS is straightforward for most cases.
  1. Right click your HTML project and select "Google -> GWT Compile"
  2. Keep the default settings and click compile
  3. A window will appear asking you to select the WAR directory. It is in your HTML project's folder.
  4. GWT will proceed to cross-compile your code into javascript. It will take awhile, but you can view the progress by pulling up the console in Eclipse.

Once the compile is complete everything you need to run your game on the web will be inside the WAR directory. Unless you are using RPC calls or other server side operations in your game, there are only a few things left to do.
  1. Copy the "index.html" file, your assets folder, and the folder ending in "mygdxgame.GWTDefinition" to a directory on your web server
  2. Navigate to the URL and you should see a loading screen for your game

Notes:
  * If you are using server-side operations in your code, you will need to install Tomcat or similar software on your web server and place the full contents of your project's WAR directory in the "webapp" directory. More details [here.](https://tomcat.apache.org/tomcat-6.0-doc/appdev/deployment.html)  