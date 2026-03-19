# Vyin Chat SDK for Android - Push Notifications

## Overview

Using Chat SDK for Android, you can send notification messages to your user's device when the device is either idle or running the client app in the background. For group channels, notifications can be configured to display an alert, play a sound, or place a badge on the client app's icon.

Push notifications for Android client apps are sent using Firebase Cloud Messaging (FCM) which contains custom data that your app needs in order to respond to the notifications. When a message is sent to the Vyin Chat server through our Chat SDK, the server communicates with FCM which then delivers a push notification to an Android device where the client app is installed.

By default, when a user's device is disconnected from the Vyin Chat server, the server sends push notification requests to FCM for the messages. The Chat SDK automatically detects when a user's client app goes into the background from the foreground, and updates the user's connection status to disconnected. Therefore, under normal circumstances, there is no need to explicitly call the `disconnect()` method.

Push notifications support both single and multi-device users and they are delivered only when a user is fully offline from all of their devices. In other words, if a user is online on one or more devices, notifications aren't delivered and thus not displayed on any device.

---

## Push notification with multi-device support

Push notifications with multi-device support provide the same features as our general push notifications, but with additional support for multi-device users. When implemented, notifications are delivered to all online and offline devices of a multi-device user but through our Chat SDK, push notifications are displayed only on offline devices and ignored by online devices. As a result, client apps are able to display push notifications on all offline devices, regardless of whether there are one or more online devices.

For example, let's say a multi-device user who has six devices with your client app installed is online on one of the devices and offline on the remaining five. When general push notifications are implemented, notifications aren't delivered to any of their devices. But when push notifications with multi-device support are implemented, notifications are delivered to all six devices and displayed on the five offline devices.

By default, when a user's device is disconnected from the Vyin Chat server, the server sends push notification requests to FCM for the messages. The Chat SDK automatically detects when a user's client app goes into the background from the foreground, and updates the user's connection status to disconnected. Therefore, under normal circumstances, there is no need to call the `disconnect()` method.

To find out which type of push notification best fits your use case, see the following table.

|  | General push notifications | Push notifications with multi-device support |
| :---- | :---- | :---- |
| Sent when | All devices are offline. | At least one device is offline. |
| Notification messages | Single-device user: Displayed when disconnected from the Vyin Chat server and thus offline. Multi-device user: Displayed only when all devices are offline. | Single-device user: Displayed when disconnected from the Vyin Chat server and thus offline. Multi-device user: Displayed on all offline devices, regardless of whether one or more are online. |
| SDK's event callback | Invoked only when the app is connected to the Vyin Chat server. | Invoked only when the app is connected to the Vyin Chat server. |
| App instance's registration token | Can be manually registered to the Vyin Chat server. | Automatically registered to the Vyin Chat server. |
| Notification preferences | Can be set by application and group channel. | N/A |
| Push notification templates | Supported. | Supported. |
| Push notification translation | Supported by [Google Cloud Translation API](https://cloud.google.com/translate/). | Supported by [Google Cloud Translation API](https://cloud.google.com/translate/). |

---

## Functionalities by topic

Users can set their preferences for receiving notifications in their devices. The following is a list of functionalities that our Chat SDK provides.

#### Managing notifications

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Set up push notifications for FCM | Users can receive FCM messages in their Android app. | ✓ | |
| Configure push notification preferences | Turns push notifications on or off in a user's app using the user's registration token. | | ✓ |
| Push notification content templates | Displays customized push notification messages on a user's device using templates. | | ✓ |
| Push notification tester | Test on Vyin Chat Console whether the push notification credentials and notification services are working properly. | | ✓ |
| Push notification translation | Users can receive notification messages translated into their preferred languages. | | ✓ |

#### Additional support for multi-device users

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Set up push notifications for FCM | Multi-device users can receive FCM messages on multiple devices. | | ✓ |

---

## Set up push notifications for FCM

There are two types of [FCM messages](https://firebase.google.com/docs/cloud-messaging/concept-options): notification messages and data messages. Vyin Chat uses data messages, which are handled by the client app. They allow users to customize the message payload, which consists of key-value items.

The following is a set of step-by-step instructions on how to set up push notifications for FCM.

- Step 1: Generate private key for FCM
- Step 2: Register private key to Vyin Chat Console
- Step 3: Set up an FCM client app on your Android project
- Step 4: Register a registration token to the Vyin Chat server
- Step 5: Handle an FCM message payload

---

### Step 1: Generate private key for FCM

The Vyin Chat server requires your private key to send notification requests to FCM on behalf of your server. This is required for FCM to authorize HTTP requests.

> **Note:** If you already have your private key, skip this step and go directly to Step 2: Register private key to Vyin Chat Console.

1. Go to the [Firebase console](https://console.firebase.google.com/). If you don't have a Firebase project for a client app, create a new project.

2. Select your project card to move to the Project Overview.

3. Click the gear icon at the upper left corner and select **Project settings**.

4. Go to **Service accounts** and click on **Generate a new private key**.

5. Go to the **General** tab and select your Android app to add Firebase to. During the registration process, enter your package name, download the `google-services.json` file, and place it in your Android app module root directory.

---

### Step 2: Register private key to Vyin Chat Console

Register your private key to the Vyin Chat server through the dashboard as follows.

1. Sign in to your dashboard and go to **Settings > Application > Push notifications**.
2. Turn on **Push notifications** and select **Send when all devices are offline**.
3. Scroll down to the **FCM** section and click on **Add credentials**.
4. Under **Service account key (HTTP v1)**, upload the JSON file containing the key that was downloaded in Step 1.

> **Note:** Your private key can also be registered using our add an FCM push configuration API.

---

### Step 3: Set up an FCM client app on your Android project

Add the following dependency for the Cloud Messaging Android library to your `build.gradle` file.

> **Note:** The firebase-messaging version should be 19.0.1 or higher.

```kotlin
dependencies {
    // ...

    implementation 'com.google.firebase:firebase-messaging:23.0.1'
}
```

> **Note:** To learn more about this step, refer to Firebase's [Set Up a Firebase Cloud Messaging client app on Android](https://firebase.google.com/docs/cloud-messaging/android/client#set-up-firebase-and-the-fcm-sdk) guide. The [Google FCM sample project](https://github.com/firebase/quickstart-android/tree/master/messaging) is another helpful reference.

---

### Step 4: Register a registration token to the Vyin Chat server

In order to send notification messages to a specific client app on an Android device, FCM requires an app instance's registration token which has been issued by the client app. Therefore, the Vyin Chat server also needs every registration token of client app instances to send notification requests to FCM on behalf of your server.

A user can have up to 20 FCM registration tokens. If a user who already has the maximum number of tokens attempts to add another one, the newest token replaces the oldest.

Upon the initialization of your app, the FCM SDK generates a unique, app-specific registration token for the client app instance on your user's device. FCM uses this registration token to determine which device to send notification messages to. After the FCM SDK has successfully generated the registration token, it is passed to the `onNewToken()` callback. Registration tokens must be registered to the Vyin Chat server by passing it as an argument to the parameter in the `registerPushToken()` method as in the following code.

```kotlin
override fun onNewToken(token: String) {
    GIMChat.registerPushToken(token) { status, e ->
        if (e != null) {
            // Handle error.
        }

        if (status == PushTokenRegistrationStatus.PENDING) {
            // A token registration is pending.
            // Try registering again after a connection has been successfully established.
        }
    }
}
```

> **Note:** If `PushTokenRegistrationStatus.PENDING` is returned through the handler, this means that your user isn't being connected to the Vyin Chat server when `registerPushToken()` is called. In this case, you must first get a pending registration token using `getToken()`, and then register the token by calling the `registerPushToken()` method in the `onSuccess()` callback when your user has been connected to the server.

```kotlin
GIMChat.connect(USER_ID) { user, e ->
    if (e != null) {
        // Handle error.
    }
    FirebaseMessaging.getInstance().token.addOnCompleteListener { task ->
        GIMChat.registerPushToken(task.result) { status, e ->
            if (e != null) {
                // Handle error.
            }

            // ...
        }
    }
}
```

---

### Step 5: Handle an FCM message payload

The Vyin Chat server sends push notification payloads as [FCM data messages](https://firebase.google.com/docs/cloud-messaging/concept-options), which contain notification-related data in the form of key-value pairs. Unlike notification messages, the client app needs to parse and process these data messages in order to display them as local notifications.

The following code shows how to receive a push notification payload and parse it as a local notification. The payload consists of two properties: `message` and `gim`. The `message` property is a string generated according to a push notification template you set on the Vyin Chat Console. The `gim` property is a JSON object which contains all the information about the message a user has sent.

> **Note:** See Firebase's [Receive messages in an Android app](https://firebase.google.com/docs/cloud-messaging/android/receive) guide to learn more about how to implement code to receive and parse an FCM notification message.

```kotlin
override fun onMessageReceived(remoteMessage: RemoteMessage) {
    try {
        if (remoteMessage.getData().containsKey("gim")) {
            val gim = JSONObject(remoteMessage.getData().get("gim"))
            val channel = gim.get("channel") as JSONObject
            val channelUrl = channel["channel_url"] as String
            val messageTitle = gim.get("push_title") as String
            val messageBody = gim.get("message") as String
            // If you want to customize a notification with the received FCM message,
            // write your method like sendNotification() below.
            sendNotification(context, messageTitle, messageBody, channelUrl)
        }
    } catch (e: JSONException) {
        e.printStackTrace()
    }
}

fun sendNotification(
    context: Context,
    messageTitle: String,
    messageBody: String,
    channelUrl: String
) {
    // Implement your own way to create and show a notification containing the received FCM message.
    val notificationBuilder = NotificationCompat.Builder(context, channelUrl)
        .setSmallIcon(R.drawable.img_notification)
        .setColor(Color.parseColor("#7469C4")) // small icon background color
        .setLargeIcon(BitmapFactory.decodeResource(context.resources, R.drawable.logo_gim))
        .setContentTitle(messageTitle)
        .setContentText(messageBody)
        .setAutoCancel(true)
        .setSound(defaultSoundUri)
        .setPriority(Notification.PRIORITY_MAX)
        .setDefaults(Notification.DEFAULT_ALL)
        .setContentIntent(pendingIntent)
}
```

The following is a complete payload format of the `gim` property, which contains a set of provided key-value items. Some fields in the push notification payload can be customized in **Settings > Chat > Notifications** on the Vyin Chat Console.

```json
{
    "category": "messaging:offline_notification",
    "type": "string",                        // Message type: MESG, FILE, or ADMM
    "message": "string",                     // User input message
    "custom_type": "string",                 // Custom message type
    "message_id": "long",                    // Message ID
    "created_at": "long",                    // The time that the message was created, in a 13-digit Unix milliseconds format
    "app_id": "string",                      // Application's unique ID
    "unread_message_count": "int",           // Total number of new messages unread by the user
    "channel": {
        "channel_url": "string",             // Group channel URL
        "name": "string",                    // Group channel name
        "custom_type": "string",             // Custom Group channel type
        "channel_unread_count": "int"        // Total number of unread new messages from the specific channel
    },
    "channel_type": "string",               // messaging = distinct group channel, group_messaging = private group channel, chat = all other cases
    "sender": {
        "id": "string",                      // Sender's unique ID
        "name": "string",                    // Sender's nickname
        "profile_url": "string",             // Sender's profile image URL
        "require_auth_for_profile_image": false,
        "metadata": {}
    },
    "recipient": {
        "id": "string",                      // Recipient's unique ID
        "name": "string"                     // Recipient's nickname
    },
    "files": [],                             // An array of data regarding the file message, such as filename
    "translations": {},                      // The items of locale:translation
    "push_title": "string",                  // Title of a notification message that can be customized on the Vyin Chat Console
    "push_sound": "string",                  // The location of a sound file for notifications
    "audience_type": "string",              // The type of audiences for notifications
    "mentioned_users": {
        "user_id": "string",                 // The ID of a mentioned user
        "nickname": "string",                // The nickname of a mentioned user
        "profile_url": "string",             // Mentioned user's profile image URL
        "require_auth_for_profile_image": false
    }
}
```

---

## Configure push notification preferences

By registering or unregistering the current user's registration token to the Vyin Chat server as below, you can turn push notifications on or off in the user's client app. The registration and unregistration methods in the code below should be called after a user has established a connection with the Vyin Chat server by calling the `GIMChat.connect()` method.

Through `PushTriggerOption`, the current user can choose which type of messages will trigger push notifications, or opt to turn it off completely. Additionally, they can enable the Do Not Disturb and snooze features through `setDoNotDisturb()` and `setSnoozePeriod()` methods.

```kotlin
fun setPushNotification(enable: Boolean) {
    if (enable) {
        GIMChat.registerPushToken(pushToken) { status, e ->
            if (e != null) {
                // Handle error.
            }
        }
    } else {
        // If you want to unregister the current device only, call this method.
        GIMChat.unregisterPushToken(pushToken) { e ->
            if (e != null) {
                // Handle error.
            }

            // ...
        }

        // If you want to unregister all devices of the user, call this method.
        GIMChat.unregisterPushTokenAll { e ->
            if (e != null) {
                // Handle error.
            }

            // ...
        }
    }
}
```

The `GIMChat.PushTriggerOption` method allows users to configure when to receive notification messages as well as what types of messages trigger notification messages on the application level. The following are the available options.

#### List of push trigger options

| Option | Description |
| :---- | :---- |
| `ALL` | When disconnected from the Vyin Chat server, the current user receives notifications for all new messages including messages the user has been mentioned in. |
| `MENTION_ONLY` | When disconnected from the Vyin Chat server, the current user only receives notifications for messages the user has been mentioned in. |
| `OFF` | The current user doesn't receive any notifications. |

```kotlin
// Send notifications only when the current user is mentioned in messages.
GIMChat.setPushTriggerOption(GIMChat.PushTriggerOption.MENTION_ONLY) { e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

The `GroupChannel.PushTriggerOption` method also allows users to configure the trigger for notification messages as well as turn notifications on or off for each group channel. The following are the available options.

| Option | Description |
| :---- | :---- |
| `DEFAULT` | The current user's push notification trigger settings are automatically applied to the channel. This is the default setting. |
| `ALL` | When disconnected from the Vyin Chat server, the current user receives notifications for all new messages in the channel including messages the user has been mentioned in. |
| `MENTION_ONLY` | When disconnected from the Vyin Chat server, the current user only receives notifications for messages in the channel the user has been mentioned in. |
| `OFF` | The current user doesn't receive any notifications in the channel. |

```kotlin
// Apply the user's push notification setting to the channel.
groupChannel.setMyPushTriggerOption(GroupChannel.PushTriggerOption.DEFAULT) { e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

---

### Do not disturb

If you want to routinely turn off push notifications on the current user's client app according to a specified schedule, use our Do Not Disturb feature like the following.

> **Note:** The Do Not Disturb feature can also be set for a user with our update push notification preferences API.

```kotlin
// The current user won't be receiving notification messages during the specified time of the day.
GIMChat.setDoNotDisturb(true, START_HOUR, START_MIN, END_HOUR, END_MIN, TimeZone.getDefault().getID()) { e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

---

### Snooze

To snooze notification messages for a specific period of time, use our snooze feature as below.

> **Note:** The snooze notifications can also be set for a user with the update push preferences API.

```kotlin
// The current user won't be receiving notification messages during the specified period of time from START_TS to END_TS.
GIMChat.setSnoozePeriod(true, START_TS, END_TS) { e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

---

## Push notification content templates

Push notification content templates are pre-formatted forms that can be customized to display your own push notification messages on a user's device. Vyin Chat provides two types: default and alternative. Both templates can be customized in **Settings > Chat > Push notifications > Push notification content templates** on Vyin Chat Console.

As for Android, an additional implementation is required in order to display your customized push notifications. The push notification payload sent from the Vyin Chat server includes the content of the template, such as the title and message, and the client app can determine how to present the notification through Android-provided code. For more information on how to create a custom notification, refer to [Notifications](https://developer.android.com/guide/topics/ui/notifiers/notifications) in Android's documentation for app developers.

---

### Content templates

There are two types of push notification content template: default and alternative. The default template applies to users with the initial notification message setting. The alternative template is used when a user selects a different notification content template instead of the default template.

The content in the template is set at the application level while the selection of templates is determined by a user or through the Platform API.

> **Note:** When a custom channel type has its own customized push notification content template, it takes precedence over the default or alternative template.

Both templates can be customized using the variables of `sender_name`, `filename`, `message`, `channel_name`, and `file_type_friendly` which are replaced with the corresponding string values. As for `file_type_friendly`, it indicates the type of the file sent, such as Audio, Image, and Video.

#### List of content templates

|  | Text message | File message |
| :---- | :---- | :---- |
| Default template | `{sender_name}: {message}` — An example can be `Cindy: Hi!` | `{filename}` — An example can be `hello_world.jpg` |
| Alternative template | `New message arrived` | `New file arrived` |

---

## Push notification tester

After registering a credential for push notification services, you can test on Vyin Chat Console whether the push notification credentials and notification services work properly.

In **Settings > Notifications > Push notification credentials** on the dashboard, each credential has a **Send** button that allows you to send a test message. The test message is sent to a target user and their device, which sets off a push notification on the device. At the same time, a new group channel containing only the target user is created for this test. If you've already tested push notification with the same target user and the same target device token, the message is sent to the existing test channel.

> **Note:** If there is another member in the test channel or the target user has left the channel, delete the test channel before testing.

1. Go to **Settings > Notifications > Push notification credentials** on Vyin Chat Console. Find a **Send** button in the right-end column of each table.

2. You can search for a target user with either their User ID or Nickname in the popup window. Once a user has been selected, choose a target device from a list of device tokens linked to the user.

3. Double check to see if you've selected the correct device token of the user because this push notification shows up on an actual device. Confirm the target channel name and URL, then send the message.

> **Note:** If this is your first test, a new test channel is created. For future tests with the same target user and the same target device token, the name and URL of the existing test channel will appear above the message box.

4. Check to see if the push notification has arrived on the mobile device or emulator. The test message should include the time when the Vyin Chat server sent the message.

```
Test message sent at {HH:MM:SS} on {MM-DD-YYYY}
```

> **Note:** In order to display push notifications on Android devices and emulators, an additional implementation is required on the client app. To learn more, see Push notification for FCM.

---

## Push notification translation

Push notification translation allows your users to receive notification messages in their preferred languages. Users can set up to four preferred languages. If messages are sent in one of those languages, the notification messages won't be translated. If messages are sent in a language other than the preferred languages, the notification messages are translated into the user's first preferred language. In addition, the messages translated into other preferred languages are provided in the `gim` property of a notification message payload.

> **Note:** A specific user's preferred languages can be set using the update a user API.

The following shows how to set the current user's preferred languages using the `updateCurrentUserInfo()` method.

```kotlin
val preferredLanguages = listOf("fr", "de", "es", "ko") // French, German, Spanish, Korean
GIMChat.updateCurrentUserInfo(preferredLanguages) { e ->
    if (e != null) {
        // Handle error.
    }

    // The current user's preferred languages have been updated successfully.
}
```

If successful, the following notification message payload is delivered to the current user's device.

```json
{
    "message": {
        "token": "ad3nNwPe3H0:ZI3k_HHwgRpoDKCIZvvRMExUdFQ3P1...",
        "notification": {
            "title": "Greeting!",
            "body": "Bonjour comment allez vous"    // A notification message is translated into the first language listed in the preferred languages.
        }
    },
    "gim": {
        "category": "messaging:offline_notification",
        "type": "User",
        "message": "Hello, how are you?",
        "translations": {
            "fr": "Bonjour comment allez vous",
            "de": "Hallo wie geht es dir",
            "es": "Hola como estas"
        }
    }
}
```

---

## Marking push notifications as delivered

You can track whether push notifications have been successfully delivered to all the intended devices by the Vyin Chat server. Using the `VyinPushHelper.markPushNotificationAsDelivered()` method, you can mark a push notification as delivered to a device. To ensure proper functionality, the `GIMChat.init()` method must be called within the `Application.onCreate()` method.

In order to check the delivery rate of push notifications, the SDK retrieves the tracking ID for push notifications and sends it to the server to identify which push notification has been successfully delivered to the device.

```kotlin
class FirebaseMessagingServiceEx : FirebaseMessagingService() {
    override fun onMessageReceived(remoteMessage: RemoteMessage) {
        // GIMChat.init() must be called prior to this call.
        VyinPushHelper.markPushNotificationAsDelivered(remoteMessage.data)
    }
}
```

> **Note:** If your app is using multi-device support and has implemented `GIMPushHandler`, this will be called internally so that the app doesn't have to handle it directly.

> **Note:** This feature is not yet implemented.

---

## Multi-device support — Set up push notifications for FCM

This section covers the step-by-step instructions for push notifications with multi-device support for FCM.

- Step 1: Generate private key for FCM
- Step 2: Register private key to Vyin Chat Console
- Step 3: Set up Firebase and the FCM SDK
- Step 4: Implement multi-device support in your Android app
- Step 5: Handle an FCM message payload
- Step 6: Enable multi-device support on Vyin Chat Console

---

### Step 1: Generate private key for FCM

The Vyin Chat server requires your private key to send notification requests to FCM on behalf of your server. This is required for FCM to authorize HTTP requests.

If you already have your server key, you can skip this step and go directly to Step 2: Register private key to Vyin Chat Console.

1. Go to the [Firebase console](https://console.firebase.google.com/). If you don't have a Firebase project for your client app, create a new project.

2. Select your project card to move to Project Overview.

3. Click the gear icon at the upper left corner and select **Project settings**.

4. Go to **Service accounts** and click on **Generate a new private key**.

5. Go to the **General** tab and select your Android app to add Firebase to. During the registration process, enter your package name, download the `google-services.json` file, and place it in your Android app module root directory.

---

### Step 2: Register private key to Vyin Chat Console

Register your private key to Vyin Chat Console as follows. Your private key can also be registered using the add an FCM push configuration API.

1. Sign in to your dashboard and go to **Settings > Chat > Push notifications**.
2. Turn on **Notifications** and select **Send to devices both offline and online**.
3. Click **Add credentials** and register the app ID and app secret acquired at Step 1.

---

### Step 3: Set up Firebase and the FCM SDK

Add the following dependency for the Cloud Messaging Android library to your `build.gradle` file.

> **Note:** The firebase-messaging version should be 19.0.1 or higher.

```kotlin
dependencies {
    // ...
    implementation 'com.google.firebase:firebase-messaging:23.0.1'
}
```

The Chat SDK writes and declares push notifications with multi-device support in the manifest while you build your client app. If you declare another push service that extends `FirebaseMessagingService` in your client app's manifest, multi-device support won't work in the app.

> **Note:** To learn more about this step, refer to Firebase's [Set Up a Firebase Cloud Messaging client app on Android](https://firebase.google.com/docs/cloud-messaging/android/client#set-up-firebase-and-the-fcm-sdk) guide.

---

### Step 4: Implement multi-device support in your Android app

The following classes and interface are provided to implement push notifications with multi-device support.

| Class or interface | Description |
| :---- | :---- |
| `GIMPushHandler` | A class that provides the `onNewToken()` and `onMessageReceived()` callbacks as well as other callbacks to handle a user's registration token and receive notification messages from FCM. |
| `VyinPushHelper` | A class that provides the methods to register and unregister a `GIMPushHandler` handler, check if the same message has already been received, and more. |
| `OnPushTokenReceiveListener` | An interface that contains the `onReceived()` callback to receive a user's registration token from FCM. |

These are used to inherit your `MyFirebaseMessagingService` class from the `GIMPushHandler` class and implement the following.

```kotlin
class MyFirebaseMessagingService : GIMPushHandler() {
    // This is invoked when a notification message has been delivered to the current user's client app.
    override fun onMessageReceived(context: Context, remoteMessage: RemoteMessage) {
        try {
            val pushTitle = remoteMessage.data["push_title"]
            val message = remoteMessage.data["message"]
            val payload = remoteMessage.data["gim"] ?: return
            val gimJson = JSONObject(payload)
            val channelJson = gimJson.getJSONObject("channel")
            val channelUrl = channelJson.getString("channel_url")

            // If you want to customize a notification with the received FCM message,
            // write your method like sendNotification() below.
            sendNotification(context, pushTitle, message, channelUrl)
        } catch (e: JSONException) {
            e.printStackTrace()
        }
    }

    override fun onNewToken(newToken: String?) {
        pushToken = newToken
    }

    override val isUniquePushToken: Boolean
        get() = false

    // The alwaysReceiveMessage() method determines whether push notifications for new messages
    // are delivered even when the app is in foreground.
    // The default is false, meaning push notifications are delivered
    // only when the app is in the background.
    override fun alwaysReceiveMessage(): Boolean {
        return false
    }

    fun sendNotification(context: Context, messageTitle: String?, messageBody: String?, channelUrl: String) {
        // Customize your notification containing the received FCM message.
    }

    companion object {
        var pushToken: String? = null
        fun getPushToken(callback: ((pushToken: String?, e: GIMException?) -> Unit)) {
            if (!pushToken.isNullOrEmpty()) {
                callback(pushToken, null)
                return
            }

            VyinPushHelper.getPushToken { token, e ->
                if (token != null) {
                    pushToken = token
                }
                callback(pushToken, e)
            }
        }
    }
}
```

> **Note:** Upon initial startup of your app, the FCM SDK generates a unique and app-specific registration token for the client app instance on your user's device. FCM uses this registration token to determine which device to send notification messages to.

In order to receive information about push notification events for the current user from the Vyin Chat server, register a `MyFireBaseMessagingService` instance to `VyinPushHelper` as an event handler.

To receive a push notification when the app is in the background or closed, you must register the `MyFireBaseMessagingService` instance. This instance isn't dependent on Vyin Chat SDK, so even in cases where `GIMChat.init()` isn't called in `Application`, `MyFireBaseMessagingService` must be registered in the `Application` instance's `onCreate()` method as shown in the code below.

```kotlin
class MyApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        GIMChat.init(
            InitParams(APP_ID, applicationContext, true),
            object : InitResultHandler {
                override fun onMigrationStarted() {}
                override fun onInitFailed(e: GIMException) {}
                override fun onInitSucceed() {}
            }
        )
        // Not dependent on GIMChat.init(). Must be called in Application.
        VyinPushHelper.registerPushHandler(MyFirebaseMessagingService())
    }
}
```

Also, register a `MyFireBaseMessagingService` instance when a user logs into the Vyin Chat server as follows.

```kotlin
GIMChat.connect(USER_ID) { user, e ->
    if (e != null) {
        // Handle error.
    }
    // ...

    VyinPushHelper.registerPushHandler(MyFirebaseMessagingService())
}
```

The instance should be unregistered when a user logs out from the Vyin Chat server as follows.

```kotlin
VyinPushHelper.unregisterPushHandler(
    IS_UNREGISTER_ALL,
    object : VyinPushHelper.OnPushRequestCompleteListener {
        override fun onComplete(isRegistered: Boolean, token: String?) {
            GIMChat.disconnect {
                // Clear user-related data.
            }
        }

        override fun onError(e: GIMException) {
            // Handle error.
        }
    }
)
```

---

### Step 5: Handle an FCM message payload

The Vyin Chat server sends push notification payloads as [FCM data messages](https://firebase.google.com/docs/cloud-messaging/concept-options), which contain notification-related data in the form of key-value pairs. Unlike notification messages, the client app needs to parse and process these data messages in order to display them as local notifications.

```kotlin
override fun onMessageReceived(remoteMessage: RemoteMessage) {
    try {
        if (remoteMessage.getData().containsKey("gim")) {
            val gim = JSONObject(remoteMessage.getData().get("gim"))
            val channel = gim.get("channel") as JSONObject
            val channelUrl = channel["channel_url"] as String
            val messageTitle = gim.get("push_title") as String
            val messageBody = gim.get("message") as String
            // If you want to customize a notification with the received FCM message,
            // write your method like sendNotification() below.
            sendNotification(context, messageTitle, messageBody, channelUrl)
        }
    } catch (e: JSONException) {
        e.printStackTrace()
    }
}

fun sendNotification(
    context: Context,
    messageTitle: String,
    messageBody: String,
    channelUrl: String
) {
    // Implement your own way to create and show a notification containing the received FCM message.
    val notificationBuilder = NotificationCompat.Builder(context, channelUrl)
        .setSmallIcon(R.drawable.img_notification)
        .setColor(Color.parseColor("#7469C4")) // small icon background color
        .setLargeIcon(BitmapFactory.decodeResource(context.resources, R.drawable.logo_gim))
        .setContentTitle(messageTitle)
        .setContentText(messageBody)
        .setAutoCancel(true)
        .setSound(defaultSoundUri)
        .setPriority(Notification.PRIORITY_MAX)
        .setDefaults(Notification.DEFAULT_ALL)
        .setContentIntent(pendingIntent)
}
```

The following is a complete payload format of the `gim` property.

```json
{
    "category": "messaging:offline_notification",
    "type": "string",                // Message type: MESG, FILE, or ADMM
    "message": "string",             // User input message
    "custom_type": "string",         // Custom message type
    "message_id": "long",            // Message ID
    "created_at": "long",            // The time that the message was created, in a 13-digit Unix milliseconds format
    "app_id": "string",              // Application's unique ID
    "unread_message_count": "int",   // Total number of new messages unread by the user
    "channel": {
        "channel_url": "string",     // Group channel URL
        "name": "string",            // Group channel name
        "custom_type": "string",     // Custom Group channel type
        "channel_unread_count": "int" // Total number of unread new messages from the specific channel
    },
    "channel_type": "string",        // messaging = distinct group channel, group_messaging = private group channel, chat = all other cases
    "sender": {
        "id": "string",              // Sender's unique ID
        "name": "string",            // Sender's nickname
        "profile_url": "string",     // Sender's profile image URL
        "require_auth_for_profile_image": false,
        "metadata": {}
    },
    "recipient": {
        "id": "string",              // Recipient's unique ID
        "name": "string"             // Recipient's nickname
    },
    "files": [],                     // An array of data regarding the file message, such as filename
    "translations": {},              // The items of locale:translation
    "push_title": "string",          // Title of a notification message
    "push_alert": "string",          // Body text of a notification message
    "push_sound": "string",          // The location of a sound file for notifications
    "audience_type": "string",       // The type of audiences for notifications
    "mentioned_users": {
        "user_id": "string",         // The ID of a mentioned user
        "nickname": "string",        // The nickname of a mentioned user
        "profile_url": "string",     // Mentioned user's profile image URL
        "require_auth_for_profile_image": false
    }
}
```

---

### Step 6: Enable multi-device support on Vyin Chat Console

After the above implementation has been completed, multi-device support should be enabled on Vyin Chat Console by going to **Settings > Application > Notifications > Push notifications for multi-device users**.

---

## HMS push notifications

> **Note:** HMS push notifications are not yet supported.

HMS (Huawei Mobile Services) push notification integration is planned for a future release. At this time, only FCM-based push notifications are available. Please use the FCM setup instructions above for your Android push notification implementation.
