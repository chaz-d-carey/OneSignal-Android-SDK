# OneSignal Android SDK

The OneSignal Android SDK adds push notifications, in-app messaging, and related engagement features to your Android app. It supports Google (FCM) and Huawei (HMS) push services.

## Features

- **Push notifications** — Send and receive push notifications via Firebase Cloud Messaging (FCM) or Huawei Push Kit (HMS)
- **In-app messages** — Display in-app messages and control their lifecycle
- **Notification handlers** — Handle notification open, foreground display, and optional notification service extension
- **User identity** — Set external user IDs, email, and SMS for targeting
- **Outcomes** — Track attribution and outcomes for campaigns
- **Badges** — App icon badge support across major device manufacturers (Samsung, Huawei, Xiaomi, etc.)

## Requirements

- **minSdkVersion:** 16  
- **compileSdkVersion / targetSdkVersion:** 31 (or compatible)  
- **AndroidX** — The SDK uses AndroidX; your app must use AndroidX  
- **Google Play builds:** Firebase (FCM) and Google Play Services  
- **Huawei builds:** Huawei Mobile Services (HMS) Push and optional Location

## Installation

### Gradle

Add the OneSignal dependency in your app-level `build.gradle`:

```gradle
dependencies {
    implementation 'com.onesignal:OneSignal:4.8.6'
}
```

### Google Play (FCM) setup

1. Add the Google Services plugin in your project-level `build.gradle`:

```gradle
buildscript {
    dependencies {
        classpath 'com.google.gms:google-services:4.3.10'
    }
}
```

2. Apply it in your app-level `build.gradle`:

```gradle
apply plugin: 'com.google.gms.google-services'
```

3. Add your `google-services.json` from the Firebase console to the app module.

### Huawei setup

For Huawei-only or flavor-specific builds, use HMS instead of FCM:

```gradle
// Replace OneSignal dependency with HMS-compatible one
implementation('com.onesignal:OneSignal:4.8.6') {
    exclude group: 'com.google.android.gms', module: 'play-services-gcm'
    exclude group: 'com.google.firebase', module: 'firebase-messaging'
}
implementation 'com.huawei.hms:push:6.3.0.304'
// Optional: for location
implementation 'com.huawei.hms:location:4.0.0.300'
```

Apply the Huawei AGConnect plugin where needed and add the Huawei `agconnect-services.json` configuration.

## Quick start

1. **Set your OneSignal App ID** (from the OneSignal dashboard):

```java
OneSignal.setAppId("YOUR_ONESIGNAL_APP_ID");
```

2. **Initialize in your `Application` class** (e.g. in `onCreate()`):

```java
OneSignal.setAppId("YOUR_ONESIGNAL_APP_ID");
OneSignal.initWithContext(this);
```

3. **Optional: set handlers before `initWithContext`**:

```java
OneSignal.setNotificationOpenedHandler(result -> {
    // User opened a notification
});

OneSignal.setNotificationWillShowInForegroundHandler(event -> {
    // Notification received while app is in foreground
    event.complete(event.getNotification());
});
```

## Project structure

- **`OneSignalSDK/`** — SDK and unit tests  
  - **`onesignal/`** — Main OneSignal Android library (AAR)  
  - **`unittest/`** — Instrumented and unit tests  
- **`Examples/OneSignalDemo/`** — Sample app (GMS and Huawei flavors) showing integration and API usage

## Building from source

1. Open the **OneSignalSDK** project (its `settings.gradle` includes the `onesignal` and `unittest` modules).
2. To run the demo app against the local SDK, use the included OneSignalSDK project: it substitutes the published `com.onesignal:OneSignal` dependency with `project(':onesignal')`.
3. Build the library:

```bash
cd OneSignalSDK
./gradlew :onesignal:assembleRelease
```

Output AAR: `OneSignalSDK/onesignal/build/outputs/aar/onesignal-release.aar`

## ProGuard

The SDK ships with `consumer-proguard-rules.pro`, so required ProGuard rules are applied automatically when the library is used. If you use custom rules, avoid stripping observer interfaces or classes referenced in the [consumer rules](OneSignalSDK/onesignal/consumer-proguard-rules.pro).

## Documentation

- [OneSignal Android SDK Setup](https://documentation.onesignal.com/docs/android-sdk-setup)  
- [OneSignal Android Native SDK](https://documentation.onesignal.com/docs/android-native-sdk)  
- [NotificationReceivedHandler](https://documentation.onesignal.com/docs/android-native-sdk#notificationreceivedhandler)  
- [NotificationOpenedHandler](https://documentation.onesignal.com/docs/android-native-sdk#notificationopenedhandler)
