---
title: Install Fastlane
published: true
date: 2017-08-15 21:00:00 +0000
last_updated: ''
---
# Install Fastlane

After [preparing your Ionic Cordova projects](prepare-your-ionic-project-for-fastlane.md) you can now install Fastlane on your machine and initialize it for both your iOS and Android projects.

Note: Unfortunately Fastlane can only be installed on Mac OS and Linux systems as some of its internal dependencies depend on functionality, that is not available for Windows (yet). Bummer. [Are you using Windows and really interested in using Fastlane? Click here to learn more.](fastlane-and-windows.md)
{:.message}

## `fastlane` CLI

The work of Fastlane is done by a CLI tool (unsurprisingly) called `fastlane`:

> CLI for 'fastlane' - The easiest way to automate beta deployments and releases for your iOS and Android apps

`fastlane` can be conveniently installed using "Homebrew" (If you dont have Homebrew yet, follow it's installation instructions on [brew.sh](https://brew.sh/)): `brew cask install fastlane`

![Installation of `fastlane` with `brew cask install fastlane`]({{ site.url }}/assets/fastlane/installation-brew.png)

It downloads and then installs `fastlane` to `/Users/username/.fastlane/`. It also installs some `gems`, you maybe can derive that one is for manipulating .xcodeproj files and one for interacting with Google APIs.

After adding Fastlane to your path in `.bash_profile` (see output of the previous command), restart your terminal and execute `fastlane --version` to confirm the working installation:

![`fastlane --version`]({{ site.url }}/assets/fastlane/fastlane-version.png)

(Official documentation with alternative installation methods is available at [https://docs.fastlane.tools/getting-started/ios/setup/#installing-fastlane](https://docs.fastlane.tools/getting-started/ios/setup/#installing-fastlane) - depending on your local setup you might want to try them until you find one that works for you.)

Now that the `fastlane` CLI tool is installed and working, you can continue to [initialize Fastlane in your project](install-and-initialize-fastlane-for-your-cordova-ios-and-android-apps.md).