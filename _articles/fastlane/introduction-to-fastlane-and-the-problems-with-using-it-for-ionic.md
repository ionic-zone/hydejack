---
title: 
published: true
date: 2017-08-29 16:00:00 +0000
last_updated: ''
---
# Introduction to Fastlane and possible problems using it for Ionic Cordova projects

## Fastlane?
[Fastlane](https://fastlane.tools/) describes itself as 

> [...] the tool to release your iOS and Android app 🚀 It handles all tedious tasks, like generating screenshots, dealing with code signing, and releasing your application. 

or 

> The easiest way to automate building and releasing your iOS and Android apps 

Many people probably come into contact with Fastlane because it also promises to get rid of the certification and signing chaos often created when developing for iOS. I know that alone was enough to sell me on the tool.

But Fastlane also offers much more: creating screenshots, managing and uploading metadata to the stores, creating apps entries, even building and (beta) distribution. With all the tools in its toolchain, the whole "deploy + release" process can be covered:

- `produce` = Create apps on Apple Developer Center and iTunes Connect
- `precheck` = Check if your app and its data will pass the app review
- `deliver` = Upload metadata for iOS apps
- `supply` = Upload metadata for Android apps
- `match` = Manage iOS certificates and provisioning profiles
- `snapshot` = Screenshots for iOS apps
- `screengrab` = Screengrabs for Android apps
- `gym` = Build your iOS apps

You can also manually manage your certificates with `sigh`, `cert` and `pem`, handle your testing with `scan`, `pilot` and `boarding`, use `frameit` to put your iOS screenshots into a template or manually interface with Apple Dev center and iTunes Connect with `spaceship`.

{::comment}
### Fastlane Bascis

TODO lane?
Ruby?
*file?
{:/comment}

## Fastlane + Ionic? (and Cordova?)

So how about Ionic support with its hybrid apps based on Cordova? Well, it won't work out of the box. Some of the reasons:

* Fastlane expects projects to be iOS _or_ Android. Ionic/Cordova projects can be both, which makes the first difficulty. Fortunately, fastlane uses different file and folder structures for iOS and Android so the same setup can actually be used for both platforms at the same time.

* It also normally wants to be installed into the native projects, and as these are generated with Ionic/Cordova projects (`/platforms/ios` and `/platforms/android`), handled as build artifacts and not checked into `git`, the `fastlane` installation and configuration would get lost with each new checkout of the project. Fortunately we can work around this by supplying explicit paths to our native projects in `/platforms` and install Fastlane in our normal project folder that contains our `www` folder and `config.xml`.

* The commands you execute via the `ionic` CLI are also unique for Ionic (`ionic cordova build`, `ionic cordova prepare` etc.) [same if you are using the `cordova` CLI directly for `cordova build`, `cordova prepare` etc], and so are of course not taken into consideration in all documentation of Fastlane (where everything is done with Xcode for iOS or Gradle for Android) and the published tutorials around the web.

(In the process you will also discover several other stumbling blocks.)

This requires some creative handling of things and also usage of some plugins. But: It is perfectly doable!

Start by [preparing your Ionic/Cordova project for Fastlane](prepare-your-ionic-project-for-fastlane.md).