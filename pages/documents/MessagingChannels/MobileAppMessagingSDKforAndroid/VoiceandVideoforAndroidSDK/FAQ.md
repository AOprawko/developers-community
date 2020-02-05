---
pagename: FAQ
redirect_from:
  - consumer-experience-voice-video-android-FAQ.html
  - voice-and-video-for-android-sdk-beta-faq.html
sitesection: Documents
categoryname: "Messaging Channels"
documentname: Mobile App Messaging SDK for Android
subfoldername: Voice & Video for Android SDK (BETA)
permalink: mobile-app-messaging-sdk-for-android-voice-and-video-for-android-sdk-beta-faq.html
indicator: messaging
---

<div class="important">
<h2>Deprecation Notice</h2>

the CoApp product is deprecated and will be discontinued from February 28th, 2020 on.
</div>

### How much diskspace does the SDK need?

* Universal build: **+40MB**
* Architecture-specific builds
    * smallest: **+7MB** (armeabi)
    * biggest: **+14MB** (arm64-v8a)

**Note:** the SDK for Android comes packaged with prebuilt libraries for all supported architectures. Since we use native code, the SDK `aar` already contains precompiled binaries for all architectures. The size of the `aar` is *21.6 MB*. Note that when adding the `aar` dependency, the Android build system only binds the binaries for the desired architecture.

### Which ABIs/architectures are supported?

Currently, we support the following architectures/ABIs:

* x86
* x86_64
* armeabi
* armeabi-v7a
* arm64-v8a

**Note:** we do not support MIPS architectures at the moment!
