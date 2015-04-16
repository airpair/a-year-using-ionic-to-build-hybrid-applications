## Introduction

It is been exactly a year since we started using Ionic Framework here at [MateProfiler.com](https://mateprofiler.com/). At that time we researched a lot on technologies to create Hybrid mobile applications (applications made with Web standards that run inside of a native container). 

After some time digging we ended up having the choice between jQuery Mobile (which was already production ready) and an emerging framework based on AngularJs named Ionic (which was on beta2 and not production ready yet). 

Seeing that the community behind Ionic was already active and growing extensively we decided to give it a shot (coming from an AngularJS background also helped to choose).

Basing your product on Software that are not stable yet can be dangerous but fortunately for us, the Ionic team made an excellent work not to break too much things between releases and even added new features from time to time.

This post is about what we love about Ionic (UI, Build, Debug, Performance) and what we learnt (limitations) throughout our journey.

## UI

Ionic proposes dozens of UI components ready to use with extended options available. To have a clear understanding of the UI available, go check the Ionic [component page](http://ionicframework.com/docs/components/) it speaks for itself.

![Ionic ui](http://julienrenaux.fr/wp-content/uploads/2015/04/ionic_ui-e1429052586589.png)

> Collection of component

We have been amazed by the quality of these components, how they all interact with each other and specific platforms. 

### OS specific styles, behaviors and transitions

Since beta7 the application layout depends on the device you use. Ionic follows the platform specific guidelines to display elements in the right way (e.g the menu toggle button will be on the left on iOS and on the right on Android) 

Ionic does not only focus on specific layouts but also on specific transitions and behaviors (e.g on iOS ionic allow scrolling to bounce past the edges of the content by default but not on Android). 

Having specific styles, behaviors and transitions by default on each platform is really nice but having a way to overwrite them in a single place is even better. That is why beta14 introduced the ```$ionicConfigProvider```! 

During the configuration phase of your app (before the init) you can use ```$ionicConfigProvider``` to overwrite any specific style, behavior or transition. During runtime you can also use ```$ionicConfig``` to set and get config values within your controllers or services.

[More info](http://blog.ionic.io/platform-continuity/)

## Build

If you have any Cordova experience you know that building an application is far from being easy. Getting your machine ready to build is complicated due to platform specific prerequisites. For instance, to be able to build iOS application you must run OSX and have Xcode installed.

An alternative exists if you do want to take care of building your app. [Phonegap build](https://build.phonegap.com/) makes all the durty work for you but only accepts [official plugins](http://plugins.cordova.io/), which is great for most needs but limitating for big applications.

If you still want to build yourself, here are some advices:

### Prerequisite

First prerequisite to build hybrid applications is to have NodeJs and NPM running.

Then install Cordova globally:

```bash
sudo npm install -g cordova
```

Cordova allows you to add platforms, plugins, run and build your application. It is sufficient by itself but for following demonstration we will be using [Ionic CLI](https://www.npmjs.com/package/ionic) which is a Cordova decorator, meaning that you can use every command that Cordova proposes plus some more. 

```bash
sudo npm install -g ionic
```

Ionic CLI was in our opinion game changing. It drastically made everything easier: installation (e.g save plugin dependencies within package.json), build (e.g building with Crosswalk) and debug (e.g live reload, console logs).

### iOS

#### iOS Prerequisite

- OSX
- Xcode

If you do not have any Apple device you should install the iOS emulator, which is really good, or at least much better than the one for Android.

```bash
sudo npm install -g ios-sim
```

You are now ready to build your code for iOS:

```bash
# Add iOS platform for cordova
ionic platform add ios

# Build Debug version
ionic build ios

# Build Prod version
ionic build ios --release 

# Run your code within the emulator
ionic emulate ios

# Run your code on real device if connected
ionic run ios
```

### Android

#### Android Prerequisite

- Android SDK
- Ant

```bash
# MacOs
brew install ant

# Debian/Ubuntu
sudo apt-get install ant
```

- Having ANDROID_HOME environment variable set (in your .bashrc or .bash_profile file)


```bash
# Path to your sdk in your file system
export ANDROID_HOME=~/Library/Android/sdk 
```

On Android the emulator is part of the sdk so you do not need to download it.

You are now ready to build your code for Android:

```bash
# Add iOS platform for cordova
ionic platform add android

# Build Debug version
ionic build android

# Build Prod version
ionic build android --release 

# Run your code within the emulator
ionic emulate android

# Run your code on real device if connected
ionic run android
```

### Add plugins

Adding plugin and re-installing is really easy using Ionic. 

```bash
# add a toast plugin
ionic plugin add nl.x-services.plugins.toast
```

Adding plugins will automatically register the plugin within the ```package.json``` under ```cordovaPlugins``` key.

```json
 "cordovaPlugins": [
    "add nl.x-services.plugins.toast"
  ]
```

Now if you reinstall from scratch (when “platform” and “plugins” folders are missing) Ionic will be able to add plugins back.

### Splashscreens and Icons

If you have experienced developing hybrid applications, you know that creating splashscreens and icons is a pain! There are so many parameters that come into play:

* Width and Height
* Density
* OS
* Device
* Landscape / Portrait

If you want to correctly address those parameters you will end up creating a dozen splashscreens and icons per OS. By experience do not even try to do it yourself (e.g using Photoshop), it is a waste of time.

There are some online tools that can help you generating splashscreens and icons but Ionic is also here to help!

All you need to do is creating two files (either .psd, .ai or .png) within the ```resources``` directory (create if not exist). One named ```icon``` that needs to follow [this template](http://code.ionicframework.com/resources/icon.psd) and the other named ```splash``` that needs to follow [this template](http://code.ionicframework.com/resources/splash.psd)

Then run one of the following commands (depending on your needs):

```bash
# generate both icons and splashscreens
ionic resources

# generate only icons
ionic resources --icon

# generate only splashscreens
ionic resources --splash
```

It will upload your files to Ionic’s servers and create everything for you (create the correct platforms folders and even edit the ```config.xml``` file). 
## Debug

Debugging hybrid applications can be difficult if you do not know some technologies and tricks.

During this year we tried [Intel XDK], IDE (based on brackets). What it does is gathering most of your needs in one place. You have got the code editor (obviously), the emulator, a way to build your application through Intel’s servers and a [mobile application](https://software.intel.com/en-us/xdk/docs/intel-xdk-app-preview-overview) to preview your work directly on your mobile.

[Intel XDK](https://software.intel.com/en-us/html5/tools) sounded really promissing but turned out not to be mature enough for our needs. Fortunately for us Ionic was!

Ionic also provides you a lot of tools to debug easily:

### Ionic Serve

The ```ionic serve``` command starts a local development server (based on express) and allows live reload via websockets. It is exactly the same as running ```cordova serve```.

That being said Ionic is once again ahead of Cordova with the ```--lab``` option. Running ```ionic serve --lab``` will open your app in a browser, but it now shows you what your app will look like on a phone, with both iOS and Android side by side.

![Ionic serve --lab](http://julienrenaux.fr/wp-content/uploads/2015/04/ionic_serve_lab-e1429052761372.png)

### Live reload and Console logs

The ```run``` or ```emulate``` command will deploy the app to the specified platform devices/emulators. You can also run **live reload** on the specified platform device by adding the ```--livereload``` option. The live reload functionality is similar to ```ionic serve```, but instead of developing and debugging an app using a standard browser, the compiled hybrid app itself is watching for any changes to its files and reloading the app when needed.

With live reload enabled, an app's console logs can also be printed to the terminal/command prompt by including the ```--consolelogs``` or ```-c``` option.

### Ionic View

 [Ionic view](http://view.ionic.io/) is really similar to what XDK provides with it’s own “app preview” application. 

Once your application is ready to be tested, upload your app to Ionic’s servers and share it to your friends!

```bash
ionic login
ionic upload
ionic share EMAIL
```

This feature allows you to bypass app stores alfa or beta testing.

## Performance

Performance is a key player when creating hybrid apps. Indeed Webviews (technology that made hybrid app possible) are often neglected by OS constructors (Android and iOS are no exception. Though it is evolving, web views can now be updated via the Play store on Android) and that one of the reason behind the poor performance that we all noticed when using Hybrid apps.

Ionic once again tackled this very problematic by creating optimized UI components and embedding Crosswalk into their CLI.

### JavaScript

AngularJS is known as being slow when it comes to render (and watch) thousands of items. Rendering thousands of items is something really common on modern apps (e.g display a list of friends) that is why Ionic introduced the ```collection-repeat``` directive. 

It allows an app to show huge lists of items with much better performance than ```ng-repeat```. It is only possible because under the hood, Ionic only renders items that are currently visible and VOILA!

It means that on a phone screen that can fit eight items, only the eight items matching the current scroll position will be rendered.

```javascript,linenums=true
<ion-content>
  <ion-item collection-repeat="item in items">
    {{item}}
  </ion-item>
</ion-content>
```

### WebView

[Crosswalk](https://crosswalk-project.org/) allows you to build Android applications with an up to date chromium embeded! With the Crosswalk Project, you can:

* Develop around device fragmentation
* Provide a feature rich experience on all Android 4.x devices
* Easily debug with Chrome DevTools
* Improve the performance of your HTML, CSS, and JavaScript

Crosswalk is not easy to install, do not even try to do it yourself, Ionic is here to help! 

```bash
# add the Crosswalk browser to your Android project
ionic browser add crosswalk
```

If you would like to specify a different version of Crosswalk, run ```ionic browser list``` to see which browsers are available and what versions. Then install the version needed. 

```bash
ionic browser add crosswalk@version
```

Now building android will automatically embedded Crosswalk (adding 20Mb+ along the way)

## Limitations

### Mobile only

In case you wondered Ionic is only for **native/hybrid** mobile apps (Android, iOS, Windows phone, Chrome and even Apple Watch) and cannot be used to develop desktop applications. One of the reasons is the browser compatibility (Ionic does not support IE9 for instance, see the Grid system limitation below).

As for our product a desktop application was necessary, we ended up creating three projects. 
Common: Contains the services and all the controller logic that can be shared.
Web & Mobile: inherit Common, extend controllers and set its own views.

### Grid system

The grid system is not as fancy as Bootstrap 3 grid system. It is actually similar to Bootstrap 2 approach (without the 12 columns restriction).

It should be noted that Ionic’s grid system is using [CSS Flexible Box](http://www.w3.org/TR/css3-flexbox/) that is a pretty new CSS feature that [is not supported by old browsers](http://caniuse.com/#feat=flexbox) such as IE9.

Here is how you can create a row of five columns:

```html
<div class="row">
  <div class="col">.col</div>
  <div class="col">.col</div>
  <div class="col">.col</div>
  <div class="col">.col</div>
  <div class="col">.col</div>
</div>
```


Now if you want to have a particular behavior on mobile for example you could use the ```responsive-sm``` class:

```html
<div class="row responsive-sm">
  <div class="col">.col</div>
  <div class="col">.col</div>
  <div class="col">.col</div>
  <div class="col">.col</div>
  <div class="col">.col</div>
</div>
```

For most needs this grid system is good enough but we had problems when trying to have let’s say three columns on landscape tablet, two on portrait tablet and one on mobile. It cannot be made easily.

### Reinstallation

Ionic CLI is great to create and build projects but there is one thing that it does not support yet: ```--variable```. 

For instance Facebook cordova plugin requires to be install using ```APP_ID``` and ```APP_NAME``` variables:

```bash,
cordova plugin add https://github.com/Wizcorp/phonegap-facebook-plugin.git --variable APP_ID="123456789" --variable APP_NAME="myApplication"
```

These variables will not be stored in the ```package.json``` file which makes reinstallations impossible.

We ended up creating the following script to reinstall (OSX).

```bash,linenums=true
#!/bin/sh

set -e

# cleanup
rm -rf platforms/
rm -rf plugins/

read -p "Which platforms do you want to build? (android ios): " platforms
platforms=${platforms:-"android ios"}

# install platforms and plugin
echo "installing platforms "$platforms
cordova platform add $platforms

# reinstall specific plugins
cordova plugin add nl.x-services.plugins.toast@2.0.2
cordova plugin add nl.x-services.plugins.launchmyapp@3.2.2 --variable URL_SCHEME='mateprofiler'
cordova plugin add https://github.com/Wizcorp/phonegap-facebook-plugin.git --variable APP_ID="123456789" --variable APP_NAME="mateprofiler"
# etc

```

It does the job.!

## Conclusion

> Choosing Ionic turned out to be the best decision we made

Today Ionic has proved to be more than just a framework, it is a complete ecosystem that helps you develop, build and ship great Software.

Despite few problems Ionic is certainly the most advanced ecosystem to build hybrid apps and knowing that they created all of this in a year is amazing. 

Choosing Ionic turned out to be the best decision we made for our project [MateProfiler.com](https://mateprofiler.com/).
