# Vyin Chat SDK for iOS - Local Caching

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

