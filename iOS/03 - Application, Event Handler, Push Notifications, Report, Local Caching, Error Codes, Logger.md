# Vyin Chat SDK for iOS - Application, Event Handler, Push Notifications, Report, Local Caching, Error Codes, Logger

## Application

## Authenticating a user

## Authentication

In order to use the features of the Chat SDK for iOS in your client apps, a GIMChat instance should be initiated in each client app through user authentication with the Vyin Chat server. The instance communicates and interacts with the server based on the authenticated user account and is allowed to use the Chat SDK's features. This page explains how to authenticate your user with the server.

### Initialize the Chat SDK with APP\_ID

To use our chat features, you must initialize the GIMChat instance by passing APP\_ID of your Vyin Chat application as an argument to a parameter in the GIMChat.init() method. The GIMChat.init() method must be called once across your client app. Typically, initialization is implemented in the user login view.

With the implementation of local caching, you must determine whether you would like to use local caching and configure its settings through InitParams. The params contains properties such as isLocalCachingEnabled and set isLocalCachingEnabled to true so that the SDK can use local cache for its collection instances

Additionally, completionHandler() has been added to the initialization code as shown below. The completionHandler() gets the initialization status through different event handlers and informs the client app whether the initialization is successful or not. On the other hand, the migrationStartHandler() is called when the migration for local caching has started.

```swift
let applicationId: String = defaultsStorage.appID ?? AppDefaultConfig.shared.applicationId
    AppDefaultConfig.shared.updateApplicationId(applicationId)

    let params = InitParams(applicationId: applicationId, isLocalCachingEnabled: true, logLevel: .verbose)
    GIMChat.initialize(params: params, migrationStartHandler: {

    }, completionHandler: { error in

    })
```

### Connect to the Vyin Chat server with a user ID

By default, the Vyin Chat server can authenticate a user with just a unique user ID. Then, the server queries the database to check for a match upon connection request. If no matching user ID is found, the server creates a new user account with the user ID. The ID should be unique within a Vyin Chat application to be distinguishable from other identifiers such as a hashed email address or a phone number in your service.

While authenticating with just the user ID is convenient in the developing and testing stages of a service, a more secure authentication process using tokens is strongly recommended for most production environments.

First, you need to get a callback from completionHandler in order to connect. When the user is confirmed without any error, the SDK will proceed to connect with the Vyin Chat server.

When one of the error codes 400300, 400301, 400302, and 400310 returns, the SDK clears all user data cached in the local storage and tries to reconnect to the Vyin Chat server. Except when these errors occur, the client app can still draw a channel list view and a chat view in an offline mode using locally cached data. The SDK will receive an user object through a callback and try to reconnect later on. When the connection is made, ConnectionHandler.onReconnectSucceeded() will be called.

For Chat SDKs that don't use local caching, the connection process remains the same. When an error occurs, the SDK must attempt to reconnect again.

Note: Apart from initializing the GIMChat instance, you should connect to the Vyin Chat server before calling almost every method through the Chat SDK. If you attempt to call a method without connecting, an ERR\_CONNECTION\_REQUIRED (800101) error will be returned.

### Connect to the Vyin Chat server with a user ID and a token

For a more secure way of authenticating a user, you can require an authentication token, which can be an access token or a session token, in addition to a unique user ID. Any token issued for a user must be provided to the Vyin Chat server each time the user logs in by passing the token as an argument to the authToken parameter of the GIMChat.connect() method.

#### Using an access token

Through our Chat Platform API, an access token can be generated when creating a user. You can also issue an access token for an existing user. Once an access token is issued, a user is required to provide the access token in the GIMChat.connect() method which is used for logging in.

1. Using the Chat API, create a Vyin Chat user account with information submitted when a user signs up or logs in to your service.  
2. Save the user ID along with the issued access token to your persistent storage which is securely managed.  
3. When the user attempts to log in to a client app, load the user ID and access token from the storage, and then pass them to the GIMChat.connect() method.  
4. Periodically replacing the user's access token is recommended to protect the account.

```swift
GIMChat.connect(USER_ID, AUTH_TOKEN) { user, e in
    if (user != nil) {
        if (e !=nil) {
            // Proceed in offline mode with the data stored in the local database.
            // Later, connection will be made automatically.
            // The connection will be notified through ConnectionHandler.onReconnectSucceeded()
        } else {
            // Proceed in online mode.
        }
    } else {
        // Handle error.
    }
}
```

For Chat SDKs that don't use local caching, the connection process remains the same. If the user has ever been connected and their data exists in the local storage, the SDK can be connected to the Vyin Chat server. When an error occurs, the SDK must attempt to reconnect again.

#### Using a session token

You can also use a session token instead of an access token to authenticate a user. Session tokens are a more secure option because they expire after a certain period whereas access tokens don't. See Chat Platform API guides for further explanation about the difference between access token and session token, how to issue a session token, and how to revoke all session tokens.

### Set a session handler

When a user is authenticated with a session token, the Chat SDK connects the user to the Vyin Chat server and can send data requests to the server for ten minutes as long as the session token hasn't expired or hasn't been revoked.

Upon the user's session expiration, the Chat SDK will refresh the session internally using a SessionDelegate. However, if the session token has expired or has been revoked, the Chat SDK can't do so. In that case, the client app needs to implement a SessionDelegate instance to refresh the token and pass it back to the SDK so that it can refresh the session again.

Note: A SessionDelegate instance must be set before the server connection is requested.

The following code shows how to implement the handler methods.

```swift
GIMChat.setSessionDelegate(self)

func sessionTokenDidRequire(successCompletion success: @escaping (String?) -> Void, failCompletion fail: @escaping () -> Void)
    /// Called when the SDK can't refresh the session.
    ///
    /// App should force a user to a login page to connect again.
    func sessionWasClosed()
    /// Called when SDK refreshed the session key.
    func sessionWasRefreshed()
    /// Called when the SDK run into an error while refreshing the session key.
    ///
    /// - Parameter error: Error object
    func sessionDidHaveError(_ error: GIMError)
```

When the delegate.sessionTokenDidRequire(successCompletion: { }) is invoked, the SDK waits for a specific amount of time to receive a new session token from the client app.

If neither sessionWasClosed() nor sessionDidHaveError() are called within the specified timeout period, the socket connection will be disconnected. If this occurs, the client app has to manually call GIMChat.connect(USER\_ID, AUTH\_TOKEN) for a new socket connection.

### Disconnect from the Vyin Chat server

Disconnect a user from the Vyin Chat server when they no longer need to receive messages from an online state. This is equivalent to the logout behavior rather than disconnecting the socket. If you want to disconnect only the WebSocket without clearing locally cached data, please refer to the disconnect websocket only. However, unless the user unregisters the push token, the user will still receive push notifications for new messages from group channels they've joined.

When a client app is disconnected, all event handlers registered through GIMChat.addChannelDelegate() stop receiving event callbacks from the server. Then, all internally cached data in the client app are flushed. This includes channels that are cached when the getChannel() method of OpenChannel or GroupChannel is called, as well as locally cached channels and messages.

```swift
GIMChat.disconnect {
    // The current user is disconnected from the Vyin Chat server and all locally saved data is cleared.
    // ...
}
```

#### Disconnect the WebSocket only

While calling GIMChat.disconnect() disconnects the WebSocket as well as clear local cache data, you can call GIMChat.disconnectWebSocket to disconnect the WebSocket only and keep the locally cached data.

```swift
GIMChat.disconnectWebSocket {
    //
}
```

To reconnect after calling disconnectWebSocket, use the GIMChat.connect()

Note: The current version of Vyin Chat SDK does not support `localCacheConfig` for configuring local cache size limits and clear order.

## Understanding rate limits

## Rate limits

Vyin Chat applications are rate-limited to ensure the best experience for users. If you exceed a rate limit, the Vyin Chat server will return an error message that includes the encountered rate limit as well as how long you should wait before retrying.

Note : Vyin Chat organizations created after May 28, 2020, 00:00:00 UTC are automatically rate-limited, while those created before this date are given sufficient time to adjust their applications before rate limits apply. Go to Settings \> Application \> General on Vyin Chat Dashboard to see which rate limits your organizations are subject to. If you need a higher rate limit for API requests, contact our sales team for further assistance.  

---

### Default settings

To prevent abnormal user activity, Vyin Chat application has the following default limits on the number of messages per second which a user can send and an open channel can display.

#### Limits on number of messages per second

| Imposed on | Limit | If exceeded |
| :---- | :---- | :---- |
| User | Five messages per second | Excess messages are neither sent to the channel nor saved in the database but are displayed in the user's channel view. |
| Channel | Five messages per second | Excess messages aren't displayed but saved in the database. |

---

### Rate-limited methods

Rate limits apply to SDK methods associated with objects including channel, user, message, and more. For example, a user can send up to five messages per second to an open channel or a group channel. The following table lists the default rate limits per user.

#### Limits on SDK methods

| SDK method | Calls per second | Calls per minute |
| :---- | :---- | :---- |
| Listing objects | 20 | 120 |
| Retrieving objects | 20 | 120 |
| Creating objects | 10 | 120 |
| Updating objects | 10 | 120 |
| Deleting objects | 10 | 120 |
| Profile image upload | 2 | 40 |
| File upload | 4 | 60 |
| Updating channel metacounter | 2 | 60 |

---

### Rate-limited real-time events

Several real-time events taking place in client apps are rate-limited in order to maintain stable and efficient operation. The following table lists the default rate limits for those events.

#### Limits on events

| Event | Calls per second |
| :---- | :---- |
| Sending a message | 5 |
| Marking messages as read | 3 |
| Sending typing indicator to other members\* The limit applies to both users and channels. | 0.1 |
| Other events\* These include sending a file message, editing a message, or marking messages as delivered. | 3 |

---

### Error responses

When a request is rate-limited, a `RATE_LIMIT_EXCEEDED (500910)` or `TOO_MANY_MESSAGES (900200)` error is returned. See the error codes page for more information.

## Event handler

Vyin Chat's Chat SDK for iOS provides three types of event handlers for client apps: channel event handlers, user event handlers, and connection event handlers. Through the channel event handlers and user event handlers, the Vyin Chat server notifies client apps in the foreground of events that are happening in channels and with users. Through the connection event handler, the Vyin Chat server detects changes in the connection status of a client app. When the client app is disconnected from the server, the SDK tries to reconnect to the server and notifies the client app of the result through the connection event handler.

The Chat SDK communicates with our server through persistent WebSocket connections and multi-thread processing, receiving callbacks of asynchronous channel, user, and connection events through event handlers in real-time. This interaction enables you to track the events you desire and implement your own chat features associated with those events.  

---

### Functionalities by event handlers

The following is a list of event handler functionalities that our Chat SDK provides.

#### Managing a channel event handler

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Add or remove a channel event handler | Adds or removes an event handler that receives information about events happening in channels from the Vyin Chat server. | ✓ | ✓ |

#### Managing a user event handler

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Add or remove a user event handler | Adds or removes an event handler that receives information about events related to users connected to the Vyin Chat server. | ✓ | ✓ |

#### Managing a connection event handler

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Add or remove a connection event handler | Adds or removes an event handler that receives information about changes related to the Vyin Chat server connection status. | ✓ | ✓ |

## Managing channel event handlers

## Add or remove a channel event handler

To receive and retrieve information about events happening in channels from the Vyin Chat server, you can add a channel event handler with its `UNIQUE_HANDLER_ID` by calling the `GIMChat.addChannelDelegate()` method. You should declare `UNIQUE_HANDLER_ID` when registering multiple concurrent handlers.

There are three types of channel handlers: `BaseChannelDelegate`, `GroupChannelDelegate`, and `OpenChannelDelegate`. The following tables show a list of events for each channel event handler.

If you want to stay informed of changes related to channels and notify other users' client apps of those changes, define and register multiple channel event handlers to each `UIViewController` instance.  

---

### Channel event types

#### List of open channel events

| Method | Invoked when | Notified devices |
| :---- | :---- | :---- |
| func channel(\_ channel: BaseChannel, didReceive message: BaseMessage) | A message has been received in an open channel. | All devices where client apps with the channel are in the foreground, except the device that sent the message. |
| func channel(\_ channel: BaseChannel, didUpdate message: BaseMessage) | A message has been updated in an open channel. | All devices where client apps with the channel are in the foreground, except the device where the message was updated. |
| func channel(\_ channel: BaseChannel, messageWasDeleted messageId: Int64) | A message has been deleted in an open channel. | All devices where client apps with the channel are in the foreground, including the device where the message was deleted. |
| func channel(\_ channel: BaseChannel, didReceiveMention message: BaseMessage) | A user has been mentioned in a message sent in an open channel. | All devices of mentioned users (up to 10\) in the channel, including the device that was used to mention other users. |
| func channelWasChanged(\_ channel: BaseChannel) | One of the following open channel properties has been changed: name, cover image, data, custom type, or operators. | All devices that are connected to the changed channel, including the device where the channel was changed. |
| func channelWasDeleted(\_ channelURL: String, channelType: ChannelType) | An open channel has been deleted. | All devices where client apps with the channel are in the foreground, including the device where the channel was deleted. |
| public func channelWasFrozen(\_ channel: BaseChannel) | An open channel has been frozen. | All devices where client apps with the channel are in the foreground, including the device where the channel was frozen. |
| public func channelWasUnfrozen(\_ channel: BaseChannel) | An open channel has been unfrozen. | All devices where client apps with the channel are in the foreground, including the device where the channel was unfrozen. |
| public func channel(\_ channel: BaseChannel, createdMetaData: \[String : String\]?) | A metadata for an open channel has been created. | All devices where client apps with the channel are in the foreground, including the device where the metadata was created. |
| public func channel(\_ channel: BaseChannel, updatedMetaData: \[String : String\]?) | A metadata for an open channel has been updated. | All devices where client apps with the channel are in the foreground, including the device where the metadata was updated. |
| public func channel(\_ channel: BaseChannel, deletedMetaDataKeys: \[String\]?) | A metadata for an open channel has been deleted. | All devices where client apps with the channel are in the foreground, including the device where the metadata was deleted. |
| public func channel(\_ channel: BaseChannel, createdMetaCounters: \[String : Int\]?) | A metacounter for an open channel has been created. | All devices where client apps with the channel are in the foreground, including the device where the metacounter was created. |
| public func channel(\_ channel: BaseChannel, updatedMetaCounters: \[String : Int\]?) | A metacounter for an open channel has been updated. | All devices where client apps with the channel are in the foreground, including the device where the metacounter was updated. |
| public func channel(\_ channel: BaseChannel, deletedMetaCountersKeys: \[String\]?) | A metacounter for an open channel has been deleted. | All devices where client apps with the channel are in the foreground, including the device where the metacounter was deleted. |
| public func channel(\_ channel: OpenChannel, userDidJoin user: User) | A user has entered an open channel. | All devices where client apps with the channel are in the foreground, including the device that the user entered that channel. |
| public func channel(\_ channel: OpenChannel, userDidLeave user: User) | A user has exited an open channel. | All devices where client apps with the channel are in the foreground, except the device that the user exited that channel. |
| func channel(\_ channel: BaseChannel, userWasMuted user: RestrictedUser) | A user has been muted in an open channel. | All devices where client apps with the channel are in the foreground, including the device where the user was muted. |
| func channel(\_ channel: BaseChannel, userWasUnmuted user: User) | A user has been unmuted in an open channel. | All devices where client apps with the channel are in the foreground, including the device where the user was unmuted. |
| func channel(\_ channel: BaseChannel, userWasBanned user: RestrictedUser) | A user has been banned from an open channel. | All devices where client apps with the channel are in the foreground, including the device where the user was banned. |
| func channel(\_ channel: BaseChannel, userWasUnbanned user: User) | A user has been unbanned from an open channel. | All devices where client apps with the channel are in the foreground, including the device where the user was unbanned. |
| public func channelDidChangeParticipantCount(\_ channels: \[OpenChannel\]) | One or more open channels' participant count has changed. | All connected devices where client apps have the affected open channels in their channel list. |

#### List of group channel events

| Method | Invoked when | Notified devices |
| :---- | :---- | :---- |
| func channel(\_ channel: BaseChannel, didReceive message: BaseMessage) | A message has been received in a group channel. | All devices where client apps with the channel are in the foreground, except the device that sent the message. |
| func channel(\_ channel: BaseChannel, didUpdate message: BaseMessage) | A message has been updated in a group channel. | All devices where client apps with the channel are in the foreground, except the device where the message was updated. |
| func channel(\_ channel: BaseChannel, messageWasDeleted messageId: Int64) | A message has been deleted in a group channel. | All devices where client apps with the channel are in the foreground, including the device where the message was deleted. |
| func channel(\_ channel: BaseChannel, didReceiveMention message: BaseMessage) | A user has been mentioned in a message sent in a group channel. | All devices of mentioned users (up to 10\) in the channel, except the device that was used to mention other users. |
| func channelWasChanged(\_ channel: BaseChannel) | A message reaction has been updated in a group channel. | All devices where client apps with the channel are in the foreground, including the device that reacted to the message. |
| func channelWasChanged(\_ channel: BaseChannel) | One of the following group channel properties has been changed: distinct, push notification preferences, last message (except when the silent admin message), unread message count, name, cover image, data, or custom type. | All devices that are connected to the changed channel, including the device where the channel was changed. |
| func channelWasDeleted(\_ channelURL: String, channelType: ChannelType) | A group channel has been deleted. | All devices where client apps with the channel are in the foreground, including the device where the channel was deleted. |
| public func channelWasFrozen(\_ channel: BaseChannel) | A group channel has been frozen. | All devices where client apps with the channel are in the foreground, including the device where the channel was frozen. |
| public func channelWasUnfrozen(\_ channel: BaseChannel) | A group channel has been unfrozen. | All devices where client apps with the channel are in the foreground, including the device where the channel was unfrozen. |
| public func channel(\_ channel: BaseChannel, createdMetaData: \[String : String\]?) | A metadata for a group channel has been created. | All devices where client apps with the channel are in the foreground, including the device where the metadata was created. |
| public func channel(\_ channel: BaseChannel, updatedMetaData: \[String : String\]?) | A metadata for a group channel has been updated. | All devices where client apps with the channel are in the foreground, including the device where the metadata was updated. |
| public func channel(\_ channel: BaseChannel, deletedMetaDataKeys: \[String\]?) | A metadata for a group channel has been deleted. | All devices where client apps with the channel are in the foreground, including the device where the metadata was deleted. |
| public func channel(\_ channel: BaseChannel, createdMetaCounters: \[String : Int\]?) | A metacounter for a group channel has been created. | All devices where client apps with the channel are in the foreground, including the device where the metacounter was created. |
| public func channel(\_ channel: BaseChannel, updatedMetaCounters: \[String : Int\]?) | A metacounter for a group channel has been updated. | All devices where client apps with the channel are in the foreground, including the device where the metacounter was updated. |
| public func channel(\_ channel: BaseChannel, deletedMetaCountersKeys: \[String\]?) | A metacounter for a group channel has been deleted. | All devices where client apps with the channel are in the foreground, including the device where the metacounter was deleted. |
| public func channelWasHidden(\_ channel: GroupChannel) | A group channel has been hidden from the list. | All devices of a user who hided the channel. |
| public func channel(\_ channel: GroupChannel, didReceiveInvitation invitees: \[User\]?, inviter: User?) | A user has been invited to a group channel. | All devices where client apps with the channel are in the foreground, including the device of a user who received an invitation. |
| public func channel(\_ channel: GroupChannel, didDeclineInvitation invitee: User, inviter: User?) | A user has declined an invitation to a group channel. | All devices where client apps with the channel are in the foreground, including the device of a user who declined an invitation. |
| public func channel(\_ channel: GroupChannel, userDidJoin user: User) | A user has joined a group channel. | All devices where client apps with the channel are in the foreground, including all devices of the user who joined the channel, and except the devices of users who haven't accepted invitations. |
| public func channel(\_ channel: GroupChannel, userDidLeave user: User) | A user has left a group channel. | All devices where client apps with the channel are in the foreground, including all devices of the user who left the channel, and except the devices of users who haven't accepted invitations. |
| public func channelDidUpdateDeliveryStatus(\_ channel: GroupChannel) | A message has been delivered in a group channel. | All devices where client apps with the channel are in the foreground, and except the device where the message was marked as delivered and has invoked this event. |
| public func channelDidUpdateTypingStatus(\_ channel: GroupChannel) | A user starts typing a message to a group channel. | All devices where client apps with the channel are in the foreground, except the devices of the users who are invited to the channel, and except all devices of the user who typed a message. |
| public func channelDidUpdateReadStatus(\_ channel: GroupChannel) | A user has read a specific unread message in a group channel. | All devices where client apps with the channel are in the foreground, including all devices of the users who are invited to the channel, and except the device of the user who read the unread message. |
| func channel(\_ channel: BaseChannel, didUpdateThreadInfo threadInfoUpdateEvent: ThreadInfoUpdateEvent) | A reply message has been created or deleted from a thread. | All devices where client apps with the channel are in the foreground, including devices of all users who are in the message thread, and except the device of the user who created or deleted the reply message. |
| public func channel(\_ channel: BaseChannel, userWasMuted user: RestrictedUser) | A user has been muted in a group channel. | All devices where client apps with the channel are in the foreground, including the device where the user was muted. |
| public func channel(\_ channel: BaseChannel, userWasUnmuted user: User) | A user has been unmuted in a group channel. | All devices where client apps with the channel are in the foreground, including the device where the user was unmuted. |
| public func channel(\_ channel: BaseChannel, userWasBanned user: RestrictedUser) | A user has been banned from a group channel. | All devices where client apps with the channel are in the foreground, including the device where the user was banned. |
| public func channel(\_ channel: BaseChannel, userWasUnbanned user: User) | A user has been unbanned from a group channel. | All devices where client apps with the channel are in the foreground, including the device where the user was unbanned. |
| func channelDidChangeMemberCount(\_ channels: \[GroupChannel\]) | One or more group channels' member count has changed. | All connected devices where client apps have the affected group channels in their channel list. |
| func channelDidUpdatePinnedMessages(\_ channel: BaseChannel) | A message has been pinned or unpinned. | All connected devices where client apps have the affected group channels in their channel list. |

---

### Add a channel event handler

The following code samples show a full set of supported event callbacks with their parameters and how to add a channel event handler to the unique `GIMChat` instance.  

```swift
// Add GroupChannelHandler.
GIMChat.addChannelDelegate(_ delegate: BaseChannelDelegate, identifier: String)
/// Base Channel Delegate
    /// Invoked when a message is received.
    ///
    /// - Parameter channel: The channel where the message is received.
    /// - Parameter message: The pending message.
    func channel(_ channel: BaseChannel, didPending message: BaseMessage)

    /// Invoked when a message is received.
    ///
    /// - Parameter channel: The channel where the message is received.
    /// - Parameter message: The failure message.
    func channel(_ channel: BaseChannel, didFailure message: BaseMessage)

    /// Invoked when a message is received.
    ///
    /// - Parameter channel: The channel where the message is received.
    /// - Parameter message: The received message.
    func channel(_ channel: BaseChannel, didReceive message: BaseMessage)

    /// Invoked when a message is updated.
    ///
    /// - Parameter channel: The channel where the message is updated.
    /// - Parameter message: The updated message.
    func channel(_ channel: BaseChannel, didUpdate message: BaseMessage)

    /// A delegate is called when someone mentioned the user.
    ///
    /// - Parameter channel: The channel mention was occured in.
    /// - Parameter message: The message mention was occured about.
    func channel(_ channel: BaseChannel, didReceiveMention message: BaseMessage)

    /// Invoked when a user was muted in the channel.
    /// - Parameter channel: The channel.
    /// - Parameter user: The user who was muted.
    func channel(_ channel: BaseChannel, userWasMuted user: RestrictedUser)

    /// Invoked when a user was unmuted in the channel.
    ///
    /// - Parameter channel: The channel.
    /// - Parameter user: The user who was unmuted.
    func channel(_ channel: BaseChannel, userWasUnmuted user: User)

    /// Invoked when a user was banned in the channel.
    ///
    /// - Parameter channel: The channel.
    /// - Parameter user: The user who was banned.
    func channel(_ channel: BaseChannel, userWasBanned user: RestrictedUser)

    /// Invoked when a user was unbanned in the channel.
    ///
    /// - Parameter channel: The channel.
    /// - Parameter user: The user who was unbanned.
    func channel(_ channel: BaseChannel, userWasUnbanned user: User)

    /// Invoked when a channel was frozen.
    ///
    /// - Parameter channel: The channel.
    func channelWasFrozen(_ channel: BaseChannel)

    /// Invoked when an channel was unfrozen.
    ///
    /// - Parameter channel: The channel
    func channelWasUnfrozen(_ channel: BaseChannel)

    /// Invoked for when channel property is changed.
    ///
    /// - Parameter channel: The channel the property is changed of.
    func channelWasChanged(_ channel: BaseChannel)

    /// Invoked for when a channel was deleted.
    ///
    /// - Parameter channelURL: The channel url.
    /// - Parameter channelType: The type of channel that is deleted.
    func channelWasDeleted(_ channelURL: String, channelType: ChannelType)

    /// Invoked when a message was removed in the channel.
    ///
    /// - Parameter channel: The base channel.
    /// - Parameter messageId: The message ID which was removed.
    func channel(_ channel: BaseChannel, messageWasDeleted messageId: Int64)

    /// Invoked when meta data was created in the channel.
    ///
    /// - Parameter channel: The channel that the meta data was created.
    /// - Parameter createdMetaData: The created meta data.
    func channel(_ channel: BaseChannel, createdMetaData: [String : String]?)

    /// Invoked when meta data was updated in the channel.
    ///
    /// - Parameter channel: The channel that the meta data was updated.
    /// - Parameter updatedMetaData: The updated meta data.
    func channel(_ channel: BaseChannel, updatedMetaData: [String : String]?)

    /// Invoked when meta data was deleted in the channel.
    ///
    /// - Parameter channel: The channel that the meta data was deleted.
    /// - Parameter deletedMetaDataKeys: The keys of the deleted meta data.
    func channel(_ channel: BaseChannel, deletedMetaDataKeys: [String]?)

    /// Invoked when meta counters were created in the channel.
    ///
    /// - Parameter channel: The channel that the meta counters were created.
    /// - Parameter createdMetaCounters: The created meta counters.
    func channel(_ channel: BaseChannel, createdMetaCounters: [String : Int]?)

    /// Invoked when meta counters were updated in the channel.
    ///
    /// - Parameter channel: The channel that the meta counters were updated.
    /// - Parameter updatedMetaCounters: The updated meta counters.
    func channel(_ channel: BaseChannel, updatedMetaCounters: [String : Int]?)

    /// Invoked when meta counters were deleted in the channel.
    ///
    /// - Parameter channel: The channel that the meta counters were deleted.
    /// - Parameter deletedMetaCountersKeys: The keys of the deleted meta counters.
    func channel(_ channel: BaseChannel, deletedMetaCountersKeys: [String]?)

    /// Invoked when a reaction was updated.
    ///
    /// - Parameter channel: The channel that the reaction was updated.
    /// - Parameter reactionEvent: The updated reaction event.
    func channel(_ channel: BaseChannel, updatedReaction reactionEvent: ReactionEvent)

    /// Invoked when operators were updated in the channel.
    ///
    /// - Parameter channel: The channel that the operators was updated.
    func channelDidUpdateOperators(_ channel: BaseChannel)

    /// Invoked when the thread information is updated.
    ///
    /// - Parameter channel: The channel that has the message thread.
    /// - Parameter threadInfoUpdateEvent: `ThreadInfoUpdateEvent` object that has the latest information about the thread.
    func channel(_ channel: BaseChannel, didUpdateThreadInfo threadInfoUpdateEvent: ThreadInfoUpdateEvent)

    /// Invoked when pinned messages are added or deleted.
    ///
    /// - Parameter channel: The channel.
    func channelDidUpdatePinnedMessages(_ channel: BaseChannel)

    /// Invoked when pinned message changed
    /// - Parameters:
    ///   - channel: The channel.
    ///   - pinnedMessageIds: pinned message list ids
    ///   - lastPinMessage: last pinned message
    func channel(_ channel: BaseChannel, pinnedMessageIds: [Int64], lastPinMessage: BaseMessage?)

    /// Invoked when
    /// - Parameters:
    ///   - channel: The channel.
    ///   - systemMessage: System message
    func channel(_ channel: BaseChannel, systemMessage: BaseMessage)

    /// Invoked when a message is translated.
    ///
    /// - Parameter channel: The channel where the message is translated.
    /// - Parameter message: The translated message.
    func channel(_ channel: BaseChannel, didTranslateOnDemand message: BaseMessage)

    /// Invoked when a message is translated.
    ///
    /// - Parameter channel: The channel where the message is translated.
    /// - Parameter message: The translated message.
    func channel(_ channel: BaseChannel, didAutoTranslate message: BaseMessage)

/// Group channel delegate
/// A callback when read status updated.
    ///
    /// - Parameter channel: The group channel where the read status updated.
    func channelDidUpdateReadStatus(_ channel: GroupChannel)
    /// A callback when delivery status updated.
    ///
    /// - Parameter channel: The group channel where the delivery status updated.
    func channelDidUpdateDeliveryStatus(_ channel: GroupChannel)
    /// A callback when user sends typing status.
    ///
    /// - Parameter channel: The group channel where the typing status updated.
    func channelDidUpdateTypingStatus(_ channel: GroupChannel)
    /// A callback when member count has been changed for broadcast channel
    ///
    /// - Parameter channels: The group channel that member count has been updated
    func channelDidChangeMemberCount(_ channels: [GroupChannel])
    /// A callback when users are invited by inviter.
    ///
    /// - Parameter channel: The group channel where the invitation is occured.
    /// - Parameter invitees: Invitees.
    /// - Parameter inviter: Inviter.
    func channel(_ channel: GroupChannel, didReceiveInvitation invitees: [User]?, inviter: User?)
    /// A callback when user declined the invitation.
    ///
    /// - Parameter channel: The group channel where the invitation is occured.
    /// - Parameter inviter: Invitee.
    /// - Parameter invitee: Inviter.
    func channel(_ channel: GroupChannel, didDeclineInvitation invitee: User, inviter: User?)
    /// A callback when new member joined to the group channel.
    ///
    /// - Parameter channel: The group channel.
    /// - Parameter user: The new user joined to the channel.
    func channel(_ channel: GroupChannel, userDidJoin user: User)
    /// A callback when current member left from the group channel.
    ///
    /// - Parameter channel: The group channel.
    /// - Parameter user: The member left from the channel.
    func channel(_ channel: GroupChannel, userDidLeave user: User)
    /// A callback when the channel was hidden on the other device or by Platform API.
    ///
    ///  - Parameter channel: The channel that was hidden on the other device or by Platform API.
    func channelWasHidden(_ channel: GroupChannel)
    /// A callback when the poll has been updated.
    ///
    /// - Parameter channel: The channel that has the message thread.
    /// - Parameter event: event object contains updated poll information.
    func channel(_ channel: GroupChannel, didUpdatePoll event: PollUpdateEvent)

    /// A callback when the AI summary has been created.
    ///
    /// - Parameter channel: The channel that has the message thread.
    /// - Parameter event: event object contains created AI summary message information.
    func channel(_ channel: GroupChannel, didGetAISummary message: UserMessage)

    func channel(_ channel: GroupChannel, didUpdateSummaryStatus status: AISummaryStatus)

//// Open Channel Delegate
/// A callback when participant count has been changed for open channel
    ///
    /// - Parameter channels: The open channel that member count has been updated
    func channelDidChangeParticipantCount(_ channels: [OpenChannel])
    /// A callback when a user enter an open channel.
    ///
    /// - Parameter channel: The open channel.
    /// - Parameter user: A user who enters the channel
    func channel(_ channel: OpenChannel, userDidEnter user: User)
    /// A callback when a user exit an open channel.
    ///
    /// - Parameter channel: The open channel.
    /// - Parameter user: A user who exits the channel.
    func channel(_ channel: OpenChannel, userDidExit user: User)
```
---

### Remove a channel event handler

The following code shows how to remove the channel event handler.  

```swift
GIMChat.removeChannelDelegate(forIdentifier identifier: String)
```

## Managing connection event handlers

## Add or remove a connection event handler

To detect changes in the connection status of a client app, add a connection event handler with its unique user-defined ID by calling `GIMChat.addConnectionHandler()`.

If you want to stay informed of changes related to the Vyin Chat server connection status and notify client apps of these changes, define and register multiple connection event handlers to each `UIViewController` instance.  

---

### Connection event types

#### List of connection events

| Method | Invoked when | Notified devices |
| :---- | :---- | :---- |
| func didConnect(userId: String) | The Chat SDK has been connected to the Vyin Chat server for the first time. | The device that is connected to the Vyin Chat server. |
| func didDisconnect(userId: String) | The user has been logged out. It is different from WebSocket disconnection where a client app goes to the background. This handler is called after `GIMChat.disconnect()` is called. | The device that is disconnected from the Vyin Chat server. |
| dynamic public func didStartReconnection() | The Chat SDK has started reconnecting to the Vyin Chat server. | The device where `reconnect()` was automatically called by the Chat SDK or manually by the client app. |
| func didSucceedReconnection() | The Chat SDK has succeeded in reconnecting to the Vyin Chat server. | The device that successfully reconnected to the server. |
| dynamic public func didFailReconnection() | The Chat SDK has failed to reconnect to the Vyin Chat server. | The device that failed to reconnect to the server. |

---

### Add a connection event handler

The following code shows a full set of supported event callbacks and how to add a connection event handler to the unique `GIMChat` instance.  

```swift
GIMChat.addConnectionDelegate(_ delegate: ConnectionDelegate, identifier: String)

/// Invoked when reconnection starts.
    func didStartReconnection()
    /// Invoked when reconnection is succeeded.
    func didSucceedReconnection()
    /// Invoked when reconnection is failed.
    func didFailReconnection()
    /// Invoked when connected.
    func didConnect(userId: String)
    /// Invoked when disconnected.
    func didDisconnect(userId: String)
```

---

### Remove a connection event handler

The following code shows how to remove the connection event handler.  

```swift
GIMChat.removeConnectionDelegate(forIdentifier identifier: String)
```

## Managing user event handlers

## Add or remove a user event handler

To receive information about events related to users connected to the Vyin Chat server, add a user event handler with its unique user-defined ID by calling `GIMChat.addUserEventHandler()`.

If you want to stay informed of changes related to users and notify users' client apps of these changes, define and register multiple user event handlers to each `UIViewController` instance.  

---

### User event types

#### List of user events

The `onTotalUnreadMessageCountChanged()` attribute is turned off by default. Contact us if you wish to use this feature.

| Method | Invoked when | Notified devices |
| :---- | :---- | :---- |
| func didUpdateTotalUnreadMessageCount(\_ totalCount: Int, totalCountByCustomType: \[String : Int\]?) | A user has read messages in the joined group channels and there is an update on the total number of the user's unread messages. | The user's devices running client apps. The devices are notified of the total number of unread messages along with a collection of the number of unread messages by custom channel type. |

---

### Add a user event handler

The following code shows a full set of supported event callbacks with their parameters and how to add a user event handler to the unique `GIMChat` instance.  

```swift
GIMChat.addUserEventDelegate(_ delegate: GIMUserEventDelegate, identifier: String)

/// Invoked when list of users has been discovered
    ///
    /// - Parameter friends: list of user
    func didDiscoverFriends(_ friends: [User]?)
    /// Invoked when total unread message count has been updated
    ///
    /// - Parameters:
    ///    - totalCount: New total unread count
    ///    - totalCountByCustomType: Dictionary with key of custom tyeps and value of unread count
    func didUpdateTotalUnreadMessageCount(_ totalCount: Int, totalCountByCustomType: [String : Int]?)

    /// Invoked when user updated
    ///
    /// - Parameter user:
    func didUpdateUsers(_ users: [User])
```

---

### Remove a user event handler

The following code shows how to remove the user event handler.  

```swift
GIMChat.removeUserEventDelegate(forIdentifier identifier: String)
```

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

## Report

## Overview

With our Chat SDK, you can build your own in-app system for reporting and removing inappropriate content.  

---

### Functionalities by topic

The Chat SDK supports the following functionalities to help monitor chat activity.

#### Creating a report

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Get report category info list | Get report category info list to using report api. | ✓ | ✓ |
| Report a message, user, or channel | Lets users report inappropriate content. | ✓ | ✓ |

Note: This feature is not implemented yet\!

## Creating a report

## Report a message, user, or channel

Users can report suspicious or harassing messages and other users who use abusive language in a channel. They can also report the channel itself if there is any inappropriate content or activity in the channel. Based on this functionality and our report API, you can build your own in-app system for managing objectionable content and subject.

To use report API, you can use getReportCategoryInfoList API to retrieve the list of available report categories. Once the category list is fetched, allow the user to select one of the categories. The selected category can be included as a parameter when invoking report API.  

```swift
GIMChat.getReportCategoryInfoList { reportCategoryInfoList, error in
    if (reportCategoryInfoList == nil) {
        // Handle error.
        return
    }

    let reportCategoryInfo = reportCategoryInfoList[0]

    // Report a message
    channel.reportMessage(message, reportCategoryInfo: ReportCategoryInfo, reportDescription) { e in
        if (e != nil) {
            // Handle error.
        }
    }

    // Report a user
    channel.reportUser(offendingUser, reportCategoryInfo: ReportCategoryInfo, reportDescription) { e in
        if (e != nil) {
            // Handle error.
        }
    }

    // Report a channel
    channel.report(reportCategoryInfo: ReportCategoryInfo, reportDescription) { e in
        if (e != nil) {
            // Handle error.
        }
    }
}
```

### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| `message` | BaseMessage | Specifies the message to report for its suspicious, harassing, or inappropriate content. |
| `offendingUser` | User | Specifies the user who uses offensive or abusive language such as sending explicit messages or inappropriate comments. |
| `reportCategoryInfo` | ReportCategoryInfo | Specifies a report category that indicates the reason for reporting. You can retrieve ReportCategoryInfo through `getReportCategoryInfoList()`. The default categories are `suspicious`, `harassing`, `inappropriate`, and `spam`, but if you have enabled Advanced Moderation, you can add custom categories through the Vyin Chat Dashboard. |
| `reportDescription` | String | Specifies additional information to include in the report. |

Note: This feature is not implemented yet\!

## Local caching

Local caching enables Vyin Chat SDK for iOS to locally cache and retrieve group channel and message data. Its benefits include reducing refresh time and allowing a client app to create a channel list or a chat view that can work online as well as offline, which can be used for offline messaging.

You can enable local caching when initializing the Chat SDK by setting the optional parameter `useCaching` to `true`. See Initialize the Chat SDK and Connect to the Vyin Chat server to learn more.  
To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Showing how events are delivered to a relevant collection through an event controller.

Local caching relies on the `GroupChannelCollection` and `MessageCollection` classes, which are used to build a channel list view and a chat view, respectively. Collections are designed to react to events that can cause change in a channel list or chat view. An event controller in the SDK oversees such events and passes them to a relevant collection. For example, if a new group channel is created while the current user is looking at a chat view, the current view won't be affected by this event.

Note: You can still use these collections when building your app without local caching.  

---

### Functionalities by topic

The following is a list of functionalities our Chat SDK supports.

#### Using group channel collection

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Group channel collection | Keeps the client app's channel list synced with that on the Vyin Chat server. |  | ✓ |

#### Using message collection

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Message collection | Keeps the client app's chat view synced with that on the Vyin Chat server. |  | ✓ |

---

### GroupChannelCollection

You can use `GroupChannelCollection` to swiftly create a channel list view that stays up to date on all channel-related events. A `GroupChannelCollection` instance is composed of the following methods and variables.  

```swift
// Create a GroupChannelListQuery object first.
let query = GroupChannel.createMyGroupChannelListQuery(
    GroupChannelListQueryParams(builder: { params in
        params.includeEmpty = true
        params.order = .chronological
    }
)
let channelCollection = GIMChat.createGroupChannelCollection(query)
channelCollection.delegate = self

extension YourViewController: GroupChannelCollectionDelegate {

  /// Handles the event when channels are deleted from the collection.
  ///
  /// This method is called when channels are deleted from the collection. It updates the delegate
  /// with the changes to the channel list.
  /// - Parameters:
  ///   - collection: The `GroupChannelCollection` where the channels were deleted.
  ///   - context: The `ChannelContext` providing information about the deletion event.
  ///   - deletedChannelURLs: The URLs of the deleted channels.
  public func channelCollection(_ collection: GroupChannelCollection,
                                context: ChannelContext,
                                deletedChannelURLs: [String]) {
    // Handle deleted channels, e.g. remove from channel list.
  }

  /// Handles the event when new channels are added to the collection.
  ///
  /// This method is called when new channels are added to the collection. It updates the delegate
  /// with the changes to the channel list.
  /// - Parameters:
  ///   - collection: The `GroupChannelCollection` where the channels were added.
  ///   - context: The `ChannelContext` providing information about the addition event.
  ///   - addedChannels: The newly added channels.
  public func channelCollection(_ collection: GroupChannelCollection,
                                context: ChannelContext,
                                addedChannels channels: [GroupChannel]) {
    // Handle added channels, e.g. insert into channel list.
  }

  /// Handles the event when channels in the collection are updated.
  ///
  /// This method is called when channels in the collection are updated. It updates the delegate
  /// with the changes to the channel list.
  /// - Parameters:
  ///   - collection: The `GroupChannelCollection` where the channels were updated.
  ///   - context: The `ChannelContext` providing information about the update event.
  ///   - updatedChannels: The updated channels.
  public func channelCollection(_ collection: GroupChannelCollection,
                                context: ChannelContext,
                                updatedChannels channels: [GroupChannel]) {
    // Handle updated channels, e.g. refresh channel list.
  }
}
```

When the `createGroupChannelCollection()` method is called, the group channel data stored in the local cache and the Vyin Chat server are fetched and sorted based on the values in `GroupChannelListQuery`. Also, `groupChannelCollectionHandler` lets you set the event listeners that can subscribe to channel-related events when creating the collection.

As for pagination, `hasMore` checks if there are more group channels to load whenever a user hits the bottom of the channel list. If so, `loadMore()` retrieves channels from the local cache and the Vyin Chat server to display on the list.

To learn more about the collection and how to create a channel list view with it, see Group channel collection.  

---

### MessageCollection

You can use `MessageCollection` to swiftly create a chat view that includes all data. A `MessageCollection` instance is composed of the following methods and variables.  

```swift
func createMessageCollection() {
    // You can use a MessageListParams instance for MessageCollection.
    let params = MessageListParams()
    self.messageCollection = GIMChat.createMessageCollection(
      channel: channel,
      startingPoint: startingPoint,
      params: self.params
    )
}

// Initialize messages from startingPoint.
func initialize() {
self.messageCollection?.startCollection(
      initPolicy: initPolicy,
      cacheResultHandler: { [weak self] cacheResult, error in
        guard let self = self else { return }
        // Handle cached messages.
      }, apiResultHandler: { [weak self] apiResult, error in
        guard let self = self else { return }
        // Handle messages from server.
      })
}

extension YourViewController: MessageCollectionDelegate {

  public func messageCollection(_ collection: MessageCollection, context: MessageContext, channel: GroupChannel, didGetSummary message: BaseMessage) {
    // Handle message summary update.
  }

  public func messageCollection(_ collection: ChatSDK.MessageCollection,
                                context: ChatSDK.MessageContext,
                                channel: ChatSDK.GroupChannel,
                                didTranslateOnDemand message: ChatSDK.BaseMessage) {
    // Handle on-demand translation result.
  }

  public func messageCollection(_ collection: MessageCollection, context: MessageContext, channel: GroupChannel, didAutoTranslate message: BaseMessage) {
    // Handle auto-translated message.
  }

  public func messageCollection(_ collection: MessageCollection,
                                context: MessageContext,
                                channel: GroupChannel,
                                addedMessages messages: [BaseMessage]) {
    // Handle newly added messages.
  }

  public func messageCollection(
    _ collection: MessageCollection,
    context: MessageContext,
    channel: GroupChannel,
    updatedMessages messages: [BaseMessage]
  ) {
    // Handle updated messages.
  }

  public func messageCollection(
    _ collection: MessageCollection,
    context: MessageContext,
    channel: GroupChannel,
    deletedMessages messages: [BaseMessage]
  ) {
    // Handle deleted messages.
  }

  public func messageCollection(
    _ collection: MessageCollection,
    context: MessageContext,
    updatedChannel channel: GroupChannel
  ) {
    // Handle channel update.
  }

  public func messageCollection(
    _ collection: MessageCollection,
    context: MessageContext,
    deletedChannel channelURL: String
  ) {
    // Handle channel deletion.
  }

  public func didDetectHugeGap(_ collection: MessageCollection) {
    // Handle huge gap detected, dispose and recreate collection.
  }
}
```

In the `MessageCollection` class, the initialization is dictated by `MessageCollectionInitPolicy`. This determines which data to use for the collection. Currently, we only support `.cacheAndReplaceByApi`. According to this policy, the collection loads the messages stored in the local cache through `onCachedResult()`. Then `onApiResult()` replaces them with the messages fetched from the Vyin Chat server through an API request.

As for pagination, `hasNext` checks if there are more messages to load whenever a user hits the bottom of the chat view. If so, the `loadNext()` method retrieves messages from the local cache to display in the view. `hasPrevious` and `loadPrevious()` work the same way when the scroll reaches the top of the chat view.

To learn more about the collection and how to create a chat view with it, see Message collection.  

---

### Gap and synchronization

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Showing what a gap and a huge gap is.

A gap is created when messages or channels that exist on the Vyin Chat server are missing from the local cache. Such discrepancy occurs when a client app isn't able to properly load new events due to connectivity issues. To prevent such a gap, the Chat SDK constantly communicates with the Vyin Chat server and fetches data through synchronization, pulling a chunk of messages before and after `startingPoint`.

Such gap can significantly widen if a user has been offline for a long time. If more than 300 messages are missing in the local cache compared to the Vyin Chat server, Vyin Chat SDK classifies this as a huge gap and `onHugeGapDetected()` is called. In case of a huge gap, it is more effective to discard the existing message collection and create a new one. On the other hand, a relatively small gap created when the SDK was offline can be filled in through changelog sync.  

---

### Database management

You can configure how much space in local cache Vyin Chat SDK can use for its database and in which order to clear the cached data when it reaches the limit. The `LocalCacheConfig` class helps you manage the local cache settings and its instance can be passed in `InitParams` during SDK initialization.

#### LocalCacheConfig

The `LocalCacheConfig` class has two properties related to database management, which are `maxSize` and `clearOrder`.

The `maxSize` property sets the size limit for cached data. The default value of `maxSize` is 256 MB, but you can choose a value between 64 MB and `Int64.MAX_VALUE`. Once the database reaches its limit, the SDK starts to clear cached messages according to `clearOrder`.

Note: The minimum value for `maxSize` is 64 MB. If you specify a value lower than 64 MB, the value is systematically changed to 64MB.

The `clearOrder` property determines which data to clear first when local cache hits the limit. You can use the default setting, `CachedDataClearOrder.messageCollectionAccessedAt`, to clear cached messages of a group channel based on the time when the current user accessed the channel's `MessageCollection` instance. The longer the current user hasn't accessed the instance, the sooner it is removed. Or you can set it to `CachedDataClearOrder.custom` and customize the settings with `customClearOrderComparator`. You can use `CachedBaseChannelInfo` for the comparator to determine which cached messages to clear first.

Note: If `clearOrder` is set to `CachedDataClearOrder.custom` but `customClearOrderComparator` isn't implemented, the SDK uses the default value of `CachedDataClearOrder.messageCollectionAccessedAt` instead.  

```swift
let params = InitParams(applicationId: applicationId, isLocalCachingEnabled: true, logLevel: .verbose)
GIMChat.initialize(params: params)
```

#### How it works

Once `LocalCacheConfig` is set, Vyin Chat SDK checks the size of cached data through `getCachedDataSize()` when the `connect()` method is called with the current user's `user_id` for the first time after the SDK initialization. When the cached database size reaches its `maxSize` value, `clearCachedMessages()` is called to clear messages in the database according to `clearOrder` as explained above. You can use either the default setting, `.messageCollectionAccessedAt`, or your own `CUSTOM` order to clear data in local cache.

If you wish to clear all cached data at once, use `clearCachedData()`.  

---

### Data encryption

Data in local cache can be encrypted for data protection and security. Local caching for iOS uses Zetetic LLC's [SQLCipher](https://www.zetetic.net/sqlcipher/about/) for database encryption, which is a widely-used library for SQLite. To use the functionality, follow the instructions below.

1. Add the `[SQLCipher](https://www.zetetic.net/sqlcipher/about/)` dependency to your iOS project. To learn more, visit [SQLCipher](https://www.zetetic.net/sqlcipher/about/)'s documentation.  
2. Set `sqlcipherConfig` to `true` during SDK initialization. The `SqlcipherConfig` class has two properties, which are `password` and `licenseCode`. `password` is a key required during encryption and decryption while `licenseCode` is required for those who are subscribing to [SQLCipher](https://www.zetetic.net/sqlcipher/about/)'s Commercial or Enterprise Edition.

Note: [SQLCipher](https://www.zetetic.net/sqlcipher/about/) is an external 3rd party library and is not endorsed by Vyin Chat. You must administer your own [SQLCipher](https://www.zetetic.net/sqlcipher/about/) license and an encryption key.

#### Troubleshooting

When the `password` value in `sqlcipherConfig` doesn't match with the one you set, the already encrypted data can't be decrypted. In that case, initialization fails and `onInitFailed` is called with an error code, `GIMError.ERR_DATABASE_ERROR_ENCRYPTION`.  

---

### Auto resend

A message is normally sent through the WebSocket connection which means the connection must be secured and confirmed before sending any messages. With local caching, you can temporarily keep an unsent message in the local cache if the WebSocket connection is lost. The Chat SDK with local caching enabled marks the failed message as pending, stores it locally, and automatically resends the pending message when the WebSocket is reconnected. This is called auto resend.

The following cases are eligible for auto resend.

* A user message couldn't be sent because the WebSocket connection was lost even before it was established.  
* A file message couldn't upload the attached file to the Vyin Chat server.  
* An attached file was uploaded to the Vyin Chat server but the file message itself couldn't be sent because the WebSocket connection was closed.

To view the image in full screen, click on the

> **[IMAGE PLACEHOLDER]** Showing how a pending message can be automatically re-sent using local caching.

#### User message

A user message is sent through the WebSocket. If a message couldn't be sent because the WebSocket connection was lost, the Chat SDK receives an error through a callback and queues the pending message in the local cache for auto resend. When the client app is reconnected, the SDK then attempts to resend the message.

If the message is successfully sent, the client app receives a response from the Vyin Chat server. Then `onMessagesUpdated()` is called to change the pending message to a sent message in the data source and updates the chat view.

#### File message

A file message can be sent through either the WebSocket connection or an API request.

When sending a file message, the attached file must be uploaded to the Vyin Chat server as an HTTP request. To do so, the Chat SDK checks the status of the network connection. If the network isn't connected, the file can't be uploaded to the server. In this case, the SDK handles the file message as a pending message and adds to the queue for auto resend.

If the network is connected and the file is successfully uploaded to the server, its URL is delivered in a response and the SDK replaces the file with its URL String. At first, the SDK attempts to send the message through the WebSocket. If the WebSocket connection is lost, the client app checks the network connection once again to make another HTTP request for the message. If the SDK detects the network as disconnected, it gets an error code that marks the message as a pending message, allowing the message to be automatically resent when the network is reconnected.

On the other hand, if the network is connected but the HTTP request fails, the message isn't eligible for auto resend.

If the message is successfully sent, the client app receives a response from the Vyin Chat server. Then `onMessagesUpdated()` is called to change the pending message to a sent message in the data source and updates the chat view.

#### Failed message

If a message couldn't be sent due to some other error, `onMessagesUpdated()` is called to re-label the pending message as a failed message. Messages labeled as such can't be queued for auto resend. The following shows some of these errors.  

```swift
GIMError.ERR_NETWORK
GIMError.ERR_ACK_TIMEOUT
```

Note: A pending message can last in the queue only for three days. If the WebSocket connection is back online after three days, `onMessagesUpdated()` is called to mark any three-day-old pending message as failed messages.  

---

### Other methods

The following code block shows a list of methods that can help you leverage the local caching functionalities.

The `GIMChat.getCachedDataSize()` method lets you know the size of the data stored in the local cache. If you want to erase the entire data, use the `GIMChat.clearCachedData()` method. Meanwhile, the `GIMChat.clearCachedMessages()` method lets you get rid of unnecessary messages from the local cache by specifying the channel URL of those messages.

The `BaseMessage.messageCreateParams` property is used when drawing pending messages in the chat view. This method returns the exact same `UserMessageCreateParams` or `FileMessageCreateParams` that you used when sending messages. The params can be passed to the following methods.

* `channel.sendUserMessage(UserMessageCreateParams, SendUserMessageHandler)`  
* `channel.sendFileMessage(FileMessageCreateParams, SendFileMessageHandler)`

```swift
GIMChat.getCachedDataSize(Context): Int64
GIMChat.clearCachedData(context: Context, handler: CompletionHandler?)
GIMChat.clearCachedMessages(channelUrl: String, handler: CompletionHandler?)

BaseMessage.messageCreateParams: BaseMessageCreateParams?   // This should be used for displaying pending messages in the chat view.
```

Note: The current version of Vyin Chat SDK does not support `localCacheConfig` for configuring local cache size limits and clear order.

## Using group channel collection

## Group channel collection

A `GroupChannelCollection` instance allows you to swiftly create a channel list view that remains up to date on all channel-related events. This page explains how to draw a view using the collection.  

---

### Create a collection

You can create a `GroupChannelCollection` instance through the `createGroupChannelCollection()` method.

First, create a `GroupChannelListQuery` instance through the `createMyGroupChannelListQuery()` method and its query setters. This determines which channel to include in the channel list and how to list channels in order.

Once the collection is created, you should call `loadMore()`.  

```swift
// First, create a GroupChannelListQuery instance in GroupChannelCollection.
let params = GroupChannelListQueryParams()
params.includeEmpty = true
params.order = .chronological

let query = GroupChannel.createMyGroupChannelListQuery(params: params)
        // Acceptable values are .chronological, .latestLastMessage, .channelNameAlphabetical,
        // and .metadataValueAlphabetical.
        // You can add other params setters.

// Create a GroupChannelCollection instance.
let collection: GroupChannelCollection = GIMChat.createGroupChannelCollection(query: query)
```
---

### Pagination

A `GroupChannelCollection` instance retrieves more channels to display in the view through the `loadMore()` method.

Whenever a scroll reaches the bottom of the channel list view, the `loadMore()` method is called and `hasMore` first checks if there are more channels to load. If so, `loadMore()` fetches them.

The `loadMore()` method should also be called after you've created a `GroupChannelCollection` instance.  

```swift
// Check whether there are more channels to load before calling loadMore().
if (collection.hasNext) {
    collection.loadMore { channels, e in
        if (e != nil) {
            // Handle error.
            return
        }

        // Add channels to your data source.
    }
}
```
---

### Channel events

Use the `groupChannelCollectionHandler` property in `GroupChannelCollectionCreateParams` to determine how the client app reacts to channel-related events.

This is called whenever a new channel is created as a real-time event or changelog sync is prompted when the client app is back online.

The following table shows possible cases where each event handler can be called.

| Event | Called when |
| :---- | :---- |
| `func channelCollection(_ collection: GroupChannelCollection, context: ChannelContext, addedChannels: [GroupChannel])` | \- A new group channel is created as a real-time event.\- New group channels are fetched through changelog sync. |
| `func channelCollection(_ collection: GroupChannelCollection, context: ChannelContext, updatedChannels: [GroupChannel])` | \- The channel information that is included in the user's current chat view is updated as a real-time event.\- Updated channel information is fetched through changelog sync. |
| `func channelCollection(_ collection: GroupChannelCollection, context: ChannelContext, deletedChannelURLs: [String])` | \- A group channel is deleted as a real-time event.\- A channel deletion event is fetched through changelog sync. |

```swift
let collection: GroupChannelCollection = GIMChat.createGroupChannelCollection(query: query)
```
---

### Loaded channels

The `GroupChannelCollection` holds list of channels that are loaded by `loadMore()` or by channel events, the `channelList`. It is recommended to use this list as the datasource of your `UITableView` or `UICollectionView` as this list is the most updated list.  

```swift
let channelList = collection.channelList
```
---

### Dispose of the collection

The `dispose()` method should be called when you destroy the channel list screen to notify the SDK that this collection is no longer visible.  

```swift
collection.dispose()
```

## Using message collection

## Message collection

A `MessageCollection` instance allows you to swiftly create a chat view that includes all data. This page explains how to make a view using the collection.  

---

### Create a collection

First, create a `MessageCollection` instance through the `createMessageCollection()` method. Here, you need to set a `MessageListParams` instance to determine the message order and the starting point of the message list in the chat view.  

```swift
func createMessageCollection() {
    // Create a MessageListParams to be used in the MessageCollection.
    let params = MessageListParams()
        params.reverse = false

    // You can add other query setters.
    let collection = GIMChat.createMessageCollection(
        MessageCollectionCreateParams(groupChannel, startingPoint, params,)
    )
}
```

#### Starting point

The reference point for message retrieval in a chat view is `startingPoint`. This should be specified as a timestamp.

If you set the value of `startingPoint` to `Int64.MAX_VALUE`, messages in the view are retrieved in reverse chronological order, showing messages from the most to least recent.

If you set the value of `startingPoint` to the message last read by the current user, messages in the view are fetched in chronological order, starting from the last message seen by the user to the latest.  

---

### Initialization

After creating a `MessageCollection` instance, initialize the instance through the `initialize()` method. Here, you need to set `MessageCollectionInitPolicy`.

#### Policy

The `MessageCollectionInitPolicy` property determines how initialization deals with the message data retrieved from the local cache and API calls. Because we only support `MessageCollectionInitPolicy.CACHE_AND_REPLACE_BY_API` at this time, messages in the local cache must be cleared prior to adding the messages from the Vyin Chat server.

| Policy | Description |
| :---- | :---- |
| `.cacheAndReplaceByApi` | Retrieves cached messages from the local cache and then replaces them with the messages pulled from the Vyin Chat server through API calls. |

```swift
// Initialize messages from the startingPoint.

let initPolicy: MessageCollectionInitPolicy = .cacheAndReplaceByApi

self.messageCollection?.startCollection(
      initPolicy: initPolicy,
      cacheResultHandler: { [weak self] cacheResult, error in

      }, apiResultHandler: { [weak self] apiResult, error in

      })
```

---

### Pagination

Use the `loadPrevious()` and `loadNext()` methods to retrieve messages from the previous page and the next page.

When the `loadPrevious()` method is called, the Chat SDK first checks whether there're more messages to load from the previous page through `hasPrevious`. The same goes for `hasNext` and `loadNext()`.

These methods have to be called during initialization as well.  

```swift
// Load the previous messages when the scroll reaches the first message in the chat view.
if (collection.hasPrevious) {
    collection.loadPrevious { messages, e in
        if (e != nil) {
            // Handle error.
            return
        }
    }
}
```
---

### Message events

Use `messageCollectionHandler` to determine how the client app reacts to message-related events.

The following table shows possible cases where each event handler can be called.

| Handler | Called when |
| :---- | :---- |
| `func messageCollection(_ collection: MessageCollection, context: MessageContext, channel: GroupChannel, addedMessages: [BaseMessage])` | \- A new message is created as a real-time event.\- New messages are fetched during changelog sync. |
| `func messageCollection(_ collection: MessageCollection, context: MessageContext, channel: GroupChannel, deletedMessages: [BaseMessage])` | \- A message is deleted as a real-time event.\- Message deletion is detected during changelog sync.\- The value of the `MessageListParams` setter such as `custom_type` changes. |
| `func messageCollection(_ collection: MessageCollection, context: MessageContext, channel: GroupChannel, updatedMessages: [BaseMessage])` | \- A message is updated as a real-time event.\- Message update is detected during changelog sync.\- The send status of a pending message changes. |
| `func messageCollection(_ collection: MessageCollection, context: MessageContext, updatedChannel: GroupChannel)` | \- The channel information that is included in the user's current chat view is updated as a real-time event.\- Channel info update is detected during changelog sync. |
| `func messageCollection(_ collection: MessageCollection, context: MessageContext, deletedChannel channelURL: String)` | \- The current channel is deleted as a real-time event.\- Channel deletion is detected during changelog sync.\* In both cases, the entire view should be disposed of. |
| `func didDetectHugeGap(_ collection: MessageCollection)` | \- A huge gap is detected through background sync. In this case, you need to dispose of the view and create a new `MessageCollection` instance. |

```swift
messageCollection.delegate = self
```

---

### Loaded messages

The `MessageCollection` holds list of messages that are loaded by `initialize()`, `loadMore()`, `loadPrevious()` or by message events, the `succeededMessages`. It is recommended to use this list as the datasource of your `UITableView` or `UICollectionView` as this list is the most updated list.

Also, it has `pendingMessages` and `failedMessages` which are a list of messages that are being sent and messages that have failed to be sent respectively.  

```swift
// Loaded messages
let messages = collection.succeededMessages

// Pending messages which are being sent.
// When the message sending is done, either succeed or fail, those messages will be moved to succeededMessages or failedMessages.
let pendingMessages = collection.pendingMessages

// Messages that have been failed to be sent
let failedMessages = collection.failedMessages
```
---

### Gap

In local caching, a gap refers to a chunk of objects that are created and logged on the Vyin Chat server but missing from the local cache. Depending on the connection status, the client app could miss message events and new messages stored in the Vyin Chat server may not reach the Chat SDK in the client app. In such cases, a gap is created.

Small gaps can be filled in through changelog sync. However, if the client app has been disconnected for a while, too big of a gap can be created.

#### Huge gap

A gap can be created for many reasons, such as when a user has been offline for days. When the client app comes back online, the Chat SDK checks with the Vyin Chat server to see if there are any new messages. If it detects that more than 300 messages are missing in the local cache compared to the Vyin Chat server, the SDK determines that there is a huge gap and `onHugeGapDetected()` is called.

Note: To adjust the number of missing messages that result in a call for `onHugeGapDetected()`, contact our support team.

In this case, the existing `MessageCollection` instance should be cleared through the `dispose()` method and a new one must be created for better performance.  

```swift
collection.dispose()
```
---

### Dispose of the collection

The `dispose()` method should be called when you destroy the chat screen to notify the SDK that this collection is no longer visible.

For example, this should be used when the current user exits from the chat screen by pressing a back key or when a new collection has to be created due to a huge gap detected by `onHugeGapDetected()`.  

```swift
collection.dispose()
```

## Error codes

Vyin Chat SDK for iOS has two types of error codes.

* Client error codes: These errors are caused by the client app side, such as entering an incorrect or invalid parameter, or sending a request when disconnected from the Vyin Chat server.  
* Server error codes: These errors are caused by the Vyin Chat server side.

---

### Client error codes

The following are errors labeled with six-digit integers beginning with 800 that are defined in `GIMError`.

| Error | Code | Description |
| :---- | :---- | :---- |
| `ERR_INVALID_INITIALIZATION` | 800100 | This error occurs when the GIMChat initialization fails . This could be due to an invalid APP\_ID or repeated initialization attempts. |
| `ERR_CONNECTION_REQUIRED` | 800101 | The request from a client app failed because the device wasn't connected to the server. |
| `ERR_CONNECTION_CANCELED` | 800102 | The connection is canceled or the disconnecting method is called while the `GIMChat` instance is trying to connect to the server. |
| `ERR_INVALID_PARAMETER` | 800110 | The parameter of the method specifies an invalid value. |
| `ERR_NETWORK` | 800120 | The connection failed due to the unstable network or an unexpected error in the Chat SDK network library. |
| `ERR_NETWORK_ROUTING_ERROR` | 800121 | The request routing to the server failed. |
| `ERR_MALFORMED_DATA` | 800130 | The data format of the server response is invalid. |
| `ERR_MALFORMED_ERROR_DATA` | 800140 | The data format of the error message is invalid due to the problem with the request. |
| `ERR_WRONG_CHANNEL_TYPE` | 800150 | The specified channel type in the request is invalid. |
| `ERR_MARK_AS_READ_RATE_LIMIT_EXCEEDED` | 800160 | The interval between the successive requests is less than a second. |
| `ERR_QUERY_IN_PROGRESS` | 800170 | A retrieval request is arriving while the server is still processing the previous retrieval request for channels, messages, or users, and in preparation to send the response. |
| `ERR_ACK_TIMEOUT` | 800180 | The server failed to send a response for the request in 10 seconds. |
| `ERR_LOGIN_TIMEOUT` | 800190 | The server failed to send a response for the `GIMChat` instance's login request in 10 seconds. |
| `ERR_WEBSOCKET_CONNECTION_CLOSED` | 800200 | The request was submitted while disconnected from the server. |
| `ERR_WEBSOCKET_CONNECTION_FAILED` | 800210 | The websocket connection to the server failed to establish. |
| `ERR_REQUEST_FAILED` | 800220 | The server failed to process the request due to an internal reason. |
| `ERR_FILE_UPLOAD_CANCEL_FAILED` | 800230 | The request to cancel file upload failed due to an unexpected error. |
| `ERR_FILE_UPLOAD_CANCELED` | 800240 | The file upload request is canceled. |
| `ERR_INITIALIZATION_CANCELED` | 800103 | This error is triggered if the SDK initialization exceeds five seconds. It's a safeguard to avoid app unresponsiveness (ANR). Retrying initialization might work. |
| `ERR_DATABASE_ERROR` | 800700 | Something went wrong with the database. While the SDK continues to operate, local caching is disabled. |
| `ERR_DATABASE_ERROR_ENCRYPTION` | 800701 | This relates to sqlcipher issues, possibly due to an undeclared sqlcipher dependency or an erroneous encryption key. If the password is forgotten, clearing the data with `GIMChat.clearCachedData` is advised. |

---

### Server error codes

The following errors are six-digit integers beginning with 400, 500, and 900\.

| Code | Description |
| :---- | :---- |
| `400100(bad request)` | UNEXPECTED\_PARAMETER\_TYPEThe request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `String`. |
| `400101(bad request)` | UNEXPECTED\_PARAMETER\_TYPEThe request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `number`. |
| `400102(bad request)` | UNEXPECTED\_PARAMETER\_TYPEThe request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `list`. |
| `400103(bad request)` | UNEXPECTED\_PARAMETER\_TYPEThe request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `JSON`. |
| `400104(bad request)` | UNEXPECTED\_PARAMETER\_TYPEThe request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `Bool`. |
| `400105(bad request)` | MISSING\_REQUIRED\_PARAMETERSThe request is missing one or more required parameters. |
| `400106(bad request)` | NEGATIVE\_NUMBER\_NOT\_ALLOWEDThe parameter specifies a negative number. Its value should be a positive number. |
| `400108(bad request)` | UNAUTHORIZED\_REQUESTThe request isn't authorized and can't access the requested resource. |
| `400109(bad request)` | EXPIRED\_PAGE\_TOKENThe value of the `token` parameter for pagination has expired. |
| `400110(bad request)` | PARAMETER\_VALUE\_LENGTH\_EXCEEDEDThe length of the parameter value is too long. |
| `400111(bad request)` | INVALID\_VALUEThe request specifies an invalid value. |
| `400112(bad request)` | INCOMPATIBLE\_VALUESThe two parameters of the request, which should have unique values, specify the same value. |
| `400113(bad request)` | PARAMETER\_VALUE\_OUT\_OF\_RANGEThe request specifies one or more parameters which are outside the allowed value range. |
| `400114(bad request)` | INVALID\_URL\_OF\_RESOURCEThe resource identified with the URL in the request can't be found. |
| `400151(bad request)` | NOT\_ALLOWED\_CHARACTERThe request specifies an illegal value containing special character, empty String, or white space. |
| `400201(bad request)` | RESOURCE\_NOT\_FOUNDThe resource identified with the request's `resourceId` parameter can't be found. |
| `400202(bad request)` | RESOURCE\_ALREADY\_EXISTSThe resource identified with the request's `resourceId` parameter already exists. |
| `400203(bad request)` | TOO\_MANY\_ITEMS\_IN\_PARAMETERThe parameter specifies more items than allowed. |
| `400300(bad request)` | DEACTIVATED\_USER\_NOT\_ACCESSIBLEThe request can't retrieve the deactivated user data. |
| `400301(bad request)` | USER\_NOT\_FOUNDThe user identified with the request's `USER_ID` can't be found because either the user doesn't exist or has been deleted. |
| `400302(bad request)` | INVALID\_ACCESS\_TOKENThe access token provided for the request specifies an invalid value. |
| `400303(bad request)` | INVALID\_SESSION\_KEY\_VALUEThe session key provided for the request specifies an invalid value. |
| `400304(bad request)` | APPLICATION\_NOT\_FOUNDThe application identified with the request can't be found. |
| `400305(bad request)` | USER\_ID\_LENGTH\_EXCEEDEDThe length of the `USER_ID` is too long. |
| `400306(bad request)` | PAID\_QUOTA\_EXCEEDEDThe request can't be completed because you have exceeded your paid quota. |
| `400307(bad request)` | DOMAIN\_NOT\_ALLOWEDThe request can't be completed because it came from the restricted domain. |
| `400401(bad request)` | INVALID\_API\_TOKENThe API token provided for the request specifies an invalid value. |
| `400402(bad request)` | MISSING\_SOME\_PARAMETERSThe request is missing one or more necessary parameters. |
| `400403(bad request)` | INVALID\_JSON\_REQUEST\_BODYThe request body is an invalid JSON. |
| `400404(bad request)` | INVALID\_REQUEST\_URLThe request specifies an invalid HTTP request URL that can't be accessed. |
| `400500(bad request)` | TOO\_MANY\_USER\_WEBSOCKET\_CONNECTIONSThe number of the user's websocket connections exceeds the allowed amount. |
| `400501(bad request)` | TOO\_MANY\_APPLICATION\_WEBSOCKET\_CONNECTIONSThe number of the application's websocket connections exceeds the allowed amount. |
| `400700(bad request)` | BLOCKED\_USER.SEND\_MESSAGE\_NOT\_ALLOWEDThe request can't be completed due to one of the following reasons: The sender is blocked by the recipient or has been deactivated, the message is longer than the maximum message length, or the message contains texts or URLs blocked by application settings or filters. |
| `400701(bad request)` | BLOCKED\_USER.INVITED\_NOT\_ALLOWEDThe request can't be completed because the blocking user is trying to invite the blocked user to a channel. |
| `400702(bad request)` | BLOCKED\_USER.INVITE\_NOT\_ALLOWEDThe request can't be completed because the blocked user is trying to invite the blocking user to a channel. |
| `400750(bad request)` | BANNED\_USER.ENTER\_CHANNEL\_NOT\_ALLOWEDThe request can't be completed because the user is banned from a channel they're trying to enter. |
| `400751(bad request)` | BANNED\_USER.ENTER\_CUSTOM\_CHANNEL\_NOT\_ALLOWEDThe request can't be completed because the user is banned from a custom channel they're trying to enter. |
| `400920(bad request)` | UNACCEPTABLEThe request failed because the combination of parameter values is invalid. Even if each parameter value is valid, a combination of parameter values becomes invalid when it doesn't follow specific conditions.READ\_ONLY\_MODEWhen the application is in the read-only mode, can't complete API requests except for `GET`.NOT\_ACCESSIBLEThe request `DELETE` fails when the corresponding message has a child message. |
| `400930(bad request)` | INVALID\_ENDPOINTThe request failed because it is sent to an invalid endpoint that no longer exists. |
| `403100(forbidden)` | APPLICATION\_NOT\_AVAILABLEThe application identified with the request isn't available. |
| `500601(internal server error)` | INTERNAL\_ERROR.PUSH\_TOKEN\_NOT\_REGISTEREDThe server encounters an error while trying to register the user's push token. |
| `500602(internal server error)` | INTERNAL\_ERROR.PUSH\_TOKEN\_NOT\_UNREGISTEREDThe server encounters an error while trying to unregister the user's push token. |
| `500901(internal server error)` | INTERNAL\_ERRORThe server encounters an unexpected exception while trying to process the request. |
| `500910(too many request)` | RATE\_LIMIT\_EXCEEDEDThe request can't be completed because you have exceeded your rate limits. |
| `503(service unavailable)` | SERVICE\_UNAVAILABLEThe request failed due to a temporary failure of the server. |
| `900010(request failed)` | USER\_LOGIN\_REQUIREDThe request failed because the user isn't logged in to the server. |
| `900020(request failed)` | USER\_NOT\_MEMBERThe request failed because the user isn't a member of the channel. |
| `900021(request failed)` | USER\_DEACTIVATEDThe request failed because the user is deactivated. |
| `900022(request failed)` | USER\_NOT\_OWNER\_OF\_MESSAGEThe request failed because the user has no permission to edit the other user's message. |
| `900023(request failed)` | PENDING\_USER\_SEND\_MESSAGE\_NOT\_ALLOWEDThe request failed because the user is trying to send the messages in the channel of which they are not the member. |
| `900025(request failed)` | INVALID\_MENTION\_FOR\_MESSAGEThe specified mention type in the request is invalid. |
| `900026(request failed)` | INVALID\_PUSH\_OPTION\_FOR\_MESSAGEThe specified push option in the request is invalid. |
| `900027(request failed)` | TOO\_MANY\_META\_KEY\_FOR\_MESSAGEThe request failed because it specifies more meta data keys for the message than allowed. |
| `900028(request failed)` | TOO\_MANY\_META\_VALUE\_FOR\_MESSAGEThe request failed because it specifies more meta data values for the message than allowed. |
| `900029(request failed)` | INVALID\_META\_ARRAY\_FOR\_MESSAGEThe request failed because it specifies an invalid value in the meta data for the message. |
| `900030(request failed)` | GUEST\_NOT\_ALLOWEDThe request failed because the guest isn't allowed to perform this operation. |
| `900040(request failed)` | MUTED\_USER\_IN\_APPLICATION\_SEND\_MESSAGE\_NOT\_ALLOWEDThe request failed because the user is muted in the application and isn't allowed to send the message. |
| `900041(request failed)` | MUTED\_USER\_IN\_CHANNEL\_SEND\_MESSAGE\_NOT\_ALLOWEDThe request failed because the user is muted in the channel and isn't allowed to send the message. |
| `900050(request failed)` | CHANNEL\_FROZENThe request failed because the channel is frozen and no one can send the message to the channel. |
| `900060(request failed)` | PROFANITY\_MESSAGE\_BLOCKEDThe request failed because it specifies the message containing a profanity word. |
| `900061(request failed)` | BANNED\_URLS\_BLOCKEDThe request failed because it specifies the message containing a URL that isn't allowed. |
| `900065(request failed)` | RESTRICTED\_DOMAIN\_BLOCKEDThe request failed because it comes from the domain that isn't allowed. |
| `900066(request failed)` | MODERATED\_FILE\_BLOCKEDThe request failed because it contains the file violating at least one of the content management policies. |
| `900070(request failed)` | ENTER\_DELETED\_CHANNELThe request failed because the user is trying to enter a deleted channel. |
| `900080(request failed)` | BLOCKED\_USER\_RECEIVE\_MESSAGE\_NOT\_ALLOWEDThe request failed because the blocking user is trying to send the message to the blocked user in a 1-to-1 distinct channel. |
| `900081(request failed)` | DEACTIVATED\_USER\_RECEIVE\_MESSAGE\_NOT\_ALLOWEDThe request failed because the user is trying to send the message to the deactivated user in a 1-to-1 distinct channel. |
| `900090(request failed)` | WRONG\_CHANNEL\_TYPEThe request failed because it specifies a wrong channel type. |
| `900100(request failed)` | BANNED\_USER\_SEND\_MESSAGE\_NOT\_ALLOWEDThe request failed because the user is banned from the channel and isn't allowed to send the message. |
| `900200(request failed)` | TOO\_MANY\_MESSAGESThe number of the sent messages exceeds the allowed amount. |
| `900300(request failed)` | MESSAGE\_NOT\_FOUNDThe request failed because the message to edit can't be retrieved. |
| `900400(request failed)` | TOO\_MANY\_PARTICIPANTSThe number of the channel's participants exceeds the allowed amount. |
| `900500(request failed)` | CHANNEL\_NOT\_FOUNDThe request failed because there is no channel to perform this operation. |

## Logger

Vyin Chat SDK for iOS offers a logging system that allows you to keep track of a number of events and activities including data flow, error, and information while running your app. You can closely monitor the operation of the Vyin Chat SDK and improve debug efficiency using our logging system.  

---

### Log levels

Vyin Chat SDK's logger is based on the log levels classified by iOS. Log levels can be used to categorize and control log outputs. There are a total of six log levels, which are `ALL` (verbose), `DEBUG`, `INFO`, `WARN`, `ERROR`, and `NONE`. Apart from `NONE`, the other five log levels take precedence over the other in the following order: `ALL` (verbose) \> `DEBUG` \> `INFO` \> `WARN` \> `ERROR`. If you would rather not use the logger, you can select `NONE` to opt out of recording `GIM` logs.

| Level | Description |
| :---- | :---- |
| `NONE` | No logs recorded. |
| `ALL` | Logs detailed information about all events and activities, including the log messages in `DEBUG`, `INFO`, `WARN`, and `ERROR`. |
| `DEBUG` | Logs what's happening inside the SDK that could be helpful to debug unexpected behaviors, as well as the log messages in `INFO`, `WARN`, and `ERROR`. |
| `INFO` | Logs the general events that take place in the Vyin Chat SDK, as well as the log messages in `WARN` and `ERROR`. |
| `WARN` | Logs unexpected events which wouldn’t affect the operation of Vyin Chat SDK but might cause other issues. This log level also shows the log messages in `ERROR`. |
| `ERROR` | Logs things that have caused failures in specific events, but not an SDK-wide failure. |

Note: If an unexpected behavior is caused by a user's mistake or environmental factors, the behavior is classified as `WARN`. If it is due to the Vyin Chat SDK or Vyin Chat internal operations, it is classified as `ERROR`.  

---

### How to configure the log level

The default log level set for Vyin Chat SDK for iOS is `LogLevel.WARN`, which means that Vyin Chat SDK keeps logs of both errors and warning messages. You can change the settings through the `logLevel` property in the `InitParams` class and pass it on when initializing the SDK as follows.  

```swift
let params = InitParams(applicationId: applicationId, isLocalCachingEnabled: true, logLevel: .verbose)
GIMChat.initialize(params: params)
```

| Parameter | Type | Description |
| :---- | :---- | :---- |
| `logLevel` | LogLevel | Specifies the severity level of the log to retrieve. One takes precedence over the other in the order of `VERBOSE`, `DEBUG`, `INFO`, `WARN`, and `ERROR`. You can also use `NONE` to stop recording `GIM` logs. |

---

### Log filtering

All Vyin Chat SDK log messages start with `GIM`. If you want to see the log messages from Vyin Chat SDK, search for messages with the keyword `GIM`.















