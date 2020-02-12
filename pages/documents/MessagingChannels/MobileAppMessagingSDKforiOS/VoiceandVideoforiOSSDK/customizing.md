---
pagename: Customizing
redirect_from:
  - consumer-experience-voice-video-ios-appearance-behavior.html
  - voice-and-video-for-ios-sdk-beta-customizing-appearance-and-behavior.html
  - consumer-experience-voice-video-ios-appearance-wording.html
  - voice-and-video-for-ios-sdk-beta-customizing-wording.html
  - voice-and-video-for-ios-sdk-beta-customizing.html
sitesection: Documents
categoryname: "Messaging Channels"
documentname: Mobile App Messaging SDK for iOS
subfoldername: Voice and Video for iOS SDK (BETA)
permalink: mobile-app-messaging-sdk-for-ios-voice-and-video-for-ios-sdk-beta-customizing.html
indicator: messaging
---

<div class="important">
<h2>Deprecation Notice</h2>

the CoApp product is deprecated and will be discontinued from February 28th, 2020 on.
</div>

### Appearance and Behavior

**Note**: Full white-labeling is currently not supported. If you are missing an important customization feature, don't hesitate to contact you LivePerson account manager for help.

Below you'll learn how to customize various SDK settings to fit your app's needs. You can customize different aspects of the SDK:

   * [General Behavior](#behavior)
   * [Appearance](#appearance)
   * [Sounds](#sounds)

***

#### Step 1: Setup

In your Xcode project create a new `plist` file named:

  * `LPCoAppSDK.plist` and include it into your app target resources

#### Step 2: Add custom settings

Open your `LPCoAppSDK.plist` in Xcode and add the settings you wish to customize.

#### Behavior
<style>
td:first-child {
  width: 200px!important;
}
</style>

| Key        | Type | Default Value | About  |
| ------------- |:-------------:|:-------------:|:-----|
|  **acceptCallOnNotificationTap** | BOOL  | YES  | Determines if a tap on a Push-call notification is considered **Accepting** the call. If set to `NO`, the user is presented the call screen and must accept explicitly. |
| **ringRepeatTimes** | Number | 3 | The number of times a push-call is presented to the user when the app is in background. _Hint_: On average a notification (with ring) is presented _7 seconds_, followed by a _3 second_ pause. So the total ring duration of **3** repeats amounts to approx. 3 * 10 = 30 seconds. iOS will limit to **4x repeats at max** |
| **speakerOnVideo** | BOOL | YES | Determines if the phone's speaker should be automatically enabled when a video call is started. This is recommended, as the volume in non-speaker mode is too low to be heard from a viewing distance. |
| **useProximitySensor** | BOOL | YES | Determines if the device's screen should be switched off and camera streaming should be paused, while the device is close to a user's ear (e.g. in a voice-call)  |

#### Appearance

| Key        | Type | Default Value | About  |
| ------------- |:-------------:|:-------------:|:-----|
|  **cobrowseFrameColor** | String  | #004dc9   | The color of the frame surrounding the app's screen during a CoBrowse session. Use a HEX color value as String, including a leading hash (#) |
| **cobrowseTitleColor** | String | #ffffff | The color of the title on the top bar during a CoBrowse session. Make sure the color is sufficiently visible against the `cobrowseFrameColor` background |

#### Sounds

| Key        | Type | Default Value | About  |
| ------------- |:-------------:|:-------------:|:-----|
|  **ringSound** | String  | ring.caf   | The ring sound file in you `Main.bundle` played when an incoming call is presented to the user. __Note__: This does not affect the ring of push-calls. Push-calls always use a file named `ring.caf` |
| **escalationSound** | String | escalation.caf | The sound in your `Main.bundle` played, when a call escalation (permissions request) is presented to the user. This sound is played once and should be no more than `3 seconds` long |

**Hint:** `caf` files are optimized audio files for iOS. You can convert any `aiff` file using the command below:

`afconvert -v -f 'caff' -d aac -s 1 -b 192000 MySource.aif MyOutput.caf`

### Wording

These settings should only be used to customize the wording according to you app's needs. If you plan to offer a new language not yet available in the SDK, please to speak to your LivePerson account manager.

#### Step 1: Setup

In your Xcode project, create a new file named:

  * `LPCoAppSDK.strings` and enable __Localization__ for this file.

This will put the file under your `[Language].lproj` (e.g. `EN.lproj`) folders.

#### Step 2: Edit Wording
<style>
td:first-child {
  width: 150px!important;
}
</style>

Open your `LPCoAppSDK.strings` in the language you wish to edit and add (ONLY!) those keys you wish to edit. Non specified keys will use the default wording.

| Key        | Default Value (EN) | Note  |
| ------------- |:-------------:|:-----|
|   **CALLER_NAME**   | Agent  | The name displayed to the user as the caller, if the actual agent's name could not yet be resolved OR was set to `default`. This should most likely be your **Brand's name**  |

**NOTE:** More options will be made available once the SDK is out of BETA
