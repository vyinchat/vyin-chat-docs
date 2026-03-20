# Vyin Chat SDK for iOS - Get Started

## Getting started

## Send first message

---

### Get started

To send a message in a client app, you should build and configure an in-app chat using Vyin Chat SDK, which can be installed through SPM or CocoaPods by accessing our remote repository.

#### Step 1-1 Install the Chat SDK \- Configuring the Repository

You can install the Chat SDK for iOS through `CocoaPods`.

#### CocoaPods

If you are using Cocoapods, add the following to your `Podfile` file:

```swift
 pod :git => ‘https://gitlab.gamania.com/gim/ios/chatsdk.git’, :tag => ‘1.5.0’
```

#### Swift package manager (SPM)

If you are using SPM, add the following code to your root `Package Dependencies` file:

```swift
    https://gitlab.gamania.com/gim/ios/chatsdk.git
    Branch: main or using tag version
```

#### Step 1-2 Install the Chat SDK

If using CocoaPods then run pod install  
Else using SPM go to Package Dependencies at bottom project panel after that Resolve package version

**Step 2 Request to access system permissions**  
The Chat SDK requires system permissions. These permissions allow the Chat SDK to communicate with the Vyin Chat server and read from and write on a user device’s storage, remote notification. To request system permissions, add the following lines to your `Info.plist` file.  
> **[IMAGE PLACEHOLDER]** 

#### Step 3 Initialize the Chat SDK

Now, initialize the Chat SDK in the app to allow the Chat SDK to respond to changes in the connection status of iOS client apps. Initialization requires your Vyin Chat application's Application ID, which can be found on the Vyin Chat Dashboard.

Note: All methods in the following steps are asynchronous. This means that when using asynchronous methods, your client app must receive success callbacks from the Vyin Chat server through their callback handlers in order to proceed to the next step. A good way to do this is using nested methods. Go to Step 5: how you can create the `Groupchannel.getChannel()` method.

Before initializing, you should determine whether to enable local caching using `InitParams` in the `GIMChat.init()` method. The `isLocalCachingEnabled` property of `InitParams` determines whether or not the client app is going to use local storage through the SDK. If you want to build a client app without our local caching functionalities, set the value of `isLocalCachingEnabled` to `false`.

To learn more, see Initialize the Chat SDK with APP\_ID.

```swift
    let params = InitParams(applicationId: applicationId, isLocalCachingEnabled: true, logLevel: .verbose)
    GIMChat.initialize(params: params)
```

The `GIMChat.init()` method must be called at least once across a client app. It is recommended to initialize the Chat SDK using the `func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?)` method of the `AppDelegate` instance.

If ChatSDK initialization fails or handle migration, handle the error in the following closure:

```swift
    GIMChat.initialize(params: params) {
      // handle migrate
    } completionHandler: { error in
      // handle error
    }
```

#### Step 4 Connect to the Vyin Chat server

You need a user connected to the Vyin Chat server first before sending a message to a channel.

* If you already have a user in your Vyin Chat application: In `connect()`, specify a user ID from the user list in your Vyin Chat application.  
* If you don't have an existing user: Specifying a unique `userId` in `connect()` will automatically generate a new user in your Vyin Chat application before being connected.

Note: Vyin Chat supports user authentication via access tokens, but defaults to allowing access without a token for ease of initial use. For enhanced security, we recommend adjusting access settings under Settings \> Application \> Security \> Access token permission on the dashboard for new users. To learn about access token and authentication, see the Authentication guide.  

```swift
// Use the ID of a user you've created on the dashboard.
// If there isn't one, specify a unique ID so that a new user can be created with the value.
GIMChat.connect(userId) { user, e in
    if (user != nil) {
        if (e != nil) {
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

Note: You must receive the result of `GIMChat.initialize completionHandler` before calling the `connect()` method. Any method can be called once the user is connected to the Vyin Chat server.

For Chat SDKs that don't use local caching, the connection process remains the same. When an error occurs, the SDK must attempt to reconnect again.

#### Step 5 Create a new groupchannel

Create a channel using the following code. Group channels are where all users in your Vyin Chat application can easily participate without an invitation.  

```swift
var listUserString = listUserInvite?.map { $0.user.userId }

        let params = GroupChannelCreateParams()
        params.name = name
        params.coverImage = image?.pngData()

        if let currentUser = GIMChat.getCurrentUser() {
            params.operatorUserIds = [currentUser.userId]
            listUserString?.append(currentUser.userId)
        }

        params.addUserIds(listUserString ?? [])

GroupChannel.createChannel(params: params) { channel, e in
    if (e != nil) {
        // Handle error.
    }

    // A group channel is successfully created.
    // You can get the group channel's data from the result object passed to the callback handler.
}
```

Note: You can also create a group channel to send a message. To learn more, see Create a channel.

#### Step 6 GET the channel

Enter the channel to send and receive messages.  

```swift
// The following sample code continues from Step 5.
OpenChannel.getChannel(url: CHANNEL_URL) { channel, e in
    if (e != nil) {
        // Handle error.
    }
    // Call the instance method of the result object in the openChannel parameter
    // of the callback handler.
    channel?.enter { e in
        if (e != nil) {
            // Handle error.
        }

        // The current user has successfully entered the group channel,
        // and can chat with other users in the channel by using APIs.
    }
}
```

#### Step 7 Send a message to the channel

Finally, send a message to the channel. To learn about the message types you can send, refer to Message overview in Chat Platform API.

You can check the message you've sent in the Vyin Chat Dashboard. To learn about receiving a message, refer to the received messages through a channel event handler page.  

```swift
groupChannel.sendUserMessage(UserMessageCreateParams(MESSAGE)) { message, e in
    if (e != nil) {
        // Handle error.
    }

    // The message has been successfully sent to the channel.
    // The current user can receive messages from other users
    // through the onMessageReceived() method of an event handler.
}
```

