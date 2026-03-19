# Vyin Chat SDK for Android - Event Handler

## Event handler

Vyin Chat SDK for Android provides three types of event handlers for client apps: channel event handlers, user event handlers, and connection event handlers. Through the channel event handlers and user event handlers, the Vyin Chat server notifies client apps in the foreground of events that are happening in channels and with users. Through the connection event handler, the Vyin Chat server detects changes in the connection status of a client app. When the client app is disconnected from the server, the SDK tries to reconnect to the server and notifies the client app of the result through the connection event handler.

The Chat SDK communicates with our server through persistent WebSocket connections and multi-thread processing, receiving callbacks of asynchronous channel, user, and connection events through event handlers in real-time. This interaction enables you to track the events you desire and implement your own chat features associated with those events.

---

## Functionalities by event handlers

The following is a list of event handler functionalities that our Chat SDK provides.

#### Managing a channel event handler

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Add or remove a channel event handler | Adds or removes an event handler that receives information about events happening in channels from the Vyin Chat server. | | |

#### Managing a user event handler

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Add or remove a user event handler | Adds or removes an event handler that receives information about events related to users connected to the Vyin Chat server. | | |

#### Managing a connection event handler

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Add or remove a connection event handler | Adds or removes an event handler that receives information about changes related to the Vyin Chat server connection status. | | |

---

## Add or remove a channel event handler

To receive and retrieve information about events happening in channels from the Vyin Chat server, you can add a channel event handler with its `UNIQUE_HANDLER_ID` by calling the `GIMChat.addChannelHandler()` method. You should declare `UNIQUE_HANDLER_ID` when registering multiple concurrent handlers.

There are three types of channel handlers: `BaseChannelHandler`, `GroupChannelHandler`, and `OpenChannelHandler`. The following tables show a list of events for each channel event handler.

If you want to stay informed of changes related to channels and notify other users' client apps of those changes, define and register multiple channel event handlers to each [Activity](https://developer.android.com/reference/android/app/Activity) instance.

---

## Channel event types

#### List of open channel events

| Method | Invoked when | Notified devices |
| :---- | :---- | :---- |
| `onMessageReceived()` | A message has been received in an open channel. | All devices where client apps with the channel are in the foreground, except the device that sent the message. |
| `onMessageUpdated()` | A message has been updated in an open channel. | All devices where client apps with the channel are in the foreground, except the device where the message was updated. |
| `onMessageDeleted()` | A message has been deleted in an open channel. | All devices where client apps with the channel are in the foreground, including the device where the message was deleted. |
| `onMentionReceived()` | A user has been mentioned in a message sent in an open channel. | All devices of mentioned users (up to 10) in the channel, including the device that was used to mention other users. |
| `onChannelChanged()` | One of the following open channel properties has been changed: name, cover image, data, custom type, or operators. | All devices that are connected to the changed channel, including the device where the channel was changed. |
| `onChannelDeleted()` | An open channel has been deleted. | All devices where client apps with the channel are in the foreground, including the device where the channel was deleted. |
| `onChannelFrozen()` | An open channel has been frozen. | All devices where client apps with the channel are in the foreground, including the device where the channel was frozen. |
| `onChannelUnfrozen()` | An open channel has been unfrozen. | All devices where client apps with the channel are in the foreground, including the device where the channel was unfrozen. |
| `onMetaDataCreated()` | A metadata for an open channel has been created. | All devices where client apps with the channel are in the foreground, including the device where the metadata was created. |
| `onMetaDataUpdated()` | A metadata for an open channel has been updated. | All devices where client apps with the channel are in the foreground, including the device where the metadata was updated. |
| `onMetaDataDeleted()` | A metadata for an open channel has been deleted. | All devices where client apps with the channel are in the foreground, including the device where the metadata was deleted. |
| `onMetaCounterCreated()` | A metacounter for an open channel has been created. | All devices where client apps with the channel are in the foreground, including the device where the metacounter was created. |
| `onMetaCounterUpdated()` | A metacounter for an open channel has been updated. | All devices where client apps with the channel are in the foreground, including the device where the metacounter was updated. |
| `onMetaCounterDeleted()` | A metacounter for an open channel has been deleted. | All devices where client apps with the channel are in the foreground, including the device where the metacounter was deleted. |
| `onUserEntered()` | A user has entered an open channel. | All devices where client apps with the channel are in the foreground, including the device that the user entered that channel. |
| `onUserExited()` | A user has exited an open channel. | All devices where client apps with the channel are in the foreground, except the device that the user exited that channel. |
| `onUserMuted()` | A user has been muted in an open channel. | All devices where client apps with the channel are in the foreground, including the device where the user was muted. |
| `onUserUnmuted()` | A user has been unmuted in an open channel. | All devices where client apps with the channel are in the foreground, including the device where the user was unmuted. |
| `onUserBanned()` | A user has been banned from an open channel. | All devices where client apps with the channel are in the foreground, including the device where the user was banned. |
| `onUserUnbanned()` | A user has been unbanned from an open channel. | All devices where client apps with the channel are in the foreground, including the device where the user was unbanned. |
| `onChannelParticipantCountChanged()` | One or more open channels' participant count has changed. | All connected devices where client apps have the affected open channels in their channel list. |

#### List of group channel events

| Method | Invoked when | Notified devices |
| :---- | :---- | :---- |
| `onMessageReceived()` | A message has been received in a group channel. | All devices where client apps with the channel are in the foreground, except the device that sent the message. |
| `onMessageUpdated()` | A message has been updated in a group channel. | All devices where client apps with the channel are in the foreground, except the device where the message was updated. |
| `onMessageDeleted()` | A message has been deleted in a group channel. | All devices where client apps with the channel are in the foreground, including the device where the message was deleted. |
| `onMentionReceived()` | A user has been mentioned in a message sent in a group channel. | All devices of mentioned users (up to 10) in the channel, except the device that was used to mention other users. |
| `onReactionUpdated()` | A message reaction has been updated in a group channel. | All devices where client apps with the channel are in the foreground, including the device that reacted to the message. |
| `onChannelChanged()` | One of the following group channel properties has been changed: distinct, push notification preferences, last message, unread message count, name, cover image, data, or custom type. | All devices that are connected to the changed channel, including the device where the channel was changed. |
| `onChannelDeleted()` | A group channel has been deleted. | All devices where client apps with the channel are in the foreground, including the device where the channel was deleted. |
| `onChannelFrozen()` | A group channel has been frozen. | All devices where client apps with the channel are in the foreground, including the device where the channel was frozen. |
| `onChannelUnfrozen()` | A group channel has been unfrozen. | All devices where client apps with the channel are in the foreground, including the device where the channel was unfrozen. |
| `onMetaDataCreated()` | A metadata for a group channel has been created. | All devices where client apps with the channel are in the foreground, including the device where the metadata was created. |
| `onMetaDataUpdated()` | A metadata for a group channel has been updated. | All devices where client apps with the channel are in the foreground, including the device where the metadata was updated. |
| `onMetaDataDeleted()` | A metadata for a group channel has been deleted. | All devices where client apps with the channel are in the foreground, including the device where the metadata was deleted. |
| `onMetaCounterCreated()` | A metacounter for a group channel has been created. | All devices where client apps with the channel are in the foreground, including the device where the metacounter was created. |
| `onMetaCounterUpdated()` | A metacounter for a group channel has been updated. | All devices where client apps with the channel are in the foreground, including the device where the metacounter was updated. |
| `onMetaCounterDeleted()` | A metacounter for a group channel has been deleted. | All devices where client apps with the channel are in the foreground, including the device where the metacounter was deleted. |
| `onChannelHidden()` | A group channel has been hidden from the list. | All devices of a user who hid the channel. |
| `onUserReceivedInvitation()` | A user has been invited to a group channel. | All devices where client apps with the channel are in the foreground, including the device of a user who received an invitation. |
| `onUserDeclinedInvitation()` | A user has declined an invitation to a group channel. | All devices where client apps with the channel are in the foreground, including the device of a user who declined an invitation. |
| `onUserJoined()` | A user has joined a group channel. | All devices where client apps with the channel are in the foreground, including all devices of the user who joined the channel, and except the devices of users who haven't accepted invitations. |
| `onUserLeft()` | A user has left a group channel. | All devices where client apps with the channel are in the foreground, including all devices of the user who left the channel, and except the devices of users who haven't accepted invitations. |
| `onDeliveryStatusUpdated()` | A message has been delivered in a group channel. | All devices where client apps with the channel are in the foreground, and except the device where the message was marked as delivered and has invoked this event. |
| `onReadStatusUpdated()` | A user has read a specific unread message in a group channel. | All devices where client apps with the channel are in the foreground, including all devices of the users who are invited to the channel, and except the device of the user who read the unread message. |
| `onTypingStatusUpdated()` | A user starts typing a message to a group channel. | All devices where client apps with the channel are in the foreground, except the devices of the users who are invited to the channel, and except all devices of the user who typed a message. |
| `onThreadInfoUpdated()` | A reply message has been created or deleted from a thread. | All devices where client apps with the channel are in the foreground, including devices of all users who are in the message thread, and except the device of the user who created or deleted the reply message. |
| `onUserMuted()` | A user has been muted in a group channel. | All devices where client apps with the channel are in the foreground, including the device where the user was muted. |
| `onUserUnmuted()` | A user has been unmuted in a group channel. | All devices where client apps with the channel are in the foreground, including the device where the user was unmuted. |
| `onUserBanned()` | A user has been banned from a group channel. | All devices where client apps with the channel are in the foreground, including the device where the user was banned. |
| `onUserUnbanned()` | A user has been unbanned from a group channel. | All devices where client apps with the channel are in the foreground, including the device where the user was unbanned. |
| `onChannelMemberCountChanged()` | One or more group channels' member count has changed. | All connected devices where client apps have the affected group channels in their channel list. |
| `onPinnedMessageUpdated()` | A message has been pinned or unpinned. | All connected devices where client apps have the affected group channels in their channel list. |

---

## Add a channel event handler

The following code samples show a full set of supported event callbacks with their parameters and how to add a channel event handler to the unique `GIMChat` instance.

```kotlin
// Add GroupChannelHandler.
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : GroupChannelHandler() {
        override fun onMessageReceived(channel: BaseChannel, message: BaseMessage) {}

        override fun onMentionReceived(channel: BaseChannel, message: BaseMessage) {}

        override fun onMessageDeleted(channel: BaseChannel, msgId: Long) {}

        override fun onMessageUpdated(channel: BaseChannel, message: BaseMessage) {}

        override fun onChannelChanged(channel: BaseChannel) {}

        override fun onChannelDeleted(channelUrl: String, channelType: ChannelType) {}

        override fun onReactionUpdated(channel: BaseChannel, reactionEvent: ReactionEvent) {}

        override fun onUserMuted(channel: BaseChannel, user: User) {}

        override fun onUserUnmuted(channel: BaseChannel, user: User) {}

        override fun onUserBanned(channel: BaseChannel, user: User) {}

        override fun onUserUnbanned(channel: BaseChannel, user: User) {}

        override fun onChannelFrozen(channel: BaseChannel) {}

        override fun onChannelUnfrozen(channel: BaseChannel) {}

        override fun onMetaDataCreated(channel: BaseChannel, metaDataMap: Map<String, String>) {}

        override fun onMetaDataUpdated(channel: BaseChannel, metaDataMap: Map<String, String>) {}

        override fun onMetaDataDeleted(channel: BaseChannel, keys: List<String>) {}

        override fun onMetaCountersCreated(channel: BaseChannel, metaCounterMap: Map<String, Int>) {}

        override fun onMetaCountersUpdated(channel: BaseChannel, metaCounterMap: Map<String, Int>) {}

        override fun onMetaCountersDeleted(channel: BaseChannel, keys: List<String>) {}

        override fun onOperatorUpdated(channel: BaseChannel) {}

        override fun onThreadInfoUpdated(channel: BaseChannel, threadInfoUpdateEvent: ThreadInfoUpdateEvent) {}

        // GroupChannelHandler's events
        override fun onReadStatusUpdated(channel: GroupChannel) {}

        override fun onDeliveryStatusUpdated(channel: GroupChannel) {}

        override fun onTypingStatusUpdated(channel: GroupChannel) {}

        override fun onUserReceivedInvitation(channel: GroupChannel, inviter: User?, invitees: List<User>) {}

        override fun onUserJoined(channel: GroupChannel, user: User) {}

        override fun onUserDeclinedInvitation(channel: GroupChannel, inviter: User?, invitee: User) {}

        override fun onUserLeft(channel: GroupChannel, user: User) {}

        override fun onChannelHidden(channel: GroupChannel) {}

        override fun onPinnedMessageUpdated(channel: BaseChannel) {}

        override fun onChannelMemberCountChanged(groupChannels: List<GroupChannel>) {}
    }
)

// Add OpenChannelHandler.
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : OpenChannelHandler() {
        override fun onMessageReceived(channel: BaseChannel, message: BaseMessage) {}

        override fun onUserEntered(channel: OpenChannel, user: User) {}

        override fun onUserExited(channel: OpenChannel, user: User) {}

        override fun onChannelParticipantCountChanged(openChannels: List<OpenChannel>) {}
    }
)
```

---

## Remove a channel event handler

The following code shows how to remove the channel event handler.

```kotlin
GIMChat.removeChannelHandler(UNIQUE_HANDLER_ID)
```

---

## Add or remove a connection event handler

To detect changes in the connection status of a client app, add a connection event handler with its unique user-defined ID by calling `GIMChat.addConnectionHandler()`.

If you want to stay informed of changes related to the Vyin Chat server connection status and notify client apps of these changes, define and register multiple connection event handlers to each [Activity](https://developer.android.com/reference/android/app/Activity) instance.

---

## Connection event types

#### List of connection events

| Method | Invoked when | Notified devices |
| :---- | :---- | :---- |
| `onConnected()` | The Chat SDK has been connected to the Vyin Chat server for the first time. | The device that is connected to the Vyin Chat server. |
| `onDisconnected()` | The user has been logged out. It is different from WebSocket disconnection where a client app goes to the background. This handler is called after `GIMChat.disconnect()` is called. | The device that is disconnected from the Vyin Chat server. |
| `onReconnectStarted()` | The Chat SDK has started reconnecting to the Vyin Chat server. | The device where `reconnect()` was automatically called by the Chat SDK or manually by the client app. |
| `onReconnectSucceeded()` | The Chat SDK has succeeded in reconnecting to the Vyin Chat server. | The device that successfully reconnected to the server. |
| `onReconnectFailed()` | The Chat SDK has failed to reconnect to the Vyin Chat server. | The device that failed to reconnect to the server. |

---

## Add a connection event handler

The following code shows a full set of supported event callbacks and how to add a connection event handler to the unique `GIMChat` instance.

```kotlin
GIMChat.addConnectionHandler(
    UNIQUE_HANDLER_ID,
    object : ConnectionHandler {
        override fun onConnected(userId: String) {}

        override fun onDisconnected(userId: String) {}

        override fun onReconnectStarted() {}

        override fun onReconnectSucceeded() {}

        override fun onReconnectFailed() {}
    }
)
```

---

## Remove a connection event handler

The following code shows how to remove the connection event handler.

```kotlin
GIMChat.removeConnectionHandler(UNIQUE_HANDLER_ID)
```

---

## Add or remove a user event handler

To receive information about events related to users connected to the Vyin Chat server, add a user event handler with its unique user-defined ID by calling `GIMChat.addUserEventHandler()`.

If you want to stay informed of changes related to users and notify users' client apps of these changes, define and register multiple user event handlers to each [Activity](https://developer.android.com/reference/android/app/Activity) instance.

---

## User event types

#### List of user events

> **Note:** The `onTotalUnreadMessageCountChanged()` attribute is turned off by default. Contact us if you wish to use this feature.

| Method | Invoked when | Notified devices |
| :---- | :---- | :---- |
| `onTotalUnreadMessageCountChanged()` | A user has read messages in the joined group channels and there is an update on the total number of the user's unread messages. | The user's devices running client apps. The devices are notified of the total number of unread messages along with a collection of the number of unread messages by custom channel type. |

---

## Add a user event handler

The following code shows a full set of supported event callbacks with their parameters and how to add a user event handler to the unique `GIMChat` instance.

```kotlin
GIMChat.addUserEventHandler(
    UNIQUE_HANDLER_ID,
    object : UserEventHandler() {
        override fun onTotalUnreadMessageCountChanged(totalCount: Int, totalCountByCustomType: Map<String, Int>) {}
    }
)
```

---

## Remove a user event handler

The following code shows how to remove the user event handler.

```kotlin
GIMChat.removeUserEventHandler(UNIQUE_HANDLER_ID)
```
