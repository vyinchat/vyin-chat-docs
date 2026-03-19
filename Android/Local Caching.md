# Vyin Chat SDK for Android - Local Caching

## Local caching

Local caching enables Vyin Chat SDK for Android to locally cache and retrieve group channel and message data. Its benefits include reducing refresh time and allowing a client app to create a channel list or a chat view that can work online as well as offline, which can be used for offline messaging.

You can enable local caching when initializing the Chat SDK by setting the optional parameter `useCaching` to `true`. See Initialize the Chat SDK and Connect to the Vyin Chat server to learn more.

Local caching relies on the `GroupChannelCollection` and `MessageCollection` classes, which are used to build a channel list view and a chat view, respectively. Collections are designed to react to events that can cause change in a channel list or chat view. An event controller in the SDK oversees such events and passes them to a relevant collection. For example, if a new group channel is created while the current user is looking at a chat view, the current view won't be affected by this event.

> **Note:** You can still use these collections when building your app without local caching.

---

## Functionalities by topic

The following is a list of functionalities our Chat SDK supports.

#### Using group channel collection

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Group channel collection | Keeps the client app's channel list synced with that on the Vyin Chat server. | | ✓ |

#### Using message collection

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Message collection | Keeps the client app's chat view synced with that on the Vyin Chat server. | | ✓ |

---

## GroupChannelCollection

You can use `GroupChannelCollection` to swiftly create a channel list view that stays up to date on all channel-related events. A `GroupChannelCollection` instance is composed of the following methods and variables.

```kotlin
// Create a GroupChannelListQuery object first.
val query = GroupChannel.createMyGroupChannelListQuery(
    GroupChannelListQueryParams().apply {
        includeEmpty = true
        order = GroupChannelListQueryOrder.CHRONOLOGICAL
    }
)
val params = GroupChannelCollectionCreateParams(query).apply {
    groupChannelCollectionHandler =
        object : GroupChannelCollectionHandler {
            override fun onChannelsAdded(context: GroupChannelContext, channels: List<GroupChannel>) {}

            override fun onChannelsUpdated(context: GroupChannelContext, channels: List<GroupChannel>) {}

            override fun onChannelsDeleted(context: GroupChannelContext, deletedChannelUrls: List<String>) {}
        }
}
// Create a GroupChannelCollection instance and set the event handlers.
val collection = GIMChat.createGroupChannelCollection(params)

// Load channels.
if (collection.hasMore) {
    collection.loadMore { channels, e ->
        if (e != null) {
            // Handle error.
            return@loadMore
        }
    }
}

// Clear the collection.
collection.dispose()
```

When the `createGroupChannelCollection()` method is called, the group channel data stored in the local cache and the Vyin Chat server are fetched and sorted based on the values in `GroupChannelListQuery`. Also, `groupChannelCollectionHandler` lets you set the event listeners that can subscribe to channel-related events when creating the collection.

As for pagination, `hasMore` checks if there are more group channels to load whenever a user hits the bottom of the channel list. If so, `loadMore()` retrieves channels from the local cache and the Vyin Chat server to display on the list.

To learn more about the collection and how to create a channel list view with it, see Group channel collection.

---

## MessageCollection

You can use `MessageCollection` to swiftly create a chat view that includes all data. A `MessageCollection` instance is composed of the following methods and variables.

```kotlin
fun createMessageCollection() {
    // You can use a MessageListParams instance for MessageCollection.
    val params = MessageListParams().apply {
        reverse = false
        // ...
    }

    val messageCollectionHandler = object : MessageCollectionHandler {
        override fun onMessagesAdded(
            context: MessageContext,
            channel: GroupChannel,
            messages: List<BaseMessage>
        ) {
            when (context.messagesSendingStatus) {
                SendingStatus.SUCCEEDED -> {}
                SendingStatus.PENDING -> {}
            }
        }

        override fun onMessagesUpdated(
            context: MessageContext,
            channel: GroupChannel,
            messages: List<BaseMessage>
        ) {
            when (context.messagesSendingStatus) {
                SendingStatus.SUCCEEDED -> {}
                SendingStatus.PENDING -> {} // failed -> pending
                SendingStatus.FAILED -> {} // pending -> failed
                SendingStatus.CANCELED -> {} // pending -> canceled
            }
        }

        override fun onMessagesDeleted(
            context: MessageContext,
            channel: GroupChannel,
            messages: List<BaseMessage>
        ) {
            when (context.messagesSendingStatus) {
                SendingStatus.SUCCEEDED -> {}
                SendingStatus.FAILED -> {}
            }
        }

        override fun onChannelUpdated(context: GroupChannelContext, channel: GroupChannel) {}

        override fun onChannelDeleted(context: GroupChannelContext, channelUrl: String) {}

        override fun onHugeGapDetected() {
            // This is called when there are more than 300 messages missing in the local cache
            // compared to the Vyin Chat server.

            // Clear the collection.
            collection.dispose()

            // Create a new message collection object.
            createMessageCollection()

            // An additional implementation is required for initialization.
        }
    }

    val collection = GIMChat.createMessageCollection(
        MessageCollectionCreateParams(groupChannel, params).apply {
            this.startingPoint = STARTING_POINT
            this.messageCollectionHandler = messageCollectionHandler
        }
    )
}

// Initialize messages from startingPoint.
fun initialize() {
    collection.initialize(
        MessageCollectionInitPolicy.CACHE_AND_REPLACE_BY_API,
        object : MessageCollectionInitHandler {
            override fun onCacheResult(cachedList: List<BaseMessage>?, e: GIMException?) {
                // Messages are retrieved from the local cache.
            }

            override fun onApiResult(apiResultList: List<BaseMessage>?, e: GIMException?) {
                // Messages are retrieved from the Vyin Chat server through API.
                // According to the InitPolicy.CACHE_AND_REPLACE_BY_API,
                // the existing data source needs to be cleared
                // before adding retrieved messages to the local cache.
            }
        }
    )
}

// Load the next set of messages.
fun loadNext() {
    if (collection.hasNext) {
        collection.loadNext { messages, e ->
            if (e != null) {
                // Handle error.
                return@loadNext
            }
        }
    }
}

// Load previous messages.
fun loadPrevious() {
    if (collection.hasPrevious) {
        collection.loadPrevious { messages, e ->
            if (e != null) {
                // Handle error.
                return@loadPrevious
            }
        }
    }
}

// Clear the collection.
fun dispose() {
    collection.dispose()
}
```

In the `MessageCollection` class, the initialization is dictated by `MessageCollectionInitPolicy`. This determines which data to use for the collection. Currently, we only support `CACHE_AND_REPLACE_BY_API`. According to this policy, the collection loads the messages stored in the local cache through `onCacheResult()`. Then `onApiResult()` replaces them with the messages fetched from the Vyin Chat server through an API request.

As for pagination, `hasNext` checks if there are more messages to load whenever a user hits the bottom of the chat view. If so, the `loadNext()` method retrieves messages from the local cache to display in the view. `hasPrevious` and `loadPrevious()` work the same way when the scroll reaches the top of the chat view.

To learn more about the collection and how to create a chat view with it, see Message collection.

---

## Gap and synchronization

A gap is created when messages or channels that exist on the Vyin Chat server are missing from the local cache. Such discrepancy occurs when a client app isn't able to properly load new events due to connectivity issues. To prevent such a gap, the Chat SDK constantly communicates with the Vyin Chat server and fetches data through synchronization, pulling a chunk of messages before and after `startingPoint`.

Such a gap can significantly widen if a user has been offline for a long time. If more than 300 messages are missing in the local cache compared to the Vyin Chat server, Vyin Chat SDK classifies this as a huge gap and `onHugeGapDetected()` is called. In case of a huge gap, it is more effective to discard the existing message collection and create a new one. On the other hand, a relatively small gap created when the SDK was offline can be filled in through changelog sync.

---

## Auto resend

A message is normally sent through the WebSocket connection which means the connection must be secured and confirmed before sending any messages. With local caching, you can temporarily keep an unsent message in the local cache if the WebSocket connection is lost. The Chat SDK with local caching enabled marks the failed message as pending, stores it locally, and automatically resends the pending message when the WebSocket is reconnected. This is called auto resend.

The following cases are eligible for auto resend.

- A user message couldn't be sent because the WebSocket connection was lost even before it was established.
- A file message couldn't upload the attached file to the Vyin Chat server.
- An attached file was uploaded to the Vyin Chat server but the file message itself couldn't be sent because the WebSocket connection was closed.

### User message

A user message is sent through the WebSocket. If a message couldn't be sent because the WebSocket connection was lost, the Chat SDK receives an error through a callback and queues the pending message in the local cache for auto resend. When the client app is reconnected, the SDK then attempts to resend the message.

If the message is successfully sent, the client app receives a response from the Vyin Chat server. Then `onMessagesUpdated()` is called to change the pending message to a sent message in the data source and updates the chat view.

### File message

A file message can be sent through either the WebSocket connection or an API request.

When sending a file message, the attached file must be uploaded to the Vyin Chat server as an HTTP request. To do so, the Chat SDK checks the status of the network connection. If the network isn't connected, the file can't be uploaded to the server. In this case, the SDK handles the file message as a pending message and adds it to the queue for auto resend.

If the network is connected and the file is successfully uploaded to the server, its URL is delivered in a response and the SDK replaces the file with its URL string. At first, the SDK attempts to send the message through the WebSocket. If the WebSocket connection is lost, the client app checks the network connection once again to make another HTTP request for the message. If the SDK detects the network as disconnected, it gets an error code that marks the message as a pending message, allowing the message to be automatically resent when the network is reconnected.

On the other hand, if the network is connected but the HTTP request fails, the message isn't eligible for auto resend.

If the message is successfully sent, the client app receives a response from the Vyin Chat server. Then `onMessagesUpdated()` is called to change the pending message to a sent message in the data source and updates the chat view.

### Failed message

If a message couldn't be sent due to some other error, `onMessagesUpdated()` is called to re-label the pending message as a failed message. Messages labeled as such can't be queued for auto resend. The following shows some of these errors.

```kotlin
GIMError.ERR_NETWORK
GIMError.ERR_ACK_TIMEOUT
```

> **Note:** A pending message can last in the queue only for three days. If the WebSocket connection is back online after three days, `onMessagesUpdated()` is called to mark any three-day-old pending message as a failed message.

---

## Other methods

The following code block shows a list of methods that can help you leverage the local caching functionalities.

The `BaseMessage.messageCreateParams` property is used when drawing pending messages in the chat view. This property returns the exact same `UserMessageCreateParams` or `FileMessageCreateParams` that you used when sending messages. The params can be passed to the following methods.

- `channel.sendUserMessage(UserMessageCreateParams, SendUserMessageHandler)`
- `channel.sendFileMessage(FileMessageCreateParams, SendFileMessageHandler)`

If you want to erase the entire cached data, use the `GIMChat.clearCachedData()` method.

```kotlin
GIMChat.clearCachedData(context: Context, handler: CompletionHandler?)

val messageCreateParams: BaseMessageCreateParams? = baseMessage.messageCreateParams
// This should be used for displaying pending messages in the chat view.
```

---

## Using group channel collection

A `GroupChannelCollection` instance allows you to swiftly create a channel list view that remains up to date on all channel-related events. This page explains how to draw a view using the collection.

---

### Create a collection

You can create a `GroupChannelCollection` instance through the `createGroupChannelCollection()` method.

First, create a `GroupChannelListQuery` instance through the `createMyGroupChannelListQuery()` method and its query setters. This determines which channel to include in the channel list and how to list channels in order.

Once the collection is created, you should call `loadMore()`.

```kotlin
// First, create a GroupChannelListQuery instance in GroupChannelCollection.
val query = GroupChannel.createMyGroupChannelListQuery(
    GroupChannelListQueryParams().apply {
        includeEmpty = true
        order = GroupChannelListQueryOrder.CHRONOLOGICAL
        // Acceptable values are CHRONOLOGICAL, LATEST_LAST_MESSAGE, CHANNEL_NAME_ALPHABETICAL,
        // and METADATA_VALUE_ALPHABETICAL.
        // You can add other params setters.
    }
)
// Create a GroupChannelCollection instance.
val collection: GroupChannelCollection = GIMChat.createGroupChannelCollection(
    GroupChannelCollectionCreateParams(query)
)
```

---

### Pagination

A `GroupChannelCollection` instance retrieves more channels to display in the view through the `loadMore()` method.

Whenever a scroll reaches the bottom of the channel list view, the `loadMore()` method is called and `hasMore` first checks if there are more channels to load. If so, `loadMore()` fetches them.

The `loadMore()` method should also be called after you've created a `GroupChannelCollection` instance.

```kotlin
// Check whether there are more channels to load before calling loadMore().
if (collection.hasMore) {
    collection.loadMore { channels, e ->
        if (e != null) {
            // Handle error.
            return@loadMore
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
| `onChannelsAdded()` | A new group channel is created as a real-time event, or new group channels are fetched through changelog sync. |
| `onChannelsUpdated()` | The channel information included in the user's current chat view is updated as a real-time event, or updated channel information is fetched through changelog sync. |
| `onChannelsDeleted()` | A group channel is deleted as a real-time event, or a channel deletion event is fetched through changelog sync. |

```kotlin
val collection: GroupChannelCollection = GIMChat.createGroupChannelCollection(
    GroupChannelCollectionCreateParams(query).apply {
        groupChannelCollectionHandler = object : GroupChannelCollectionHandler {
            override fun onChannelsAdded(context: GroupChannelContext, channels: List<GroupChannel>) {
                // Add new channels to your data source.
            }

            override fun onChannelsUpdated(context: GroupChannelContext, channels: List<GroupChannel>) {
                // Update the existing channels in your data source.
            }

            override fun onChannelsDeleted(context: GroupChannelContext, deletedChannelUrls: List<String>) {
                // Delete the channels with the matching deletedChannelUrls from your data source.
            }
        }
    }
)
```

---

### Loaded channels

The `GroupChannelCollection` holds the list of channels that are loaded by `loadMore()` or by channel events in the `channelList` property. It is recommended to use this list as the data source of your `RecyclerView` as it represents the most up-to-date list.

```kotlin
val channelList = collection.channelList
```

---

### Dispose of the collection

The `dispose()` method should be called when you destroy the channel list screen to notify the SDK that this collection is no longer visible.

```kotlin
collection.dispose()
```

---

## Using message collection

A `MessageCollection` instance allows you to swiftly create a chat view that includes all data. This page explains how to make a view using the collection.

---

### Create a collection

First, create a `MessageCollection` instance through the `createMessageCollection()` method. Here, you need to set a `MessageListParams` instance to determine the message order and the starting point of the message list in the chat view.

```kotlin
fun createMessageCollection() {
    // Create a MessageListParams to be used in the MessageCollection.
    val params = MessageListParams().apply {
        reverse = false
    }
    // You can add other query setters.
    val collection = GIMChat.createMessageCollection(
        MessageCollectionCreateParams(groupChannel, params).apply {
            startingPoint = STARTING_POINT
        }
    )
}
```

#### Starting point

The reference point for message retrieval in a chat view is `startingPoint`. This should be specified as a timestamp.

If you set the value of `startingPoint` to `Long.MAX_VALUE`, messages in the view are retrieved in reverse chronological order, showing messages from the most to least recent.

If you set the value of `startingPoint` to the message last read by the current user, messages in the view are fetched in chronological order, starting from the last message seen by the user to the latest.

---

### Initialization

After creating a `MessageCollection` instance, initialize the instance through the `initialize()` method. Here, you need to set `MessageCollectionInitPolicy`.

#### Policy

The `MessageCollectionInitPolicy` property determines how initialization deals with the message data retrieved from the local cache and API calls. Because we only support `MessageCollectionInitPolicy.CACHE_AND_REPLACE_BY_API` at this time, messages in the local cache must be cleared prior to adding the messages from the Vyin Chat server.

| Policy | Description |
| :---- | :---- |
| `CACHE_AND_REPLACE_BY_API` | Retrieves cached messages from the local cache and then replaces them with the messages pulled from the Vyin Chat server through API calls. |

```kotlin
// Initialize messages from the startingPoint.
fun initialize() {
    collection.initialize(
        MessageCollectionInitPolicy.CACHE_AND_REPLACE_BY_API,
        object : MessageCollectionInitHandler {
            override fun onCacheResult(cachedList: List<BaseMessage>?, e: GIMException?) {
                // Messages are retrieved from the local cache.
                // They can be outdated or far from the startingPoint.
            }

            override fun onApiResult(apiResultList: List<BaseMessage>?, e: GIMException?) {
                // Messages are retrieved through API calls from the Vyin Chat server.
                // According to MessageCollectionInitPolicy.CACHE_AND_REPLACE_BY_API,
                // the existing data source needs to be cleared
                // before adding retrieved messages to the local cache.
            }
        }
    )
}
```

---

### Pagination

Use the `loadPrevious()` and `loadNext()` methods to retrieve messages from the previous page and the next page.

When the `loadPrevious()` method is called, the Chat SDK first checks whether there are more messages to load from the previous page through `hasPrevious`. The same goes for `hasNext` and `loadNext()`.

These methods have to be called during initialization as well.

```kotlin
// Load the previous messages when the scroll reaches the first message in the chat view.
if (collection.hasPrevious) {
    collection.loadPrevious { messages, e ->
        if (e != null) {
            // Handle error.
            return@loadPrevious
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
| `onMessagesAdded()` | A new message is created as a real-time event, or new messages are fetched during changelog sync. |
| `onMessagesDeleted()` | A message is deleted as a real-time event, message deletion is detected during changelog sync, or the value of the `MessageListParams` setter changes. |
| `onMessagesUpdated()` | A message is updated as a real-time event, message update is detected during changelog sync, or the send status of a pending message changes. |
| `onChannelUpdated()` | The channel information included in the user's current chat view is updated as a real-time event, or channel info update is detected during changelog sync. |
| `onChannelDeleted()` | The current channel is deleted as a real-time event, or channel deletion is detected during changelog sync. In both cases, the entire view should be disposed of. |
| `onHugeGapDetected()` | A huge gap is detected through background sync. In this case, you need to dispose of the view and create a new `MessageCollection` instance. |

```kotlin
val collection = GIMChat.createMessageCollection(
    MessageCollectionCreateParams(groupChannel, messageListParams).apply {
        messageCollectionHandler = object : MessageCollectionHandler {
            override fun onMessagesAdded(context: MessageContext, channel: GroupChannel, messages: List<BaseMessage>) {
                when (context.messagesSendingStatus) {
                    SendingStatus.SUCCEEDED -> {
                        // Add messages to your data source.
                    }
                    SendingStatus.PENDING -> {
                        // Add pending messages to your data source.
                    }
                }
            }

            override fun onMessagesUpdated(context: MessageContext, channel: GroupChannel, messages: List<BaseMessage>) {
                when (context.messagesSendingStatus) {
                    SendingStatus.SUCCEEDED -> {
                        // Update the message status of sent messages.
                    }
                    SendingStatus.PENDING -> { // failed -> pending
                        // Update the message status from failed to pending in your data source.
                        // Remove the failed message from the data source.
                    }
                    SendingStatus.FAILED -> { // pending -> failed
                        // Update the message status from pending to failed in your data source.
                        // Remove the pending message from the data source.
                    }
                    SendingStatus.CANCELED -> { // pending -> canceled
                        // Remove the pending message from the data source.
                        // Implement code to process canceled messages on your end. The Chat SDK doesn't store canceled messages.
                    }
                }
            }

            override fun onMessagesDeleted(context: MessageContext, channel: GroupChannel, messages: List<BaseMessage>) {
                when (context.messagesSendingStatus) {
                    SendingStatus.SUCCEEDED -> {
                        // Remove the sent message from your data source.
                    }
                    SendingStatus.FAILED -> {
                        // Remove the failed message from your data source.
                    }
                }
            }

            override fun onChannelUpdated(context: GroupChannelContext, channel: GroupChannel) {
                // Change the chat view with the updated channel information.
            }

            override fun onChannelDeleted(context: GroupChannelContext, channelUrl: String) {
                // This is called when a channel is deleted. So the current chat view should be cleared.
            }

            override fun onHugeGapDetected() {
                // The Chat SDK detects that more than 300 messages are missing.

                // Clear the collection.
                collection.dispose()

                // Create a new message collection object.
                createMessageCollection()

                // An additional implementation is required for initialization.
            }
        }
    }
)
```

---

### Loaded messages

The `MessageCollection` holds the list of messages that are loaded by `initialize()`, `loadMore()`, `loadPrevious()`, or by message events in the `succeededMessages` property. It is recommended to use this list as the data source of your `RecyclerView` as it represents the most up-to-date list.

Also, it has `pendingMessages` and `failedMessages` which are lists of messages that are being sent and messages that have failed to be sent, respectively.

```kotlin
// Loaded messages
val messages = collection.succeededMessages

// Pending messages which are being sent.
// When the message sending is done, either succeed or fail, those messages will be moved to succeededMessages or failedMessages.
val pendingMessages = collection.pendingMessages

// Messages that have been failed to be sent.
val failedMessages = collection.failedMessages
```

---

### Gap

In local caching, a gap refers to a chunk of objects that are created and logged on the Vyin Chat server but missing from the local cache. Depending on the connection status, the client app could miss message events and new messages stored in the Vyin Chat server may not reach the Chat SDK in the client app. In such cases, a gap is created.

Small gaps can be filled in through changelog sync. However, if the client app has been disconnected for a while, too large of a gap can be created.

#### Huge gap

A gap can be created for many reasons, such as when a user has been offline for days. When the client app comes back online, the Chat SDK checks with the Vyin Chat server to see if there are any new messages. If it detects that more than 300 messages are missing in the local cache compared to the Vyin Chat server, the SDK determines that there is a huge gap and `onHugeGapDetected()` is called.

> **Note:** To adjust the number of missing messages that result in a call for `onHugeGapDetected()`, contact our support team.

In this case, the existing `MessageCollection` instance should be cleared through the `dispose()` method and a new one must be created for better performance.

```kotlin
collection.dispose()
```

---

### Dispose of the collection

The `dispose()` method should be called when you destroy the chat screen to notify the SDK that this collection is no longer visible.

For example, this should be used when the current user exits from the chat screen by pressing a back key or when a new collection has to be created due to a huge gap detected by `onHugeGapDetected()`.

```kotlin
collection.dispose()
```
