# Vyin Chat SDK for Android - Get Started

# Send first message

---

## Get started

To send a message in a client app, you should build and configure an in-app chat using Vyin Chat SDK, which can be installed through [Gradle](https://gradle.org/) or by accessing our remote repository.

### Step 1-1 Install the Chat SDK - Configuring the Repository

You can install the Chat SDK for Android through [`Gradle`](https://gradle.org/).

#### Gradle 6.8 or higher

If you are using Gradle 6.8 or higher, add the following to your `settings.gradle` file:

```kotlin
dependencyResolutionManagement {
    repositories {
        maven { setUrl("https://maven.pkg.github.com/gamania-beanfun/gim-chat-sdk-android") }
    }
}
```

#### Gradle 6.7 or lower

If you are using Gradle 6.7 or lower, add the following code to your root `build.gradle` file:

```kotlin
allprojects {
    repositories {
        maven { setUrl("https://maven.pkg.github.com/gamania-beanfun/gim-chat-sdk-android") }
    }
}
```

Note: Make sure the above code block isn't added to your module `build.gradle` file.

### Step 1-2 Install the Chat SDK - Adding the Dependency

Add the dependency to your module `build.gradle` file.

```kotlin
dependencies {
    implementation("com.gamania.gim:gim-chat-development:LATEST_VERSION")
}
```

### Step 2 Request to access system permissions

The Chat SDK requires system permissions. These permissions allow the Chat SDK to communicate with the Vyin Chat server and read from and write on a user device's storage. To request system permissions, add the following lines to your `AndroidManifest.xml` file.

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

### Step 3 Initialize the Chat SDK

Now, initialize the Chat SDK in the app to allow the Chat SDK to respond to changes in the connection status of Android client apps. Initialization requires your Vyin Chat application's Application ID, which can be found on the Vyin Chat Console.

Note: All methods in the following steps are asynchronous. This means that when using asynchronous methods, your client app must receive success callbacks from the Vyin Chat server through their callback handlers in order to proceed to the next step. A good way to do this is using nested methods. Go to Step 6: Enter the channel to learn more about how you can nest `openChannel.enter()` inside the `OpenChannel.getChannel()` method.

Before initializing, you should determine whether to enable local caching using `InitParams` and `InitResultHandler()` in the `GIMChat.init()` method. The `useCaching` property of `InitParams` determines whether or not the client app is going to use local storage through the SDK. If you want to build a client app without our local caching functionalities, set the value of `useCaching` to `false`.

Note: If you aren't using local caching, `onMigrationStarted()` and `onInitFailed()` in `InitResultHandler()` won't be called. To learn more, see Initialize the Chat SDK with APP_ID.

```kotlin
GIMChat.init(
    InitParams(APP_ID, applicationContext, useCaching = true),
    object : InitResultHandler {
        override fun onMigrationStarted() {
            Log.i("Application", "Called when there's an migration in local cache.")
        }

        override fun onInitFailed(exception: GIMException) {
            Log.i("Application", "Called when initialize failed. SDK will still operate properly as if useLocalCaching is set to false.")
        }

        override fun onInitSucceed() {
            Log.i("Application", "Called when initialization is completed.")
        }
    }
)
```

The `GIMChat.init()` method must be called at least once across a client app. It is recommended to initialize the Chat SDK using the `onCreate()` method of the [`Application`](https://developer.android.com/reference/android/app/Application) instance.

While it is also possible to invoke `GIMChat.init()` from the `onCreate()` method of an [`Activity`](https://developer.android.com/reference/android/app/Activity) or a [`Fragment`](https://developer.android.com/reference/android/app/Fragment) as shown below, this approach requires additional considerations. For instance, if you need to mark a push notification as delivered when the app is in the background or terminated, an `Activity` or `Fragment` may not be created, leaving only the `Application` active to call the `GIMChat.init()` method. For more details, please refer to the Mark push notifications as delivered page.

```kotlin
GIMChat.init(
    InitParams("APP_ID", applicationContext, useCaching = true).apply {
        // let SDK know it's called from foreground
        isForeground = true
    },
    object : InitResultHandler {
        override fun onMigrationStarted() {
            Log.i("Application", "Called when there's an update in Vyin Chat server.")
        }

        override fun onInitFailed(exception: GIMException) {
            Log.i("Application", "Called when initialize failed. SDK will still operate properly as if useLocalCaching is set to false.")
        }

        override fun onInitSucceed() {
            Log.i("Application", "Called when initialization is completed.")
        }
    }
)
```

### Step 4 Connect to the Vyin Chat server

You need a user connected to the Vyin Chat server first before sending a message to a channel.

- If you already have a user in your Vyin Chat application: In `connect()`, specify a user ID from the user list in your Vyin Chat application.
- If you don't have an existing user: Specifying a unique `userId` in `connect()` will automatically generate a new user in your Vyin Chat application before being connected.

Note: Vyin Chat supports user authentication via access tokens, but defaults to allowing access without a token for ease of initial use. For enhanced security, we recommend adjusting access settings under Settings > Application > Security > Access token permission on the console for new users. To learn about access token and authentication, see the Authentication guide.

```kotlin
// Use the ID of a user you've created on the console.
// If there isn't one, specify a unique ID so that a new user can be created with the value.
// The connect() method also accepts optional parameters: authToken, apiHost, and wsHost.
GIMChat.connect(userId) { user, e ->
    if (user != null) {
        if (e != null) {
            // Proceed in offline mode with the data stored in the local database.
            // Later, connection is made automatically,
            // and can be notified through ConnectionHandler.onReconnectSucceeded().
        } else {
            // Proceed in online mode.
        }
    } else {
        // Handle error.
    }
}
```

Note: You must receive the result of `InitResultHandler()` before calling the `connect()` method. Any method can be called once the user is connected to the Vyin Chat server.

For Chat SDKs that don't use local caching, the connection process remains the same. When an error occurs, the SDK must attempt to reconnect again.

### Step 5 Create a new open channel

Create an open channel using the following code. Open channels are where all users in your Vyin Chat application can easily participate without an invitation.

```kotlin
OpenChannel.createChannel(OpenChannelCreateParams()) { channel, e ->
    if (e != null) {
        // Handle error.
    }

    // An open channel is successfully created.
    // You can get the open channel's data from the result object passed to the callback handler.
}
```

Note: You can also create a group channel to send a message. To learn more, see Create a channel.

### Step 6 Enter the channel

Enter the channel to send and receive messages.

```kotlin
// The following sample code continues from Step 5.
OpenChannel.createChannel(OpenChannelCreateParams()) { channel, e ->
    if (e != null) {
        // Handle error.
    }

    // Call the instance method of the result object in the openChannel parameter
    // of the callback handler.
    channel?.enter { e ->
        if (e != null) {
            // Handle error.
        }

        // The current user has successfully entered the open channel,
        // and can chat with other users in the channel by using APIs.
    }
}
```

### Step 7 Send a message to the channel

Finally, send a message to the channel. To learn about the message types you can send, refer to Message overview in Chat Platform API.

You can check the message you've sent in Vyin Chat Console. To learn about receiving a message, refer to the receive messages through a channel event handler page.

```kotlin
openChannel.sendUserMessage(UserMessageCreateParams(MESSAGE)) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // The message has been successfully sent to the channel.
    // The current user can receive messages from other users
    // through the onMessageReceived() method of an event handler.
}
```
