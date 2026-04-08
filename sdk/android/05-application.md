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

## Session management

The GIM SDK manages session keys internally and automatically handles token refresh in most cases. This section explains how the session mechanism works, what the SDK handles for you, and what your app needs to implement.

### Overview

When a user connects to the GIM Chat server, the SDK exchanges the user's **access token** (issued by your App Backend) for an internal **session key** managed by GIM. The session key has a limited lifetime. When it expires, the SDK automatically requests a new access token from your app and renews the session key — without interrupting the user experience.

### How session renewal works

The SDK detects session expiry from three sources:

| Trigger | When it occurs |
|---|---|
| HTTP API response | A REST API call returns an expiry error (e.g. `400309`) |
| WebSocket `EXPR` event | Server pushes a session expiry notification while connected |
| LOGI failure on reconnect | Session expired while the app was offline or backgrounded |

In all three cases, the SDK automatically starts the refresh flow and retries the original operation after a new session key is obtained. **Your app does not need to call `connect()` again.**

### Setting up SessionHandler

Register a `SessionHandler` before calling `connect()`. The SDK calls this handler whenever it needs a new access token.

```kotlin
GIMChat.setSessionHandler(object : SessionHandler() {

    override fun onSessionTokenRequired(sessionTokenRequester: SessionTokenRequester) {
        // Fetch a new access token from your App Backend.
        yourAppBackend.getAccessToken { newToken, error ->
            if (error != null) {
                // Failed to get a token — tell the SDK to stop retrying.
                sessionTokenRequester.onFail()
            } else {
                // Provide the new access token to the SDK.
                sessionTokenRequester.onSuccess(newToken)
            }
        }
    }

    override fun onSessionClosed() {
        // The session has been permanently revoked and cannot be recovered.
        // Navigate the user to the login screen.
        navigateToLogin()
    }
})
```

Call `setSessionHandler()` **before** `connect()` to ensure no token request is missed.

### SessionHandler callbacks

#### `onSessionTokenRequired(sessionTokenRequester)`

Called when the SDK needs a new access token to renew the session key. Your app must fetch a fresh token from your App Backend and respond using the `SessionTokenRequester`:

| Method | Description |
|---|---|
| `sessionTokenRequester.onSuccess(newToken)` | Provide the new access token |
| `sessionTokenRequester.onFail()` | Decline the refresh (SDK will stop retrying) |

> **Timeout:** The SDK waits up to **10 seconds** for a response. If neither `onSuccess()` nor `onFail()` is called within this window, the SDK treats it as a failure.

> **Retry limit:** The SDK retries up to **3 times** before giving up and calling `onSessionClosed()`.

#### `onSessionClosed()`

Called when the session is permanently revoked (error `400310`) or when all retry attempts have been exhausted. This is not recoverable — your app should clear local credentials and redirect the user to sign in again.

#### `onSessionRefreshed()` *(optional)*

Called after the SDK has successfully renewed the session key. You can use this callback for logging or telemetry.

#### `onSessionError(exception: GIMException)` *(optional)*

Called when an error occurs during the session refresh process. Useful for debugging.

### Full interaction flow

The following diagram shows the complete session renewal flow across your App, the SDK, GIM Backend, and App Backend:

```
   App              SDK              GIM BE         App BE
    │                │                  │               │
    │  connect()     │                  │               │
    │───────────────►│                  │               │
    │                │──── LOGI ───────►│               │
    │                │◄─── session key ─│               │
    │                │                  │               │
    │           ... time passes, session key expires ... │
    │                │                  │               │
    │                │──── API call ───►│               │
    │                │◄── 400309 error ─│               │
    │                │                  │               │
    │◄───────────────│ onSessionTokenRequired()          │
    │                │                  │               │
    │───────────────────────────────────────────────────►│
    │◄─────────────────────────────────── new token ─────│
    │                │                  │               │
    │────────────────► onSuccess(newToken)               │
    │                │                  │               │
    │                │──── LOGI (new token) ───────────►│               │
    │                │◄─── new session key ─────────────│               │
    │                │                  │               │
    │                │──── retry API call ────────────►│               │
    │                │◄─── success ────────────────────│               │
```

**Offline / background scenario**

If the app goes offline before receiving the `EXPR` event, the session key may have already expired when the app returns to the foreground. The SDK handles this automatically:

1. SDK reconnect loop sends LOGI
2. GIM BE returns session expiry error
3. SDK calls `onSessionTokenRequired()` — same flow as above
4. After receiving a new token, SDK retries LOGI and reconnects

Your app does not need to detect or handle this case separately.

### App Backend responsibilities

Your App Backend is responsible for:

1. **Issuing access tokens** — Generate a short-lived access token for each user upon request.
2. **Token endpoint** — Expose an endpoint that the App can call from `onSessionTokenRequired()`.

> **Recommendation:** Cache a valid token in memory on the App side so that `onSessionTokenRequired()` can respond immediately without a network round-trip. This reduces the risk of hitting the 10-second timeout.

### Session error codes

| Code | Name | Description |
|---|---|---|
| `400302` | `ERR_INVALID_ACCESS_TOKEN` | The access token provided is invalid |
| `400309` | `ERR_SESSION_KEY_EXPIRED` | The session key has expired |
| `400310` | `ERR_SESSION_TOKEN_REVOKED` | The session has been permanently revoked |
| `800502` | `ERR_SESSION_KEY_REFRESH_FAILED` | Client-side session refresh failure |

### Best practices

- **Set `SessionHandler` before `connect()`** to avoid missing any token requests during the initial handshake.
- **Cache the access token** in your App so `onSessionTokenRequired()` can respond within the 10-second window without a network round-trip.
- **Do not call any SDK methods** inside `onSessionTokenRequired()`. The SDK is in a suspended state during this callback, and making SDK calls can cause a deadlock.
- **Always implement `onSessionClosed()`** to handle the revoked session case. Without this, the user may be stuck in a broken state with no path to recovery.
- **Use `onSessionError()` for logging** to help diagnose token refresh failures in production.
- **Use `authToken` in `connect()`** for authenticated users. Guest connections cannot recover from session expiry automatically.

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
