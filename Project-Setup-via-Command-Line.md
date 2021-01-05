## Creating a libGDX project using the command line

This section is to create your project from the command line. This is not required if you use the wizard above.
IF you run it from the command line, specify the following arguments.

* **dir**: the directory to write the project to, relative or absolute
* **name**: the name of the application, lower-case with minuses is usually a good idea, e.g. mygame
* **package**: the Java package under which your code will live, e.g. com.badlogic.mygame
* **mainClass**: the name of the main ApplicationListener of your app, e.g. MyGame
* **sdkLocation**: the location of your android sdk, Intellij uses this if ANDROID_HOME is not set
* **excludeModules**: the modules to exclude (Desktop; Android; iOS; HTML) separated by ';' and not case sensitive, e.g. Android;ios. Optional. Default it includes all the modules
* **extensions**: the extensions to include (same name as in GUI: Bullet; Freetype; Tools; Controllers; Box2d; Box2dlights; Ashley; Ai) separated by ';' and not case sensitive, e.g. box2d;box2dlights;Ai. Optional

Putting it all together, you can run the project generator on the command line as follows:

`java -jar gdx-setup.jar --dir mygame --name mygame --package com.badlogic.mygame --mainClass MyGame --sdkLocation mySdkLocation [--excludeModules <modules>] [--extensions <extensions>]`