---
title: TLDR, Fastlane + Ionic
date: 2017-09-07 16:00:00 +0000
published: true
last_updated: ''
---
# TLDR: Fastlane + Ionic

This is the "Too Long Didn't Read" version of all the articles about Ionic and Fastlane: Just the commands, necessary input or checks to do.

## Prepare
- Make sure iOS and Android are added and up to date
    - If missing:
        - `ionic cordova platform add ios`
        - `ionic cordova platform add android`
- Edit `config.xml`: Customize `widget id` and `widget version`
- `ionic cordova prepare`
- `ionic cordova resources`

## Install
- Install [Homebrew](https://brew.sh/) 
- `brew cask install fastlane`
- `fastlane --version` to check if everything works

## Initialize

### iOS
- `fastlane init`
    - "Is this project an iOS project?" `y`
    - "Couldn't automatically detect the project file, please provide a path:" `platforms/ios/FastlaneIonic.xcodeproj`
    - "Your Apple ID (e.g. fastlane@krausefx.com):" Enter your Apple ID
    - "Passwort (for mail@example.org):" Enter your Apple ID password
    - "App Identifier (com.krausefx.app):" `zone.ionix.fastlane`
    - "Would you like to create your app on iTunes Connect and the Developer Portal?" `n` (!)
    - "Optional: The scheme name of your app (If you don't need one, just hit Enter):" <kbd>Enter</kbd>
    - Creates some files:
        ```
        fastlane/
        ├── Appfile
        ├── Fastfile
        └── actions/
        ```

### Android
- Do it manually
    - Add at the bottom of `Appfile`:
        ```
        [...]
        json_key_file ""
        package_name "zone.ionic.fastlane"
        ```
    - Add at the bottom of `Fastfile`:
        ```
        [...]

        platform :android do
        before_all do
            # ENV["SLACK_URL"] = "https://hooks.slack.com/services/..."
        end

        desc "Runs all the tests"
        lane :test do
            gradle(task: "test")
        end

        desc "Submit a new Beta Build to Crashlytics Beta"
        lane :beta do
            gradle(task: "assembleRelease")
            crashlytics

            # sh "your_script.sh"
            # You can also use other beta testing services here
        end

        desc "Deploy a new version to the Google Play"
        lane :deploy do
            gradle(task: "assembleRelease")
            supply
        end

        # You can define as many lanes as you want

        after_all do |lane|
            # This block is called, only if the executed lane was successful

            # slack(
            #   message: "Successfully deployed new App Update."
            # )
        end

        error do |lane, exception|
            # slack(
            #   message: exception.message,
            #   success: false
            # )
        end
        end
        ```
- [Get a google_play_key.json file](https://docs.fastlane.tools/getting-started/android/setup/#collect-your-google-credentials)
- Edit `Appfile` with the path to your file: 
    ```
    json_key_file "../google_play_key.json"
    ```
- `fastlane supply init --verbose` to test
    - You want to see `Fetching a new access token from Google...` and then a error `Google Api Error: applicationNotFound: No application was found for the given package name`

## Create remote apps

### iOS
- `fastlane produce`
    - "App name:" `Fastlane Ionic``
    - Check “My Apps” in iTunes Connect and “Identifiers” => “App IDs” in the Developer Center 

### Android

- Open [Google Play Console](https://play.google.com/apps/publish/) and manually “Create Application”
- Go to “App Releases” -> “Manage Alpha” -> “Create Release” and “Upload APK”
    - Upload a [normal Ionic `--release` APK](TODO)
    - Check if `package_name` now appears in application list

## Create local metadata

### iOS
- `fastlane deliver init`
- Creates local file structure for metadata:
    ```
    fastlane
    ├── Appfile
    ├── Deliverfile
    ├── Fastfile
    ├── metadata
    │   ├── copyright.txt
    │   ├── en-US
    │   │   ├── description.txt
    │   │   ├── keywords.txt
    │   │   ├── marketing_url.txt
    │   │   ├── name.txt
    │   │   ├── privacy_url.txt
    │   │   ├── promotional_text.txt
    │   │   ├── release_notes.txt
    │   │   ├── subtitle.txt
    │   │   └── support_url.txt
    │   ├── primary_category.txt
    │   ├── primary_first_sub_category.txt
    │   ├── primary_second_sub_category.txt
    │   ├── review_information
    │   │   ├── demo_password.txt
    │   │   ├── demo_user.txt
    │   │   ├── email_address.txt
    │   │   ├── first_name.txt
    │   │   ├── last_name.txt
    │   │   ├── notes.txt
    │   │   └── phone_number.txt
    │   ├── secondary_category.txt
    │   ├── secondary_first_sub_category.txt
    │   ├── secondary_second_sub_category.txt
    │   └── trade_representative_contact_information
    │       ├── address_line1.txt
    │       ├── city_name.txt
    │       ├── country.txt
    │       ├── is_displayed_on_app_store.txt
    │       ├── postal_code.txt
    │       └── trade_name.txt
    └── screenshots
        └── README.txt
    ```
    - Delete `Deliverfile`

### Android
- `fastlane supply init`
- Creates additional local files:
    ```
    metadata/android
    └── en-US
        ├── full_description.txt
        ├── images
        │   ├── phoneScreenshots
        │   ├── sevenInchScreenshots
        │   ├── tenInchScreenshots
        │   ├── tvScreenshots
        │   └── wearScreenshots
        ├── short_description.txt
        ├── title.txt
        └── video.txt
    ```

## Add metadata and upload

### iOS
- Fill files in `fastlane/metadata`
- `fastlane deliver`
    - Creates a HTML preview
    - "Does the Preview look okay for you?" `y`
    - Check iTunes Connect for success

### Android
- Fill files in `fastlane/metadata/android`
    - Create `en-US/featureGraphic.png` (1024x500px) and `en-US/icon.png` (512x512px)
- `fastlane supply`
    - Check the Play Console for confirmation it worked
 

## Build and upload for testing
- ...

## Take screenshots
- ...

## Build and upload for release
- ...