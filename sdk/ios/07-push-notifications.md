# Vyin Chat SDK for iOS - Push Notifications

## Push notifications

## Overview

Using Chat SDK for iOS, you can send notification messages to your user's device when the device is either idle or running the client app in the background. For group channels, notifications can be configured to display an alert, play a sound, or place a badge on the client app's icon.

Push notifications for iOS client apps are sent using Firebase Cloud Messaging (FCM) which contains custom data that your app needs in order to respond to the notifications. When a message is sent to the Vyin Chat server through our Chat SDK, the server communicates with FCM which then delivers a push notification to an iOS device where the client app is installed.

By default, when a user's device is disconnected from the Vyin Chat server, the server sends push notification requests to FCM for the messages. The Chat SDK automatically detects when a user's client app goes into the background from the foreground, and updates the user's connection status to disconnected. Therefore, under normal circumstances, there is no need to explicitly call the `disconnect()` method.

Push notifications support both single and multi-device users and they are delivered only when a user is fully offline from all of their devices. In other words, if a user is online on one or more devices, notifications aren't delivered and thus not displayed on any device.  

---

### Push notification with multi-device support

Push notifications with multi-device support provide the same features as our general push notifications, but with additional support for multi-device users. When implemented, notifications are delivered to all online and offline devices of a multi-device user but through our Chat SDK, push notifications are displayed only on offline devices and ignored by online devices. As a result, client apps are able to display push notifications on all offline devices, regardless of whether there are one or more online devices.

For example, let’s say a multi-device user who has six devices with your client app installed is online on one of the devices and offline on the remaining five. When general push notifications are implemented, notifications aren’t delivered to any of their devices. But when push notifications with multi-device support are implemented, notifications are delivered to all six devices and displayed on the five offline devices.

By default, when a user's device is disconnected from the Vyin Chat server, the server sends push notification requests to FCM for the messages. The Chat SDK automatically detects when a user's client app goes into the background from the foreground, and updates the user's connection status to disconnected. Therefore, under normal circumstances, there is no need to call the `disconnect()` method.

To find out which type of push notification best fits your use case, see the following table.

|  | General push notifications | Push notifications with multi-device support |
| :---- | :---- | :---- |
| Sent when | All devices are offline. | At least one device is offline. |
| Notification messages | Single-device user: Displayed when disconnected from the Vyin Chat server and thus offline.Multi-device user: Displayed only when all devices are offline. | Single-device user: Displayed when disconnected from the Vyin Chat server and thus offline.Multi-device user: Displayed on all offline devices, regardless of whether one or more are online. |
| SDK's event callback | Invoked only when the app is connected to the Vyin Chat server. | Invoked only when the app is connected to the Vyin Chat server. |
| App instance's registration token | Can be manually registered to the Vyin Chat server. | Automatically registered to the Vyin Chat server. |
| Notification preferences | Can be set by application and group channel. | N/A |
| Push notification templates | Supported. | Supported. |
| Push notification translation | Supported by [Google Cloud Translation API](https://cloud.google.com/translate/). | Supported by [Google Cloud Translation API](https://cloud.google.com/translate/). |

---

### Functionalities by topic

Users can set their preferences for receiving notifications in their devices. The following is a list of functionalities that our Chat SDK provides.

#### Managing notifications

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Set up push notifications for FCM | Users can receive [FCM messages](https://firebase.google.com/docs/cloud-messaging/concept-options) in their iOS app. | ✓ |  |
| Configure push notification preferences | Turns push notifications on or off in a user's app using the user's registration token. |  | ✓ |
| Push notification content templates | Displays customized push notification messages on a user's device using templates. |  | ✓ |
| Push notification tester | Test on Vyin Chat Dashboard whether the push notification credentials and notification services are working properly. |  | ✓ |
| Push notification translation | Users can receive notification messages translated into their preferred languages. |  | ✓ |

#### Additional support for multi-device users

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Set up push notifications for FCM | Multi-device users can receive [FCM messages](https://firebase.google.com/docs/cloud-messaging/concept-options) on multiple devices. |  | ✓ |

## Managing push notifications

## Set up push notifications for FCM

There are two types of [FCM messages](https://firebase.google.com/docs/cloud-messaging/concept-options): notification messages and data messages. Vyin Chat uses data messages, which are handled by the client app. They allow users to customize the message payload, which consists of key-value items.

The following is a set of step-by-step instructions on how to set up push notifications for FCM.

* Step 1 Generate private key for FCM  
* Step 2 Register private to Vyin Chat Dashboard  
* Step 3 Set up an FCM client app on your iOS project  
* Step 4 Register a registration token to the Vyin Chat server  
* Step 5 Handle an FCM message payload

---

### Step 1 Generate private key for FCM

The Vyin Chat server requires your private key to send notification requests to FCM on behalf of your server. This is required for FCM to authorize HTTP requests.

Note: If you already have your private key, skip this step and go directly to Step 2: Register private key to Vyin Chat Dashboard.

1. Go to the [Firebase console](https://console.firebase.google.com/). If you don't have a Firebase project for a client app, create a new project.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Creating your project in the [Firebase console](https://console.firebase.google.com/).

2. Select your project card to move to the Project Overview.  
3. Click the gear icon at the upper left corner and select Project settings.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Opening project settings to configure your Firebase project.

4. Go to Service accounts and click on Generate a new private key.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** [Firebase console](https://console.firebase.google.com/) screen to generate a new private key.

5. Go to the General tab and select your iOS app to add Firebase to. During the registration process, enter your package name, download the `GoogleService-Info.plist` file, and place it in your iOS app module root directory.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Downloading google-services.json and placing it in your iOS app project.  

---

### Step 2 Register private key to Vyin Chat Dashboard

Register your private key to the Vyin Chat server through the dashboard as follows.

1. Sign in to your dashboard and go to Settings \> Application \> Push notifications.  
2. Turn on Push notifications and select Send when all devices are offline.  
3. Scroll down to the FCM section and click on Add credentials.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Adding your private key for FCM on Vyin Chat Dashboard.

4. Under Service account key (HTTP v1), upload the JSON file containing the key that was downloaded in Step 1.

Note: Your private key can also be registered using our add an FCM push configuration API.  

---

### Step 3 Set up an FCM client app on your iOS project

Add Firebase Cloud Messaging to your iOS project. You can use CocoaPods or Swift Package Manager to add the Firebase Messaging dependency.

Note: Refer to the Firebase documentation for the latest supported version.

Note: To learn more about this step, refer to Firebase's [Set Up a Firebase Cloud Messaging client app on iOS](https://firebase.google.com/docs/cloud-messaging/ios/client) guide.  

---

### Step 4 Register a registration token to the Vyin Chat server

In order to send notification messages to a specific client app on an iOS device, FCM requires an app instance's registration token which has been issued by the client app. Therefore, the Vyin Chat server also needs every registration token of client app instances to send notification requests to FCM on behalf of your server.

A user can have up to 20 FCM registration tokens. If a user who already has the maximum number of tokens attempts to add another one, the newest token replaces the oldest.

Upon the initialization of your app, the FCM SDK generates a unique, app-specific registration token for the client app instance on your user's device. FCM uses this registration token to determine which device to send notification messages to. After the FCM SDK has successfully generated the registration token, it is passed to the `registerDevicePushToken(_ devToken:)`. Registration tokens must be registered to the Vyin Chat server by passing it as an argument to the parameter in the `registerDevicePushToken()` method as in the following code.

Before calling that function please register remote push notification.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
     application.registerForRemoteNotifications()
     return true
}

// Delegate

func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    let tokenParts = deviceToken.map { data in String(format: "%02.2hhx", data) }
    let token = tokenParts.joined()
    // register token here
    GIMChat.registerDevicePushToken(token, unique: false, completeHandler:{
        //TODO
    })
}

public class func registerDevicePushToken(_ devToken: Data, unique: Bool, completionHandler: ((_ registrationStatus: PushTokenRegistrationStatus, _ error: GIMError?) -> Void)? = nil) {
    // Register push token with the server.
}
```

Note: If `PushTokenRegistrationStatus.pending` is returned through the handler, this means that your user isn't being connected to the Vyin Chat server when `registerDevicePushToken()` is called. In this case, you must first get a pending registration token using [`getToken()`](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging#getToken())), and then register the token by calling the `registerPushToken()` method in the `completionHandler` callback when your user has been connected to the server.  

```swift
GIMChat.connect(USER_ID) { user, e in
    if (e != nil) {
        // Handle error.
    }
        GIMChat.registerPushToken(task.result) { status, e in
            if (e != nil) {
                // Handle error.
            }

            // ...
        }
}
```
---

### Step 5 Handle an FCM message payload

The Vyin Chat server sends push notification payloads as [FCM data messages](https://firebase.google.com/docs/cloud-messaging/concept-options), which contain notification-related data in the form of key-value pairs. Unlike notification messages, the client app needs to parse and process these data messages in order to display them as local notifications.  
Note: See Firebase’s [Receive messages in an iOS app](https://firebase.google.com/docs/cloud-messaging/ios/receive) guide to learn more about how to implement code to receive and parse a [FCM notification message](https://firebase.google.com/docs/cloud-messaging/concept-options), how notification messages are handled depending on the state of the receiving app, or how to override the `UNUserNotificationCenterDelegate` method.  

```swift
extension AppDelegate: UNUserNotificationCenterDelegate {
  func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
    process(notification)
    completionHandler([.badge, .sound, .banner])
  }
  func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
    process(response.notification)
    completionHandler()
  }
  func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
    completionHandler(.newData)
  }
}
```

The following is a complete payload format of the `gim` property, which contains a set of provided key-value items. Some fields in the push notification payload can be customized in Settings \> Chat \> Notifications on the Vyin Chat Dashboard. For example, `push_title` and `push_alert` are created based on the Title and Body text you set in Push notification content templates, respectively, in the Notifications menu. In order to display them in a local notification, use `push_title` and `push_alert` of the push notification payload to configure the notification content in your `UNMutableNotificationContent`. Also, the `channel_unread_count` field can be added into or removed from the payload in the same menu on the Vyin Chat Dashboard.  

```json
{
    "category": "messaging:offline_notification",
    "type": "String",                   // Message type: MESG, FILE, or ADMM
    "message": "String",                // User input message
    "custom_type": "String",            // Custom message type
    "message_id": "Int64",              // Message ID
    "created_at": "Int64",              // The time that the message was created, in a 13-digit Unix milliseconds format
    "app_id": "String",                 // Application's unique ID
    "unread_message_count": "int",      // Total number of new messages unread by the user
    "channel": {
        "channel_url": "String",        // Group channel URL
        "name": "String",              // Group channel name
        "custom_type": "String",       // Custom Group channel type
        "channel_unread_count": "int"  // Total number of unread new messages from the specific channel
    },
    "channel_type": "String",           // messaging = distinct group channel, group_messaging = private group channel, chat = all other cases
    "sender": {
        "id": "String",                // Sender's unique ID
        "name": "String",             // Sender's nickname
        "profile_url": "String",      // Sender's profile image URL
        "require_auth_for_profile_image": false,
        "metadata": {}
    },
    "recipient": {
        "id": "String",                // Recipient's unique ID
        "name": "String"              // Recipient's nickname
    },
    "files": [],                        // An array of data regarding the file message, such as filename
    "translations": {},                 // The items of locale:translation
    "push_title": "String",            // Title of a notification message, customizable on Vyin Chat Dashboard
    "push_sound": "String",            // The location of a sound file for notifications
    "audience_type": "String",          // The type of audiences for notifications
    "mentioned_users": {
        "user_id": "String",           // The ID of a mentioned user
        "nickname": "String",         // The nickname of a mentioned user
        "profile_url": "String",      // Mentioned user's profile image URL
        "require_auth_for_profile_image": false
    }
}
```

## Set up push notifications for HMS

We have not configured HMS devices yet.

## Configure push notification preferences

By registering or unregistering the current user's registration token to the Vyin Chat server as below, you can turn push notifications on or off in the user's client app. The registration and unregistration methods in the code below should be called after a user has established a connection with the Vyin Chat server by calling the `GIMChat.connect()` method.

Through `PushTriggerOption`, the current user can choose which type of messages will trigger push notifications, or opt to turn it off completely. Additionally, they can enable the Do Not Disturb and snooze features through `setDoNotDisturb()` and `setSnoozePeriod()` methods.  

```swift
func setPushNotification(_ enable: Bool) {
    if (enable) {
        GIMChat.registerPushToken(pushToken) { status, e in
            if (e != nil) {
                // Handle error.
            }
        }
    }
    else {
        // If you want to unregister the current device only, call this method.
        GIMChat.unregisterPushToken(pushToken) { e in
            if (e != nil) {
                // Handle error.
            }

            // ...
        }

        // If you want to unregister all devices of the user, call this method.
        GIMChat.unregisterPushTokenAll { e in
            if (e != nil) {
                // Handle error.
            }

            // ...
        }
    }
}
```

The `GIMChat.PushTriggerOption` method allows users to configure when to receive notification messages as well as what types of messages trigger notification messages on the application level. The following are the available options.

### List of push trigger options

| Option | Description |
| :---- | :---- |
| `.all` | When disconnected from the Vyin Chat server, the current user receives notifications for all new messages including messages the user has been mentioned in. |
| `.mentionOnly` | When disconnected from the Vyin Chat server, the current user only receives notifications for messages the user has been mentioned in. |
| `.off` | The current user doesn't receive any notifications. |

```swift
// Send notifications only when the current user is mentioned in messages.
GIMChat.setPushTriggerOption(.mentionOnly) { e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```

The `GroupChannel.PushTriggerOption` method also allows users to configure the trigger for notification messages as well as turn notifications on or off for each group channel. The following are the available options.

| Option | Description |
| :---- | :---- |
| `.default` | The current user’s push notification trigger settings are automatically applied to the channel. This is the default setting. |
| `.all` | When disconnected from the Vyin Chat server, the current user receives notifications for all new messages in the channel including messages the user has been mentioned in. |
| `.mentionOnly` | When disconnected from the Vyin Chat server, the current user only receives notifications for messages in the channel the user has been mentioned in. |
| `.off` | The current user doesn't receive any notifications in the channel. |

```swift
// Apply the user's push notification setting to the channel.
groupChannel.setMyPushTriggerOption(.default) { e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```
---

### Do not disturb

If you want to routinely turn off push notification on the current user's client app according to a specified schedule, use our `Do Not Disturb` feature like the following.

Note: The `Do Not Disturb` feature can also be set for a user with our update push notification preferences API.  

```swift
// The current user won't be receiving notification messages during the specified time of the day.

let entity = DoNotDisturbEntity()

entity.enabled = true
entity.startHour = “”
entity.startMin = “”
entity.endMin = “”
entity.endHour = “”
entity.timezone = “”
entity.pushTriggerOption = pushTriggerOption
entity.dndDaysOfWeek = [“0”, “1”]

GIMChat.setDoNotDisturb(entity: entity) { e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```
---

### Snooze

To snooze notification messages for a specific period of time, use our `snooze` feature as below.

Note: The `snooze` notifications can also be set for a user with the update push preferences API.  

```swift
// The current user won't be receiving notification messages during the specified period of time from START_TS to END_TS.
GIMChat.setSnoozePeriod(true, START_TS, END_TS, timeZone: TIME_ZONE) { e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```

## Push notification content templates

Push notification content templates are pre-formatted forms that can be customized to display your own push notification messages on a user’s device. Vyin Chat provides two types: default and alternative. Both templates can be customized in Settings \> Chat \> Push notifications \> Push notification content templates on Vyin Chat Dashboard.

As for iOS, an additional implementation is required in order to display your customized push notifications. The push notification payload sent from the Vyin Chat server includes the content of the template, such as the title and message, and the client app can determine how to present the notification through iOS-provided code. For more information on how to create a custom notification, refer to Apple's [User Notifications](https://developer.apple.com/documentation/usernotifications) documentation.  

---

### Content templates

There are two types of push notification content template: default and alternative. Default template is a template that applies to users with the initial notification message setting. Alternative template is used when a user selects a different notification content template instead of the default template.

The content in the template is set at the application level while the selection of templates is determined by a user or through the Platform API.

Note: When a custom channel type has its own customized push notification content template, it takes precedence over the default or alternative template.

Both templates can be customized using the variables of `sender_name`, `filename`, `message`, `channel_name`, and `file_type_friendly` which are replaced with the corresponding String values. As for `file_type_friendly`, it indicates the type of the file sent, such as Audio, Image, and Video.

See the following table for the usage of content templates.

#### List of content templates

|  | Text message | File message |
| :---- | :---- | :---- |
| Default template | {sender\_name}: {message}An example can be `Cindy: Hi!` | {filename}An example can be `hello_world.jpg` |
| Alternative template | `New message arrived` | `New file arrived` |

To apply a template to push notifications, use the `setPushTemplate()` method. Specify the type of the template with the name as either `GIMChat.PUSH_TEMPLATE_DEFAULT` or `GIMChat.PUSH_TEMPLATE_ALTERNATIVE`.  

```swift
GIMChat.setPushTemplate(GIMChat.PUSH_TEMPLATE_ALTERNATIVE) { e in
    if (e != nil) {
        // Handle error.
    }

    // GIMChat.PUSH_TEMPLATE_ALTERNATIVE has been successfully set.
}
```

To check the current user's push notification template, use the `getPushTemplate()` method as follows:  

```swift
GIMChat.getPushTemplate { templateName, e in
    if (e != nil) {
        // Handle error.
    }
    switch (templateName) {
        case GIMChat.PUSH_TEMPLATE_DEFAULT:
            // This sets the default template in use

        case GIMChat.PUSH_TEMPLATE_ALTERNATIVE:
            // This sets the alternative template in use
    }
}
```

## Push notification tester

After registering a credential for push notification services, you can test on Vyin Chat Dashboard whether the push notification credentials and notification services work properly.

In Settings \> Notifications \> Push notification credentials on the dashboard, each credential has a Send button that allows you to send a test message. The test message is sent to a target user and their device, which sets off a push notification on the device. At the same time, a new group channel containing only the target user is created for this test. If you've already tested push notification with the same target user and the same target device token, the message is sent to the existing test channel.

Note: If there is another member in the test channel or the target user has left the channel, delete the test channel before testing.

1. Go to Settings \> Notifications \> Push notification credentials on Vyin Chat Dashboard. Find a Send button in the right-end column of each table.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Push notification tester for FCM on Vyin Chat Dashboard

2. You can search for a target user with either their User ID or Nickname in the popup window. Once a user has been selected, choose a target device from a list of device tokens linked to the user.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** A popup showing certification information, target user ID, target device token, and test message content

3. Double check to see if you've selected the correct device token of the user because this push notification shows up on an actual device. Confirm the target channel name and URL, then send the message.

Note: If this is your first test, a new test channel is created. For future tests with the same target user and the same target device token, the name and URL of the existing test channel will appear above the message box.

4. Check to see if the push notification has arrived on the mobile device or emulator. The test message should include the time when the Vyin Chat server sent the message.

```swift
Test message sent at {HH:MM:SS} on {MM-DD-YYYY}
```

Note: In order to display push notifications on iOS devices and emulators, an additional implementation is required on the client app. To learn more, see Push notification for FCM.

## Push notification translation

Push notification translation allows your users to receive notification messages in their preferred languages. Users can set up to four preferred languages. If messages are sent in one of those languages, the notification messages won’t be translated. If messages are sent in a language other than the preferred languages, the notification messages are translated into the user's first preferred language. In addition, the messages translated into other preferred languages are provided in the `gim` property of a notification message payload.

Note: A specific user's preferred languages can be set using the update a user API.

The following shows how to set the current user's preferred languages using the `updateCurrentUserInfo()` method.  

```swift
let preferredLanguages = ["fr", "de", "es", "ko"] // French, German, Spanish, Korean
GIMChat.updateCurrentUserInfo(preferredLanguages) { e in
        if (e != nil) {
            // Handle error.
        }

        // The current user's preferred languages have been updated successfully.
}
```

If successful, the following notification message payload is delivered to the current user's device.  

```
{
    "message": {
        "token": "ad3nNwPe3H0:ZI3k_HHwgRpoDKCIZvvRMExUdFQ3P1...",
        "notification": {
            "title": "Greeting!",
            "body": "Bonjour comment allez vous"    // A notification message is translated into the first language listed in the preferred languages.
        },
        // ...

    },
    "gim": {
        "category": "messaging:offline_notification",
        "type": "User",
        "message": "Hello, how are you?",
        // ...

        "translations": {   // Translations of the message in the other three preferred languages.
            "fr": "Bonjour comment allez vous",
            "de": "Hallo wie geht es dir",
            "es": "Hola como estas"
        },
        // ...

    }
}
```

## Marking push notifications as delivered

## Mark push notifications as delivered

You can now track whether push notifications has been successfully delivered to all the intended devices by the Vyin Chat server. Using the `VyinPushHelper.markPushNotificationAsDelivered()` method, you can mark a push notification as delivered to a device. To ensure proper functionality, the `GIMChat.init()` method must be called within the `Application.onCreate()` method.

In order to check the delivery rate of push notifications, the SDK retrieves the tracking ID for push notifications and is sent to the server to identify which push notification has been successfully delivered to the device.  

```swift
// In AppDelegate or notification handling code:
NotificationCenter.default.post(name: PushNotification.didGetMessage, object: nil)
```

Note: If your app is using `Multi-device support` and has implemented either of `GIMPushHandler`, this will be called internally so that the app doesn't have to handle it directly.

Note: This feature is not implemented yet\!

## Multi-device support

## Set up push notifications for Fcm

## Set up push notifications for FCM

This part covers the following step-by-step instructions of our push notifications for FCM.

* Step 1: Generate private key for FCM  
* Step 2: Register private key to Vyin Chat Dashboard  
* Step 3: Set up Firebase and the FCM SDK  
* Step 4: Implement multi-device support in your iOS app  
* Step 5: Handle an FCM message payload  
* Step 6: Enable multi-device support on Vyin Chat Dashboard

---

### Step 1: Generate private key for FCM

The Vyin Chat server requires your private key to send notification requests to FCM on behalf of your server. This is required for FCM to authorize HTTP requests.

If you already have your server key, you can skip this step and go directly to Step 2: Register private key to Vyin Chat Dashboard.

1. Go to the [Firebase console](https://console.firebase.google.com/). If you don't have a Firebase project for your client app, create a new project.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Creating your project in the [Firebase console](https://console.firebase.google.com/).

2. Select your project card to move to Project Overview.  
3. Click the gear icon at the upper left corner and select Project settings.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Opening project settings to configure your Firebase project.

4. Go to Service accounts and click on Generate a new private key.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** [Firebase console](https://console.firebase.google.com/) screen to generate a new private key.

5. Go to the General tab and select your iOS app to add Firebase to. During the registration process, enter your package name, download the `GoogleService-Info.plist` file, and place it in your iOS app module root directory.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Downloading google-services.json and placing it in your iOS app project.  

---

### Step 2: Register private key to Vyin Chat Dashboard

Register your private key to Vyin Chat Dashboard as follows. Your private key can also be registered using the add an FCM push configuration API.

1. Sign in to your dashboard and go to Settings \> Chat \> Push notifications.  
2. Turn on Notifications and select Send to devices both offline and online.  
3. Click Add credentials and register the app ID and app secret acquired at Step 1\.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Adding your private key for FCM on Vyin Chat Dashboard.  

---

### Step 3: Set up Firebase and the FCM SDK

Add Firebase Cloud Messaging to your iOS project using CocoaPods or Swift Package Manager.

The Chat SDK handles push notifications with multi-device support internally.

Note: To learn more about this step, refer to Firebase's [Set Up a Firebase Cloud Messaging client app on iOS](https://firebase.google.com/docs/cloud-messaging/ios/client) guide.  

---

### Step 4: Handle an FCM message payload

The Vyin Chat server sends push notification payloads as [FCM data messages](https://firebase.google.com/docs/cloud-messaging/concept-options), which contain notification-related data in the form of key-value pairs. Unlike notification messages, the client app needs to parse and process these data messages in order to display them as local notifications.

The following code shows how to receive a push notification payload and parse it as a local notification. The payload consists of two properties: `message` and `gim`. The `message` property is a String generated according to a push notification template you set on the Vyin Chat Dashboard. The `gim` property is a `JSON` object which contains all the information about the message a user has sent. Within `MyFirebaseMessagingService.kotlin`, you can show the parsed messages to users as a notification using your custom `sendNotification()` method.

Note: See Firebase’s [Receive messages in an iOS app](https://firebase.google.com/docs/cloud-messaging/ios/receive) guide to learn more about how to implement code to receive and parse a [FCM notification message](https://firebase.google.com/docs/cloud-messaging/concept-options), how notification messages are handled depending on the state of the receiving app, or how to override the [`onMessageReceived`](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessagingService#onMessageReceived(com.google.firebase.messaging.RemoteMessage))) method.  

```swift
func process(_ notification: UNNotification) {
    let userInfo = notification.request.content.userInfo
    guard let gimPayload = userInfo["gim"] as? [String: Any],
          let channel = gimPayload["channel"] as? [String: Any],
          let channelUrl = channel["channel_url"] as? String,
          let messageTitle = gimPayload["push_title"] as? String,
          let messageBody = gimPayload["message"] as? String else { return }
    // Handle the notification with channelUrl, messageTitle, messageBody.
}
```

The following is a complete payload format of the `gim` property, which contains a set of provided key-value items. Some fields in the push notification payload can be customized in Settings \> Chat \> Notifications on Vyin Chat Dashboard. For example, `push_title` and `push_alert` are created based on Title and Body text you set in Push notification content templates, respectively, in the Push notifications menu. In order to display them in a local notification, use `push_title` and `push_alert` of the push notification payload to configure the notification content in your `UNMutableNotificationContent`. Also, the `channel_unread_count` field can be added into or removed from the payload in the same menu on the Vyin Chat Dashboard.  

```json
{
    "category": "messaging:offline_notification",
    "type": "String",                   // Message type: MESG, FILE, or ADMM
    "message": "String",                // User input message
    "custom_type": "String",            // Custom message type
    "message_id": "Int64",              // Message ID
    "created_at": "Int64",              // The time that the message was created, in a 13-digit Unix milliseconds format
    "app_id": "String",                 // Application's unique ID
    "unread_message_count": "int",      // Total number of new messages unread by the user
    "channel": {
        "channel_url": "String",        // Group channel URL
        "name": "String",              // Group channel name
        "custom_type": "String",       // Custom Group channel type
        "channel_unread_count": "int"  // Total number of unread new messages from the specific channel
    },
    "channel_type": "String",           // messaging = distinct group channel, group_messaging = private group channel, chat = all other cases
    "sender": {
        "id": "String",                // Sender's unique ID
        "name": "String",             // Sender's nickname
        "profile_url": "String",      // Sender's profile image URL
        "require_auth_for_profile_image": false,
        "metadata": {}
    },
    "recipient": {
        "id": "String",                // Recipient's unique ID
        "name": "String"              // Recipient's nickname
    },
    "files": [],                        // An array of data regarding the file message, such as filename
    "translations": {},                 // The items of locale:translation
    "push_title": "String",            // Title of a notification message, customizable on Vyin Chat Dashboard
    "push_alert": "String",            // Body text of a notification message, customizable on Vyin Chat Dashboard
    "push_sound": "String",            // The location of a sound file for notifications
    "audience_type": "String",          // The type of audiences for notifications
    "mentioned_users": {
        "user_id": "String",           // The ID of a mentioned user
        "nickname": "String",         // The nickname of a mentioned user
        "profile_url": "String",      // Mentioned user's profile image URL
        "require_auth_for_profile_image": false
    }
}
```
---

### Step 5: Enable multi-device support on Vyin Chat Dashboard

After the above implementation has been completed, multi-device support should be enabled on Vyin Chat Dashboard by going to Settings \> Application \> Notifications \> Push notifications for multi-device users.  
To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Turning on FCM push notifications for multi-device users on Vyin Chat Dashboard.

## Set up push notifications for Hms

Currently We do not support HMS devices.

