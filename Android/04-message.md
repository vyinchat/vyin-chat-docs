# Vyin Chat SDK for Android - Message

## Overview

Vyin Chat SDK for Android provides expansive messaging features to enhance users' chat experience. Basic and foundational messaging features such as sending, copying, updating, and deleting a message make up the essential chat experience. You can also load as well as receive messages through channel event handlers. Features related to marking messages as read, counting the number of unread messages, adding extra data to a message, reporting a message, and local caching are available to support a wide range of user needs.

---

## Message types

There are three types of messages in Vyin Chat.

- Text: A message containing a string of text. The message must be sent by a user and, in the case of group channels, the user must be a member of the channel to be able to send the message.
- File: A message containing a file, which can be directly uploaded to the Vyin Chat server or specified by its URL. The message must be sent by a user and, in the case of group channels, the user must be a member of the channel to be able to send the message.
- Multiple files: A message containing more than two files, which can be directly uploaded to the Vyin Chat server or specified by their URL. These messages are supported only in group channels and, just like a regular file message, the sender must be a member of the channel. The maximum number of file attachments is 30 and the default maximum size per file is set to 25MB. To adjust the numbers, contact our Sales team.
- Admin: A system message sent without a sender and, in the case of group channels, without the sender needing to be a member of the channel. Admin message can be sent using the Chat API or Vyin Chat Console. Common use cases include general notices to a channel like "Livestream will begin in 1 minute! Your messages may be monitored to ensure a safe, welcoming environment." and auto event messages like "User 1 joined the channel."

#### Comparing message types

Characteristics and supported features of each message type are shown below.

|  | Text | File | Admin |
| :---- | :---- | :---- | :---- |
| Class | `UserMessage` | `FileMessage`, `MultipleFilesMessage` | `AdminMessage` |
| Content | Text only | File only | Text only |
| Send by user | Required | Required | Not required |
| Join group channel | Required | Required | Not required |
| Delivery receipts | Supported | Supported | N/A |
| Message search | Supported | Supported | N/A |
| Message threading | Supported | Supported | N/A |
| Reactions | Supported | Supported | Supported |
| Read receipts | Supported | Supported | N/A |
| Translations | Supported | N/A | N/A |

---

## Functionalities by topic

Users can interact with other users in a channel by sending, receiving, replying to messages, and more. The following is a list of functionalities that our Chat SDK provides.

#### Sending a message

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Send a message | Sends a text or file message to a channel. |  |  |
| Send a critical alert message to iOS device users | Sends a critical alert notification to iOS devices even when Mute or Do Not Disturb feature is on. |  |  |
| Send an admin message | Sends a message from an admin to a channel on the Vyin Chat Console or using the Chat Platform API. |  |  |
| Track file upload progress using a handler | Tracks the progress of a file upload. |  |  |
| Share an encrypted file | Encrypts files and thumbnail images for secure access to users in a channel. |  |  |
| Spam flood protection | Customizes the number of messages a user can send to a channel per second. |  |  |
| Smart throttling | Customizes the number of messages displayed in a channel per second. |  |  |

#### Receiving messages through event handler

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Receive messages in a channel | Receives messages sent by other users in a channel through the channel event handler. |  |  |

#### Using message threading

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Create a message thread | Sends a reply to a specific message in a channel. This can create a message thread. |  |  |
| List replies in a message thread | Retrieves replies to a parent message. |  |  |

#### Retrieving messages

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Retrieve a message | Retrieves a specific message in a channel. |  |  |
| Retrieve a list of messages | Retrieves messages in a channel. You can load a set number of previous messages or messages based on its timestamp or message ID. |  |  |
| Retrieve the last message of a channel | Retrieves the last message in a channel. |  |  |

#### Searching messages

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Search messages by a keyword | Retrieves a list of messages that contain a search query or a specified keyword. |  |  |

#### Managing a message

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Copy a message | Copies a message in a channel and sends it to the same channel or another channel. |  |  |
| Update a message | Updates a text or file message in a channel. Users can only update their own messages. Operators can update any messages in the channel. |  |  |
| Delete a message | Deletes a text or file message in a channel. Users can only delete their own messages. Operators can delete any messages in the channel. |  |  |
| React to a message | Adds an emoji to a message. |  |  |
| Clear the chat history in a channel | Clears the chat history from the channel view of the current user. Messages aren't deleted from the Vyin Chat system's database and other users in the channel can still view all the messages in their own channel view. |  |  |
| Send typing indicators to other users | Indicates to other users in the channel that a user is typing up a message to the channel. |  |  |
| Display Open Graph tags in a message | Displays a URL link preview when a message contains a web page URL. |  |  |
| Generate thumbnails of a file message | Creates a thumbnail of an image file. |  |  |

#### Listing changelogs

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| List changelogs of messages | Retrieves message changelogs by timestamp or token. |  |  |

#### Marking messages as read

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Mark messages as read | Marks messages in a channel as read when a user enters the channel or brings the opened channel view to the foreground. |  |  |

#### Marking messages as delivered

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Mark messages as delivered | Marks messages as delivered when a user who is online receives messages from the Vyin Chat server. |  |  |

#### Mentioning other users in a message

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Mention other users in a message | Includes other users in the channel in a message. |  |  |

#### Retrieving unread count

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Retrieve number of unread messages in a channel | Retrieves the number of messages in a channel a user hasn't read. |  |  |
| Retrieve number of unread messages in all channels | Retrieves the number of messages a user hasn't read in all channels they are in. |  |  |
| Retrieve number of channels with unread messages | Retrieves the number of channels where a user hasn't read one or more messages. |  |  |
| Retrieve number of members who haven't received a message | Retrieves the number of users in a channel who have yet to receive a specific message. |  |  |
| Retrieve number of members who haven't read a message | Retrieves the number of users in a channel who haven't read a specific message. |  |  |

#### Categorizing messages

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Categorize messages by custom type | Specifies a custom message type to search or filter messages by. |  |  |

#### Translating messages

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Auto-translate messages | Translates messages into specified languages and sends to channel. |  |  |
| Translation engine | Vyin Chat's message auto-translation and on-demand features are powered by Google Cloud Translation API's recognition engine. The recognition engine supports a wide variety of languages for the Neural Machine Translation (NMT) model. |  |  |

#### AI Summary messages

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Config setting for summary message | Implement a configuration setting that controls the "summary message" feature. This setting retrieves the current configuration value to determine whether the summary message feature should be enabled or disabled dynamically, allowing toggling the feature. |  |  |
| Get summary messages | Retrieves the summary messages in a channel that have not been read by the user. |  |  |

---

## Sending a message

### Send a message

For both open and group channels, users can send messages of the following types to the channel they are in.

| Message type | Class | Description |
| :---- | :---- | :---- |
| Text | UserMessage | A text message sent by a user. |
| File | FileMessage | A binary file message sent by a user. |

In addition to these message types, you can further subclassify a message by specifying its custom type. This custom type takes on the form of `String` and can be used to search or filter messages. It allows you to append information to your message and customize message categorization.

The following code shows several types of parameters that you can configure to customize text messages using `UserMessageCreateParams`. Under the `UserMessageCreateParams` object, you can assign specific values to `message`, `data`, and other properties. By assigning arbitrary string to the `data` property, you can set custom font size, font type, or `JSON` object. To send your messages, you need to pass the `UserMessageCreateParams` object as an argument to the parameter in the `sendUserMessage()` method.

Through the callback handler of the `sendUserMessage()` method, the Vyin Chat server always notifies whether your message has been successfully sent to the channel. When there is a delivery failure due to network issues, an exception is returned through the callback method.

```kotlin
val userIdsToMention = listOf("Jeff", "Julia")
val targetLanguages = listOf("fr", "de") // French, German
val params = UserMessageCreateParams(TEXT_MESSAGE).apply {
    customType = CUSTOM_TYPE
    data = DATA
    mentionType = MentionType.USERS
    mentionedUserIds = userIdsToMention
    metaArrays = listOf(
        MessageMetaArray("itemType", listOf("tablet")),
        MessageMetaArray("quality", listOf("best", "good"))
    )
    translationTargetLanguages = targetLanguages
    pushNotificationDeliveryOption = PushNotificationDeliveryOption.DEFAULT
}
channel.sendUserMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // A text message with detailed configuration is successfully sent to the channel.
    // By using message.messageId, message.message, message.customType, and so on,
    // you can access the result object from the Vyin Chat server to check your UserMessageCreateParams configuration.
    // The current user can receive messages from other users through the onMessageReceived() method of an event handler.
}
```

A user can also send a binary file through the Chat SDK as the file itself or through sending a URL.

Sending a raw file means you're uploading it to the Vyin Chat server where it can be downloaded on client apps. When you upload a file directly to the server, there is a size limit imposed on the file depending on your plan. You can see the limit on your console and contact our sales team to change the limit.

The other option is to send a file hosted on your server. You can pass the file's URL, which represents its location, as an argument to a parameter. In this case, your file isn't hosted on the Vyin Chat server and it can only be downloaded from your own server. When you send a file message with a URL, there is no limit on the file size since it isn't directly uploaded to the Vyin Chat server.

Note: You can use `sendFileMessages()`, which is another method that allows you to send up to 20 file messages per one method call. Refer to our API Reference to learn more.

The following code shows several types of parameters that you can configure to customize file messages by using `FileMessageCreateParams`. Under the `FileMessageCreateParams` object, you can assign specific values to `customType` and other properties. To send your messages, you need to pass the `FileMessageCreateParams` object as an argument to the parameter in the `sendFileMessage()` method.

Through the callback handler of the `sendFileMessage()` method, the Vyin Chat server always notifies whether your message has been successfully sent to the channel. When there is a delivery failure due to network issues, an exception is returned through the callback method.

```kotlin
// Send a file message with a raw file.
// Create and add a ThumbnailSize object.
// Up to three thumbnail images are allowed.
val thumbnailSizeList = listOf(
    ThumbnailSize(100, 100),
    ThumbnailSize(200, 200)
)

val userIdsToMention = listOf("Jeff", "Julia")
val params = FileMessageCreateParams().apply {
    file = FILE // Or fileUrl = FILE_URL
    fileName = FILE_NAME
    fileSize = FILE_SIZE
    mimeType = MIME_TYPE
    thumbnailSizes = thumbnailSizeList
    customType = CUSTOM_TYPE
    mentionType = MentionType.USERS      // Either USERS or CHANNEL
    mentionedUserIds = userIdsToMention  // Or mentionedUsers = LIST_OF_USERS_TO_MENTION
    pushNotificationDeliveryOption = PushNotificationDeliveryOption.DEFAULT  // Either DEFAULT or SUPPRESS
}
channel.sendFileMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // A file message with detailed configuration is successfully sent to the channel.
    // By using message.messageId, message.fileName, message.customType, and so on,
    // you can access the result object from the Vyin Chat server to check your FileMessageCreateParams configuration.
    // The current user can receive messages from other users through the onMessageReceived() method of an event handler.
}
```

Note: Vyin Chat does not currently implement sending a message with multiple files.

---

### Send a critical alert message to iOS device users

Your app is most likely to have users from different devices that run on iOS, Android, or web, which calls for the need to implement platform-specific code when it comes to sending a notification such as a critical alert. A critical alert is a notification that can be sent to iOS device users even when Mute or Do Not Disturb is turned on. Use a `UserMessageCreateParams` instance to properly notify critical alert messages to iOS device users in your app.

First, create an `AppleCriticalAlertOptions` instance and set it as an attribute of a `UserMessageCreateParams` instance. Then, pass the `UserMessageCreateParams` instance as an argument to a parameter in the `sendUserMessage()` method. The same applies to `FileMessageCreateParams` and `sendFileMessage()`.

Note: To learn more about how to set critical alerts, visit Apple critical alerts.

#### User message

```kotlin
// Send a critical alert user message.
val params = UserMessageCreateParams().apply {
    appleCriticalAlertOptions = AppleCriticalAlertOptions(
        "name",
        0.7
    ) // Acceptable values for volume are 0 to 1.0, inclusive.
}
channel.sendUserMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }
}
```

#### File message

```kotlin
// Send a critical alert file message.
val params = FileMessageCreateParams().apply {
    appleCriticalAlertOptions = AppleCriticalAlertOptions(
        "name",
        0.7
    ) // Acceptable values for volume are 0 to 1.0, inclusive.
}
channel.sendFileMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }
}
```

---

### Send an admin message

You can send an admin message to a channel on Vyin Chat Console or using Chat Platform API.

To send an admin message to an open channel through your console, go to Chat > Open channels, select an open channel from the list, find the message box at the bottom of the page, click the Admin message tab, and then type your message in the box. The maximum length of an admin message is 1,000 characters.

To send an admin message to a group channel through your console, go to Chat > Group channels, select a group channel from the list, find the message box at the bottom of the page, click the Admin message tab, and then type your message in the box. The maximum length of an admin message is 1,000 characters. You can send a push notification for the message by clicking the box next to Send push notification.

Alternatively, for both open and group channels, you can send an admin message from outside the chat view by navigating to the channel list under Chat > Open channels or Chat > Group channels and clicking the box by the channel you are going to send the admin message to. After clicking Admin message, you can enter your message to the text box.

---

### Track file upload progress using a handler

If needed, you can track the progress of file upload by passing `FileMessageWithProgressHandler` as an argument to a parameter when calling the `sendFileMessage()` method.

```kotlin
val params = FileMessageCreateParams(FILE).apply {
    fileName = FILE_NAME
    data = DATA
    customType = CUSTOM_TYPE
}

val fileMessage = channel.sendFileMessage(
    params,
    object : FileMessageWithProgressHandler {
        override fun onProgress(bytesSent: Int, totalBytesSent: Int, totalBytesToSend: Int) {
            val percent = (totalBytesSent * 100) / totalBytesToSend
        }

        override fun onResult(message: FileMessage?, e: GIMException?) {
            if (e != null) {
                // Handle error.
            }

            // You can handle actions relevant to the sent file message.
            // ...
        }
    }
)
```

---

### Cancel an in-progress file upload

Note: Vyin Chat does not currently implement cancelling an in-progress file upload.

---

### Share an encrypted file

This file encryption feature prevents users without access from opening and reading encrypted files that have been shared within a group of users. When this feature is turned on, all types of sent files and thumbnail images are first uploaded to the Vyin Chat server, and then encrypted by `AES256`.

In a channel, encrypted files and thumbnail images are decrypted and accessed securely only by the users in the channel. Anyone outside of the channel and application won't have access to these files and thumbnail images. The following explains how this data security works and what to do at the SDK level to apply it to your client apps.

The Vyin Chat system enables secure encryption and decryption of files by generating and distributing an opaque and unique encryption key for each user. An encryption key is managed internally by the system, and is valid for three days. It is generated every time the user logs in to the Vyin Chat server through the Chat SDK, which then gets delivered to the Chat SDK from the server.

When the Chat SDK requests an encrypted file by its URL, the `auth` parameter should be added to the URL to access the file, which is specified with an encryption key of the user such as `?auth=RW5jb2RlIHaXMgdGV4eA==`. With the specified key in the `auth` parameter, the Vyin Chat server first decrypts the file, then checks if the user belongs to the channel, and finally, allows the user to access and open the file in the channel.

This can be easily done using the `fileMessage.url` property, which automatically adds the `auth` parameter with an encryption key of the current user to the file URL, and returns the unique URL for the user.

---

### Spam flood protection

This feature allows you to customize the number of messages a user can send in an open or group channel per second. By doing so, all excess messages from a user are deleted and only the number of messages allowed to be sent per user per second are delivered. This feature protects your app from some users spamming other users in the channel with the same messages.

Note: Vyin Chat's default system setting is five messages per second. This limit can be manually adjusted only from our side. Contact our sales team for more information.

---

### Smart throttling

In open channels, group channels, and Supergroup channels, you can use smart throttling to customize the number of messages displayed in a channel per second. By doing so, you can adjust the pace of the conversation in a chat so that users can have sufficient time to read messages.

Note: Vyin Chat's default system setting is five messages per second. This limit can only be adjusted by contacting our sales team.

---

## Receiving messages through event handler

### Receive messages in an open channel

Users can receive messages from other participants through the `onMessageReceived()` method in the channel event handler. A `BaseMessage` object for each received message is one of the following message types.

| Message type | Class | Description |
| :---- | :---- | :---- |
| Text | UserMessage | A text message sent by a user. |
| File | FileMessage | A binary file message sent by a user. |
| Admin | AdminMessage | A text message sent by an admin through the Chat API |

To register multiple concurrent handlers, pass a `UNIQUE_HANDLER_ID` argument as a unique identifier into the `GIMChat.addChannelHandler()` method.

```kotlin
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : OpenChannelHandler() {
        override fun onMessageReceived(channel: BaseChannel, message: BaseMessage) {
            // You can customize how to display the different types of messages with the result object in the message parameter.
            when (message) {
                is UserMessage -> {}
                is FileMessage -> {}
                is AdminMessage -> {}
            }
        }
    }
)
```

When the UI isn't valid anymore, remove the channel event handler.

```kotlin
GIMChat.removeChannelHandler(UNIQUE_HANDLER_ID)
```

---

#### Event handler for message threading

Once a reply is created or deleted from a thread, the `onThreadInfoUpdated()` event handler is invoked. The method returns a `ThreadInfoUpdateEvent` object that has the latest information about the thread. This object needs to be applied to the parent message object.

Note: Like other messages, when a reply is created in a channel, the `onMessageReceived()` method of the channel event handler in client apps is called.

```kotlin
override fun onThreadInfoUpdated(channel: BaseChannel, threadInfoUpdateEvent: ThreadInfoUpdateEvent) {
    // Look for a message that has threadInfoUpdateEvent.targetMessageId
    // Apply the event to the message.
    parentMessage.applyThreadInfoUpdateEvent(threadInfoUpdateEvent)
}
```

#### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| `channel` | BaseChannel | Specifies the channel that has the message thread. |
| `threadInfoUpdateEvent` | ThreadInfoUpdateEvent | Specifies a `ThreadInfoUpdateEvent` object that has the latest information about the thread. |

---

### Receive messages in a group channel

Messages sent by other group channel members can be received through the `onMessagesAdded()` method in message collection handler.

#### Message type

Each `BaseMessage` object received in a group channel falls into one of the following three message types.

| Message type | Class | Description |
| :---- | :---- | :---- |
| Text | UserMessage | A text message sent by a user. |
| File | FileMessage | A binary file message sent by a user. |
| Admin | AdminMessage | A text message sent by an admin through the Chat API |

#### Event handler

When a new message arrives in a group channel, `onMessageAdded()` and `onChannelUpdated()` in the message collection are called.

Whenever an event related to message or group channel occurs, the collection is informed of the event through `MessageContext` and `GroupChannelContext`, respectively. The `MessageContext` and `GroupChannelContext` instances have `CollectionEventSource`, which contains the information of the event that took place. For example, when a new message is received, `EVENT_MESSAGE_RECEIVED` is passed as the `MessageContext.CollectionEventSource` value to `onMessagesAdded()`. Meanwhile, when a message is delivered to group channel members who are online, the message's delivery status is automatically marked as delivered. The value for `GroupChannelContext.CollectionEventSource` is set to `EVENT_DELIVERY_STATUS_UPDATED` and channel members are notified of the event through `onChannelUpdated()`. To learn more about this topic, see the Message events section in the Message collection page.

You can also register other message-related event handlers using the `MessageCollection` object.

```kotlin
messageCollection.messageCollectionHandler = object : MessageCollectionHandler {
    override fun onMessagesAdded(
        context: MessageContext,
        channel: GroupChannel,
        messages: List<BaseMessage>
    ) {
    }

    override fun onMessagesUpdated(
        context: MessageContext,
        channel: GroupChannel,
        messages: List<BaseMessage>
    ) {
    }

    override fun onMessagesDeleted(
        context: MessageContext,
        channel: GroupChannel,
        messages: List<BaseMessage>
    ) {
    }

    override fun onChannelUpdated(context: GroupChannelContext, channel: GroupChannel) {
    }

    override fun onChannelDeleted(context: GroupChannelContext, channelUrl: String) {
    }

    override fun onHugeGapDetected() {
    }
}
```

When the message UI is no longer needed, call `dispose()`. Note that the handlers don't work when the message collection is disposed.

```kotlin
// Dispose the collection
messageCollection.dispose()
// Set the handler to null
messageCollection.messageCollectionHandler = null
```

---

## Using message threading

### Create a message thread

When a user replies to a message in a channel, it creates a message thread, which refers to a collection of messages consisting of a parent message and its replies. Message threading lets users ask questions, give feedback, or add context to a specific message without disrupting the flow of conversation. It can have the following elements.

- A message can have a thread of replies.
- A message that has a thread of replies is a parent message.
- A parent message and its threaded replies are collectively called a message thread.
- Every message within a thread, whether it's parent or reply, is a threaded message.
- A message that doesn't have any replies is an unthreaded message.

Message threading has the following limitations.

- Only 1-depth threads are supported, meaning you can only add reply messages to non-reply messages. You can't add a reply to a reply message.
- Message threading is limited to text and file messages. You can't send admin messages as replies or add replies to admin messages.

You can reply to a message in a channel through the `sendUserMessage()` or `sendFileMessage()` method. To do so, you should create a `UserMessageCreateParams` or a `FileMessageCreateParams` object and then specify the `parentMessageId` property of the object. Sending reply messages works the same way as sending regular messages to a channel except that replies have an additional `parentMessageId` property.

---

#### Reply with a text message

When replying to a message through the `sendUserMessage()` method, you should specify and pass a `UserMessageCreateParams` object to the method as a parameter. The `UserMessageCreateParams` class is derived from the `BaseMessageCreateParams` class and can access all the methods and properties of `BaseMessageCreateParams`.

```kotlin
// Create a UserMessageCreateParams object.
val params = UserMessageCreateParams().apply {
    parentMessageId = PARENT_MESSAGE_ID
    message = MESSAGE_TEXT
    replyToChannel = false
}

// Pass the params to the parameter of the sendUserMessage() method.
channel.sendUserMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // A reply to a specific message is successfully sent as a text message.
}
```

#### UserMessageCreateParams

To see the comprehensive list of all available methods and properties, see `UserMessageCreateParams`.

| Property name | Type | Description |
| :---- | :---- | :---- |
| `parentMessageId` | Long | Specifies the unique ID of a parent message which has a thread of replies. If the message sent through the `sendUserMessage()` method is a parent message, the value of this property is set to `0`. If the message is a reply to a parent message, the value is the message ID of the parent message. |
| `message` | String | Specifies the message to send. |
| `replyToChannel` | Boolean | Determines whether to send the message to the channel as well. To use this property, the value of `parentMessageId` must be specified. (Default: `false`) |

---

#### Reply with a file message

When replying with a file message through the `sendFileMessage()` method, you should specify and pass a `FileMessageCreateParams` object to the method as a parameter. The `FileMessageCreateParams` class is derived from the `BaseMessageCreateParams` class and can access all the methods and properties of `BaseMessageCreateParams`.

```kotlin
// Create a FileMessageCreateParams object.
val params = FileMessageCreateParams().apply {
    parentMessageId = PARENT_MESSAGE_ID
    file = FILE
    fileName = FILE_NAME
    mimeType = MIME_TYPE
    fileSize = FILE_SIZE
}

// Pass the params to the parameter in the sendFileMessage() method.
channel.sendFileMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // A reply to a specific message is successfully sent as a file message.
}
```

#### FileMessageCreateParams

To see the comprehensive list of all available methods and properties, see `FileMessageCreateParams`.

| Property name | Type | Description |
| :---- | :---- | :---- |
| `parentMessageId` | Long | Specifies the unique ID of a parent message which has a thread of replies. If the message sent through the `sendFileMessage()` method is a parent message, the value of this property is set to `0`. If the message is a reply to a parent message, the value is the message ID of the parent message. |
| `file` | File | Specifies the binary file data. When the value of `file` is specified, the value of `fileUrl` can't be specified. (Default: `null`) |
| `fileName` | String | Specifies the file name. (Default: `null`) |
| `mimeType` | String | Specifies the file MIME type. (Default: `null`) |
| `fileSize` | Int | Specifies the file size. (Default: `null`) |

---

#### Event handler for message threading

When a reply is created in a channel, the `onMessageReceived()` and `onThreadInfoUpdated()` methods of the channel event handler in client apps are called. When a reply is deleted from a thread, the `onThreadInfoUpdated()` event handler is invoked. In both cases, `onThreadInfoUpdated()` takes a `ThreadInfoUpdateEvent` object as an argument that has the latest information about the thread. Apply the object to the parent message object through the `parentMessage.applyThreadInfoUpdateEvent()` method.

```kotlin
override fun onThreadInfoUpdated(channel: BaseChannel, threadInfoUpdateEvent: ThreadInfoUpdateEvent) {
    // Look for a message that has threadInfoUpdateEvent.targetMessageId.
    // Apply the event to the message.
    parentMessage.applyThreadInfoUpdateEvent(threadInfoUpdateEvent)
}
```

#### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| `channel` | BaseChannel | Specifies the channel that has the message thread. |
| `threadInfoUpdateEvent` | ThreadInfoUpdateEvent | Specifies a `ThreadInfoUpdateEvent` object that has the latest information about the thread. |

---

### List replies in a message thread

You can retrieve replies to a message by identifying the parent message like the following. First, create a `ThreadMessageListParams` object and set properties related to the thread where the target replies belong to.

```kotlin
// Create a ThreadMessageListParams object.
val params = ThreadMessageListParams().apply {
    previousResultSize = PREV_RESULT_SIZE
    nextResultSize = NEXT_RESULT_SIZE
    inclusive = INCLUSIVE
    reverse = REVERSE
    messagePayloadFilter = MessagePayloadFilter().apply {
        includeThreadInfo = INCLUDE_THREAD_INFO
        includeParentMessageInfo = INCLUDE_PARENT_MESSAGE_INFO
    }
}
```

#### ThreadMessageListParams

| Property name | Type | Description |
| :---- | :---- | :---- |
| `previousResultSize` | int | Specifies the number of messages to retrieve that were sent before the specified timestamp. |
| `nextResultSize` | int | Specifies the number of messages to retrieve that were sent after the specified timestamp. |
| `inclusive` | boolean | Determines whether to include the messages with the matching timestamp in the result. |
| `reverse` | boolean | Determines whether to sort the retrieved messages in reverse order. If set to `false`, the result is displayed in ascending order. |
| `includeThreadInfo` | boolean | Determines whether to include information about the message thread in the result. |
| `includeParentMessageInfo` | boolean | Determines whether to include information about the parent message in the result. (Default: `false`) |

Using the timestamp of the parent message, you can retrieve the parent message with its replies by passing a `ThreadMessageListParams` object as an argument to the parameter in the `getThreadedMessagesByTimestamp()` method.

```kotlin
parentMessage.getThreadedMessagesByTimestamp(ts, params) { parentMessage, messages, e ->
    if (e != null) {
        // Handle error.
    }

    // A list of replies of the specified parent message timestamp is successfully retrieved.
    // ...
}
```

#### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| `ts` | long | Specifies the timestamp to be the reference point of message retrieval in Unix milliseconds format. |

---

## Retrieving messages

### Retrieve a list of messages

Open channels and group channels use different methods to retrieve messages. For open channels, use `MessageListParams` or `PreviousMessageListQuery` to retrieve messages from a certain reference point. Meanwhile, `loadPrevious()` and `loadNext()` in `Message collection` retrieve messages sent and received in a group channel.

---

#### Messages in open channels

Using the `getMessagesByTimestamp()` method or the `getMessagesByMessageId()` method, you can retrieve messages before or after a reference point, which can be a specific timestamp in Unix milliseconds or a message ID in an open channel.

Those methods have two parameters, which are a reference point of message retrieval and a `params` parameter for `MessageListParams`. Under the `MessageListParams` object, you can specify values to properties such as `previousResultSize`, `messageTypeFilter`, and `customType`. The following code shows several types of parameters that you can configure to customize a message query.

```kotlin
val params = MessageListParams().apply {
    inclusive = true
    previousResultSize = 5
    nextResultSize = 5
    reverse = false
    messageTypeFilter = ALL
    customType = "promotion"
    // ...
}
// Use the following code to get messages based on timestamp.
// TIMESTAMP should be in Unix milliseconds.
channel.getMessagesByTimestamp(TIMESTAMP, params) { messages, e ->
    if (e != null) {
        // Handle error.
    }
    // A list of previous and next messages of a specified timestamp is successfully retrieved.
    // Through the messages parameter of the callback handler,
    // you can access and display the data of each message from the result list
    // that the Vyin Chat server has passed to the callback method.
}
// Use the following code to get messages based on message ID.
channel.getMessagesByMessageId(MESSAGE_ID, params) { messages, e ->
    if (e != null) {
        // Handle error.
    }
    // A list of previous and next messages of a specified message ID is successfully retrieved.
    // Through the messages parameter of the callback handler,
    // you can access and display the data of each message from the result list
    // that the Vyin Chat server has passed to the callback method.
}
```

#### MessageListParams

This table only contains properties shown in the code above. To see the comprehensive list of all available methods and properties, see MessageListParams in API reference.

| Property name | Type | Description |
| :---- | :---- | :---- |
| `inclusive` | boolean | Determines whether to include messages sent exactly on the specified timestamp or have the matching message ID. |
| `previousResultSize` | int | Specifies the number of messages to retrieve, which are sent previously before a specified timestamp. Note that the actual number of results may be larger than the set value when there are multiple messages with the same timestamp as the earliest message. |
| `nextResultSize` | int | Specifies the number of messages to retrieve, which are sent later after a specified timestamp. Note that the actual number of results may be larger than the set value when there are multiple messages with the same timestamp as the latest message. |
| `reverse` | boolean | Determines whether to sort the retrieved messages in reverse order. If set to `true`, messages are returned from the most recent to the oldest. (Default: `false`) |
| `messageTypeFilter` | enum | Specifies the message type to filter the messages with the corresponding type. Acceptable values are `MessageTypeFilter.ALL`, `MessageTypeFilter.USER`, `MessageTypeFilter.FILE`, and `MessageTypeFilter.ADMIN`. |
| `customType` | string | Specifies the custom message type to filter the messages with the corresponding custom type. |

---

#### Messages in group channels

A chat view of messages in a group channel should be drawn with a `MessageCollection` instance. To retrieve messages within the collection, check if there are more messages to load through `hasPrevious` or `hasNext` first, then call `loadPrevious` or `loadNext`, respectively. Then the previous or next page of messages is retrieved. To learn more about message collection, see the Message collection page under Local caching.

```kotlin
// Get the next page of messages.
if (collection.hasNext) {
    collection.loadNext { messages, e ->
        if (e != null) {
            // Handle error.
            return@loadNext
        }
        // Add messages to your data source.
    }
}
// Get the previous page of messages.
if (collection.hasPrevious) {
    collection.loadPrevious { messages, e ->
        if (e != null) {
            // Handle error.
            return@loadPrevious
        }
        // Add messages to your data source.
    }
}
```

Note: Vyin Chat does not currently implement getting previous messages using pagination.

---

### Retrieve a message

You can retrieve a specific message in an open or group channel by creating and passing the `MessageRetrievalParams` object as an argument into the `getMessage()` method.

```kotlin
// Create a MessageRetrievalParams object.
val params = MessageRetrievalParams(channelUrl = CHANNEL_URL, channelType = CHANNEL_TYPE, messageId = MESSAGE_ID)
// ...

// Pass the params as an argument to the parameter of the getMessage() method.
BaseMessage.getMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // The specified message is successfully retrieved.
    // ...
}
```

#### List of properties

| Property name | Type | Description |
| :---- | :---- | :---- |
| `channelUrl` | String | Specifies the URL of the channel. |
| `channelType` | ChannelType | Specifies the type of the channel. |
| `messageId` | Long | Specifies the unique ID of the message. |

---

#### Retrieve the last message of a group channel

You can retrieve and view the last message of a group channel.

```kotlin
val lastMessage = groupChannel.lastMessage
```

---

## Searching messages in a group channel

### Search messages by a keyword

Message search allows you to retrieve a list of messages that contain a search query or a specified keyword in group channels by implementing `MessageSearchQuery`. The query retrieves a list of messages that contain a search term and meet the optional parameter values set in the `MessageSearchQueryParams` class.

Note: Punctuations and special characters are ignored while indexing, so unless they're being used for advanced search functionalities, they should be removed or replaced in the search term for best results.

You can create the query instance in two ways. First, you can do so with the default values.

```kotlin
val query = MessageSearchQuery.Builder().apply {
    keyword = KEYWORD
}.build()
```

Or, you can create an instance by changing those values to your preference.

```kotlin
val query = MessageSearchQuery.Builder().apply {
    keyword = "hello"
    limit = 10
    // ...
}.build()
```

Then, the query retrieves a list of match results. Calling the `next()` method returns the next page of the results.

```kotlin
query.next { messages, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

Use the `hasNext` method to see if there is a next page.

```kotlin
query.hasNext
```

Use the `isLoading` method to see if the search results are loading.

```kotlin
query.isLoading
```

---

#### Advanced search

Advanced search allows the SDK to create and support complicated search conditions, improving search results. Search functionalities such as wildcard, fuzzy search, logical operators, and synonym search are supported for the `keyword` parameter. You can use these functionalities by setting `advancedQuery` to `true`. See the advanced search section in our Platform API Docs for more information.

- Wildcard: Include `?` or `*` in search terms. `?` matches a single character while `*` can match zero or more characters.
- Fuzzy search: Add `~` at the end of search terms. Fuzzy search shows similar terms to the search term determined by a Levenshtein edit distance. If your search term is less than two characters, only exact matches are returned. If the search term has between three and five characters, only one character is edited. If the search term is longer than five characters, up to two characters are edited.
- Logical operators: Use `AND` and `OR` to search for more than one term. The logical operators must be uppercase. You can also use parentheses to group multiple search terms or specify target fields. If you want to look for search terms in not only the content of the message but also specified target fields of the message, such as custom type or data, you can specify the field and search term as a key-value item.

#### MessageSearchQueryParams

You can build the query class using the following parameters which allow you to add various search options.

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| keyword | string | Specifies the search term. Special characters and punctuations aren't indexed, so including them in the keyword may return unexpected search results. |
| channelUrl | string | Specifies the URL of the target channel. |
| channelCustomType | string | Specifies the custom channel type. |
| limit | int | Specifies the number of messages to return per page. Acceptable values are `1` to `99`, inclusive. (Default: `20`) |
| exactMatch | boolean | Determines whether to search for messages that exactly match the search term. If set to `false`, it returns partially matched messages that contain the search term. (Default: `false`) |
| messageTimestampFrom | long | Restricts the search scope to the messages sent after the specified value in Unix milliseconds format. This includes the messages sent exactly on the timestamp. (Default: `0`) |
| messageTimestampTo | long | Restricts the search scope to the messages sent before the specified value in Unix milliseconds format. This includes the messages sent exactly on the timestamp. (Default: `Long.MAX_VALUE`) |
| order | enum | Determines which field the results are sorted by. Acceptable values are the following: `SCORE` (default): the search relevance score. `TIMESTAMP`: the time when a message was created. (Default: `SCORE`) |
| reverse | boolean | Determines whether to sort the results in reverse order. If set to `false`, they will be sorted in descending order. (Default: `false`) |

---

## Managing a message

### Copy a message

Note: Message copying functionality is not natively supported in the current Vyin Chat SDK.

---

### Update a message

A user can update any of their own text and file messages sent using `UserMessageUpdateParams` and `FileMessageUpdateParams`. An error is returned if a user attempts to update another user's messages. In addition, channel operators can update any messages sent in a channel.

#### User message

```kotlin
val params = UserMessageUpdateParams().apply {
    message = NEW_TEXT_MESSAGE
    customType = NEW_CUSTOM_TYPE
    data = NEW_DATA
}

// The MESSAGE_ID argument below indicates the unique message ID of a UserMessage object to update.
channel.updateUserMessage(MESSAGE_ID, params) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // The message is successfully updated.
    // Through the message parameter of the callback handler,
    // you can check if the update operation has been performed correctly.
}
```

#### Open channel

```kotlin
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : OpenChannelHandler() {
        override fun onMessageUpdated(channel: BaseChannel, message: BaseMessage) {
            // ...
        }
        // ...
    }
)
```

#### Group channel

```kotlin
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : GroupChannelHandler() {
        override fun onMessageUpdated(channel: BaseChannel, message: BaseMessage) {
            // ...
        }
    }
)
```

Note: Vyin Chat does not currently implement updating a file message.

---

### Delete a message

Users can delete any message they themselves have sent. An error is returned if a user attempts to delete messages sent by other users. Also channel operators can delete any messages in a channel.

```kotlin
// The BASE_MESSAGE argument below indicates a BaseMessage object to delete.
channel.deleteMessage(BASE_MESSAGE) { e ->
    if (e != null) {
        // Handle error.
    }

    // The message is successfully deleted from the channel.
}
```

After a message has been deleted, the `onMessageDeleted()` method in the channel event handler is invoked on all users' devices including the device where the message was deleted.

#### Open channel

```kotlin
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : OpenChannelHandler() {
        override fun onMessageDeleted(channel: BaseChannel, messageId: Long) {
            // ...
        }
        // ...
    }
)
```

#### Group channel

```kotlin
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : GroupChannelHandler() {
        override fun onMessageDeleted(channel: BaseChannel, messageId: Long) {
            // ...
        }
    }
)
```

---

### React to a message in a group channel

Message reactions help you build a more engaging chat experience that goes beyond text messages. They are a quick and easy way for users to respond to a message. Users can express their acknowledgement of or feelings about a message without written text by simply adding reactions. They can also view and delete their reactions to the message.

Note: Message reactions are currently available only in group channels.

```kotlin
val emojiKey = "smile"
// The BASE_MESSAGE argument below indicates a BaseMessage object to add a reaction to.
groupChannel.addReaction(BASE_MESSAGE, emojiKey) { reactionEvent, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}

// The BASE_MESSAGE argument below indicates a BaseMessage object to delete a reaction from.
groupChannel.deleteReaction(BASE_MESSAGE, emojiKey) { reactionEvent, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}

// To add or remove an emoji reaction made to the message on the current user's chat view,
// the applyReactionEvent() method should be called in the channel event handler's onReactionUpdated() method.
```

You can decide how to display reactions that were added to messages in the current user's chat view.

```kotlin
val params = MessageListParams().apply {
    inclusive = INCLUSIVE
    previousResultSize = PREVIOUS_RESULT_SIZE
    nextResultSize = NEXT_RESULT_SIZE
    reverse = REVERSE
    messagePayloadFilter = MessagePayloadFilter().apply {
        includeReactions = INCLUDE_REACTIONS
    }
    // ...
}

groupChannel.getMessagesByTimestamp(TIMESTAMP, params) { messages, e ->
    if (e != null) {
        // Handle error.
    }

    messages.forEach { message ->
        // ...
        message.reactions.forEach { reaction ->
            val userIds = reaction.userIds

            // Check if this emoji has been used when the current user reacted to the message.
            if (userIds.contains(GIMChat.currentUser?.userId)) {
                val key = reaction.key
                val updatedAt = reaction.updatedAt

                // Show the emoji however you want on the current user's chat view.
            }
        }
    }
}
```

Note: By using the `PreviousMessageListQuery` instance's `load()` method, messages along with their reactions can also be retrieved. To learn more, see the Retrieve a list of messages page.

When one of the channel members reacts to a message, the `onReactionUpdated()` method in the channel event handler is invoked on all channel members' devices including the one that belongs to the current user. The `applyReactionEvent()` method reflects the reaction change to the message in real time.

```kotlin
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    // ...
    object : GroupChannelHandler() {
        override fun onReactionUpdated(channel: BaseChannel, reactionEvent: ReactionEvent) {
            // ...

            // If there is a message with reactionEvent.getMessageId(),
            // you can apply the reaction change to the message by calling the applyReactionEvent() method.
            message.applyReactionEvent(reactionEvent)

            // Add or remove an emoji below the message on the current user's chat view.
        }
    }
)
```

---

### Clear the chat history in a group channel

By using the `resetMyHistory()` method, you can help the current user clear the chat history in a group channel and start a fresh conversation with other members in the same channel. As the method's name implies, the chat history will be cleared only from the channel view of the current user, and will no longer be shown in that view. But the messages aren't deleted from the database of the Vyin Chat system, and other members can still see all the messages in their channel views.

This method simply clears the messages for the user by updating the `lastMessage` and `messageOffsetTimestamp` properties of a group channel object in addition to other internally managed data such as the number of the user's unread message.

```kotlin
groupChannel.resetMyHistory { e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

---

### Send typing indicators to other members

If the `startTyping()` and `endTyping()` methods are called while the current user is typing a message in a group channel, `onTypingStatusUpdated()` in the channel event handler will be invoked on all channel members' devices except the one that belongs to the current user.

```kotlin
groupChannel.startTyping()
groupChannel.endTyping()
// ...

// To listen to an update from all the other channel members' client apps,
// implement onTypingStatusUpdated() with things to do when notified.
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : GroupChannelHandler() {
        override fun onTypingStatusUpdated(channel: GroupChannel) {
            if (currentGroupChannel.url == channel.url) {
                val members = channel.typingUsers

                // Refresh typing status of members within the channel.
                // ...
            }
        }
    }
)
```

---

### Display Open Graph tags in a message

The Chat SDK supports the URL link preview for both open channels and group channels when a message text contains the URL of a web page. This feature is turned on by default for Vyin Chat applications. If this isn't available for your Vyin Chat application, see this page and contact us on the Vyin Chat Console to enable the feature.

Note: The above image shows a chat view completed with our UIKit components. Your client app's channel view may look different depending on channel type and UI settings.

#### OGMetaData

If a `BaseMessage` object includes a valid URL of a website, the object can contain `OGMetaData`, a class that holds OG metadata information such as title, URL, description, and default image of an OG object.

Note: Some websites don't provide the OG metadata. In that case, even though the OG protocol states them as requirements, all of the four properties can be `null`.

#### List of properties

| Property name | Description |
| :---- | :---- |
| `title` | The title of the OG object as it should appear within the graph. The value can be `null`. |
| `url` | The canonical URL of the object that can be used as its permanent ID in the graph. The value can be set to `null`. |
| `description` | The description of the object. The value can be set to `null`. |
| `ogImage` | An `OGImage` instance that contains information about the image that the Open Graph points to. The `OGImage` also holds its own properties such as type, URL, and size. However, the value can be set to `null`. |

#### OGImage

The `OGImage` instance holds image-related data for an `OGMetaData` and can also have six optional structured properties of URL, secure URL, type, width, height, and alt. Except for width and height, other fields such as URL, secure URL, type, and alt can be set to `null`. If the target website doesn't provide width and height data, the value of those two fields are set to `0`.

#### List of properties

| Property name | Description |
| :---- | :---- |
| `url` | The URL of an image object within the Open Graph. The value can be set to `null`. |
| `secureURL` | An alternative URL to use if the webpage requires `HTTPS`. The value can be set to `null`. |
| `type` | A media type or MIME type of this image. The value can be set to `null`. |
| `width` | The number of pixels horizontal. When the value is unavailable, this method returns `0`. |
| `height` | The number of pixels vertical. When the value is unavailable, this method returns `0`. |
| `alt` | The description of what is in the image, not a caption of the image. The alt attribute is designed to provide a fuller context of the `OGImage` object and help users better understand it when they can't load or see the image. The value can be set to `null`. |

#### How it works

If a user sends a message with a web page URL and the linked web page possesses Open Graph (OG) tags, or OG metadata, the Vyin Chat server parses the message content, extracts the URL in the message, gets the OG metadata from it, and creates an OG metadata object for the message. Then message recipients will get the parsed message with its OG metadata through the `onMessageReceived()` method in the channel event handler of the SDK. On the other hand, the message sender will do the same through `onMessageUpdated()`.

Displaying an OG metadata object is available for two subtypes of `BaseMessage`: `UserMessage` and `AdminMessage`. If the content of either a `UserMessage` or `AdminMessage` object includes a web page URL containing OG metadata, the `BaseMessage.ogMetaData` property returns `OGMetaData` and `OGImage` objects.

If the Vyin Chat server doesn't have cache memory of the OG metadata of the given URL, `ogMetaData` can be `null` due to the time it takes to fetch the OG metadata from a remote web page. In the meantime, the message text containing the URL will be delivered first to message recipients' client app through the `onMessageUpdated()` method. When the server completes fetching, the `onMessageUpdated()` method will be called and the message with its `OGMetaData` object will be delivered to the recipients' client app. However, if the Vyin Chat server has already cached the OG metadata of the URL, `BaseMessage.ogMetaData` returns the message and its `OGMetaData` object instantly and the `onMessageUpdated()` method won't be called.

#### Open channel

```kotlin
// Send a user message containing the URL of a web page.
val params = UserMessageCreateParams("vyin.chat")
openChannel.sendUserMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}

// Receive a user message containing OG metadata of the web page through a channel event handler.
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : OpenChannelHandler() {
        override fun onMessageReceived(channel: BaseChannel, message: BaseMessage) {
            val ogMetaData = message.ogMetaData
            if (ogMetaData == null) {
                // The getOgMetaData() method returns null if the message doesn't have an OGMetadata object yet.
                return
            } else {
                // You can create and customize the URL link preview by using the Open Graph metadata of the message.
                val url = ogMetaData.url
                // ...
            }
            // ...
        }

        override fun onMessageUpdated(channel: BaseChannel, message: BaseMessage) {
            val ogMetaData = message.ogMetaData
            if (ogMetaData == null) {
                return
            } else {
                // You can create and customize the URL link preview by using the Open Graph metadata of the message.
                val url = ogMetaData.url
                // ...
            }
        }
    }
)
```

#### Group channel

```kotlin
// Send a user message containing the URL of a web page.
val params = UserMessageCreateParams("vyin.chat")
groupChannel.sendUserMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}

// Receive a user message containing OG metadata of the web page through a channel event handler.
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : GroupChannelHandler() {
        override fun onMessageReceived(channel: BaseChannel, message: BaseMessage) {
            val ogMetaData = message.ogMetaData
            if (ogMetaData == null) {
                // The getOgMetaData() returns null if the message doesn't have an OGMetadata object yet.
            } else {
                // You can create and customize the URL link preview by using the Open Graph metadata of the message.
                val url = ogMetaData.url
            }
        }

        override fun onMessageUpdated(channel: BaseChannel, message: BaseMessage) {
            val ogMetaData = message.ogMetaData
            if (ogMetaData == null) {
                return
            } else {
                // You can create and customize the URL link preview by using the Open Graph metadata of the message.
                val url = ogMetaData.url
            }
        }
    }
)
```

---

### Generate thumbnails of a file message

When sending image or video files, you can create thumbnails of the multimedia files and render them into your UI. You can specify up to three different sizes for thumbnail images when sending a message both with a single file and with multiple files.

Note: Supported file types are `image/*` and `video/*`. The Chat SDK doesn't support creating thumbnails when sending a file message via URL.

The `sendFileMessage()` method requires passing a `FileMessageCreateParams` object as an argument to a parameter. It contains an array of `ThumbnailSize` objects which specify the maximum values of width and height of each thumbnail image with the `ThumbnailSize` constructor. The `completionHandler` callback subsequently returns the array of `ThumbnailSize` objects that each contain the URL of the generated thumbnail image file.

A thumbnail image is generated to fit within the bounds of the provided `maxWidth` and `maxHeight`. If the size of the original image is smaller than the specified dimensions, the original image will have the width and height of the specified dimensions. The URL of the thumbnail returns the location of the generated thumbnail file within the Vyin Chat server.

```kotlin
// Create and add a thumbnailSizeList object. Up to three thumbnail sizes are allowed.
val thumbnailSizeList = listOf(
    ThumbnailSize(100, 200),
    ThumbnailSize(200, 200)
)
val params = FileMessageCreateParams().apply {
    file = FILE
    fileName = FILE_NAME
    fileSize = FILE_SIZE
    mimeType = MIME_TYPE
    thumbnailSizes = thumbnailSizeList
}

groupChannel.sendFileMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }
    if (message == null) return@sendFileMessage
    val first = message.thumbnails[0]
    val second = message.thumbnails[1]
    val maxHeightFirst = first.maxHeight // 100
    val maxHeightSecond = second.maxHeight // 200

    val urlFirst = first.url // The URL of first thumbnail file.
    val urlSecond = second.url // the URL of second thumbnail file.
}
```

---

## Managing scheduled messages in group channel

### Create a scheduled message

You can create a scheduled user message to send at a later time by passing `ScheduledUserMessageCreateParams` as an argument to the `createScheduledUserMessage()` method.

```kotlin
// Creates a scheduled user message after 5 minutes.
val params = ScheduledUserMessageCreateParams(
    scheduledAt = System.currentTimeMillis() + TimeUnit.MINUTES.toMillis(5)
).apply {
    message = "Text message"
    translationTargetLanguages = listOf("en", "ko")
    data = null
    customType = null
    mentionType = MentionType.USERS
    mentionedUserIds = listOf(MENTIONED_USER_ID)
    metaArrays = null
    appleCriticalAlertOptions = null
    pushNotificationDeliveryOption = null
}

// The returned message is a pending message instance for the scheduled message.
// It can be used in the same way as pending message from channel.sendUserMessage().
val pendingScheduledUserMessage = groupChannel.createScheduledUserMessage(params) { scheduledUserMessage, e ->
    if (e != null) {
        // Handle error.
    }

    // A user message has been successfully scheduled.
    // scheduledUserMessage.scheduledInfo will contain scheduled information.
    // scheduledUserMessage.sendingStatus will be SendingStatus.SCHEDULED.
}
```

Note: Vyin Chat does not currently implement creating a scheduled file message.

---

### Retrieve a scheduled message

You can retrieve a specific scheduled message by passing `ScheduledMessageRetrievalParams` as an argument to the `getScheduledMessage()` method.

```kotlin
val params = ScheduledMessageRetrievalParams(
    channelUrl = CHANNEL_URL,
    scheduledMessageId = scheduledMessageId
)

// Retrieves a scheduled message.
BaseMessage.getScheduledMessage(params) { scheduledMessage, e ->
    if (e != null) {
        // Handle error.
    }

    // The scheduled message has been successfully retrieved.
}
```

#### ScheduledMessageRetrievalParams

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| channelUrl | string | Specifies the URL of the channel. |
| scheduledMessageId | long | Specifies the ID of the scheduled message. |

---

### Update a scheduled message

You can update a scheduled user message by passing `ScheduledUserMessageUpdateParams` as an argument to the `updateScheduledUserMessage()` method.

```kotlin
// Updates a scheduled user message.
val params = ScheduledUserMessageUpdateParams().apply {
    scheduledAt = System.currentTimeMillis() + TimeUnit.MINUTES.toMillis(5)
    message = "Text message"
    translationTargetLanguages = listOf("en", "ko")
    data = null
    customType = null
    mentionType = MentionType.USERS
    mentionedUserIds = listOf(MENTIONED_USER_ID)
    metaArrays = null
    appleCriticalAlertOptions = null
    pushNotificationDeliveryOption = null
}

groupChannel.updateScheduledUserMessage(scheduledMessageId, params) { scheduledUserMessage, e ->
    if (e != null) {
        // Handle error.
    }

    // A scheduled user message has been successfully updated.
}
```

---

### Cancel a scheduled message

Use the `cancelScheduledMessage()` method to cancel a message that a user has scheduled to send at a later time.

```kotlin
// Cancels a scheduled message.
groupChannel.cancelScheduledMessage(scheduledMessageId) { e ->
    if (e != null) {
        // Handle error.
    }
    // A scheduled message has been successfully canceled.
}
```

---

### Send scheduled message immediately

Use the `sendScheduledMessageNow()` method to send a scheduled message immediately instead of sending it at the scheduled time.

```kotlin
// Sends a scheduled message immediately.
groupChannel.sendScheduledMessageNow(scheduledMessageId) { e ->
    if (e != null) {
        // Handle error.
    }

    // A scheduled message has been sent immediately.
}
```

---

### List all scheduled messages

Create a `ScheduleMessageListQuery` instance to retrieve all scheduled messages matching the specifications set by `ScheduledMessageListQueryParams`. After a list of all scheduled messages is successfully retrieved, you can access the data of each scheduled message from the result list through the `messageList` parameter of the callback handler.

```kotlin
val params = ScheduledMessageListQueryParams(
    order = ScheduledMessageListQuery.Order.CREATED_AT,
    messageTypeFilter = MessageTypeFilter.ALL,
    reverse = false,
    channelUrl = null,
    scheduledStatus = null,
    limit = 20
)

val query = GIMChat.createScheduledMessageListQuery(params)
query.next { messageList, e ->
    if (e != null) {
        // Handle error.
    }
    // You can handle actions relevant to the scheduled message list of the channel.
}
```

#### ScheduledMessageListQueryParams

| Property name | Type | Description |
| :---- | :---- | :---- |
| order | enum | Determines the order of the retrieved messages. Acceptable values are `CREATED_AT` and `SCHEDULED_AT`. (Default: `CREATED_AT`) |
| messageTypeFilter | enum | Specifies the message type to filter the messages with the corresponding type. Acceptable values are `ALL`, `USER`, `FILE`, and `ADMIN`. (Default: `ALL`) |
| reverse | boolean | Determines whether to sort the retrieved messages in reverse order. If `false`, the results are in ascending order. (Default: `false`) |
| channelUrl | string | Specifies a target channel to query the scheduled messages from. If `null`, it retrieves scheduled messages from all channels. (Default: `null`) |
| scheduledStatus | list | Specifies list of `ScheduledStatus` where scheduled messages with only given `ScheduledStatus` are retrieved. (Default: `null`) |
| limit | int | Specifies the number of results to return per call. Acceptable values are `1` to `100`, inclusive. (Default: `20`) |

---

### Retrieve the total number of scheduled messages

Note: Vyin Chat does not currently implement retrieving the total number of scheduled messages.

---

## Managing pinned messages in group channels

### Pin a message

Vyin Chat SDK for Android allows you to pin messages in group channels. You can pin all types of messages including text message, file message, multiple file message, and admin message. The pinned messages feature allows users to mark or highlight specific messages which can be announcements, updates, instructions, or any other messages deemed important in a channel. This makes it easier for channel members to find and access important information, even in large or active channels where there may be a high volume of incoming messages.

#### Limitations

- The table below shows the types of channels that support pinned messages. See the channel types section to learn about the differences among various channel types.

|  | Open channel | Group channel | Supergroup channel |
| :---- | :---- | :---- | :---- |
| Pinned messages | Supported, only through the Moderator message feature | Supported, except ephemeral channels | Not supported |

- For open channels: The pinned messages feature is only supported through the Advanced Moderation's moderator messaging feature. For more information, see the Moderator messaging page.
- Pinned messages can't be sent as silent messages. The `isSilent` property should be set to `false` when pinning or unpinning messages.
- By default, the maximum number of messages that can be pinned is 10 per channel. To increase this limit, contact our sales team.

---

#### Pinning a message on send

You can pin a new message that you're sending in a channel by setting the `isPinnedMessage` property to `true`. This property belongs to the `BaseMessageCreateParams` class. The default value for this property is `false`, meaning that the message being sent is not automatically pinned unless specified.

#### Pinning an existing message

You can pin a message, that is already sent in a channel, using the `pinMessage()` method of the `GroupChannel` class. Specify the `messageId` to pin as shown in the code below.

Note: A group channel has to be created before implementing the code below.

```kotlin
channel.pinMessage(messageId) { e ->
    if (e != null) {
        // handle error
    }
}
```

The following table shows a list of properties related to the pinned messages feature. The `pinnedMessageIds`, and `lastPinnedMessage` properties belong to the `GroupChannel` class.

#### List of properties

| Property name | Type | Description |
| :---- | :---- | :---- |
| pinnedMessageIds | List | Specifies an array of message IDs of the pinned messages in a group channel. |
| lastPinnedMessage | BaseMessage? | Specifies the last message that was pinned in a group channel. |

#### Getting notified when a message is pinned

Once a pinned message is sent or an existing message is pinned, the `onPinnedMessageUpdated(channel: BaseChannel)` event handler is invoked. For further information on `GroupChannelHandler`, see the Add or remove a channel event handler page.

---

### Unpin a message

Vyin Chat SDK for Android allows you to unpin messages in group channels. Unpinning messages that are no longer relevant or important helps to keep the pinned messages organized in your channel.

#### Unpinning a message in a channel

You can unpin a message using the `unpinMessage()` method of the `GroupChannel` class. Specify the `messageId` of a message to unpin as shown in the code below.

```kotlin
channel.unpinMessage(messageId) { e ->
    if (e != null) {
        // handle error
    }
}
```

The following table shows a list of properties related to the pinned messages feature. The `pinnedMessageIds`, and `lastPinnedMessage` properties belong to the `GroupChannel` class.

#### List of properties

| Property name | Type | Description |
| :---- | :---- | :---- |
| pinnedMessageIds | List | Specifies an array of message IDs of the pinned messages in a group channel. |
| lastPinnedMessage | BaseMessage? | Specifies the last message that was pinned in a group channel. |

#### Getting notified when a message is unpinned

Once a message is unpinned, the `onPinnedMessageUpdated(channel: BaseChannel)` event handler is invoked. For further information on `GroupChannelHandler`, see the Add or remove a channel event handler page.

---

### List pinned messages

To retrieve all the pinned messages in a group channel, use `getChannelPinnedList` function.

```kotlin
// Retrieve pinned messages.
channel.getChannelPinnedList { list, e ->
    if (e != null) {
        // Handler error
    }
}
```

---

## Listing changelogs

### List changelogs of messages

Each message changelog has distinct properties such as the timestamp of an updated message or the unique ID of a deleted message. Based on these two properties, you can retrieve message changelogs using either the timestamp or the token. Both the `getMessageChangeLogsSinceToken()` and `getMessageChangeLogsSinceTimestamp()` methods require a parameter `MessageChangeLogsParams` object to determine which messages to return.

```kotlin
val params = MessageChangeLogsParams().apply {
    replyType = ReplyType.ALL // or ReplyType.ONLY_REPLY_TO_CHANNEL, ReplyType.NONE
    messagePayloadFilter = MessagePayloadFilter().apply {
        includeThreadInfo = INCLUDE_THREAD_INFO
        includeParentMessageInfo = PARENT_MESSAGE_INFO
    }
}
```

#### MessageChangeLogsParams

| Property name | Type | Description |
| :---- | :---- | :---- |
| `replyType` | string | Specifies the type of message to include in the results. `NONE` (default): Includes unthreaded messages and only the parent messages of threaded messages. `ALL`: Includes both threaded and unthreaded messages. `ONLY_REPLY_TO_CHANNEL`: Includes unthreaded messages, parent messages of threaded messages, and messages sent to the channel as replies with the `reply_to_channel` property set to `true`. |
| `includeThreadInfo` | boolean | Determines whether to include the thread information of the messages in the result. (Default: `false`) |
| `includeParentMessageInfo` | boolean | Determines whether to include the information of the parent messages in the result. (Default: `false`) |

---

#### By timestamp

You can retrieve the message changelogs by specifying a timestamp. The results only include changelogs that were created after the specified timestamp.

Note: The `getMessageChangeLogsByTimestamp` method is deprecated. Accordingly, use the `getMessageChangeLogsSinceTimestamp()` method instead.

```kotlin
channel.getMessageChangeLogsSinceTimestamp(TIMESTAMP, params) { updatedMessages, deletedMessageIds, hasMore, token, e ->
    if (e != null) {
        // Handle error.
    }

    // A list of message changelogs created after the specified timestamp is successfully retrieved.
}
```

#### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| `ts` | long | Specifies a timestamp to be the reference point for the changelogs to be retrieved, in Unix milliseconds format. |
| `params` | MessageChangeLogsParams | Contains a set of parameters you can use when retrieving changelogs. |

---

#### By token

You can also retrieve message changelogs by specifying a token. The token is an opaque string that marks the starting point of the next page in the result set and it's included in the callback of the previous call. Based on the token, the next page starts with changelogs that were created after the specified token.

```kotlin
channel.getMessageChangeLogsSinceToken(TOKEN, params) { updatedMessages, deletedMessageIds, hasMore, token, e ->
    if (e != null) {
        // Handle error.
    }

    // A list of message changelogs created after the specified token is successfully retrieved.
}
```

#### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| `token` | string | Specifies a token to be the reference point for the changelogs to be retrieved. |
| `params` | MessageChangeLogsParams | Contains a set of parameters you can use when retrieving changelogs. |

---

## Managing read status in a group channel

### Mark messages as read

In a group channel, the read status of messages is crucial for understanding which messages have been read by which members. This document explains how to mark messages as read in a group channel.

To keep the most up-to-date and accurate read status of messages for all group channel members, the `markAsRead()` method should be called in the following cases:

- When a channel member reads messages by entering the channel from a channel list.
- When a channel member switches the chat view from the background to the foreground.

---

#### Read receipts

When a channel member sends a message to the channel, the Vyin Chat server immediately updates the sender's read receipt to the time when the message was sent. The read receipts of other channel members are updated when the `markAsRead()` method is called.

---

#### Unread message count

If a member opens a channel and the `markAsRead()` method is called, the Vyin Chat server updates the following:

- The unread message count of the individual channel.
- The total unread message count of all the group channels joined by the member.

The server then triggers the `onReadStatusUpdated()` method of the channel event handler to notify the change of the read status to all other channel members' devices.

```kotlin
// Call markAsRead() when the current user views unread messages in a group channel.
groupChannel.markAsRead(null)

// To listen to an update from other channel members' client apps,
// implement onReadStatusUpdated() with actions to perform when notified.
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : GroupChannelHandler() {
        override fun onMessageReceived(channel: BaseChannel, message: BaseMessage) {}

        override fun onReadStatusUpdated(channel: GroupChannel) {
            if (currentGroupChannel.url == channel.url) {
                // For example, you can redraw a channel view here.
            }

            // ...
        }
    }
)
```

---

#### Display past messages for new members

If a new member joins the channel, the method works differently based on the value of the `display_past_message` property of your Vyin Chat application. The `display_past_message` property determines whether to display past messages to newly joined members when they enter the channel.

If the property is set to `true`, the new member's read receipt is updated to the sent time of the last message in the channel. If set to `false`, the property's value is `0`.

Note: This property is also linked to the Chat history option, which can be managed on Vyin Chat Console under Settings > Chat > Channels > Group channels.

---

### Get read status of a message

In a group channel, the read status of messages is crucial for understanding which messages have been read by which members. This document explains how to get read status of a message in a group channel.

You can get the read status of all members in the channel through the `getReadStatus()` method in the Group channel class. If its parameter `includeAllMembers` is set to `false`, the result won't contain the read status of the current user.

```kotlin
// If includeAllMembers is false, the result won't include the current user.
val membersReadStatus = groupChannel.getReadStatus(includeAllMembers = true)
membersReadStatus.forEach { userId, status ->
    // Handle the read status of each member.
}
```

---

## Marking messages as delivered in a group channel

### Mark messages as delivered

Delivery receipt can be used to see whether a message has been successfully delivered to all the intended recipients by the Vyin Chat server.

```kotlin
groupChannel.markAsDelivered(MESSAGE_ID, CREATED_AT, TEMP_SESSION_KEY)
```

#### Receive callbacks for delivery receipts

When a message is delivered to group channel members who are online, it is automatically marked as delivered and channel members are also notified of the message delivery through the `onDeliveryStatusUpdated()` method in the channel event handler.

```kotlin
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : GroupChannelHandler() {
        override fun onMessageReceived(channel: BaseChannel, message: BaseMessage) {
            // ...
        }

        override fun onDeliveryStatusUpdated(channel: GroupChannel) {
            // ...
        }
    }
)
```

---

## Mentioning other users in a message

### Mention other users in a message

In both group channels and open channels, a user can mention other users in a message to call their attention. Users have the option of calling specific users in the channel by their user IDs or calling all users in the channel.

Up to ten mentioned users are notified when mentioned. Notification preferences for mentions can be configured for each user in a channel.

---

#### Mention by user IDs

To mention specific users when sending a message, add a list of user IDs to `mentionedUserIds`. Then, add the list to either `UserMessageCreateParams`, `FileMessageCreateParams`, or `MultipleFilesMessageCreateParams` and pass the params to either `sendUserMessage()`, `sendFileMessage()`, or `sendMultipleFilesMessage()`, respectively.

Mentioned users must belong to the channel where the message is being sent. Users who are mentioned but don't belong to the channel won't be included in the mentioned user IDs array of the sent message.

```kotlin
val userIdsToMention = listOf("Harry", "Jay", "Jin")
val params = UserMessageCreateParams(MESSAGE).apply {
    mentionedUserIds = userIdsToMention
}
channel.sendUserMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

---

#### Mention all channel users

When a user types "@channel" or other designated text in a message, you can set `mentionType` to `MentionType.CHANNEL` and let the user call the attention of everyone in the channel.

```kotlin
val params = UserMessageCreateParams(MESSAGE).apply {
    mentionType = MentionType.CHANNEL
}
channel.sendUserMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

---

#### Limitations

Using channel mention is subject to the following limitations.

#### Channel mentions per user

| Channel size | Allowed mentions per hour |
| :---- | :---- |
| Less than or equal to 100 users | 10 |
| Greater than 100 users | 1 |

#### Channel mentions per channel

| Channel size | Allowed mentions per hour |
| :---- | :---- |
| Less than or equal to 100 users | 100 |
| Greater than 100 users | 10 |

---

## Managing polls

### Create a poll

The polls feature allows group channel members and channel operators to create and send a poll attached to text messages. A poll usually consists of a question and at least one poll option that users can vote on. The functionality provides an easier way to gather feedback from groups of all sizes, collect data from customers, and drive user engagement. You can configure various settings for your poll, including when the poll will close and whether to allow users to add poll options or vote on multiple poll options.

#### Prerequisite

To use polls in your Vyin Chat application, you must activate the polls feature on Vyin Chat Console. Go to Settings > Chat > Features and turn on Polls.

#### Limitations

Refer to the following limitations when using polls.

- Polls can't be sent in the form of following message types: file messages, admin messages, and scheduled text messages.
- Data on polls isn't included in the result when exporting message data.
- The table below shows the types of channels that support polls. See the channel types section to learn about the differences among various channel types.

|  | Open channel | Group channel | Supergroup channel |
| :---- | :---- | :---- | :---- |
| Polls | Supported, except ephemeral channels | Supported, except ephemeral channels | Supported, except ephemeral channels |

Note: The maximum number of options that can be added to a poll differs depending on your Vyin Chat subscription plan. For further information, contact our sales team.

#### PollCreateParams

You can create a poll by creating and passing a `PollCreateParams` object as an argument to the parameter in the `create()` method.

```kotlin
val params = PollCreateParams(
    title = "Title",
    optionTexts = listOf("Option 1", "Option 2"),
    data = PollData("Additional data"),
    allowUserSuggestion = false,
    allowMultipleVotes = false,
    closeAt = -1 // If you don't want to specify a closing time, then pass -1.
)

Poll.create(params) { poll, e ->
    if (e != null) {
        // Handle error.
    }

    // After creating a poll, the poll is sent to the channel.
    if (poll != null) {
        val userMessageParams = UserMessageCreateParams("Poll").apply {
            pollId = poll.id
        }
        channel.sendUserMessage(userMessageParams) { message, e ->
            if (e != null) {
                // Handle error.
            }
        }
    }
}
```

#### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| title | string | Specifies the title of a poll. |
| optionTexts | Array of strings | Specifies the texts of possible options for which a user can vote on. Note that this property is only valid when creating a poll, but is ignored when updating a poll. |
| data | PollData | Specifies an additional data to accompany the poll. A use case might be to provide explanations for incorrect quiz answers. |
| allowUserSuggestion | Boolean | Determines whether to allow users to make suggestions. (Default: `false`) |
| allowMultipleVotes | Boolean | Determines whether to allow users to vote on more than one poll options. (Default: `false`) |
| closeAt | Long | Specifies the time when a poll has closed or will close in Unix seconds. If the value of this property is `-1`, the poll status remains `open` meaning that the poll will never close. |

#### PollHandler

Through `PollHandler`, the Vyin Chat server always notifies whether your poll has been successfully created.

```kotlin
fun interface PollHandler {
    fun onResult(poll: Poll?, e: GIMException?)
}
```

---

### Retrieve a poll

You can retrieve a specific poll by creating and passing a `PollRetrievalParams` object as an argument to the `get()` method.

```kotlin
val params = PollRetrievalParams(pollId, channel.channelType, channel.url)
Poll.get(params) { poll, e ->
    if (e != null) {
        // Handle error.
    }

    // The poll is successfully retrieved.
}
```

#### PollRetrievalParams

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| channelUrl | string | Specifies the URL of the channel. |
| channelType | channelType | Specifies the type of the channel. |
| pollId | long | Specifies the unique ID of a poll. |

---

### Update a poll

You can update a poll by creating and passing a `PollUpdateParams` object as an argument to the parameter in the `updatePoll()` method.

```kotlin
val params = PollUpdateParams(
    title = "Updated Title",
    data = PollData("Updated Poll Data"),
    allowUserSuggestion = false,
    allowMultipleVotes = false,
    closeAt = System.currentTimeMillis()
)
channel.updatePoll(pollId, params) { poll, e ->
    if (e != null) {
        // Handle error.
    }

    // The poll is successfully updated.
}
```

#### PollUpdateParams

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| title | string | Specifies the title of a poll. |
| data | PollData | Specifies an additional data to accompany the poll. A use case might be to provide explanations for incorrect quiz answers. |
| allowUserSuggestion | Boolean | Determines whether to allow users to make suggestions. (Default: `false`) |
| allowMultipleVotes | Boolean | Determines whether to allow users to vote on more than one poll options. (Default: `false`) |
| closeAt | Long | Specifies the time when a poll has closed or will close in Unix seconds. If the value of this property is `-1`, the poll status remains `open` meaning that the poll will never close. |

---

### Delete a poll

You can delete a poll by specifying the `pollId` and only the user who created the poll can delete the poll. Also channel operators can delete any poll in a channel as well.

```kotlin
channel.deletePoll(pollId) { e ->
    if (e != null) {
        // Handle error.
    }
}
```

---

### Close a poll

You can close a poll by specifying the `pollId` in the `closePoll()` method.

```kotlin
fun closePoll(channel: GroupChannel, pollId: Long) {
    channel.closePoll(pollId) { poll, e ->
        if (e != null) {
            // Handle error.
        }

        // The poll has been successfully closed.
        // Update or check `closeAt` here to reflect the change in the UI.
    }
}
```

#### The closeAt property

When integrating polls in your application, managing the `closeAt` property is crucial for a smooth user experience. Here are guidelines for handling this property effectively:

**1. Message list (Channel view)**

- Closed polls: When rendering a list of messages, check if the poll's `closeAt` time is in the past compared to the current time. If it is, display the poll using a "closed" UI state. Note that differences between device and server time might affect this comparison.

**2. Real-time updates in channels**

- Active channel scenario: If a user is already in a channel and remains in a channel past the `closeAt` time, implement a real-time timer to check `closeAt` against the current time and update the UI accordingly.

**3. Re-entering a channel**

- UI update on re-entry: Make sure the UI updates correctly to show the poll's closed state if `closeAt` has passed while the user was away in another view.

**4. User interactions with closed polls**

- Error handling: Prevent users from interacting with closed polls by updating the UI and handling errors appropriately if they attempt to vote after `closeAt`.

**5. Standalone poll screens**

- Consistent logic: Apply the same real-time checks and UI updates in standalone poll views as in message lists.

---

### Add a poll option

You can add a poll option to an existing poll by passing the `pollId` and the `optionText` as an argument to the parameter in the `addPollOption()` method.

```kotlin
channel.addPollOption(pollId, "optionText") { poll, e ->
    if (e != null) {
        // Handle error.
    }

    if (poll != null) {
        // ...
    }
}
```

---

### Retrieve a poll option

You can retrieve a specific poll option by creating and passing a `PollOptionRetrievalParams` object as an argument to the `PollOption.get()` method.

```kotlin
val params = PollOptionRetrievalParams(pollId, pollOptionId, channel.channelType, channel.url)
PollOption.get(params) { option, e ->
    if (e != null) {
        // Handle error.
    }

    // The poll option has been successfully retrieved.
}
```

#### List of properties for poll option

| Property name | Type | Description |
| :---- | :---- | :---- |
| pollId | long | Specifies the unique ID of a poll. |
| id | long | Specifies the unique ID of a poll option. This value is unique within a poll. |
| text | string | Specifies the text that describes an option. |
| createdBy | string | Specifies the unique ID of the user who created an option. |
| createdAt | long | Specifies the time when an option is created in Unix seconds. |
| voteCount | long | Specifies the number of votes casted on an option. |
| updatedAt | long | Specifies the time when an option is updated in Unix seconds. |

#### PollOptionRetrievalParams

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| pollId | long | Specifies the unique ID of a poll. |
| pollOptionId | long | Specifies the unique ID of a poll option. |
| channelType | channelType | Specifies the type of the channel. |
| channelUrl | string | Specifies the URL of the channel. |

---

### Update a poll option

You can update a poll option by specifying the `pollId`, the `pollOptionId`, and an updated string for `optionText` parameters in the `updatePollOption()` method.

```kotlin
channel.updatePollOption(pollId, pollOptionId, "Updated option1") { poll, e ->
    if (e != null) {
        // Handle error.
    }

    // The poll option is successfully updated.
}
```

---

### Delete a poll option

You can delete a poll option by specifying the `pollId` and the `pollOptionId` in the `deletePollOption()` method. Also channel operators can delete any poll option of a poll as well.

```kotlin
channel.deletePollOption(pollId, pollOptionId) { e ->
    if (e != null) {
        // Handle error.
    }
}
```

---

### Cast or cancel a vote

You can cast or cancel a vote by passing `PollVoteEvent` as an argument to a parameter when calling the `votePoll()` method. Use the `optionIds` property of the `votePoll()` method to update the user's final vote choice for a poll. This overrides previous vote actions, so to update previous votes, pass new `pollOptionIds` as a parameter. To cancel votes, pass an empty list as `pollOptionIds`.

```kotlin
channel.votePoll(pollId, optionIds) { pollVoteEvent, e ->
    if (e != null) {
        // Handle error.
    }

    // To apply the vote result to the poll, You need to take the following actions.
    // 1. Find a poll by pollVoteEvent.messageId and pollVoteEvent.pollId.
    // 2. Call userMessage.poll.applyPollVoteEvent(event: PollVoteEvent).
}
```

---

### Retrieve a list of polls

Create a `PollListQuery` instance to retrieve a list of polls matching the specifications set by `PollListQueryParams`. After a list of polls is successfully retrieved, you can access the data of each polls from the result list through the `polls` parameter of the callback handler.

```kotlin
val params = PollListQueryParams(channel.channelType, channel.url, limit = 10)
GIMChat.createPollListQuery(params).next { polls, e ->
    if (e != null) {
        // Handle error.
    }

    // The list of polls has been successfully retrieved.
}
```

#### PollListQueryParams

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| channelType | channelType | Specifies the type of the channel. |
| channelUrl | string | Specifies the URL of the channel. |
| limit | int | Specifies the number of results to retrieve per page. Acceptable values are `1` to `20`, inclusive. (Default: `10`) |

---

### Retrieve a list of voters

Create a `PollVoterListQuery` instance to retrieve a list of voters matching the specifications set by `PollVoterListQueryParams`. After a list of voters is successfully retrieved, you can access the data of each voter from the result list through the `votes` parameter of the callback handler.

```kotlin
val params = PollVoterListQueryParams(pollId, pollOptionId, channel.channelType, channel.url, limit = 20)
GIMChat.createPollVoterListQuery(params).next { votes, e ->
    if (e != null) {
        // Handle error.
    }

    // The list of voters for a poll option has been successfully retrieved.
}
```

#### PollVoterListQueryParams

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| pollId | long | Specifies the unique ID of a poll. |
| pollOptionId | long | Specifies the unique ID of a poll option. |
| channelType | channelType | Specifies the type of the channel. |
| channelUrl | string | Specifies the URL of the channel. |
| limit | int | Specifies the number of results to retrieve per page. Acceptable values are `1` to `100`, inclusive. (Default: `20`) |

---

### List changelogs of polls

Each poll changelog has distinct properties such as the timestamp of an updated poll or the unique ID of a deleted poll. Based on these two properties, you can retrieve poll changelogs using either the timestamp or the token.

#### By timestamp

You can retrieve the poll changelogs by specifying a timestamp. The results only include changelogs that were created after the specified timestamp.

```kotlin
channel.getPollChangeLogsSinceTimestamp(TIMESTAMP) { updatedPolls, deletedPollIds, hasMore, token, e ->
    if (e != null) {
        // Handle error.
    }

    // A list of poll changelogs created after the specified timestamp is successfully retrieved.
}
```

#### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| ts | long | Specifies a timestamp to be the reference point for the changelogs to be retrieved, in Unix seconds format. |

#### By token

You can also retrieve poll changelogs by specifying a token. The token is an opaque string that marks the starting point of the next page in the result set and it's included in the callback of the previous call. Based on the token, the next page starts with changelogs that were created after the specified token.

```kotlin
channel.getPollChangeLogsSinceToken(TOKEN) { updatedPolls, deletedPollIds, hasMore, token, e ->
    if (e != null) {
        // Handle error.
    }

    // A list of poll changelogs created after the specified token is successfully retrieved.
}
```

#### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| token | string | Specifies a token to be the reference point for the changelogs to be retrieved. |

---

## Retrieving unread count in a group channel

### Retrieve number of unread messages in a channel

Using the `unreadMessageCount` method, you can retrieve the total number of a user's unread messages in a group channel.

```kotlin
val unreadMessageCount = groupChannel.unreadMessageCount
```

---

### Retrieve number of unread messages in all channels

Using the `getTotalUnreadMessageCount` method, you can retrieve the total number of a user's unread messages in all group channels the user has joined.

```kotlin
val params = GroupChannelTotalUnreadMessageCountParams().apply {
    superChannelFilter = CHANNEL_TYPE_FILTER
    channelCustomTypes = CUSTOM_TYPE
}
GIMChat.getTotalUnreadMessageCount(params) { groupChannelCount, feedChannelCount, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

#### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| `superChannelFilter` | SuperChannelFilter | Specifies the message type to filter the messages with the corresponding type. Acceptable values are `ALL`, `NONSUPER_CHANNEL_ONLY`, `EXCLUSIVE_CHANNEL_ONLY`, and `SUPER_CHANNEL_ONLY`. |
| `channelCustomTypes` | string | Specifies the custom channel type to retrieve channels with the corresponding custom type. |

---

### Retrieve number of channels with unread messages

Using the `getTotalUnreadChannelCount` method, you can retrieve the total number of group channels in which a user has one or more unread messages.

```kotlin
GIMChat.getTotalUnreadChannelCount { totalUnreadChannelCount, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

---

### Retrieve number of members who haven't received a message

You can retrieve the number of members who haven't received a specific message in a group channel. When the value of `0` is returned, it means that the message has been successfully delivered to all members in the channel.

```kotlin
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : GroupChannelHandler() {
        override fun onMessageReceived(channel: BaseChannel, message: BaseMessage) {
            // ...
        }

        override fun onDeliveryStatusUpdated(channel: GroupChannel) {
            val isAllDelivered = channel.getUndeliveredMemberCount(message) == 0
        }
    }
)
```

---

### Retrieve number of members who haven't read a message

Using the `getUnreadMemberCount()` method, you can get the number of members who haven't read a specific message in a group channel. To get the most up-to-date value, the channel should first be updated through `markAsRead()` before calling the `getUnreadMemberCount()` method.

```kotlin
// Call markAsRead() when the current user views unread messages in a group channel.
groupChannel.markAsRead(null)
// ...

// To listen to an update from other channel members' client apps, implement onReadReceiptUpdated() with actions to perform when notified.
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : GroupChannelHandler() {
        override fun onReadStatusUpdated(channel: GroupChannel) {
            if (currentGroupChannel.url == channel.url) {
                messages.forEach { message ->
                    val unreadCount = channel.getUnreadMemberCount(message)
                    if (unreadCount <= 0) {
                        // All members have read the message.
                    } else {
                        // Some members haven't read the message yet.
                    }
                }
            }
        }
    }
)
```

---

### Retrieve number of unread items

Note: Vyin Chat does not currently implement retrieving number of unread items.

---

## Categorizing messages

### Categorize messages by custom type

When sending a message, you can specify a custom message type to subclassify messages. This custom type takes on the form of `String` and can be useful in searching or filtering messages.

The `data` and `customType` properties of a message object allow you to append information to your messages. While both properties can be used flexibly, common examples for `customType` include grouping messages into categories such as Notes or Contacts.

To embed a custom type into your message, assign a value to `customType` under the `UserMessageCreateParams` or `FileMessageCreateParams` object. Then, pass the specified object as an argument to the parameter in the `sendUserMessage()` or `sendFileMessage()` method.

To get a message's custom type, read `message.customType`.

```kotlin
val params = UserMessageCreateParams().apply {
    message = TEXT_MESSAGE
    customType = CUSTOM_TYPE
}
channel.sendUserMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

---

## Adding extra data to a message

Note: Vyin Chat does not currently implement adding meta-array data to a message.

---

## Translating messages

### Auto-translate messages

Vyin Chat's auto-translation feature lets you send text messages in different languages. When sending a text message, set `List` of language codes to a `UserMessageCreateParams` object and then pass the object as an argument to a parameter in the `sendUserMessage()` method to request messages to be translated into the desired languages. With this, you can add a real-time translation feature to your app. To enable this feature, contact our sales team.

Note: Vyin Chat's message auto-translation feature is powered by Google Cloud Translation API recognition engine. You can find the language codes supported by the engine in the translation engine page. You can also visit the language support page in Google Cloud Translation.

```kotlin
val targetLanguages = listOf(
    "es",   // Spanish
    "ko"    // Korean
)

val params = UserMessageCreateParams(TEXT_MESSAGE).apply {
    translationTargetLanguages = targetLanguages
}

channel.sendUserMessage(params) { message, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

To show the translated messages, use the `userMessage.translations` method which returns a `Map` object containing the language codes and translations as shown in the code below.

#### Open channel

```kotlin
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : OpenChannelHandler() {
        override fun onMessageReceived(channel: BaseChannel, message: BaseMessage) {
            if (message is UserMessage) {
                val map = message.translations
                val esTranslatedMessage = map["es"] // Spanish

                // Display translation in the UI.
            }
        }
    }
)
```

#### Group channel

```kotlin
GIMChat.addChannelHandler(
    UNIQUE_HANDLER_ID,
    object : GroupChannelHandler() {
        override fun onMessageReceived(channel: BaseChannel, message: BaseMessage) {
            if (message is UserMessage) {
                val map = message.translations
                val esTranslatedMessage = map["es"] // Spanish

                // Display translation in the UI.
            }
        }
    }
)
```

---

### Translate messages on-demand

You can add a feature to your app to translate select messages into other languages. Using the `openChannel.translateUserMessage()` or the `groupChannel.translateUserMessage()` method, you can translate a text message already sent to a channel into the desired languages based on your own needs.

```kotlin
val targetLanguages = listOf(
    "es",   // Spanish
    "de"    // German
)

// The USER_MESSAGE argument below indicates a UserMessage object which represents an already sent or received text message.
groupChannel.translateUserMessage(USER_MESSAGE, targetLanguages) { message, e ->
    if (message != null) {
        val map = message.translations
        val esTranslatedMessage = map["es"] // Spanish
        val deTranslatedMessage = map["de"] // German
        // ...

        // Show translations in the UI.
    }
}
```

---

### Translation engine

Vyin Chat's message auto-translation and on-demand message translation features are powered by Google Cloud Translation API recognition engine. The recognition engine supports a wide variety of languages for the Neural Machine Translation (NMT) model.

Note: Message translation using the Microsoft Translator engine is no longer available. All Vyin Chat applications that had previously used the engine have been migrated and are now using Google Cloud Translation.

#### Languages supported for translation

The following table shows the languages and codes that Google Cloud engine supports. You can translate between any of the languages listed below.

#### List of languages

| Language | Code | Language | Code |
| :---- | :---- | :---- | :---- |
| Afrikaans | af | Latvian | lv |
| Albanian | sq | Lithuanian | lt |
| Amharic | am | Luxembourgish | lb |
| Arabic | ar | Macedonian | mk |
| Armenian | hy | Malagasy | mg |
| Azerbaijani | az | Malay | ms |
| Basque | eu | Malayalam | ml |
| Belarusian | be | Maltese | mt |
| Bengali | bn | Maori | mi |
| Bosnian | bs | Marathi | mr |
| Bulgarian | bg | Mongolian | mn |
| Catalan | ca | Myanmar (Burmese) | my |
| Cebuano | ceb | Nepali | ne |
| Chinese (Simplified) | zh-CN or zh | Norwegian | no |
| Chinese (Traditional) | zh-TW | Nyanja (Chichewa) | ny |
| Corsican | co | Pashto | ps |
| Croatian | hr | Persian | fa |
| Czech | cs | Polish | pl |
| Danish | da | Portuguese (Portugal, Brazil) | pt |
| Dutch | nl | Punjabi | pa |
| English | en | Romanian | ro |
| Esperanto | eo | Russian | ru |
| Estonian | et | Samoan | sm |
| Finnish | fi | Scots Gaelic | gd |
| French | fr | Serbian | sr |
| Frisian | fy | Sesotho | st |
| Galician | gl | Shona | sn |
| Georgian | ka | Sindhi | sd |
| German | de | Sinhala (Sinhalese) | si |
| Greek | el | Slovak | sk |
| Gujarati | gu | Slovenian | sl |
| Haitian Creole | ht | Somali | so |
| Hausa | ha | Spanish | es |
| Hawaiian | haw | Sundanese | su |
| Hebrew | he or iw | Swahili | sw |
| Hindi | hi | Swedish | sv |
| Hmong | hmn | Tagalog (Filipino) | tl |
| Hungarian | hu | Tajik | tg |
| Icelandic | is | Tamil | ta |
| Igbo | ig | Tatar | tt |
| Indonesian | id | Telugu | te |
| Irish | ga | Thai | th |
| Italian | it | Turkish | tr |
| Japanese | ja | Turkmen | tk |
| Javanese | jv | Ukrainian | uk |
| Kannada | kn | Urdu | ur |
| Kazakh | kk | Uyghur | ug |
| Khmer | km | Uzbek | uz |
| Kinyarwanda | rw | Vietnamese | vi |
| Korean | ko | Welsh | cy |
| Kurdish | ku | Xhosa | xh |
| Kyrgyz | ky | Yiddish | yi |
| Lao | lo | Yoruba | yo |
| Latin | la | Zulu | zu |

---

## AI Summary Message

### Config AI summary messages

The AI Summary Message feature automatically generates message summaries to help users quickly grasp the main content. It intelligently detects when a conversation should be summarized for a user based on user behavior and conversation activity, without requiring manual requests. You can retrieve and update the AI summary settings via the user settings API.

To **get** the current AI summary setting, call:

```kotlin
GIMChat.getUserSettings { userSettings, e ->
    if (e != null) { return@getUserSettings }
    val settings = userSettings ?: UserSettings(aiSummary = AiSummary(isEnable = false))
}
```

To **enable or disable** the AI summary feature, update the settings and call:

```kotlin
val newSettings = UserSettings(AiSummary(isEnable = false))
GIMChat.updateUserSettings(newSettings) { e ->
    if (e != null) { return@updateUserSettings }
}
```

**AISummary**

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| isEnable | boolean | A flag that determines whether the summary feature is turned on (`true`) or off (`false`). |

---

### Retrieve a summary message

The AI summary feature automatically detects when a conversation should be summarized for a user, based on user behavior and conversation activity, without the user manually requesting it.

| Criteria | Description | Able to trigger summary if | No need to trigger if |
| :---- | :---- | :---- | :---- |
| User Absence Duration | How long the user has been offline/inactive in a specific group | The user's absence duration is between 2 hours and 2 days. | The user's absence duration is less than 2 hours or more than 2 days. |
| Conversation Type | Detect whether the recent conversation is meaningful enough for summarization | Conversation has ≤ 70% emoji/sticker-only messages, ≤ 70% short messages, and ≤ 60% casual keywords, and is not dominated by file/media sharing. | > 70% emojis/stickers, > 70% very short messages, > 60% casual keywords, or mainly file/media sharing. |
| Number of Unread Messages and Characters | Count the number of unread messages and total characters | The user has ≥ 30 unread messages OR ≥ 3000 unread characters in that group. | Fewer than 30 unread messages AND fewer than 3000 unread characters. |
| Number of Participants | Tracks the number of unique participants during the user's absence | The number of unique participants ≥ 3. | The number of participants is ≤ 2 (likely a one-on-one chat). |
| Mentions/Tags | Detects if the user was tagged or mentioned directly during their absence | The user is mentioned ≥ 2 times. | The user is mentioned < 2 times. |
| Conversation Topic Change | Detects if there were significant topic changes during the user's absence | The conversation has shifted ≥ 3 times in topics. | The conversation remains on a single topic or has very few changes. |

You can retrieve a summary message by calling `summarizeMessages` on a channel (`BaseChannel`):

```kotlin
channel?.summarizeMessages(isSentViaWs = true) { message, e ->
    // Handle the summary message result.
}
```

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| isSentViaWs | boolean | A flag that determines whether the summary message request will be sent via WebSocket (`true`) or not (`false`). If set to `true`, the request and the generated summary message will be delivered through the WebSocket connection. If set to `false`, the request will be handled via HTTP REST API. |

#### Event handler for message summarizing

Once a summary is created or not, the `onSummaryStatusChanged()` event handler is invoked. The method returns a `SummaryStatus` object containing the status of the summary message. You can use it to handle loading while waiting for the summary message to finish.

```kotlin
override fun onSummaryStatusChanged(channel: BaseChannel, status: SummaryStatus) {
    // Handle loading state of summary
}
```
