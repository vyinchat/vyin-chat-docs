# Vyin Chat SDK for Android - Channel

## Overview

Vyin Chat's APIs and SDKs provide two basic types of channels: open channel and group channel. Each type of channel is designed to support a wide variety of use cases that your business requires. This page explains the differences and characteristics of the two types.

Note: By default, the Allow creating open channels and Allow creating group channels options are turned on which means that open channels and group channels can be created by any user with Vyin Chat SDK. This may grant access to unwanted data or operations, leading to potential security concerns. To manage your access control settings, you can turn on or off each option in Settings > Application > Security > Access control list on Vyin Chat Console.

---

## Open channel

An open channel is a Twitch-style public chat where millions of users can easily join without invitations. Open channels can be used in short-lived live events, such as streaming, news-feed type messaging to massive audience, or events that don't require a permanent membership.

### Choose a type of an open channel

With our Chat SDK for Android, you can use a variety of behavior related properties when creating different types of open channels.

#### Classic vs. Dynamic partitioning

Vyin Chat SDK now supports two operation systems for open channels, which are classic and dynamic partitioning. These two systems offer the same features and functions, except that dynamic partitioning allows open channels to accommodate a large number of users. Classic open channels are the original open channels Vyin Chat has been providing, and a single classic open channel can handle up to 1,000 participants. On the other hand, dynamic partitioning open channels are designed to accommodate much greater number of users than the classic open channels using subchannels where you can divide users evenly.

Note: Vyin Chat applications created before December 15, 2020 are using classic open channels. The classic open channels will be deprecated soon, but the classic channels created up until the deprecation date will remain and function as usual. After classic open channels have been deprecated, all Chat SDK users will be automatically migrated to the new dynamic partitioning system. If you wish to convert to dynamic partitioning open channels beforehand, visit this page and contact us on the Vyin Chat Console.

Learn more about how dynamic partitioning open channel operates in the channel overview page for Platform API.

#### Ephemeral vs. Persistent open channels

Messages in ephemeral open channels aren't saved on Vyin Chat's database. This means that old messages pushed out by new ones can't be retrieved as they are one-time data. On the other hand, messages in a persistent open channel are permanently stored in the database, which is the default.

---

## Group channel

A group channel is a chat that allows close interactions among a limited number of users. In order to join a group channel, an invitation from a channel member is required. Depending on how you implement the joining process at the application level, an invited user can automatically join a group channel or manually accept the invitation to join. Various properties can be leveraged to design different types of group channels that suit your use cases, such as Twitter-style 1-to-1 direct messaging, WhatsApp-style group chat, and more.

### Choose a type of a group channel

With our Chat SDK for Android, you can use a variety of behavior related properties when creating different types of group channels. You can create a group channel after configuring these properties.

#### Private vs. Public

A private group channel can be joined only by users that have accepted an invitation from an existing channel member by default. On the other hand, a public group channel can be joined by any user without an invitation, like an open channel.

A private channel can be used for 1-on-1 conversations, such as clinical consultations and Instagram-style Direct Messages, while a public channel for 1-to-N conversations, such as small group discussions among students.

#### 1-to-1 vs. 1-to-N

The `distinct` property determines whether to resume an old channel or to create a new one when someone attempts to create a channel with the same members. For a 1-to-1 chat, it is highly recommended that you turn on the `distinct` property to keep using the existing channel.

Let's say there is a distinct group channel with users A, B, and C. If you attempt to create a new channel with the same members, the existing channel is used. This is similar to Twitter-style 1-to-1 direct messaging. If the `distinct` property of the channel is set to `false`, a new channel is created.

Note: The default value of this property is `false`. When a new member is invited or an existing member leaves the channel, then the `distinct` property of the channel is automatically turned off as well.

#### Supergroup vs. Group

For occasions that demand engagement among a relatively high volume of members, you can create a Supergroup channel, an expanded version of a group channel. It can be used for midsize conferences or large group discussions, such as company-wide stand-ups.

When the `super` property is set to `true`, a Supergroup channel is created and up to tens of thousands members can gather in the channel. The maximum number of members for a Supergroup channel can be adjusted depending on your Chat subscription plan.

Note: When the `super` property is set to `true`, `distinct` can't be supported.

#### Ephemeral vs. Persistent group channels

Messages sent in an ephemeral group channel aren't saved on Vyin Chat's database. As such, old messages that are pushed out of a user's chat view due to new messages can't be retrieved. On the other hand, messages sent in a persistent group channel are stored permanently in the database by default.

---

## Supergroup channel

A Supergroup channel is an expanded version of a group channel, which can accommodate a massive number of members while serving the same functions as a group channel. The maximum number can stretch up to tens of thousands of members depending on your Vyin Chat plan. Because a Supergroup channel is built upon the same foundation as a group channel, Platform APIs and SDK functions for both channel types are identical.

Note: Supergroup and group channels are alike in terms of Platform API and SDK design, except for the `isSuper` property. The `isSuper` property indicates whether the channel is a Supergroup channel or a group channel.

### Use cases

Supergroup channels can be used for a wide range of occasions that demand real-time engagement among a large number of users. One of the common use cases for Supergroup channels is community building, ranging from public forums to private events with a large membership.

- Ask-me-anything events: Supergroup channels also work as a platform for users to seek advice from an influencer or expert, such as Reddit Chat.
- Gaming community: Another common set-up is gaming communities, where users gather to find players to team up with or exchange ideas and strategies to advance further in the game.
- Stand-ups: Supergroup channels are a great place for midsize conferences, such as an organization-wide stand-up.

Note: To learn about differences among open channels, group channels, and Supergroup channels, see Comparing channel types.

### Limitations

To secure stable operation and optimized user experience in a Supergroup channel, a few functions are subject to limits. Some of these limitations are the following.

- Visual interactions: Read and delivery receipts aren't supported in Supergroup channels to provide an optimal performance in Supergroup channels.
- Unread counts: Unread counts are marked up to 100 only. In the case they surpass 100, we suggest you display the counts as `99+` to avoid confusion.
- Push notifications: The way push notifications for Supergroup channels work is determined by two factors: the number of members in a channel and the online status of its member.
  - When there are less than 100 members: A push notification is sent for every message created in the channel.
  - When there are 100 members or more: A ten-minute time window is applied to Supergroup channels with more than 100 members. Once the number exceeds 100 members, a push notification for a new message is sent out to members and then the next push notification comes ten minutes after that.
  - When a user becomes active: If one of the Supergroup channel members enters the channel and reads a message, the user is recognized as active. Then only for the following one minute, the user receives push notifications for all messages sent within the time frame.

---

## Comparing channel types

The optimal use cases for each channel type can vary because different channels support different features. The following tables compare open channels, group channels, and Supergroup channels in terms of possible use cases and supported features.

### Use cases by channel type

Choose which channel type to use based on key factors such as the duration of a chat and the number of users participating in it. The following table lists possible use cases for each channel type.

| Type | Used for |
| :---- | :---- |
| `Open channel` | - Short-lived live events, such as concerts and streaming. - News-feed type messaging to massive audience, such as Twitter-style feed. - Events that don't require a permanent membership. |
| `Group channel` | - Private 1-on-1 conversations, such as clinical consultations and Instagram-style Direct Messages. - Public 1-to-N conversations, such as small group discussions among students. - Invitation-only chats with a handful group of users. |
| `Supergroup channel` | - Ask-me-anything type events with a large number of users. - Midsize conferences for regular updates, such as company-wide meetings. |

### Comparison of channel types

The following table compares the difference among two types of channels.

|  | Open channel | Group channel | Supergroup channel |
| :---- | :---- | :---- | :---- |
| Maximum number in a channel | Up to millions of participants | 100 members | Up to tens of thousands of members depending on your Vyin Chat plan. |
| Accessible by | Anyone within the application | Invited users only if private or anyone if public | Invited users only if private or anyone if public |
| Ephemeral messaging | Supported | Supported | Supported |
| Online presence | Supported | Supported | Supported |
| Last message | N/A | Supported | Supported |
| UIKit | Supported | Supported | Supported |
| Operators | Supported | Supported | Supported |
| Ban users | Supported | Supported | Supported |
| Mute users | Supported | Supported | Supported |
| Freeze channels | Supported | Supported | Supported |
| Delivery receipts | N/A | Supported | N/A |
| Read receipts | N/A | Supported | N/A |
| Unread counts | N/A | Supported | Supported \* Up to 100 unread counts are supported. |
| Typing indicators | N/A | Supported | Supported \* Up to 3 concurrent indicators are supported. |
| Mention others in message | Supported | Supported | Supported |
| Mention counts | N/A | Supported | Supported |
| Reactions | N/A | Supported | Supported |
| Spam flood protection | Supported | Supported | Supported |
| Chatbot interface | Supported | Supported | Supported |
| Smart throttling | Supported (Default: `true`) | Supported (Default: `false`) | Supported (Default: `false`) |
| Push notifications | N/A \* Push notifications for announcements only. | Supported \* Push notifications for every message sent. | Supported \* Refer to Limitations. |
| Get a channel with its participant list or member list | N/A | Supported | Supported \* Only ten members are retrieved as a preview. To get an entire list of members, use the list members API instead. |
| Pagination for participant list or member list | Supported | Supported | Supported |
| Order of channel list | - Chronological | - Chronological - Latest last message - Channel name - Metadata value | - Chronological - Latest last message - Channel name - Metadata value |

---

## Functionalities by topic

Users can interact with other users in a channel by sending, receiving, replying to messages, and more. The following is a list of functionalities that our Chat SDK provides.

#### Creating a channel

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Create a channel | Creates a channel where users can chat. | ✔ | ✔ |

#### Managing operators

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Register and remove operators in a channel | Adds and removes users as operators to a channel. | ✔ | ✔ |

#### Joining and leaving a channel

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Enter and exit an open channel | Users can enter and exit open channels to receive messages. | ✔ |  |
| Join and leave a group channel | Users can join and leave group channels to receive messages. |  | ✔ |

#### Retrieving channels

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Retrieve a list of channels | Retrieves a list of channels. | ✔ | ✔ |
| Retrieve a channel by URL | Retrieves a channel by its URL. | ✔ | ✔ |

#### Inviting users to a channel

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Invite users as members | Invites new users to a channel which is performed only by the members of a group channel. |  | ✔ |
| Accept or decline an invitation | Accepts or declines an invitation to join a group channel. |  | ✔ |

#### Searching channels

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Search open channels by name, URL, or custom types | Searches for specific open channels by using a keyword. | ✔ |  |
| Search group channels by name, URL, or other filters | Searches for specific group channels by using name, URL, or several types of filters. |  | ✔ |
| Filter channels by user IDs | Filters channels using user IDs. |  | ✔ |

#### Categorizing channels

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Categorize channels by custom type | Specifies a custom channel to subclassify channels. | ✔ | ✔ |

#### Managing channels

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Delete a channel | Deletes a channel which only operators can do. | ✔ | ✔ |
| Hide or archive a channel | Hides or archives a specific channel from a list of channels. |  | ✔ |
| Refresh all data related to a channel | Updates the user's channels with the latest information. |  | ✔ |

#### Moderating a channel

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Freeze and unfreeze a channel | Temporarily disables various functions of a channel. | ✔ | ✔ |

#### Managing channel metadata

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Manage channel metadata | Store additional information to a channel. You can create, retrieve, update and delete the additional information. | ✔ | ✔ |

---

## Creating a channel

### Create a channel

Two basic types of channels you can create with Vyin Chat SDK are open channels and group channels.

An open channel is ideal for use cases that don't require a permanent membership in the channel, such as short-lived live events and news-feed style messaging to a massive audience. On the other hand, a group channel is suitable for closer interactions among a limited number of users. To learn more about the different use cases and characteristics of open and group channels, see the channel overview page.

When creating a channel, you can append additional information like cover image, description, and URL by passing several arguments to the corresponding parameters. The channel URL can only contain numbers, letters, underscores, or hyphens, and the URL's length must be between 4 and 100 characters. If you don't specify the `channelUrl` property, a URL is automatically generated.

If you want to create and continue to use a single group channel for a 1-to-1 chat, set the `isDistinct` property of the channel to `true`. Otherwise, if the `isDistinct` property is set to `false`, multiple 1-to-1 channels, each with their own chat history and data, may be created for the same two users even if a channel already exists between them.

---

#### Open channel

You can create an open channel by passing a configured `OpenChannelCreateParams` object as an argument to the parameter in the `createChannel()` method like the following.

```kotlin
val listOfOperatorUserIds = listOf("Jeff")
val params = OpenChannelCreateParams().apply {
    name = NAME
    channelUrl = UNIQUE_CHANNEL_URL
    coverImage = COVER_IMAGE            // Or coverUrl = COVER_URL
    customType = CUSTOM_TYPE
    data = DATA
}

OpenChannel.createChannel(params) { channel, e ->
    if (e != null) {
        // Handle error.
    }

    if (channel != null) {
        // An open channel of the specified users is successfully created.
        // You can get the open channel's data from the result object of the callback handler.

        val channelUrl = channel.url
    }
    // ...
}
```

#### OpenChannelCreateParams

This table only contains properties shown in the code above. See the API reference for a complete list of properties.

| Property name | Type | Description |
| :---- | :---- | :---- |
| `name` | string | Specifies the channel topic or the name of the channel. |
| `channelUrl` | string | Specifies the URL of the channel. Only numbers, letters, underscores, and hyphens are allowed. The length must be between 4 and 100 characters. If the `channelUrl` property isn't specified, a URL is automatically generated. |
| `coverImage` | file | Uploads a file for the cover image of the channel. Alternatively, you can upload a URL for the cover image of the channel using the `coverURL` property. |
| `data` | string | Specifies additional channel information such as a long description of the channel or `JSON` formatted string. |

---

#### Group channel

You can create a group channel by passing a configured `GroupChannelCreateParams` object as an argument to the parameter in the `createChannel()` method like the following.

```kotlin
val params = GroupChannelCreateParams().apply {
    name = NAME
    coverUrl = COVER_IMAGE_URL
    userIds = USER_IDS
    isDistinct = IS_DISTINCT
    customType = CUSTOM_TYPE
}

GroupChannel.createChannel(params) { channel, e ->
    if (e != null) {
        // Handle error.
    }

    if (channel != null) {
        // A group channel with the specified configuration is successfully created.
        // You can get the group channel's data from the result object of the callback handler.

        val channelUrl = channel.url
    }
    // ...
}
```

#### GroupChannelCreateParams

This table only contains properties shown in the code above. See the API reference for a complete list of properties.

| Property name | Type | Description |
| :---- | :---- | :---- |
| `name` | string | Specifies the name of the channel. |
| `coverUrl` | string | Specifies the cover image URL of the channel. |
| `userIds` | list of strings | Specifies a list of one or more IDs of the users to invite to the channel. |
| `isDistinct` | boolean | Determines whether to reuse an existing channel or create a new channel. Setting this property to `true` returns a channel with the same users in `USER_IDS` or creates a new channel if no match is found. If set to `false`, the Vyin Chat server always creates a new channel with the specified combination of users as well as the channel custom type. |
| `customType` | string | Specifies the custom channel type which is used for channel grouping. |

Note: You can also use the Chat API to create open channels and group channels. For group channels, using the Chat API helps you control channel member invitations on your server side.

---

#### Supergroup channel

Creating a Supergroup channel follows the same process as creating a group channel with one exception of the `isSuper` property. The `GroupChannelCreateParams` class has the `isSuper` property that determines whether a newly created channel is a Supergroup channel or a regular group channel. Set `isSuper` to `true` to create a new Supergroup channel.

Because the distinct option isn't available for Supergroup channels, the `isDistinct` property is set to `false` by default when creating a Supergroup channel.

```kotlin
val params = GroupChannelCreateParams().apply {
    userIds = USER_IDS
    isSuper = true
}

GroupChannel.createChannel(params) { channel, e ->
    if (e != null) {
        // Handle error.
    }

    // A group channel of the specified users is successfully created.
    // Through the groupChannel parameter of the callback handler,
    // you can get the group channel's data from the result object that
    // the Vyin Chat server has passed to the callback method.

    val channelUrl = channel?.url
}
```

---

## Managing operators

### Register and remove operators

Operators are users who can delete any messages and view all messages in an open or group channel without any filtering or throttling. Operators can moderate channels by muting or banning users as well as freezing channels.

You can register users as an operator by providing their user IDs as shown below.

```kotlin
channel.addOperators(USER_IDS) { e ->
    if (e != null) {
        // Handle error.
    }

    // The users are successfully registered as operators of the channel.
}
```

You can remove the users from being operators but leave them in the channel using the following code.

```kotlin
channel.removeOperators(USER_IDS) { e ->
    if (e != null) {
        // Handle error.
    }

    // The specified operators are removed.
    // You can notify the users of the role change through a prompt.
}
```

If you want to cancel the registration of all operators in a channel at once, use the following code.

```kotlin
channel.removeAllOperators { e ->
    if (e != null) {
        // Handle error.
    }

    // All operators are removed.
    // You can notify the users of the role change through a prompt.
}
```

---

## Joining and leaving a channel

### Enter and exit an open channel

A user can enter up to ten open channels and only receives messages from the entered channels. If the user is disconnected from the Vyin Chat server with the `disconnect()` method, they no longer receive messages. To continue receiving messages from the user's participating open channels, they have to be reconnected to the server with the `connect()` method and re-enter the open channels.

If the client app is in the background, the user is disconnected from the Vyin Chat server. However, when the client app runs in the foreground again, the Chat SDK automatically reconnects the open channels the user has entered. When a user temporarily loses connection to the Vyin Chat server due to an unstable connection, the `GIMChat` instance attempts to reconnect the user and the open channels the user has entered.

```kotlin
OpenChannel.getChannel(CHANNEL_URL) { channel, e ->
    if (e != null) {
        // Handle error.
    }

    // Invoke the callback handler.
    channel?.enter { enterError ->
        if (enterError != null) {
            // Handle error.
        }

        // The current user successfully enters the open channel
        // and can chat with other users in the channel by using APIs.
    }
}
```

A user can exit open channels as shown below. After exiting, the user can no longer receive messages from the channel.

```kotlin
OpenChannel.getChannel(CHANNEL_URL) { channel, e ->
    if (e != null) {
        // Handle error.
    }

    // Invoke the callback handler.
    channel?.exit { exitError ->
        if (exitError != null) {
            // Handle error.
        }

        // The current user successfully exits the open channel.
    }
}
```

---

### Join and leave a group channel

By default, an invitation is required to join group channels. However, any user can join public group channels as a member without invitations as shown below. Users can join up to 2,000 group channels.

```kotlin
if (groupChannel.isPublic) {
    groupChannel.join { e ->
        if (e != null) {
            // Handle error.
        }

        // The current user successfully joins the group channel.
    }
}
```

A user can leave group channels as shown below. After leaving, the user can't receive messages from the channel, and this method can't be called for deactivated users.

If the user is the channel's operator, you can remove the user from the channel's operator list by setting the `shouldRemoveOperatorStatus` parameter to `true`.

```kotlin
groupChannel.leave { e ->
    if (e != null) {
        // Handle error.
    }
}
```

---

## Retrieving channels

### Retrieve a list of channels

You can retrieve a list of `OpenChannel` objects using the `next()` method of an `OpenChannelListQuery` instance.

On the other hand, a list of `GroupChannel` objects can be retrieved through `GroupChannelCollection` and its `loadMore()` method. First, create a `GroupChannelListQuery` instance. The `GroupChannelListQuery` instance returns both public and private group channels that the current user has joined. To retrieve a list of certain group channels such as public group channels or Supergroup channels, use the list query's filters as described at the bottom of this page.

Note: You can also search for specific open channels and group channels with keywords and filters.

---

#### Open channels

Create an `OpenChannelListQuery` instance to retrieve a list of open channels matching the specifications set by `OpenChannelListQueryParams`. After a list of open channels is successfully retrieved, you can access the data of each open channel from the result list through the `channels` parameter of the callback handler.

```kotlin
val query = OpenChannel.createOpenChannelListQuery(OpenChannelListQueryParams())
query.next { channels, e ->
    if (e != null) {
        // Handle error.
    }
}
```

---

#### Group channels

A group channel list view can be drawn with a `GroupChannelCollection` instance. First, create a `GroupChannelListQuery` instance through the `createMyGroupChannelListQuery()` method and its query setters. This determines which channel to include in the channel list and how to list channels in order.

To retrieve group channels in the collection, call `hasMore` first to check whether there are more channels to load for the collection. If so, call `loadMore()`.

To learn more about how the collection works, see Group channel collection under Local caching.

```kotlin
// Call hasMore first to check if there are more channels to load.
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

### Filter group channels using GroupChannelListQuery

When creating a `GroupChannelCollection` instance, you need to set params within `GroupChannelListQuery` first. The params in `GroupChannelListQuery` works much like other search options, retrieving a list of group channels matching the specifications set by `GroupChannelListQueryParams`. For example, use `myMemberStateFilter` when searching for only the group channels that the current user belongs to.

The default value of `limit` in `GroupChannelListQueryParams` is **10**.

Once the params is set, you can use the query instance for `GroupChannelCollectionCreateParams` in `createGroupChannelCollection()`.

```kotlin
// Create a GroupChannelListQuery instance in GroupChannelCollection.
val query = GroupChannel.createMyGroupChannelListQuery(
    GroupChannelListQueryParams().apply {
        includeEmpty = true
        order = GroupChannelListQueryOrder.CHRONOLOGICAL
    }
)
```

---

### Retrieve a channel by URL

Since a channel URL is a unique identifier of a channel, you can use a URL when retrieving a channel object. We recommend that you store a user's channel URLs to handle the lifecycle or state changes of your app as well as any other unexpected situations. For example, when a user is disconnected from the Vyin Chat server by temporarily switching to another app, you can provide a smooth restoration of the user's state once the user has returned to your app by using the stored URL to fetch the appropriate channel instance.

#### Open channel

```kotlin
OpenChannel.getChannel(CHANNEL_URL) { channel, e ->
    if (e != null) {
        // Handle error.
    }

    // Through the channel parameter of the callback handler,
    // the open channel object identified with CHANNEL_URL is returned by the Vyin Chat server,
    // and you can get the open channel's data from the result object.
}
```

#### Group channel

```kotlin
GroupChannel.getChannel(CHANNEL_URL) { channel, e ->
    if (e != null) {
        // Handle error.
    }

    // Through the channel parameter of the callback handler,
    // the group channel object identified with CHANNEL_URL is returned by the Vyin Chat server,
    // and you can get the group channel's data from the result object.
}
```

---

## Inviting users to a group channel

### Invite users as members

To enter a private group channel, a user must be invited by members who are already in the group channel. On the other hand, an invitation isn't required to join a public group channel.

```kotlin
val userIds: List<String> = listOf("Tyler", "Young")
groupChannel.invite(userIds) { e ->
    if (e != null) {
        // Handle error.
    }
    // ...
}
```

You can also determine whether the newly invited user can see past messages in the channel or not. You can manage the settings on Vyin Chat Console. Go to Settings > Chat > Channels > Group channels, and you will see the Chat history option. If the option is turned on, newly joined members can view all messages sent before they joined the channel. If turned off, new members can only see messages sent after they were invited. By default, this option is turned on.

---

### Accept or decline an invitation

A user who is invited to a group channel can accept or decline the invitation. If a user accepts an invitation, they join the channel as a new member and can start chatting with other members. If the user declines the invitation, the invitation is no longer valid.

Users can join up to 2,000 group channels. When the number of group channels a user can join reaches the maximum number, new invitations are automatically declined.

```kotlin
// Accept an invitation.
groupChannel.acceptInvitation { e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}

// Decline an invitation.
groupChannel.declineInvitation { e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

You can set the channel invitation preference for your Vyin Chat application. The preference determines whether a user can automatically accept a group channel invitation or manually accept invitations. If the value of `setChannelInvitationPreference()` is set to `true`, invitations will be automatically accepted. If the value is `false`, users can either accept or decline invitations. By default, the value is set to `true`.

```kotlin
// The default value of true means that
// a user is automatically added to a group channel
// even if the user hasn't accepted the invitation.
val autoAccept = false
GIMChat.setChannelInvitationPreference(autoAccept) { e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

If the client app is in the foreground, the members of the group channel are notified of whether the newly invited user has accepted or declined the invitation. To do so, implement the `onUserReceivedInvitation()` and `onUserDeclinedInvitation()` handlers of a channel event handler. For more information, see the event handler page.

---

## Searching channels

### Search open channels by name, URL, or custom type

You can search for specific open channels by adding keywords to an `OpenChannelListQuery` instance. There are two types of keywords, which are name and URL.

The code sample below shows the query instance which returns a list of open channels that partially match the specified `nameKeyword` in their channel names.

```kotlin
val query = OpenChannel.createOpenChannelListQuery(
    OpenChannelListQueryParams().apply {
        nameKeyword = "myChannel"
    }
)
query.next { channels, e ->
    if (e != null) {
        // Handle error.
    }

    // Through the channels parameter of the callback handler, which the Vyin Chat server has passed a result list to,
    // a list of open channels whose name partially matches the filter value is returned.
    // ...
}
```

The following shows the query instance which returns a list of open channels that partially match the specified `urlKeyword` in their channel URLs.

```kotlin
val query = OpenChannel.createOpenChannelListQuery(
    OpenChannelListQueryParams().apply {
        urlKeyword = "seminar"
    }
)
query.next { channels, e ->
    if (e != null) {
        // Handle error.
    }

    // Through the channels parameter of the callback handler,
    // which the Vyin Chat server has passed a result list to,
    // a list of open channels whose URL partially matches the filter value is returned.
    // ...
}
```

You can also search for open channels with a specific custom type by setting `customTypeFilter` as in the following code.

```kotlin
val query = OpenChannel.createOpenChannelListQuery(
    OpenChannelListQueryParams().apply {
        customTypeFilter = "movie"
    }
)
query.next { channels, e ->
    if (e != null) {
        // Handle error.
    }

    // Through the channels parameter of the callback handler,
    // which the Vyin Chat server has passed a result list to,
    // a list of open channels whose custom type matches the filter value is returned.
    // ...
}
```

---

### Search group channels by name, URL, or other filters

You can search for specific group channels by using several types of search filters within the `GroupChannelListQuery` class.

#### Search private group channels

A `GroupChannelListQuery` instance provides many types of search filters such as `channelNameContainsFilter`. You can use these filters to search for specific private group channels.

The sample code below shows the query instance, which returns a list of the current user's group channels that partially match the specified keyword in `channelNameContainsFilter` in their channel names.

```kotlin
val query = GroupChannel.createMyGroupChannelListQuery(
    GroupChannelListQueryParams().apply {
        includeEmpty = true
        channelNameContainsFilter = "myChannel"
    }
)
query.next { channels, e ->
    if (e != null) {
        // Handle error.
    }

    // Through the channels parameter of the callback method
    // which the Vyin Chat server has passed a result list to,
    // a list of group channels that have "myChannel"
    // in their names is returned.
}
```

The following table shows all filters supported in `GroupChannelListQuery` to search for specific channels you want to retrieve. You can use any of the filters in a similar fashion with the sample code above.

#### List of filters

| Name | Filters |
| :---- | :---- |
| `customTypesFilter` | Group channels with one or more specified custom types. You can enable this filter using the `customTypesFilter` property. |
| `customTypeStartsWithFilter` | Group channels with a custom type that starts with the specified value. You can enable this filter using the `customTypeStartsWithFilter` property. |
| `channelNameContainsFilter` | Group channels that contain the specified value in their names. You can enable this filter using the `channelNameContainsFilter` property. |
| `channelNameStartsWithFilter` | Group channels that start with the specified value in their names. You can enable this filter using the `channelNameStartsWithFilter` property. |
| `channelNameExactMatchFilter` | Group channels that exactly match the specified value in their names. You can enable this filter using the `channelNameExactMatchFilter` property. |
| `unreadChannelFilter` | Group channels with one or more unread messages. Using the `unreadChannelFilter` property, you can enable this filter. |
| `hiddenChannelFilter` | Group channels with the specified state and operating behavior. You can enable this filter using the `hiddenChannelFilter` property. |
| `myMemberStateFilter` | Group channels based on whether the user has accepted an invitation. You can enable this filter using the `myMemberStateFilter` property. |

---

### Filter group channels by user IDs

To filter group channels by user IDs, use the `userIdsExactFilter` or `userIdsIncludeFilter` filter. Let's assume the ID of the current user is Harry and the user is a member of two group channels.

- Channel A consists of Harry, John, and Jay.
- Channel B consists of Harry, John, Jay, and Jin.

A `userIdsExactFilter` filter returns a list of the current user's group channels containing exactly the queried user IDs. In case you specify only one user ID in the filter, the filter returns a list of the current user's one or more 1-to-1 group channels with the specified user.

```kotlin
val query = GroupChannel.createMyGroupChannelListQuery(
    GroupChannelListQueryParams().apply {
        userIdsExactFilter = listOf("John", "Jay")
    }
)
query.next { channels, e ->
    // Only Channel A is returned in a result list
    // through the list parameter of the callback method.
}
```

The `userIdsIncludeFilter` filter returns a list of the current user's group channels including the queried user IDs partially and exactly. Two different results can be returned according to the value of the `queryType` parameter.

```kotlin
val query = GroupChannel.createMyGroupChannelListQuery(
    GroupChannelListQueryParams().apply {
        setUserIdsIncludeFilter(listOf("John", "Jay", "Jin"), QueryType.AND)
    }
)
query.next { channels, e ->
    // Only Channel B containing {'John', 'Jay', 'Jin'} as a
    // subset is returned in a result list.
}
```

```kotlin
val query = GroupChannel.createMyGroupChannelListQuery(
    GroupChannelListQueryParams().apply {
        setUserIdsIncludeFilter(listOf("John", "Jay", "Jin"), QueryType.OR)
    }
)
query.next { channels, e ->
    // Channel A and Channel B both have {'John'}, Channel A and Channel B both have {'Jay'},
    // and Channel B has {'Jin'}.
    // Both Channel A and Channel B are returned in a result list.
}
```

---

## Categorizing channels

### Categorize channels by custom type

When creating an open channel or a group channel, you can additionally specify a custom channel type to subclassify channels. This custom type takes on the form of `String`, and can be useful in searching or filtering channels.

The `data` and `customType` properties of a channel object allow you to append information to channels. While both properties can be used flexibly, common examples for `customType` include categorizing channels as "School" or "Work".

#### Open channel

To get an open channel's custom type, read the `openChannel.customType` property.

```kotlin
val params = OpenChannelCreateParams().apply {
    name = NAME
    coverUrl = COVER_IMAGE_URL
    data = DATA
    operators = OPERATOR_USERS
    customType = CUSTOM_TYPE
}
OpenChannel.createChannel(params) { channel, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

#### Group channel

To get a group channel's custom type, read the `groupChannel.customType` property.

```kotlin
val users = listOf("Tyler", "Nathan")
val params = GroupChannelCreateParams().apply {
    userIds = users
    name = NAME
    customType = CUSTOM_TYPE
}
GroupChannel.createChannel(params) { channel, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

---

## Managing channels

### Delete a channel

Only channel operators are allowed to delete a channel. To delete a channel, follow the code below.

#### Open channel

```kotlin
openChannel.delete { e ->
    if (e != null) {
        // Handle error.
    }

    // The channel is successfully deleted from the client app.
}
```

#### Group channel

```kotlin
groupChannel.delete { e ->
    if (e != null) {
        // Handle error.
        return@delete
    }
    // The channel is successfully deleted from the client app.
}
```

---

### Hide or archive a group channel

You can hide or archive a specific group channel from the channel list UI by following the code below.

```kotlin
// Hide or archive a group channel.
groupChannel.hide(hidePreviousMessages, allowAutoUnhide) { e ->
    if (e != null) {
        // Handle error.
    }

    // The channel is successfully hidden from the list.
    // The current user's channel view should be refreshed to reflect the change.
    // ...
}

// Unhide a group channel.
groupChannel.unhide { e ->
    if (e != null) {
        // Handle error.
    }

    // The channel is successfully unhidden from the list.
    // The current user's channel view should be refreshed to reflect the change.
}
```

#### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| `hidePreviousMessages` | boolean | Determines whether to show the messages sent and received before hiding or archiving the channel on the channel list. If set to `true`, previous messages aren't displayed in the channel. (Default: `false`) |
| `allowAutoUnhide` | boolean | Determines the state and operating behavior of a channel. If set to `true`, the channel is hidden from the channel list, but when a new message arrives, the hidden channel will show up again on the channel list. If set to `false`, the channel is archived and stays hidden from the channel list unless the `unhide()` method is called. |

You can check the channel state on the channel list by using the `hiddenState` property of a `GroupChannel` object.

```kotlin
when (groupChannel.hiddenState) {
    HiddenState.UNHIDDEN -> {
        // Show the channel in the list.
    }
    HiddenState.HIDDEN_ALLOW_AUTO_UNHIDE -> {
        // Hide the channel from the list, and get it appeared back on condition.
    }
    HiddenState.HIDDEN_PREVENT_AUTO_UNHIDE -> {
        // Archive the channel, and get it appeared back in the list only when unhide() is called.
    }
}
```

You can also filter channels by their state by following the code below.

```kotlin
val query = GroupChannel.createMyGroupChannelListQuery(
    GroupChannelListQueryParams().apply {
        includeEmpty = true
        // The filter options are ALL, UNHIDDEN, HIDDEN, HIDDEN_ALLOW_AUTO_UNHIDE, and HIDDEN_PREVENT_AUTO_UNHIDE.
        // If set to HIDDEN, hidden and archived channels are returned.
        hiddenChannelFilter = HiddenChannelFilter.HIDDEN_ALLOW_AUTO_UNHIDE
    }
)
query.next { channels, e ->
    // Only archived channels are returned in a result list
    // through the channels parameter of the callback handler.
}
```

---

### Refresh all data related to a group channel

When a user is online, all data associated with the group channels they are a member of are automatically updated. However, when a user is disconnected from the Vyin Chat server and reconnects later, the `refresh()` method should be called to update the channels with the latest information.

```kotlin
groupChannel.refresh { e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

Note: If you want to provide the updated channels with the latest information when your user's app is in the foreground, call `refresh()` in the `onReconnectSucceeded()` method which receives a callback from the Vyin Chat server when successfully reconnected.

---

## Moderating a channel

### Freeze and unfreeze a channel

Open channels and group channels can be frozen and unfrozen. When a channel is frozen, only the operators can send messages to the channel and users aren't allowed to chat.

An open channel can be frozen only by using Chat API or on Vyin Chat Console. To freeze an open channel, go to Chat > Open channels on the console, select an open channel you want to freeze, and click the Freeze icon at the upper right corner. To unfreeze, select the frozen channel and click the Freeze icon again.

Group channels can be frozen by operators using the Chat SDK as shown in the code below.

```kotlin
// Freezes a channel.
groupChannel.freeze { e ->
    if (e != null) {
        // Handle error.
    }

    // The channel is frozen.
    // You can send a message to announce that
    // chatting in the channel is unavailable,
    // or do something in response to a successful operation.
}

// Unfreezes a channel.
groupChannel.unfreeze { e ->
    if (e != null) {
        // Handle error.
    }

    // The channel is unfrozen.
    // You can send a message to announce that
    // chatting in the channel is available,
    // or do something in response to a successful operation.
}
```

---

## Managing channel metadata

You can store additional information to channels such as background color or channel description with channel metadata, which can be fetched or rendered into the UI. Channel metadata is `Map<String, String>` and it can be stored into a `Channel` object.

To receive and retrieve information about events happening in channels from the Vyin Chat server, you can add a channel event handler. For more information on channel event handlers, see Channel event types.

### Create metadata

To store channel metadata into a `Channel` object, create a new object of key-value items in which the data type of the key and value is `Map<String, String>`. Then, pass the object as an argument to a parameter when calling the `createMetaData()` method. You can put multiple key-value items in the dictionary.

```kotlin
val data = mapOf("key1" to "value1", "key2" to "value2")
channel.createMetaData(data) { map, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

### Update metadata

The process of updating channel metadata is the same as creating one. Values of existing keys can be updated and values of new keys can be added by calling the `updateMetaData()` method.

```kotlin
val data = mapOf(
    "key1" to "valueToUpdate1", // Update an existing item with a new value.
    "key2" to "valueToUpdate2", // Update an existing item with a new value.
    "key3" to "valueToAdd3"     // Add a new key-value item.
)
channel.updateMetaData(data) { map, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

### Retrieve metadata

You can retrieve channel metadata by creating a `List` of keys to retrieve and passing it as an argument to a parameter in the `getMetaData()` method. A `Map<String, String>` collection will return through the `MetaDataHandler` callback function with corresponding key-value items.

```kotlin
val keys = listOf("key1", "key2")
channel.getMetaData(keys) { map, e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```

### Retrieve cached metadata

When Vyin Chat SDK detects any of the `create`, `read`, `update`, and `delete` operations on the channel metadata, the SDK caches the metadata. The cached metadata is also updated whenever a channel list is fetched.

You can retrieve the cached metadata through the `cachedMetaData` property without having to query the server.

```kotlin
val cachedMetaData = channel.cachedMetaData
val value = cachedMetaData[key] // The key should be a string.
```

### Delete metadata

You can delete channel metadata by calling the `deleteMetaData()` method.

```kotlin
channel.deleteMetaData("key1") { e ->
    if (e != null) {
        // Handle error.
    }

    // ...
}
```
