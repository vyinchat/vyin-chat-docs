# Vyin Chat SDK for iOS - User

## User

## Overview

Users can chat with one another by participating in open channels or joining group channels in a Vyin Chat application. The operator role can be assigned to certain users to moderate other users. Users are identified by their own unique ID, and may have a customized nickname and profile image. You can manage various user attributes and actions using the Chat API.

Note: By default, the Allow retrieving user list and Allow updating user metadata options are turned on which means that any user can retrieve a list of users and their metadata as well as alter other users' nicknames and their metadata within your application. This may grant access to unwanted data or operations, leading to potential security concerns. To manage your access control settings, you can turn on or off each option in Settings \> Application \> Security \> Access control list on Vyin Chat Console.

### User types

Depending on the type of channel your users are chatting in, they are given different labels as well as access to different actions and information. Users can register other users as friends, interact with them in a private chat, or block specific users from sending direct messages. In some cases, users can be given an operator role and they can moderate other users who engage in inappropriate activities.

| Type | Scope | Definition |
| :---- | :---- | :---- |
| `User` | Application | Refers to a user who can access all the chat features of a Vyin Chat application with their own unique ID but doesn't have any administrative privileges. |
| `Participant` | Open channels | Refers to a specific user who has entered an open channel without an invitation and is remaining online in the channel. Open channel participants are provided with limited information only. These participants can enter and exit the channel at all times. |
| `Member` | Group channels | Refers to a specific user who has joined a group channel through an invitation from a channel member or on their own. Relational information such as connection status, typing indicators, and read receipts is only available to the group channel members depending on the channel settings. |

#### Operator

You can assign operators in each channel to moderate participants or members engaging in inappropriate activities by banning or muting them in the channel.

When banned, participants or members are immediately kicked out of the channel. After the ban time set by the operators has expired, they can participate back in or rejoin the channel. On the other hand, muted participants or members are allowed to remain in the channel and view the messages, but can't send messages until the operators unmute them.

Additionally, operators can also delete or update any messages sent in a channel.

### Functionalities by topic

With the Chat SDK, you can retrieve users in a channel, moderate user activity, and manage user information. The following is a list of functionalities that our SDK supports.

#### Retrieving users

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Retrieve a list of users in an application | Retrieves a list of all or a specified subset of users in a Vyin Chat application. | ✓ | ✓ |
| Retrieve a list of users in a channel | Retrieves a list of users in a channel. | ✓ | ✓ |
| Retrieve a list of users and operators in a specific order | Retrieves a list of users and operators in a channel in an alphabetical order or by another specified order. |  | ✓ |
| Retrieve users who have read a message | Retrieves users who have read a specific message in a channel. |  | ✓ |
| Retrieve a list of operators | Retrieves a list of operators who monitor and control the activities in a channel. | ✓ | ✓ |

#### Moderating a user

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Retrieve a list of blocked users | Retrieves a list of all or a specified subset of blocked users in a Vyin Chat application. |  | ✓ |
| Retrieve a list of banned users | Retrieves a list of users who are banned from a channel. | ✓ | ✓ |
| Retrieve a list of muted users | Retrieves a list of users who are muted in a channel. | ✓ | ✓ |
| Block and unblock other users | Blocks or unblocks specified users for a user in a channel. |  | ✓ |
| Ban and unban a user | Operators can ban or unban users from a channel. Banned users are immediately expelled from a channel and allowed to participate in the channel again after the time period set by the operators has passed. | ✓ | ✓ |
| Mute and unmute a user | Operators can mute or unmute users in a channel. Muted users remain in the channel and are allowed to view the messages, but can't send any messages until the operators unmute them. | ✓ | ✓ |

#### Retrieving and updating user information

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Retrieve the online status of a user | Checks if a certain user in a Vyin Chat application is currently connected to the Vyin Chat server. | ✓ | ✓ |
| Update user profile | Updates a user's nickname and profile image with a URL. | ✓ | ✓ |
| Retrieve the latest information on users | Retrieves the latest and updated information on each user who is online. | ✓ |  |

#### Managing user metadata

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Manage user metadata | Stores additional information to users. You can create, retrieve, update and delete additional information. | ✓ | ✓ |

## Retrieving users

## Retrieve a list of users in an application

You can retrieve a list of all or certain users in your Vyin Chat application using an `ApplicationUserListQuery` instance. The `loadNextPage()` method returns a list of `User` objects which contain information on users within the application.

```swift
// Retrieve all users.
let query = GIMChat.createApplicationUserListQuery(ApplicationUserListQueryParams())
query.loadNextPage { queryResult, e in
    if (e != nil) {
        // Handle error.
    }

    // A list of users is successfully retrieved.
    // Through the list parameter of the callback handler,
    // you can access the data of each user from the result list
    // that the Vyin Chat server has passed to the callback handler.
    // ...
}
```

### ApplicationUserListQuery

The `ApplicationUserListQueryParams` object is passed to the `createApplicationUserListQuery(params:)` method to make the `ApplicationUserListQuery` instance. Using a number of different parameters in the `ApplicationUserListQuery` instance, you can retrieve a list of specific users that match the set values in the parameters. Currently, the `ApplicationUserListQuery` instance supports the following parameters.

#### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| `userIdsFilter` | [String] | Specifies the unique IDs of users you want to retrieve in the filter. |
| `metaDataFilter` | (String, [String]) | Specifies a metadata key and values pair to retrieve users with metadata containing an item with the specified key and values. The `metaDataFilter` property must be turned on to use the filter. |
| `nicknameStartsWithFilter` | String | Specifies the nickname of users you want to retrieve in the filter. This filter can be used with the `nicknameStartsWithFilter` property. |

```swift
// Retrieve certain users using the UserID filter.
let userIds = ["Harry", "Jay", "Jin"]
let query = GIMChat.createApplicationUserListQuery(
    ApplicationUserListQueryParams(builder: { params in
        params.userIdsFilter = userIds
    })
)
query.loadNextPage { users, e in
    if (e != nil) {
        // Handle error.
    }

    // A list of matching users is successfully retrieved.
    // ...
}

// Retrieve certain users using the MetaDataKey filter.
let metaDataValues = ["movie", "book", "exercise"]
let query = GIMChat.createApplicationUserListQuery(
    ApplicationUserListQueryParams(builder: { params in
        params.metaDataFilter = "hobby" to metaDataValues
    })
)
query.loadNextPage { users, e in
    if (e != nil) {
        // Handle error.
    }

    // A list of matching users is successfully retrieved.
    // ...
}
```

## Retrieve a list of users in a channel

You can retrieve a list of participants who are currently online and receiving all messages from an open channel using the `createParticipantListQuery()` method. To retrieve a list of members in a group channel, call the `members` property.

### Open channel

```swift
let query = channel.createParticipantListQuery()
query.loadNextPage { users, e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```

#### Group channel

Members of a group channel are automatically updated when a user is online. But when a user is disconnected from the Vyin Chat server and then reconnected, you should call the `refresh()` method to update their channels with the latest information. See the refresh all data related to a group channel section for the sample code.

```swift
let members = channel.members
```

## Retrieve a list of members and operators in a spec

## Retrieve a list of members and operators in a specific order

The members and operators of a group channel can be retrieved by calling the `loadNextPage()` method of a `MemberListQuery` instance.

### MemberListQueryParams.Order

For a specific order, set one of the values in the following table to the `order` property of `MemberListQueryParams`.

| Value | Description |
| :---- | :---- |
| `MEMBER_NICKNAME_ALPHABETICAL` | Members are arranged in an alphabetical order. This is the default value. |
| `OPERATOR_THEN_MEMBER_ALPHABETICAL` | Operators are listed first, then the members, both in an alphabetical order. |

```swift
let query = groupChannel.createMemberListQuery(
    paramsBuilder: { params in
        params.limit = 10
        params.order = MemberListQuery.Order.OPERATOR_THEN_MEMBER_ALPHABETICAL
    }
)
query.loadNextPage { members, e in
    if (e != nil) {
        // Handle error.
    }

    // A list of matching members and operators is successfully retrieved.
    // Through the members parameter of the callback method,
    // you can access the data of each item from the result list that the Vyin Chat server has passed to the callback method.
}
```

#### MemberListQueryParams.OperatorFilter

For a specific order, set one of the values in the following table to the `operatorFilter` property of `MemberListQueryParams`.

| Value | Description |
| :---- | :---- |
| `all` | No filter is applied to the group channel list. This is the default value. |
| `operator` | Only operators are retrieved in the list. |
| `nonOperator` | All members, except for operators, are retrieved in the list. |

```swift
let query = groupChannel.createMemberListQuery(
    paramsBuilder: { params in
        params.limit = 10
        params.operatorFilter = OperatorFilter.OPERATOR // ALL, OPERATOR, and NONOPERATOR
    }
)
query.loadNextPage { members, e in
    if (e != nil) {
        // Handle error.
    }

    // A list of matching members and operators is successfully retrieved.
    // Through the members parameter of the callback method,
    // you can access the data of each item from the result list that the Vyin Chat server has passed to the callback method.
}
```

## Retrieve members who have read a message

Using the `getReadMembers()` method, you can view members who have read a specific message in a group channel. The method returns a list of channel members who have read the message by comparing the message’s creation time and the channel members’ read receipt. The list will exclude the current user and the message sender.

If you want to keep track of who has read a new message, we recommend that you use the `getReadMembers()` method in `updateChannel(_ channel: GroupChannel, context: MessageContext)` of the channel event delegate. Then the client app will receive a callback from the Vyin Chat server whenever a channel member reads a message. To do so, you should pass the new message object as an argument to a parameter in the `getReadMembers()` method through `updateChannel(_ channel: GroupChannel, context: MessageContext)`.

```swift
let readMembers = groupChannel.getReadMembers(message, false)
```

Note: Using the `getUnreadMembers()` method, you can also view members who haven't read a specific message in a group channel, except the current user and the message sender. In the meantime, you can get information on each channel member's read receipt through the `getReadStatus()` method.

## Retrieve a list of operators

You can follow the simple implementation below to retrieve a list of operators who monitor and control the activities in a channel.

```swift
 OpenChannel.getChannel(CHANNEL_URL) { channel, e in
    if (e != nil) {
        // Handle error.
    }

    // Retrieve a list of operators.
    channel.operators.forEach { operator in
        // ...
    }
}
```

## Moderating a user

## Retrieve a list of blocked users

By creating and using a `BlockedUserListQuery` instance, you can retrieve a list of all or specific blocked users in your Vyin Chat application. The `loadNextPage()` method returns a list of `User` objects that contain information about the blocked users.

```swift
 // Retrieve all blocked users.
let query = GIMChat.createBlockedUserListQuery(BlockedUserListQueryParams())
query.loadNextPage { users, e in
    if (e != nil) {
        // Handle error.
    }

    // A list of blocked users is successfully retrieved.
    // Through the list parameter of the callback handler,
    // you can access the data of each blocked user from the result list
    // that the Vyin Chat server has passed to the callback handler.
    // ...
}
```

Using the `userIds` filter of the `BlockedUserListQuery` instance, you can retrieve a list of blocked users with the specified user IDs.

```swift
// Retrieve blocked users with userIds specified in the filter.
let userIds = ["John", "Daniel", "Jeff"]
let query = GIMChat.createBlockedUserListQuery(
    BlockedUserListQueryParams(builder: { params in
        params.userIdsFilter = userIds
    })
)
query.loadNextPage { users, e in
    if (e != nil) {
        // Handle error.
    }

    // A list of matching users is successfully retrieved.
    // ...
}
```

## Retrieve a list of banned users

You can create a query to retrieve a list of banned users in both open and group channels. Only users registered as channel operators may use the following code.

```swift
 // Retrieve banned users.
let query = channel.createBannedUserListQuery()
query.loadNextPage { restrictedUsers, e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```

## Retrieve a list of muted users

You can create a query to retrieve a list of muted users in both open and group channels. Only users registered as channel operators may use the following code.

### Open channel

```swift
// Retrieve muted participants.
let query = channel.createMutedUserListQuery()
query.loadNextPage { restrictedUsers, e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```

#### Group channel

```swift
// Retrieve muted members.
let query = groupChannel.createMemberListQuery(
    MemberListQueryParams(builder: { params in
        params.mutedMemberFilter = MutedMemberFilter.MUTED
    })
)
query.loadNextPage { members, e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```

## Block and unblock other members

A user can block another user if the user doesn't wish to receive any messages or notifications from the blocked user in a 1-to-1 group channel. In a 1-to-N group channel, the user may receive messages from the blocked user, depending on the UI settings of the chat view. In any case, notifications from the blocked user won't be delivered to the 1-to-N group channel. You can also choose whether or not to allow the user to view which users are blocked in the channel UI.

### Block modes

Vyin Chat application provides two blocking options: including or excluding blocked users when sending invitations, and turning on or off notifications from blocked users. Explicit and classic block modes have been deprecated and are only supported for customers who started using them before they were deprecated.

* **Including or excluding blocked users when sending invitations:** Determines whether or not to automatically filter out blocked users when a user invites a group of users to a new group channel. By default, blocked users are included when sending invitations. The value of this option can be changed by Vyin Chat if your Vyin Chat application isn't integrated to the client app. If you want to change the value, contact our sales team.  
* **Turning on or off notifications from blocked users:** Determines whether or not to receive message notifications from the blocked user in a specific 1-to-N group channel where they are both members. By default, a user doesn't receive notifications from blocked users. The value of this option can be set to individual channels. If you want to use this option, contact our sales team.

The following table explains what happens to a user's chat experience when the user blocks another user in a 1-to-1 or 1-to-N group channel. In the case of a 1-to-1 group channel, the block mode is only maintained with the original members. If users other than the original members are added, the rules for 1-to-N group channel begin to apply.

#### Block consequences by channel type

| Channel type | Channel list | Notifications | Messages |
| :---- | :---- | :---- | :---- |
| 1-to-1 group channel | A user's channel list isn't updated in response to the blocked user's messages. | A user can't receive notifications for the messages from the blocked user. | A user can only see the messages that the blocked user has sent before being blocked. Even though messages sent from the blocked user aren't delivered to the channel, they are saved in the database and only displayed in the blocked user's channel view. This means that the blocked user isn't aware of their blocked status.\* If the blocked user is unblocked, a user can see all the messages from the blocked user except those that were sent during the blocking period. |
| 1-to-N group channel | A user's channel list is updated in response to a blocked user's message. | By default, a user can't receive message notifications from a blocked user. If the option is turned on, the user can receive message notifications from the blocked user. | All messages from blocked users are delivered to the channel. You can choose whether or not a user can view the messages from the blocked users in the channel UI. |

### How to use block modes

You can let users block and unblock other users by implementing the following code in your client app.

```swift
// Block a user.
GIMChat.blockUser(USER) { user, e in
    if (e != nil) {
        // Handle error.
    }

    // The blocked user can be retrieved through the user parameter of the onBlocked() callback method.
    // ...
}

// Unblock a user.
GIMChat.unblockUser(USER) { e in
    if (e != nil) {
        // Handle error.
    }

    // The user is successfully unblocked.
    // ...
}
```

## Ban and unban a user

Operators of an open or group channel can remove any users that behave inappropriately in the channel using the ban feature. A banned user is immediately kicked out of the channel, but allowed to participate in the channel again after a set period of time has passed. Operators can ban and unban users using the following code.

### Open channel

```swift
OpenChannel.getChannel(CHANNEL_URL) { openChannel, e in
    if (e != nil) {
        // Handle error.
    }

    if (openChannel?.isOperator(GIMChat.currentUser) == true) {
        // Ban a user.
        openChannel?.banUser(USER, seconds: SECONDS, description: DESCRIPTION) { e in
            if (e != nil) {
                // Handle error.
            }

            // The user is successfully banned from the channel.
            // You can display a message to let the user know they have been banned.
            // ...
        }

        // Unban a user.
        openChannel?.unbanUser(USER) { e in
            if (e != nil) {
                // Handle error.
            }

            // The user is successfully unbanned.
            // You can display a message to let the user know they have been unbanned.
            // ...
        }
    }
}
```

#### Group channel

```swift
GroupChannel.getChannel(CHANNEL_URL) { groupChannel, e in
    if (e != nil) {
        // Handle error.
    }

    if (groupChannel?.myRole == Role.OPERATOR) {
        // Ban a user.
        groupChannel?.banUser(USER, seconds: SECONDS, description: DESCRIPTION) { e in
            if (e != nil) {
                // Handle error.
            }

            // The user is successfully banned from the channel.
            // You can display a message to let the user know they have been banned.
            // ...
        }

        // Unban a user.
        groupChannel?.unbanUser(USER) { e in
            if (e != nil) {
                // Handle error.
            }

            // The user is successfully unbanned.
            // You can display a message to let the user know they have been unbanned.
            // ...
        }
    }
}
```

## Mute and unmute a user

Operators of an open or group channel can restrict selected users from sending messages to the channel using the mute feature. A muted user remains in the channel and is allowed to view the messages but they can't send any messages to the channel until the operators unmute them. Operators can mute and unmute users using the following code.

### Open channel

```swift
OpenChannel.getChannel(CHANNEL_URL) { openChannel, e in
    if (e != nil) {
        // Handle error.
    }

    if (openChannel?.isOperator(GIMChat.currentUser) == true) {
        // Mute a user.
        openChannel?.muteUser(USER, seconds: SECONDS, description: DESCRIPTION) { e in
            if (e != nil) {
                // Handle error.
            }

            // The user is successfully muted in the channel.
            // You can display a message to let the user know they have been muted.
            // ...
        }

        // Unmute a user.
        openChannel?.unmuteUser(USER) { e in
            if (e != nil) {
                // Handle error.
            }

            // The user is successfully unmuted in the channel.
            // You can display a message to let the user know they have been unmuted.
            // ……
   }
}
```


#### Group channel

```swift
GroupChannel.getChannel(CHANNEL_URL) { groupChannel, e in
    if (e != nil) {
        // Handle error.
    }

    if (groupChannel?.myRole == Role.OPERATOR) {
        // Mute a user.
        groupChannel?.muteUser(USER, seconds: SECONDS, description: DESCRIPTION) { e in
            if (e != nil) {
                // Handle error.
            }

            // The user is successfully muted in the channel.
            // You can display a message to let the user know they have been muted.
            // ...
        }

        // Unmute a user.
        groupChannel?.unmuteUser(USER) { e in
            if (e != nil) {
                // Handle error.
            }

            // The user is successfully unmuted in the channel.
            // You can display a message to let the user know they have been unmuted.
            // ...
        }
    }
}
```

## Retrieving and updating user information

## Retrieve the online status of a user

You can check if a user in the Vyin Chat application is currently connected to the Vyin Chat server. For group channels only, you can check whether each member is currently connected to the server.

```swift
let userIds = ["Jeff"]
let query = GIMChat.createApplicationUserListQuery(
    ApplicationUserListQueryParams(builder: { params in
        params.userIdsFilter = userIds
    })
)
query.loadNextPage { users, e in
    if (e != nil) {
        // Handle error.
    }
    // users?.first = "Jeff"
    if (users?.first?.connectionStatus == UserConnectionStatus.online) {
        // "Jeff" is currently online.
        // UserConnectionStatus consists of .online and .offline.
    }
}
```

## Update user profile

You can update a user's nickname and profile image with a profile URL using `updateCurrentUserInfo()`. A user's profile image can be a JPG (.jpg), JPEG (.jpeg), or PNG (.png) file.

```swift
let params = UserUpdateParams()
params.nickname = NICKNAME
params.profileImageURL = PROFILE_URL
GIMChat.updateCurrentUserInfo(params: params) { e in
    if (e != nil) {
        // Handle error.
    }
    // The current user's profile is successfully updated.
    // You could redraw the profile in a view in response to this operation.
}
```

Or, you can upload a profile image directly using the `profileImageData` property.

```swift
let params = UserUpdateParams()
params.nickname = NICKNAME
params.profileImageData = PROFILE_DATA
GIMChat.updateCurrentUserInfo(params: params) { e in
    if (e != nil) {
        // Handle error.
    }
    // A new profile image is successfully uploaded to the Vyin Chat server.
    // You could redraw the profile in a view in response to this operation.
}
```

## Retrieve the latest information on participants

To retrieve the latest and updated information on each online participant in an open channel, you need to create a new query instance using the `createParticipantListQuery()` method, then call the `loadNexPage()` method consecutively to retrieve the latest information.

You can also retrieve the latest information on participants at the application level. Similar to retrieving a list of users, you need to create a new query instance using `GIMChat.createApplicationUserListQuery()`, then call `loadNextPage()` until you retrieve the latest.

You can get the participant's current connection status by checking `user.connectionStatus` in the returned list. Acceptable values of the `user.connectionStatus` property are `.offline` and `.online`.

### Connection status values

| Value | Description |
| :---- | :---- |
| `UserConnectionStatus.offline` | The user isn't connected to the Vyin Chat server. |
| `UserConnectionStatus.online` | The user is connected to the Vyin Chat server. |

Note: If you need to keep track of a participant's connection status in real time, we recommend that you call periodically the `loadNextPage()` method of an `ApplicationUserListQuery` instance in intervals of one minute or more after specifying the value of userIdsFilter.

## Manage user metadata

Metadata consists of key-value items in which you can store additional information to users. You can add up to five key-value items for user metadata. Each key can have up to 128 characters and value can have up to 190 characters as `String`. This section explains how to manage user metadata.

### Create metadata

You can create additional information such as phone number, email address or other descriptions to a user, which can be fetched or rendered into the UI. As an object, user metadata in `Map<String, String>` is stored into a `User` object.

To store user metadata into a `User` object, create `Map<String, String>` of key-value items, and then pass it as an argument to a parameter when calling the `createMetaData()` method. You can add multiple key-value items in the map.

```swift
let user = GIMChat.currentUser
let data = ["key1": "value1", "key2": "value2"]
user?.createMetaData(data) { metaDataMap, e in
    if (e != nil) {
        // Handle error.
    }
    // ...
}
```

### Retrieve

You can retrieve metadata stored to a user by calling the `metaData` property of a `User` object.

```swift
let user = GIMChat.currentUser
let metaData = user?.metaData ?? [:]
let value = metaData["key"]
```

### Update

You can update metadata of a user by adding a map of key-value items, and then pass it as an argument to a parameter when calling the `updateMetaData()` method. Values of existing keys will be updated and values of new keys will be added. You can put multiple key-value items in the map.

```swift
let user = GIMChat.currentUser
let data = ["key1": "valueToUpdate1", "key2": "valueToUpdate2"]
user.updateMetaData(data) { metaDataMap, e in
    if (e != nil) {
        // Handle error.
    }
    // ...
}
```

### Delete

You can delete metadata stored to a user as below

```swift
let user = GIMChat.currentUser
user?.deleteMetaData("key1") { e in
    if (e != nil) {
        // Handle error.
    }
    // ...
}
```

