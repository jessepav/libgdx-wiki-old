# Summary #

 * [Introduction](#introduction)
 * [Configuration](#configuration)
 * [Recap](#recap)
 * [Banner](#banner)
 * [Test Ads](#test-ads)

# Introduction #

This article will show you how to add Smaato adds in your libGDX Android Games. You will be able to monetize your game with Banner, Interstitial and Rewarded Ads. This tutorial implies that you are already familiar with libGDX basics and how Android Views are handled. But there will be a short recap.
Just for a reference, the latest documentation is available on Smaato official [website](https://developers.smaato.com/publishers/nextgen-sdk-android-integration).

You will need:
1. A libGDX Android Game Project
2. Your [Smaato](https://spx.smaato.com/) account

# Configuration #
Add the following repository setup to your project’s main `build.gradle` file:
```java

buildscript {
    repositories {
        mavenCentral()
        google()
        jcenter()
        maven { url "https://s3.amazonaws.com/smaato-sdk-releases/" }
    }
}
...
allprojects {
    repositories {
        google()
        jcenter()
        maven {
            url "https://s3.amazonaws.com/smaato-sdk-releases/"
        }
    }
}
```
Set the compile options to Java 8 and minSdkVersion to at least version 16, in android module `build.gradle` file:
```java
android {
    defaultConfig {
        minSdkVersion 16
        ...
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

Add required dependencies to your application module `build.gradle` file under project(":android") --> dependencies.

If you want to add all Ad Types, then use:
`implementation 'com.smaato.android.sdk:smaato-sdk:21.5.3`

For Banner ads only:
`implementation 'com.smaato.android.sdk:smaato-sdk-banner:21.5.3'`

For Interstitial ads only:
`implementation 'com.smaato.android.sdk:smaato-sdk-interstitial:21.5.3'`

For Rewarded ads only:
`implementation 'com.smaato.android.sdk:smaato-sdk-rewarded-ads:21.5.3'`

If you’re using Proguard in your project, please add the following lines to your Proguard config file:
```java
-keep public class com.smaato.sdk.** { *; }
-keep public interface com.smaato.sdk.** { *; }
```
Add the following permissions to application AndroidManifest.xml file:
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```

If your application targets Android 5.0 (API level 21) or higher, then you need to add the following line to your application AndroidManifest.xml file:
```xml
<uses-feature android:name="android.hardware.location.network" />
```

# Recap #
LibGDX Android game is created as a [View](https://developer.android.com/reference/android/view/View). In order to add ads that are constantly displayed (like Banners) another view holding that should be used. In other words, a Relative Layout having both views should be created.
It will be shown in the following code snippet:

```java
public class AndroidLauncher extends AndroidApplication {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        RelativeLayout layout = new RelativeLayout(this);
        layout.setLayoutParams(new RelativeLayout.LayoutParams(MATCH_PARENT, MATCH_PARENT));
        layout.addView(createGameView());
        createAdView(layout);
        setContentView(layout);
    }

    private View createGameView() {
        View gameView = initializeForView(new MyGame(this), new AndroidApplicationConfiguration());
        RelativeLayout.LayoutParams params = new RelativeLayout.LayoutParams(MATCH_PARENT, WRAP_CONTENT);
        params.addRule(RelativeLayout.ALIGN_PARENT_TOP, RelativeLayout.TRUE);
        params.addRule(RelativeLayout.CENTER_HORIZONTAL, RelativeLayout.TRUE);
        gameView.setLayoutParams(params);
        return gameView;
    }
}
```
The only missing piece here is the `createAdView` method that will be created in the next step.

# Banner #
In order to add Banner ads we will use `com.smaato.sdk.banner.widget.BannerView`. As mentioned previously, the Banner view will reside the same Relative Layout as the Game view. 
```java
public class AndroidLauncher extends AndroidApplication {
    private static final String PUBLISHER_ID = "1100042525";
    private static final String BANNER_AD_SPACE_ID = "130635694";

    private BannerView smaatoBanner;

    private void createAdView(RelativeLayout layout) {
        SmaatoSdk.init(getApplication(), PUBLISHER_ID);
        createSmaatoBanner();
        layout.addView(smaatoBanner);
    }

    private void createSmaatoBanner() {
        smaatoBanner = new BannerView(this);

        RelativeLayout.LayoutParams params = new RelativeLayout.LayoutParams(MATCH_PARENT, WRAP_CONTENT);
        params.addRule(RelativeLayout.ALIGN_PARENT_BOTTOM, RelativeLayout.TRUE);
        params.addRule(RelativeLayout.CENTER_HORIZONTAL, RelativeLayout.TRUE);
        smaatoBanner.setLayoutParams(params);
        smaatoBanner.setBackgroundColor(Color.TRANSPARENT);
        smaatoBanner.setVisibility(View.INVISIBLE);
    }

    @Override
    public void onDestroy() {
        if (smaatoBanner != null) smaatoBanner.destroy();
        super.onDestroy();
    }

    public void showBannerAds() {
        runOnUiThread(() -> {
            smaatoBanner.setVisibility(View.VISIBLE);
            smaatoBanner.setEnabled(true);
            smaatoBanner.loadAd(BANNER_AD_SPACE_ID, BannerAdSize.XX_LARGE_320x50);
        });
    }

    public void hideBannerAds() {
        runOnUiThread(() -> {
            smaatoBanner.setVisibility(View.GONE);
            smaatoBanner.setEnabled(false);
        });
    }
}
```
Let me walk you through this code snippet:
* `BannerView smaatoBanner` is kept as an instance variable, so we can show and hide the ad
* `BannerView` should be always destroyed in order to release resources properly
*  The banner will be placed according to `RelativeLayout.LayoutParams params` rules
* `showBannerAds`/`hideBannerAds` are supposed to be called as per your game lifecycle. 
* `PUBLISHER_ID` and `BANNER_AD_SPACE_ID` can be used during testing. Please, refer to [Test Ads](#test-ads) section for all available test ads configurations. Just don't forget to change it to your production ones before release.

# Test Ads #
| Adspace ID    | Type               |
| ------------- |--------------------|
| 130626424     | Rich Media         |
| 130635694     | Static Image       |
| 130635706     | MRAID              |
| 130626426     | Rich Media / Video |
| 130626427     | Video              |
| 130626428     | Rewarded           |

Please, use Publisher Id 1100042525 with each of those adspaces.