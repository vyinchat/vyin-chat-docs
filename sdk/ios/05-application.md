# Vyin Chat SDK for iOS - Application

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

