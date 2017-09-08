---
title: 
published: true
date: 2017-08-29 16:00:00 +0000
last_updated: ''
---
# Build and upload your Cordova apps for testing with Fastlane

Now that we have both [iOS and Android correctly configured](install-and-initialize-fastlane-for-your-cordova-ios-and-android-apps.md), [created remote apps](create-your-remote-app-with-fastlane.md) and [added some first metadata](add-metadata-and-upload.md) it is a good time to actually do something with our Ionic project: build a first version of our app and upload it for testing.

To build we will leverage and edit the `Fastfile` for the first time and create new lanes in it.

## Testing for iOS

Apple offers [TestFlight](https://developer.apple.com/testflight/) which "makes it easy to invite users to test your apps and collect valuable feedback before you release them on the App Store. You can invite up to 10,000 testers using just their email address."

Unfortunately this "test" is actually done on a normal production build:

> Using TestFlight to beta test iOS apps involves submitting an app to iTunes Connect that is signed with your App Store distribution provisioning profile. […] This is the preferred method of beta testing because it does not require you to re-build or re-sign the submission for App Store publication.

Source: [Apple Technical Note TN2407](https://developer.apple.com/library/content/technotes/tn2407/_index.html#//apple_ref/doc/uid/DTS40014991-CH1-FOLLOW_WORKFLOWS_DOCUMENTED_BY_APPLE_TO_RUN_ON_DEVICE__BETA_TEST__OR_SUBMIT_YOUR_APP-BETA_TESTING)

So depending on what you want to do (e.g. debug the app via Safari, test the actual same version that you are using to test when developing) Testflight might not be the best tool for the job.

Fortunately there are loads of third party options. These usually _only_ work with development builds as it allows these services to mess with the certificates a bit and also install the app via a simple download. [HockeyApp](https://www.hockeyapp.net/) is one of the most popular ones, so this will be the other option we will support besides TestFlight.

### iOS: Build for and upload to HockeyApp

TODO

### iOS: Build for and upload to Testflight

With normal Xcode projects uploading to Testflight via `fastlane` is so easy, that it is one of the templates generated in the Fastfile:

```
  desc "Submit a new Beta Build to Apple TestFlight"
  desc "This will also make sure the profile is up to date"
  lane :beta do
    # match(type: "appstore") # more information: https://codesigning.guide
    gym # Build your app - more options available
    pilot

    # sh "your_script.sh"
    # You can also use other beta testing services here (run `fastlane actions`)
  end
```

This defines a `beta` lane, that uses `match` to take care of certificates (commented out right by default because not all people want to use that, we do!), then `gym` to build the app and finally `pilot` to upload to Testflight.

TODO Link to modify-cordova-ios-project-for-fastlane.md




Now on to the real lane:

1. first certificates with `match`

2. The default lane doesn't handle version numbers. 
So we get the current or default build number with `latest_testflight_build_number`, increase it and set the build number again with `increment_build_number`. This manipulates the xcode project directly.
save the new version to config.xml

3. then build the project.

4. and upload it to testflight with `pilot`

5. finally commit all the project changes to git

```
  desc "Submit a new Beta Build to Apple TestFlight"
  lane :beta do
    name = "Zählerstand"
    xcodeprojpath = "platforms/ios/" + name + ".xcodeproj"

    match
    increment_build_number({
      build_number: latest_testflight_build_number + 1,
      xcodeproj: xcodeprojpath
    })
    TODO save new version number to config.xml
    gym(
      project: xcodeprojpath
    )
    pilot(
       skip_waiting_for_build_processing: true,
       ipa: "./build/" + name + ".ipa"
    )
    TODO commit config.xml changes to git repo
  end
```

Check the testflight interface to see if the upload worked.


## Testing for Android

For Android the choice of a beta distribution platform is similar as it is for iOS: 

* The Google Play Store has "alpha" and "beta" tracks, in addition to "production", that [can be used to conduct closed and open alpha and beta tests](https://support.google.com/googleplay/android-developer/answer/3131213). But as these can be promoted to production, you can not upload development builds but ave to prepare an APK that could - potentially - be released to all users.
* Third party options like HockeyApp on the other hand also accept development builds that can be debugged.

So again we start with HockeyApp and then also show the variant with Play Store Alpha channel:

### Android: Build for and upload to HockeyApp

TODO


### Android: Build for and upload to Play Store Alpha channel

* Build APK
* Upload to Play Console

```
platform :android do
  desc "Deploy a new version to the Google Play Store"
  lane :alpha do
    gradle(
      task: "assemble",
      build_type: "Release",
      project_dir: "android/"
    )
    supply(
      track: "alpha",
      apk: "#{lane_context[SharedValues::GRADLE_APK_OUTPUT_PATH]}"
    )
  end
end
```
