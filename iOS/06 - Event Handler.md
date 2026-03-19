# Vyin Chat SDK for iOS - Event Handler

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

