---
title: Fastlane and Windows
published: true
date: 2017-08-29 16:00:00 +0000
last_updated: ''
---
# Fastlane and Windows

Unfortunately Fastlane doesn't work on Windows yet. Some of its internal dependencies, specifically the way it starts and handles processes (TODO Sources), only work on Mac OS and Linux systems. There are workarounds for these specific libraries it uses, but implementing them would mean a major refactoring of Fastlane. Because of this its maintainers are rightfully hesitant to tackle this problem.

Theoretically it would be possible to develop a small CLI that can be installed on Windows and enables usage of `fastlane` by executing the actual Fastlane on Mac machines in the cloud.

* Are you interested?
* Are you willing to pay for it? How much?

Email me at ionicdotzone@gmail.com with the subject "Fastlane and Windows".