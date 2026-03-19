# Vyin Chat SDK for Android - Application

## Authentication

In order to use the features of the Chat SDK for Android in your client apps, a GIMChat instance should be initiated in each client app through user authentication with the Vyin Chat server. The instance communicates and interacts with the server based on the authenticated user account and is allowed to use the Chat SDK's features. This page explains how to authenticate your user with the server.

## Initialize the Chat SDK with APP_ID

To use our chat features, you must initialize the GIMChat instance by passing the APP_ID of your Vyin Chat application as an argument to a parameter in the `GIMChat.init()` method. The `GIMChat.init()` method must be called once across your client app. Typically, initialization is implemented in the user login view.

With the implementation of local caching, you must determine whether you would like to use local caching and configure its settings through `InitParams`. The params contains properties such as `useCaching`. Set `useCaching` to `true` so that the SDK can use local cache for its collection instances.

Additionally, `InitResultHandler` has been added to the initialization code as shown below. The `InitResultHandler` gets the initialization status through different event handlers and informs the client app whether the initialization is successful or not. The `onMigrationStarted()` callback is called when the migration for local caching has started.

```kotlin
GIMChat.init(
    InitParams(
        APP_ID,
        applicationContext,
        useCaching = true
    ), object : InitResultHandler {
        override fun onInitFailed(exception: GIMException) {
            // If useCaching is true and init fails, the SDK will turn off the useCaching flag
            // so that you can still proceed with your app.
        }

        override fun onMigrationStarted() {
            // This won't be called if useCaching is set to false.
        }

        override fun onInitSucceed() {
            // Called when initialization is completed.
        }
    })
```

## Connect to the Vyin Chat server with a user ID

By default, the Vyin Chat server can authenticate a user with just a unique user ID. The server queries the database to check for a match upon connection request. If no matching user ID is found, the server creates a new user account with the user ID. The ID should be unique within a Vyin Chat application to be distinguishable from other identifiers such as a hashed email address or a phone number in your service.

While authenticating with just the user ID is convenient in the developing and testing stages of a service, a more secure authentication process using tokens is strongly recommended for most production environments.

First, you need to get a callback from `InitResultHandler` in order to connect. When the user is confirmed without any error, the SDK will proceed to connect with the Vyin Chat server.

When one of the error codes 400300, 400301, 400302, or 400310 returns, the SDK clears all user data cached in the local storage and tries to reconnect to the Vyin Chat server. Except when these errors occur, the client app can still draw a channel list view and a chat view in an offline mode using locally cached data. The SDK will receive a user object through a callback and try to reconnect later. When the connection is made, `ConnectionHandler.onReconnectSucceeded()` will be called.

For Chat SDKs that don't use local caching, the connection process remains the same. When an error occurs, the SDK must attempt to reconnect again.

> **Note:** Apart from initializing the GIMChat instance, you should connect to the Vyin Chat server before calling almost every method through the Chat SDK. If you attempt to call a method without connecting, an `ERR_CONNECTION_REQUIRED (800101)` error will be returned.

## Connect to the Vyin Chat server with a user ID and a token

For a more secure way of authenticating a user, you can require an authentication token — which can be an access token or a session token — in addition to a unique user ID. Any token issued for a user must be provided to the Vyin Chat server each time the user logs in by passing the token as an argument to the `authToken` parameter of the `GIMChat.connect()` method.

### Using an access token

Through our Chat Platform API, an access token can be generated when creating a user. You can also issue an access token for an existing user. Once an access token is issued, a user is required to provide the access token in the `GIMChat.connect()` method which is used for logging in.

1. Using the Chat API, create a Vyin Chat user account with information submitted when a user signs up or logs in to your service.
2. Save the user ID along with the issued access token to your persistent storage which is securely managed.
3. When the user attempts to log in to a client app, load the user ID and access token from the storage, and then pass them to the `GIMChat.connect()` method.
4. Periodically replacing the user's access token is recommended to protect the account.

```kotlin
GIMChat.connect(USER_ID, AUTH_TOKEN) { user, e ->
    if (user != null) {
        if (e != null) {
            // Proceed in offline mode with the data stored in the local database.
            // Later, connection will be made automatically.
            // The connection will be notified through ConnectionHandler.onReconnectSucceeded().
        } else {
            // Proceed in online mode.
        }
    } else {
        // Handle error.
    }
}
```

For Chat SDKs that don't use local caching, the connection process remains the same. If the user has ever been connected and their data exists in the local storage, the SDK can be connected to the Vyin Chat server. When an error occurs, the SDK must attempt to reconnect again.

### Using a session token

You can also use a session token instead of an access token to authenticate a user. Session tokens are a more secure option because they expire after a certain period whereas access tokens don't. See the Chat Platform API guides for further explanation about the difference between access tokens and session tokens, how to issue a session token, and how to revoke all session tokens.

## Set a session handler

When a user is authenticated with a session token, the Chat SDK connects the user to the Vyin Chat server and can send data requests to the server for ten minutes as long as the session token hasn't expired or hasn't been revoked.

Upon the user's session expiration, the Chat SDK will refresh the session internally using a `SessionHandler`. However, if the session token has expired or has been revoked, the Chat SDK can't do so. In that case, the client app needs to implement a `SessionHandler` instance to refresh the token and pass it back to the SDK so that it can refresh the session again.

> **Note:** A `SessionHandler` instance must be set before the server connection is requested.

The following code shows how to implement the handler methods.

```kotlin
GIMChat.setSessionHandler(
    object : SessionHandler() {
        override fun onSessionTokenRequired(sessionTokenRequester: SessionTokenRequester) {
            // A new session token is required in the SDK to refresh the session.
            // Refresh the session token and pass it onto the SDK through sessionTokenRequester.onSuccess(NEW_TOKEN).
            // If you do not want to refresh the session, pass on a null value through sessionTokenRequester.onSuccess(null).
            // If any error occurred while refreshing the token, let the SDK know about it through sessionTokenRequester.onFail().
        }

        override fun onSessionClosed() {
            // The session refresh has been denied from the app.
            // Client apps should guide the user to a login page to log in again.
        }

        override fun onSessionRefreshed() {
            // This is optional and no action is required.
            // This is called when the session is refreshed.
        }

        override fun onSessionError(exception: GIMException) {
            // This is optional and no action is required.
            // This is called when an error occurs during the session refresh.
        }
    }
)
```

When `SessionHandler.onSessionTokenRequired(SessionTokenRequester)` is invoked, the SDK waits for a specific amount of time to receive a new session token from the client app.

If neither `SessionTokenRequester.onSuccess()` nor `SessionTokenRequester.onFail()` are called within the specified timeout period, the socket connection will be disconnected. If this occurs, the client app has to manually call `GIMChat.connect(USER_ID, AUTH_TOKEN)` for a new socket connection.

## Disconnect from the Vyin Chat server

Disconnect a user from the Vyin Chat server when they no longer need to receive messages from an online state. This is equivalent to the logout behavior rather than disconnecting the socket. If you want to disconnect only the WebSocket without clearing locally cached data, refer to the disconnect WebSocket only section below. However, unless the user unregisters the push token, the user will still receive push notifications for new messages from group channels they've joined.

When a client app is disconnected, all event handlers registered through `GIMChat.addChannelHandler()` stop receiving event callbacks from the server. All internally cached data in the client app are then flushed. This includes channels that are cached when the `getChannel()` method of `OpenChannel` or `GroupChannel` is called, as well as locally cached channels and messages.

```kotlin
GIMChat.disconnect {
    // The current user is disconnected from the Vyin Chat server and all locally saved data are cleared.
    // ...
}
```

### Disconnect the WebSocket only

While calling `GIMChat.disconnect()` disconnects the WebSocket and clears local cache data, you can call `GIMChat.disconnectWebSocket()` to disconnect the WebSocket only and keep the locally cached data.

```kotlin
GIMChat.disconnectWebSocket {
    // onDisconnected
}
```

To reconnect after calling `disconnectWebSocket()`, use `GIMChat.connect()`.

---

## Rate limits

Vyin Chat applications are rate-limited to ensure the best experience for users. If you exceed a rate limit, the Vyin Chat server will return an error message that includes the encountered rate limit as well as how long you should wait before retrying.

> **Note:** Vyin Chat organizations created after May 28, 2020, 00:00:00 UTC are automatically rate-limited, while those created before this date are given sufficient time to adjust their applications before rate limits apply. Go to Settings > Application > General on Vyin Chat Console to see which rate limits your organization is subject to. If you need a higher rate limit for API requests, contact our sales team for further assistance.

### Default settings

To prevent abnormal user activity, Vyin Chat applications have the following default limits on the number of messages per second that a user can send and an open channel can display.

#### Limits on number of messages per second

| Imposed on | Limit | If exceeded |
| :---- | :---- | :---- |
| User | Five messages per second | Excess messages are neither sent to the channel nor saved in the database but are displayed in the user's channel view. |
| Channel | Five messages per second | Excess messages aren't displayed but saved in the database. |

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

### Rate-limited real-time events

Several real-time events taking place in client apps are rate-limited in order to maintain stable and efficient operation. The following table lists the default rate limits for those events.

#### Limits on events

| Event | Calls per second |
| :---- | :---- |
| Sending a message | 5 |
| Marking messages as read | 3 |
| Sending typing indicator to other members* | 0.1 |
| Other events** | 3 |

\* The limit applies to both users and channels.

\*\* These include sending a file message, editing a message, or marking messages as delivered.

### Error responses

When a request is rate-limited, a `RATE_LIMIT_EXCEEDED (500910)` or `TOO_MANY_MESSAGES (900200)` error is returned. See the error codes page for more information.
