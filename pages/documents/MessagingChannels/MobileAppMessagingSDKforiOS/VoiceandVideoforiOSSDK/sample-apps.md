---
pagename: Sample App
redirect_from:
  - consumer-experience-voice-video-ios-sample-app.html
  - voice-and-video-for-ios-sdk-beta-sample-app.html
sitesection: Documents
categoryname: "Messaging Channels"
documentname: Mobile App Messaging SDK for iOS
subfoldername: Voice and Video for iOS SDK (BETA)
permalink: mobile-app-messaging-sdk-for-ios-voice-and-video-for-ios-sdk-beta-sample-app.html
indicator: messaging
---

Bundled with our SDK is a sample application written in `Swift`. It showcases a minimal setup of the __Voice & Video SDK__ bundled together with our __Messaging SDK__.

In order to minimize setup effort, the sample app uses [_CocoaPods_](https://cocoapods.org/about).

1. `cd` to `/Sample Apps/Swift/`
2. Type `pod repo update && pod install`
3. Open **sampleapp-swift.xcworkspace** in _Xcode_
4. Replace `accountNumber` in _AppDelegate.swift_ with your LP Account Id
5. Run `sample-app-swift`target


**Hint:** Unless you implement the [VoIP setup](consumer-experience-voice-video-ios-voip-configuration.html), your sample app won't be able to receive calls outside of the messaging window.
