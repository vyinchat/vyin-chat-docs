# Vyin Chat SDK for iOS - Getting Started, User, Channel

## Preview

1. Get Started  
   1. Send the first message  
2. User  
   1. Overview  
   2. Retrieving users  
      1. Retrieve a list of users in an application  
      2. Retrieve a list of users in a channel  
      3. Retrieve a list of members and operators in a specific order  
      4. Retrieve members who have read a message  
      5.  Retrieve a list of operators  
   3. Moderating a user  
      1. Retrieve a list of blocked users  
      2. Retrieve a list of banned users  
      3. Retrieve a list of muted users  
      4. Block and unblock other members  
      5. Ban and unban a user  
      6. Mute and unmute a user  
   4. Retrieving and updating user information  
      1. Retrieve the online status of a user  
      2. Update user profile  
      3. Retrieve the latest information on participants  
   5. Managing user metadata  
      1. Manage user metadata  
3. Channel  
   1. Overview  
   2. Creating a channel  
   3. Managing operators  
   4. Joining and leaving a channel  
      1. Enter and exit an open channel  
      2. Join and leave a group channel  
   5. Retrieving channels  
      1. Retrieve a list of channels  
      2. Retrieve a channel by URL  
   6. Inviting users to a group channel  
      1. Invite users as members  
      2. Accept or decline an invitation  
   7. Searching channels  
      1. Search open channels by name, URL, or custom types  
      2. Search group channels by name, URL, or other filters  
      3. Filter group channels by user IDs  
   8. Categorizing channels  
      1. Delete a channel  
      2. Hide or archive a group channel  
      3. Refresh all data related to a group channel  
   9. Managing channels  
   10. Moderating a channel  
   11. Managing channel metadata  
   12. Managing channel metacounters  
4. Message  
   1. Overview  
   2. Sending a message  
      1. Send a message  
      2. Send a critical alert message to iOS device users  
      3. Send an admin message  
      4. Track file upload progress using a handler  
      5. Cancel an in-progress file upload  
      6. Share an encrypted file  
      7. Spam flood protection  
      8. Smart throttling  
   3. Receiving messages through event handler  
      1. Receive messages in an open channel  
      2. Receive messages in a group channel  
   4. Using message threading  
      1. Create a message thread  
      2. List replies in a message thread  
   5. Retrieving messages  
      1. Retrieve a list of messages  
      2. Retrieve a message  
   6. Searching messages in a group channel  
   7. Managing a message  
      1. Copy a message  
      2. Update a message  
      3. Delete a message  
      4. React to a message in a group channel  
      5. Clear the chat history in a group channel  
      6. Send typing indicators to other members  
      7. Display Open Graph tags in a message  
      8. Generate thumbnails of a file message  
   8. Managing scheduled messages in group channel  
      1. Create a scheduled message  
      2. Retrieve a scheduled message  
      3. Update a scheduled message  
      4. Cancel a scheduled message  
      5. Send a scheduled message immediately  
      6. List all scheduled messages  
      7. Retrieve the total number of scheduled messages  
   9. Managing pinned messages in group channels  
      1. Pin a message  
      2. Unpin a message  
      3. List pinned messages  
   10. Listing changelogs  
   11. Managing read status in a group channel  
       1. Mark message as read   
       2. Mark message as unread  
       3. Get read status of a message  
   12. Marking messages as read in a group channel  
       1. Unread messages in a channel  
       2. Unread messages in all channels  
       3. Unread channels  
       4. Members who haven't received a message  
       5. Members who haven't read a message  
       6. Unread items  
   13. Marking messages as delivered in a group channel  
   14. Mentioning other users in a message  
   15. Managing polls  
       1. Create a poll  
       2. Retrieve a poll  
       3. Update a poll  
       4. Delete a poll  
       5. Close a poll  
       6. Add a poll option  
       7. Retrieve a poll option  
       8. Update a poll option  
       9. Delete a poll option  
       10. Cast or cancel a vote  
       11. Retrieve a list of polls  
       12. Retrieve a list of voters  
       13. List changelogs of polls  
   16. Retrieving unread count in a group channel  
   17. Categorizing messages  
   18. Adding extra data to a message  
   19. Translating messages  
       1. Auto-translate messages  
       2. Translate messages on-demand  
       3. Translation engine  
5. Application  
   1. Overview  
   2. Authenticating a user  
   3. Understanding rate limits  
6. Event handler  
   1. Overview  
   2. Managing channel event handlers  
   3. Managing connection event handlers  
   4. Managing user event handlers  
7. Push notifications  
   1. Overview  
   2. Managing push notifications  
      1. Set up push notifications for FCM  
      2. Set up push notifications for HMS  
      3. Configure push notification preferences  
      4. Push notification content templates  
      5. Push notification tester  
      6. Push notification translation  
   3. Marking push notifications as delivered  
   4. Multi-device support  
      1. Set up push notifications for FCM  
      2. Set up push notifications for HMS  
8. Report  
   1. Overview  
   2. Creating a report  
9. Local caching  
   1. Overview  
   2. Using group channel collection  
   3. Using message collection  
10. Error codes  
11. Logger

			
## Getting started

## Send first message

---

### Get started

To send a message in a client app, you should build and configure an in-app chat using Vyin Chat SDK, which can be installed through SPM or CocoaPods by accessing our remote repository.

#### Step 1-1 Install the Chat SDK \- Configuring the Repository

You can install the Chat SDK for iOS through `CocoaPods`.

#### CocoaPods

If you are using Cocoapods, add the following to your `Podfile` file:

```swift
 pod :git => ‘https://gitlab.gamania.com/gim/ios/chatsdk.git’, :tag => ‘1.5.0’
```

#### Swift package manager (SPM)

If you are using SPM, add the following code to your root `Package Dependencies` file:

```swift
    https://gitlab.gamania.com/gim/ios/chatsdk.git
    Branch: main or using tag version
```

#### Step 1-2 Install the Chat SDK

If using CocoaPods then run pod install  
Else using SPM go to Package Dependencies at bottom project panel after that Resolve package version

**Step 2 Request to access system permissions**  
The Chat SDK requires system permissions. These permissions allow the Chat SDK to communicate with the Vyin Chat server and read from and write on a user device’s storage, remote notification. To request system permissions, add the following lines to your `Info.plist` file.  
> **[IMAGE PLACEHOLDER]** 

#### Step 3 Initialize the Chat SDK

Now, initialize the Chat SDK in the app to allow the Chat SDK to respond to changes in the connection status of iOS client apps. Initialization requires your Vyin Chat application's Application ID, which can be found on the Vyin Chat Dashboard.

Note: All methods in the following steps are asynchronous. This means that when using asynchronous methods, your client app must receive success callbacks from the Vyin Chat server through their callback handlers in order to proceed to the next step. A good way to do this is using nested methods. Go to Step 5: how you can create the `Groupchannel.getChannel()` method.

Before initializing, you should determine whether to enable local caching using `InitParams` in the `GIMChat.init()` method. The `isLocalCachingEnabled` property of `InitParams` determines whether or not the client app is going to use local storage through the SDK. If you want to build a client app without our local caching functionalities, set the value of `isLocalCachingEnabled` to `false`.

To learn more, see Initialize the Chat SDK with APP\_ID.

```swift
    let params = InitParams(applicationId: applicationId, isLocalCachingEnabled: true, logLevel: .verbose)
    GIMChat.initialize(params: params)
```

The `GIMChat.init()` method must be called at least once across a client app. It is recommended to initialize the Chat SDK using the `func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?)` method of the `AppDelegate` instance.

If ChatSDK initialization fails or handle migration, handle the error in the following closure:

```swift
    GIMChat.initialize(params: params) {
      // handle migrate
    } completionHandler: { error in
      // handle error
    }
```

#### Step 4 Connect to the Vyin Chat server

You need a user connected to the Vyin Chat server first before sending a message to a channel.

* If you already have a user in your Vyin Chat application: In `connect()`, specify a user ID from the user list in your Vyin Chat application.  
* If you don't have an existing user: Specifying a unique `userId` in `connect()` will automatically generate a new user in your Vyin Chat application before being connected.

Note: Vyin Chat supports user authentication via access tokens, but defaults to allowing access without a token for ease of initial use. For enhanced security, we recommend adjusting access settings under Settings \> Application \> Security \> Access token permission on the dashboard for new users. To learn about access token and authentication, see the Authentication guide.  

```swift
// Use the ID of a user you've created on the dashboard.
// If there isn't one, specify a unique ID so that a new user can be created with the value.
GIMChat.connect(userId) { user, e in
    if (user != nil) {
        if (e != nil) {
            // Proceed in offline mode with the data stored in the local database.
            // Later, connection is made automatically,
            // and can be notified through ConnectionHandler.onReconnectSucceeded().
        } else {
            // Proceed in online mode.
        }
    } else {
        // Handle error.
    }
}
```

Note: You must receive the result of `GIMChat.initialize completionHandler` before calling the `connect()` method. Any method can be called once the user is connected to the Vyin Chat server.

For Chat SDKs that don't use local caching, the connection process remains the same. When an error occurs, the SDK must attempt to reconnect again.

#### Step 5 Create a new groupchannel

Create a channel using the following code. Group channels are where all users in your Vyin Chat application can easily participate without an invitation.  

```swift
var listUserString = listUserInvite?.map { $0.user.userId }

        let params = GroupChannelCreateParams()
        params.name = name
        params.coverImage = image?.pngData()

        if let currentUser = GIMChat.getCurrentUser() {
            params.operatorUserIds = [currentUser.userId]
            listUserString?.append(currentUser.userId)
        }

        params.addUserIds(listUserString ?? [])

GroupChannel.createChannel(params: params) { channel, e in
    if (e != nil) {
        // Handle error.
    }

    // A group channel is successfully created.
    // You can get the group channel's data from the result object passed to the callback handler.
}
```

Note: You can also create a group channel to send a message. To learn more, see Create a channel.

#### Step 6 GET the channel

Enter the channel to send and receive messages.  

```swift
// The following sample code continues from Step 5.
OpenChannel.getChannel(url: CHANNEL_URL) { channel, e in
    if (e != nil) {
        // Handle error.
    }
    // Call the instance method of the result object in the openChannel parameter
    // of the callback handler.
    channel?.enter { e in
        if (e != nil) {
            // Handle error.
        }

        // The current user has successfully entered the group channel,
        // and can chat with other users in the channel by using APIs.
    }
}
```

#### Step 7 Send a message to the channel

Finally, send a message to the channel. To learn about the message types you can send, refer to Message overview in Chat Platform API.

You can check the message you've sent in the Vyin Chat Dashboard. To learn about receiving a message, refer to the received messages through a channel event handler page.  

```swift
groupChannel.sendUserMessage(UserMessageCreateParams(MESSAGE)) { message, e in
    if (e != nil) {
        // Handle error.
    }

    // The message has been successfully sent to the channel.
    // The current user can receive messages from other users
    // through the onMessageReceived() method of an event handler.
}
```

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
| Retrieve a list of users in an application | Retrieves a list of all or a specified subset of users in a Vyin Chat application. | x | x |
| Retrieve a list of users in a channel | Retrieves a list of users in a channel. | x | x |
| Retrieve a list of users and operators in a specific order | Retrieves a list of users and operators in a channel in an alphabetical order or by another specified order. |  | x |
| Retrieve users who have read a message | Retrieves users who have read a specific message in a channel. |  | x |
| Retrieve a list of operators | Retrieves a list of operators who monitor and control the activities in a channel. | x | x |

#### Moderating a user

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Retrieve a list of blocked users | Retrieves a list of all or a specified subset of blocked users in a Vyin Chat application. |  | x |
| Retrieve a list of banned users | Retrieves a list of users who are banned from a channel. | x | x |
| Retrieve a list of muted users | Retrieves a list of users who are muted in a channel. | x | x |
| Block and unblock other users | Blocks or unblocks specified users for a user in a channel. |  | x |
| Ban and unban a user | Operators can ban or unban users from a channel. Banned users are immediately expelled from a channel and allowed to participate in the channel again after the time period set by the operators has passed. | x | x |
| Mute and unmute a user | Operators can mute or unmute users in a channel. Muted users remain in the channel and are allowed to view the messages, but can't send any messages until the operators unmute them. | x | x |

#### Retrieving and updating user information

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Retrieve the online status of a user | Checks if a certain user in a Vyin Chat application is currently connected to the Vyin Chat server. | x | x |
| Update user profile | Updates a user's nickname and profile image with a URL. | x | x |
| Retrieve the latest information on users | Retrieves the latest and updated information on each user who is online. | x |  |

#### Managing user metadata

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | :---- |
| Manage user metadata | Stores additional information to users. You can create, retrieve, update and delete additional information. | x | x |

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

## Channel

## Overview

Vyin Chat's APIs and SDKs provide two basic types of channels: open channel and group channel. Each type of channel is designed to support a wide variety of use cases that your business requires. This page explains the differences and characteristics of the two types.

Note: By default, the Allow creating open channels and Allow creating group channels options are turned on which means that open channels and group channels can be created by any user with Vyin Chat SDK. This may grant access to unwanted data or operations, leading to potential security concerns. To manage your access control settings, you can turn on or off each option in Settings \> Application \> Security \> Access control list on Vyin Chat Dashboard.  

---

### Open channel

An open channel is a Twitch-style public chat where millions of users can easily join without invitations. Open channels can be used in short-lived live events, such as streaming, news-feed type messaging to massive audience, or events that don't require a permanent membership.

#### Choose a type of an open channel

With our Chat SDK for iOS, you can use a variety of behavior related properties when creating different types of open channels.

#### Classic vs. Dynamic partitioning

Vyin Chat SDK now supports two operation systems for open channels, which are classic and dynamic partitioning. These two systems offer the same features and functions, except that dynamic partitioning allows open channels to accommodate a large number of users. Classic open channels are the original open channels Vyin Chat has been providing, and a single classic open channel can handle up to 1,000 participants. On the other hand, dynamic partitioning open channels are designed to accommodate much greater number of users than the classic open channels using subchannels where you can divide users evenly.

Note: Vyin Chat applications created before December 15, 2020 are using classic open channels. The classic open channels will be deprecated soon, but the classic channels created up until the deprecation date will remain and function as usual. After classic open channels have been deprecated, all Chat SDK users will be automatically migrated to the new dynamic partitioning system. If you wish to convert to dynamic partitioning open channels beforehand, visit this page and contact us on the Vyin Chat Dashboard.

Learn more about how dynamic partitioning open channel operates in the channel overview page for Platform API.

#### Ephemeral vs. Persistent open channels

Messages in ephemeral open channels aren't saved on Vyin Chat's database. This means that old messages pushed out by new ones can't be retrieved as they are one-time data. On the other hand, messages in a persistent open channel are permanently stored in the database, which is the default.  

---

### Group channel {#group-channel}

A group channel is a chat that allows close interactions among a limited number of users. In order to [join a group channel](), an invitation from a channel member is required. Depending on how you implement the joining process at the application level, an invited user can automatically join a group channel or manually accept the invitation to join. Various properties can be leveraged to design different types of group channels that suit your use cases, such as Twitter-style 1-to-1 direct messaging, WhatsApp-style group chat, and more.

#### Choose a type of a group channel {#choose-a-type-of-a-group-channel}

With our Chat SDK for iOS, you can use a variety of behavior related properties when creating different types of group channels. You can create a group channel after configuring these properties.

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

#### Ephemeral vs. Persistent group channels {#ephemeral-vs.-persistent-group-channels}

Messages sent in an ephemeral group channel aren't saved on Vyin Chat's database. As such, old messages that are pushed out of a user's chat view due to new messages can't be retrieved. On the other hand, messages sent in a persistent group channel are stored permanently in the database by default.  

---

### Supergroup channel

A Supergroup channel is an expanded version of a group channel, which can accommodate a massive number of members while serving the same functions as a [group channel](#group-channel). The maximum number can stretch up to tens of thousands of members depending on your Vyin Chat plan. Because a Supergroup channel is built upon the same foundation as a group channel, Platform APIs and SDK functions for both channel types are identical.

Note: Supergroup and group channels are alike in terms of Platform API and SDK design, except for the `isSuper` property. The `isSuper` property indicates whether the channel is a Supergroup channel or a group channel.

#### Use cases

Supergroup channels can be used for a wide range of occasions that demand real-time engagement among a large number of users. One of the common use cases for Supergroup channels is community building, ranging from public forums to private events with a large membership.

* Ask-me-anything events: Supergroup channels also work as a platform for users to seek advice from an influencer or expert, such as Reddit Chat.  
* Gaming community: Another common set-up is gaming communities, where users gather to find players to team up with or exchange ideas and strategies to advance further in the game.  
* Stand-ups: Supergroup channels are a great place for midsize conferences, such as an organization-wide stand-up.

Note: To learn about differences among open channels, group channels, and Supergroup channels, see [Channel types](#comparing-channel-types).

#### Limitations {#limitations}

To secure stable operation and optimized user experience in a Supergroup channel, a few functions are subject to limits. Some of these limitations are the following.

* Visual interactions: Read and delivery receipts aren’t supported in Supergroup channels to provide an optimal performance in Supergroup channels.  
* Unread counts: Unread counts are marked up to 100 only. In the case they surpass 100, we suggest you display the counts as `99+` to avoid confusion.  
* Push notifications: The way push notifications for Supergroup channels work is determined by two factors: the number of members in a channel and the online status of its member.  
  * When there are less than 100 members: A push notification is sent for every message created in the channel.  
  * When there are 100 members or more: A ten-minute time window is applied to Supergroup channels with more than 100 members. Once the number exceeds 100 members, a push notification for a new message is sent out to members and then the next push notification comes ten minutes after that.  
    For example, the number of members in Channel A surpasses 100 at 13:00 and a new message is sent at 13:01, a push notification will be sent for the 13:01 message and the ten-minute time window begins. For the following ten minutes, there will be no push notifications for any messages created within the time frame. When ten minutes have passed and a new message is sent at 13:11, a second push notification will be sent for that message.  
  * When a user becomes active: If one of the Supergroup channel members enters the channel and reads a message, the user is recognized as active. Then only for the following one minute, the user receives push notifications for all messages sent within the time frame.

---

### Comparing channel types {#comparing-channel-types}

The optimal use cases for each channel type can vary because different channels support different features. The following tables compare open channels, group channels, and Supergroup channels in terms of possible use cases and supported features.

#### Use cases by channel type

Choose which channel type to use based on key factors such as the duration of a chat and the number of users participating in it. The following table lists possible use cases for each channel type.

| Type | Used for |
| :---- | :---- |
| `Open channel` | \- Short-lived live events, such as concerts and streaming.\- News-feed type messaging to massive audience, such as Twitter-style feed.\- Events that don't require a permanent membership. |
| `Group channel` | \- Private 1-on-1 conversations, such as clinical consultations and Instagram-style Direct Messages.\- Public 1-to-N conversations, such as small group discussions among students.\- Invitation-only chats with a handful group of users. |
| `Supergroup channel` | \- Ask-me-anything type events with a large number of users.\- Midsize conferences for regular updates, such as company-wide meetings. |

#### Comparison of channel types

The following table compares the difference among two types of channels.

|  | Open channel | Group channel | Supergroup channel |
| :---- | :---- | :---- | :---- |
| Maximum number in a channel | Up to millions of [participants](?tab=t.idg08sfacq58) | 100 [members](?tab=t.idg08sfacq58) | Up to tens of thousands of [members](?tab=t.idg08sfacq58) depending on your Vyin Chat plan. |
| Accessible by | Anyone within the application | [Invited users]() only if private or anyone if public | [Invited users]() only if private or anyone if public |
| Ephemeral messaging | Supported | [Supported](#ephemeral-vs.-persistent-group-channels) | [Supported](#choose-a-type-of-a-group-channel) |
| Online presence | Supported | Supported | Supported |
| Last message | N/A | [Supported](?tab=t.7x5fkf99btam) | [Supported](?tab=t.7x5fkf99btam) |
| UIKit | Supported | Supported | Supported |
| [Operators](?tab=t.idg08sfacq58) | [Supported](?tab=t.3lxru8i8hlf0) | [Supported](?tab=t.3lxru8i8hlf0) | [Supported](?tab=t.3lxru8i8hlf0) |
| Ban users | [Supported]() | [Supported]() | [Supported]() |
| Mute users | [Supported]() | [Supported]() | [Supported]() |
| Freeze channels | [Supported](?tab=t.rkovea8tczzc) | [Supported](?tab=t.rkovea8tczzc) | [Supported](?tab=t.rkovea8tczzc) |
| Delivery receipts | N/A | [Supported](?tab=t.ap7ikkmss0bb) | N/A |
| Read receipts | N/A | [Supported](?tab=t.stk272hvoj4q) | N/A |
| Unread counts | N/A | [Supported](?tab=t.yrf1x4hr819p) | [Supported](?tab=t.yrf1x4hr819p) \* Up to 100 unread counts are supported. |
| Typing indicators | N/A | [Supported](?tab=t.28ueb1jmgjnr) | [Supported](?tab=t.28ueb1jmgjnr) \* Up to 3 concurrent indicators are supported. |
| Mention others in message | [Supported](?tab=t.7gvsyif7dvtc) | [Supported](?tab=t.7gvsyif7dvtc) | [Supported](?tab=t.7gvsyif7dvtc) |
| Mention counts | N/A | Supported | Supported |
| Reactions | N/A | [Supported](?tab=t.bs0a7ajqwe1h) | [Supported](?tab=t.bs0a7ajqwe1h) |
| Spam flood protection | [Supported](?tab=t.6hp8x6tlxcog) | [Supported](?tab=t.6hp8x6tlxcog) | [Supported](?tab=t.6hp8x6tlxcog) |
| Chatbot interface | Supported | Supported | Supported |
| Smart throttling | [Supported](?tab=t.42cxg3b710ft) (Default: `true`) | [Supported](?tab=t.42cxg3b710ft) (Default: `false`) | [Supported](?tab=t.42cxg3b710ft) (Default: `false`) |
| Push notifications | N/A\* Push notifications for announcements only. | [Supported](?tab=t.z7a614ntsbtt)   \* Push notifications for every message sent. | [Supported](?tab=t.z7a614ntsbtt)   \* Refer to [Limitations](#limitations). |
| Get a channel with its participant list or member list | N/A | Supported | Supported\* Only ten members are retrieved as a preview.To get an entire list of members, use the list members API instead. |
| Pagination for participant list or member list | [Supported]() | [Supported]() | [Supported]() |
| Order of channel list | \- Chronological | \- Chronological\- Latest last message\- Channel name\- Metadata value | \- Chronological\- Latest last message\- Channel name\- Metadata value |

---

### Functionalities by topic

Users can interact with other users in a channel by sending, receiving, replying to messages, and more. The following is a list of functionalities that our Chat SDK provides.

#### Creating a channel

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | ----- | ----- |
| [Create a channel](?tab=t.ruetpdultbj9) | Creates a channel where users can chat. | ✔ | ✔ |

#### Managing operators

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | ----- | ----- |
| [Register and remove operators in a channel](?tab=t.3lxru8i8hlf0) | Adds and removes users as operators to a channel. | ✔ | ✔ |

#### Joining and leaving a channel

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | ----- | ----- |
| [Enter and exit an open channel]() | Users can enter and exit open channels to receive messages. | ✔ |  |
| [Join and leave a group channel]() | Users can join and leave group channels to receive messages. |  | ✔ |

#### Retrieving channels

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | ----- | ----- |
| [Retrieve a list of channels]() | Retrieves a list of channels. | ✔ | ✔ |
| [Retrieve a channel by URL]() | Retrieves a channel by its URL. | ✔ | ✔ |

#### Inviting users to a channel

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | :---- | ----- |
| [Invite users as members]() | Invites new users to a channel which is performed only by the members of a group channel. |  | ✔ |
| [Accept or decline an invitation]() | Accepts or declines an invitation to join a group channel. |  | ✔ |

#### Searching channels

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | ----- | ----- |
| [Search open channels by name, URL, or custom types]() | Searches for specific open channels by using a keyword. | ✔ |  |
| [Search group channels by name, URL, or other filters]() | Searches for specific group channels by using name, URL, or several types of filters. |  | ✔ |
| [Filter channels by user IDs]() | Filters channels using user IDs. |  | ✔ |

#### Categorizing channels

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | ----- | ----- |
| [Categorize channels by custom type](?tab=t.hyyds2yiwzfm) | Specifies a custom channel to subclassify channels. | ✔ | ✔ |

#### Managing channels

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | ----- | ----- |
| [Delete a channel]() | Deletes a channel which only operators can do. | ✔ | ✔ |
| [Hide or archive a channel]() | Hides or archives a specific channel from a list of channels. |  | ✔ |
| [Refresh all data related to a channel]() | Updates the user's channels with the latest information. |  | ✔ |

#### Moderating a channel

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | ----- | ----- |
| [Freeze and unfreeze a channel](?tab=t.rkovea8tczzc) | Temporarily disables various functions of a channel. | ✔ | ✔ |

#### Managing channel metadata

| Functionality | Description | Open channel | Group channel |
| :---- | :---- | ----- | ----- |
| [Manage channel metadata](?tab=t.9j22aho39dm0) | Store additional information to a channel. You can create, retrieve, update and delete the additional information. | ✔ | ✔ |

## Creating a channel

## Create a channel

Two basic types of channels you can create with Vyin Chat SDK are open channels and group channels.

An open channel is ideal for use cases that don't require a permanent membership in the channel, such as short-lived live events and news-feed style messaging to a massive audience. On the other hand, a group channel is suitable for closer interactions among a limited number of users. To learn more about the different use cases and characteristics of open and group channels.

When creating a channel, you can append additional information like cover image, description, and URL by passing several arguments to the corresponding parameters. The channel URL can only contain numbers, letters, underscores, or hyphens, and the URL's length must be between 4 and 100 characters. If you don't specify the `channelUrl` property, a URL is automatically generated.

If you want to create and continue to use a single group channel for a 1-to-1 chat, set the `isDistinct` property of the channel to `true`. Otherwise, if the `isDistinct` property is set to `false`, multiple 1-to-1 channels, each with their own chat history and data, may be created for the same two users even if a channel already exists between them.  

---

### Open channel

You can create an open channel by passing a configured `OpenChannelCreateParams` object as an argument to the parameter in the `createChannel()` method like the following.  

```swift
let params = OpenChannelCreateParams()
params.name = name
OpenChannel.createChannel(params: params) { channel, e in
    if (e != nil) {
        // Handle error.
    }

    if (channel != nil) {
        // An open channel of the specified users is successfully created.
        // You can get the open channel's data from the result object of the callback handler.

        var channelUrl = channel.url
    }
    // ...
}
```

#### OpenChannelCreateParams

This table only contains properties shown in the code above. See the API reference for a complete list of properties.

| Property name | Type | Description |
| :---- | :---- | :---- |
| `name` | String | Specifies the channel topic or the name of the channel. |
| `channelUrl` | String | Specifies the URL of the channel. Only numbers, letters, underscores, and hyphens are allowed. The length must be between 4 and 100 characters. If the `channelUrl` property isn't specified, a URL is automatically generated. |
| `coverImage` | Data | Uploads a file for the cover image of the channel. Alternatively, you can upload a URL for the cover image of the channel using the `coverURL` property. |
| `data` | String | Specifies additional channel information such as a long description of the channel or `JSON` formatted String. |

---

### Group channel

You can create a group channel by passing a configured `GroupChannelCreateParams` object as an argument to the parameter in the `createChannel()` method like the following.  

```swift
var listUserString = listUserInvite?.map { $0.user.userId }
let params = GroupChannelCreateParams()
params.name = name
params.coverImage = image?.pngData()
if let currentUser = GIMChat.getCurrentUser() {
params.operatorUserIds = [currentUser.userId]
listUserString?.append(currentUser.userId)
}
params.addUserIds(listUserString ?? [])

GroupChannel.createChannel(params: params) { channel, e in
    if (e != nil) {
        // Handle error.
    }

    if (channel != nil) {
        // A group channel with the specified configuration is successfully created.
        // You can get the group channel's data from the result object of the callback handler.

        var channelUrl = channel.url
    }
    // ...
}
```

#### GroupChannelCreateParams

This table only contains properties shown in the code above. See the API reference for a complete list of properties.

| Property name | Type | Description |
| :---- | :---- | :---- |
| `name` | String | Specifies the name of the channel. |
| `coverUrl` | String | Specifies the cover image URL of the channel. |
| `userIds` | array String | Specifies a list of one or more IDs of the users to invite to the channel. |
| `isDistinct` | Bool | Determines whether to reuse an existing channel or create a new channel. Setting this property to `true` returns a channel with the same users in `USER_IDS` or creates a new channel if no match is found. If set to `false`, the Vyin Chat server always creates a new channel with the specified combination of users as well as the channel custom type.\* You can also use this property in conjunction with `CUSTOM_TYPE` and `USER_IDS` to create distinct channels for a specified channel custom type and a set of specified users. In order to enable the functionality, visit this page and contact us on Vyin Chat Dashboard. |
| `customType` | String | Specifies the custom channel type which is used for channel grouping. |

Note: You can also use the Chat API to create open channels and group channels. For group channels, using the Chat API helps you control channel member invitations on your server side.  

---

### Supergroup channel

Creating a Supergroup channel follows the same process as [creating a group channel](?tab=t.ruetpdultbj9) with one exception of the `isSuper` property. The `GroupChannelCreateParams` class has the `isSuper` property that determines whether a newly created channel is a Supergroup channel or a regular group channel. Set `isSuper` to `true` to create a new Supergroup channel.

Because the [distinct](?tab=t.gygp5bj64m3o) option isn't available for Supergroup channels, the `isDistinct` property is set to `false` by default when creating a Supergroup channel.  

```swift
let params = GroupChannelCreateParams()
```
`params.name` `= name`

```swift
GroupChannel.createChannel(params: params) { channel, e in
    if (e != nil) {
        // Handle error.
    }

    // A group channel of the specified users is successfully created.
    // Through the groupChannel parameter of the callback handler,
    // you can get the group channel's data from the result object that
    // the the Vyin Chat server has passed to the callback method.

    var channelUrl = channel?.url
}
```

## Managing operators

## Register and remove operators

Operators are users who can delete any messages and view all messages in an open or group channel without any filtering or throttling. Operators can moderate channels by muting or banning users as well as freezing channels.

You can register users as an operator by providing their user IDs as shown below.  

```swift
channel.addOperators(userIds: ids…) { e in
    if (e != nil) {
        // Handle error.
    }

    // The users are successfully registered as operators of the channel.
}
```

You can remove the users from being operators but leave them in the channel using the following code.  

```swift
channel.removeOperators(userIds: ids…) { e in
    if (e != nil) {
        // Handle error.
    }

    // The specified operators are removed.
    // You can notify the users of the role change through a prompt.
}
```

If you want to cancel the registration of all operators in a channel at once, use the following code.  

```swift
channel.removeAllOperators { e in
    if (e != nil) {
        // Handle error.
    }

    // All operators are removed.
    // You can notify the users of the role change through a prompt.
}
```

## Joining and leaving a channel

## Enter and exit an open channel

A user can enter up to ten open channels and only receives messages from the entered channels. If the user is disconnected from the Vyin Chat server with the `disconnect()` method, they no longer receive messages. To continue receiving messages from the user's participating open channels, they have to be reconnected to the server with the `connect()` method and re-enter the open channels.

If the client app is in the background, the user is disconnected from the Vyin Chat server. However, when the client app runs in the foreground again, the Chat SDK automatically reconnects the open channels the user has entered. When a user temporarily loses connection to the Vyin Chat server due to an unstable connection, the `GIMChat` instance attempts to reconnect the user and the open channels the user has entered.  

```swift
OpenChannel.getChannel(url: CHANNEL_URL) { channel, e in
    if (e != nil) {
        // Handle error.
    }

    // Invoke the callback handler.
    channel?.enter { enterError in
        if (enterError != nil) {
            // Handle error.
        }

        // The current user successfully enters the open channel
        // and can chat with other users in the channel by using APIs.
    }
}
```

A user can exit open channels as shown below. After exiting, the user can no longer receive messages from the channel.  

```swift
OpenChannel.getChannel(url: CHANNEL_URL) { channel, e in
    if (e != nil) {
        // Handle error.
    }

    // Invoke the callback handler.
    channel?.exit { exitError in
        if (e != nil) {
            // Handle error.
        }

        // The current user successfully exits the open channel.
    }
}
```

## Join and leave a group channel

By default, an invitation is required to join group channels. However, any user can join [public group channels](?tab=t.gygp5bj64m3o) as a member without invitations as shown below. Users can join up to 2,000 group channels.  

```swift
if (groupChannel.isPublic) {
    groupChannel.join { e in
        if (e != nil) {
            // Handle error.
        }

        // The current user successfully joins the group channel.
    }
}
```

A user can leave group channels as shown below. After leaving, the user can't receive messages from the channel, and this method can't be called for deactivated users.

If the user is the channel's operator, you can remove the user from the channel's operator list by setting the `shouldRemoveOperatorStatus` parameter to `true`.  

```swift
groupChannel.leave { e in
    if (e != nil) {
        // Handle error.
    }
}
```

## Retrieving channels

## Retrieve a list of channels

You can retrieve a list of `OpenChannel` objects using the loadNextPage() method of an `OpenChannelListQuery` instance.

On the other hand, a list of `GroupChannel` objects can be retrieved through `GroupChannelCollection` and its `loadMore()` method. First, create a `GroupChannelListQuery` instance. The `GroupChannelListQuery` instance returns both public and private group channels that the current user has joined. To retrieve a list of certain group channels such as public group channels or Supergroup channels, use the list query's filters as described at the bottom of this page.

Note: You can also search for specific [open channels]() and [group channels]() with keywords and filters.  

---

### Open channels

Create an `OpenChannelListQuery` instance to retrieve a list of open channels matching the specifications set by `OpenChannelListQueryParams`. After a list of open channels is successfully retrieved, you can access the data of each open channel from the result list through the `channels` parameter of the callback handler.  

```swift
let query = OpenChannel.createOpenChannelListQuery(OpenChannelListQueryParams())
query.loadMore { channels, e in
    if (e != nil) {
        // Handle error.
    }
}
```
---

### Group channels

A group channel list view can be drawn with a [`GroupChannelCollection`](?tab=t.6ppoc2h9dwd1) instance. First, create a `GroupChannelListQuery` instance through the `createMyGroupChannelListQuery()` method and its query setters. This determines which channel to include in the channel list and how to list channels in order.

To retrieve group channels in the collection, call `hasNext` first to check whether there are more channels to load for the collection. If so, call `loadMore()`.

To learn more about how the collection works, see [Group channel collection](?tab=t.6ppoc2h9dwd1) under Local caching.  

```swift
// Call hasMore first to check if there are more channels to load.
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

#### Filter group channels using GroupChannelListQuery

When creating a `GroupChannelCollection` instance, you need to set params within `GroupChannelListQuery` first. The params in `GroupChannelListQuery` works much like other [search options](), retrieving a list of group channels matching the specifications set by `GroupChannelListQueryParams`. For example, use `myMemberStateFilter` when searching for only the group channels that the current user belongs to.

Once the params are set, you can use the query instance for `GroupChannelCollectionCreateParams` in `createGroupChannelCollection()`.  

```swift
// Create a GroupChannelListQuery instance in GroupChannelCollection.
let params = GroupChannelListQueryParams()
params.order = .latestLastMessage
params.limit = VYINUGroupChannelListViewModel.channelLoadLimit
params.includeEmptyChannel = true
params.includeMetaData = true

let query = GroupChannel.createMyGroupChannelListQuery(params: params)
```

## Retrieve a channel by URL

Since a channel URL is a unique identifier of a channel, you can use a URL when retrieving a channel object. We recommend that you store a user's channel URLs to handle the lifecycle or state changes of your app as well as any other unexpected situations. For example, when a user is disconnected from the Vyin Chat server by temporarily switching to another app, you can provide a smooth restoration of the user's state once the user has returned to your app by using the stored URL to fetch the appropriate channel instance.

### Open channel

```swift
OpenChannel.getChannel(url: CHANNEL_URL) { channel, e in
    if (e != nil) {
        // Handle error.
    }

    // Through the channel parameter of the callback handler,
    // the open channel object identified with CHANNEL_URL is returned by the Vyin Chat server,
    // and you can get the open channel's data from the result object.
}
```

#### Group channel

```swift
GroupChannel.getChannel(url: CHANNEL_URL) { channel, e in
    if (e != nil) {
        // Handle error.
    }

    // Through the channel parameter of the callback handler,
    // the group channel object identified with CHANNEL_URL is returned by the Vyin Chat server,
    // and you can get the group channel's data from the result object.
}
```

## Inviting users to a group channels

## Invite users a s members

## Invite users as members

To enter a private group channel, a user must be invited by members who are already in the group channel. On the other hand, an invitation isn't required to join a public group channel.  

```swift
let userIds: [String] = ["Tyler", "Young"]
groupChannel.inviteUserIds(userIds) { e in
    if (e != nil) {
        // Handle error.
    }
    // ...
}
```

You can also determine whether the newly invited user can see past messages in the channel or not. You can manage the settings on Vyin Chat Dashboard. Go to Settings \> Chat \> Channels \> Group channels, and you will see the Chat history option. If the option is turned on, newly joined members can view all messages sent before they joined the channel. If turned off, new members can only see messages sent after they were invited. By default, this option is turned on.

## Accept or decline an invitation

A user who is invited to a group channel can accept or decline the invitation. If a user accepts an invitation, they join the channel as a new member and can start chatting with other members. If the user declines the invitation, the invitation is no longer valid.

Users can join up to 2,000 group channels. When the number of group channels a user can join reaches the maximum number, new invitations are automatically declined.  

```swift
// Accept an invitation.
groupChannel.acceptInvitation { e in
    if (e != nil) {
        // Handle error.
    }
    // ...
}

// Decline an invitation.
groupChannel.declineInvitation { e in
    if (e != nil) {
        // Handle error.
    }
    // ...
}
```

You can set the channel invitation preference for your Vyin Chat application. The preference determines whether a user can automatically accept a group channel invitation or manually accept invitations. If the value of `setChannelInvitationPreference()` is set to `true`, invitations will be automatically accepted. If the value is `false`, users can either accept or decline invitations. By default, the value is set to `true`.  

```swift
// The default value of true means that
// a user is automatically added to a group channel
// even if the user hasn't accepted the invitation.
let autoAccept = false
GIMChat.setChannelInvitationPreference(autoAccept) { e in
    if (e != nil) {
        // Handle error.
    }
    // ...
}
```

If the client app is in the foreground, the members of the group channel are notified of whether the newly invited user has accepted or declined the invitation. To do so, implement the GroupChannelDelegate:

```swift
func channel(_ channel: GroupChannel, didReceiveInvitation invitees: [User]?, inviter: User?) {}

func channel(_ channel: GroupChannel, didDeclineInvitation invitee: User, inviter: User?) {}
```

handlers of a channel event handler. For more information, see the event handler page.

## Searching channels

## Search open channels by name, URL, or custom types

## Search open channels by name, URL, or custom type

You can search for specific open channels by adding keywords to an `OpenChannelListQuery` instance. There are two types of keywords, which are name and URL.

The code sample below shows the query instance which returns a list of open channels that partially match the specified `nameKeyword` in their channel names.  

```swift
let params = OpenChannelListQueryParams()
params.channelNameFilter = KEYWORD

let query = OpenChannel.createOpenChannelListQuery(params: params)
query.loadNextPage { channels, e in
    if (e != nil) {
        // Handle error.
    }

    // Through the channels parameter of the callback handler, which the Vyin Chat server has passed a result list to,
    // a list of open channels whose name partially matches the filter value is returned.
    // ...
}
```

The following shows the query instance which returns a list of open channels that partially match the specified `urlKeyword` in their channel URLs.  

```swift
let params = OpenChannelListQueryParams()
params.channelURLFilter = CHANNEL_KEYWORD

let query = OpenChannel.createOpenChannelListQuery(params: params)

query.loadNextPage { channels, e in
    if (e != nil) {
        // Handle error.
    }

    // Through the channels parameter of the callback handler,
    // which the Vyin Chat server has passed a result list to,
    // a list of open channels whose URL partially matches the filter value is returned.
    // ...
}
```

You can also search for open channels with a specific custom type by setting `customTypeFilter` as in the following code.  

```swift
let params = OpenChannelListQueryParams()
params.customTypeFilter = CUSTOM_TYPE

let query = OpenChannel.createOpenChannelListQuery(params: params)
query.loadNextPage { channels, e in
    if (e != nil) {
        // Handle error.
    }

    // Through the channels parameter of the callback handler,
    // which the Vyin Chat server has passed a result list to,
    // a list of open channels whose custom type matches the filter value is returned.
    // ...
}
```

## Search group channels by name, URL, or other filte

## Search group channels by name, URL, or other filters

You can search for specific group channels by using several types of search filters within the `GroupChannelListQuery` class.  

---

### Search private group channels

A `GroupChannelListQuery` instance provides many types of search filters such as `channelNameContainsFilter`. You can use these filters to search for specific private group channels.

The sample code below shows the query instance, which returns a list of the current user's group channels that partially match the specified keyword in `channelNameContainsFilter` in their channel names.  

```swift
let params = GroupChannelListQueryParams()
params.channelNameContainsFilter = KEYWORD

let query = GroupChannel.createMyGroupChannelListQuery(params: params)
query.loadNextPage { channels, e in
    if (e != nil) {
        // Handle error.
    }

    // Through the channels parameter of the callback method
    // which the Vyin Chat server has passed a result list to,
    // a list of group channels that have "GIM"
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
| `channelNameExactFilter` | Group channels that exactly match the specified value in their names. You can enable this filter using the `channelNameExactFilter` property. |
| `unreadChannelFilter` | Group channels with one or more unread messages. Using the `unreadChannelFilter` property, you can enable this filter. |
| `channelHiddenStateFilter` | Group channels with the specified state and operating behavior. You can enable this filter using the `channelHiddenStateFilter` property. |
| `myMemberStateFilter` | Group channels based on whether the user has accepted an invitation. You can enable this filter using the `myMemberStateFilter` property. |

---

## Filter group channels by user IDs

To filter group channels by user IDs, use the `userIdsExactFilter` or `userIdsIncludeFilter` filter. Let's assume the ID of the current user is Harry and the user is a member of two group channels.

* Channel A consists of Harry, John, and Jay.  
* Channel B consists of Harry, John, Jay, and Jin.

A `userIdsExactFilter` filter returns a list of the current user's group channels containing exactly the queried user IDs. In case you specify only one user ID in the filter, the filter returns a list of the current user's one or more 1-to-1 group channels with the specified user.  

```swift
let params = GroupChannelListQueryParams()
params.userIdsExactFilter = [String?]

let query = GroupChannel.createMyGroupChannelListQuery(params: params)
query.loadNextPage { channels, e in
    // Only Channel A is returned in a result list
    // through the list parameter of the callback method.
}
```

The `userIdsIncludeFilter` filter returns a list of the current user's group channels including the queried user IDs partially and exactly. Two different results can be returned according to the value of the `queryType` parameter.

```swift
let params = GroupChannelListQueryParams()
params.setUserIdsIncludeFilter(["John", "Jay", "Jin"], GroupChannelListQueryType.and)

let query = GroupChannel.createMyGroupChannelListQuery(params: params)

query.loadNextPage { channels, e in
    // Only Channel B containing {'John', 'Jay', 'Jin'} as a
    // subset is returned in a result list.
}
```

## Categorizing channels

## Categorize channels by custom type

When creating an open channel or a group channel, you can additionally specify a custom channel type to subclassify channels. This custom type takes on the form of `String`, and can be useful in searching or filtering channels.

The `data` and `customType` properties of a channel object allow you to append information to channels. While both properties can be used flexibly, common examples for `customType` include categorizing channels as "School" or "Work".

### Open channel

To get an open channel's custom type, read the `openChannel.customType` property.  

```swift
var params = OpenChannelCreateParams()
params.name = NAME
params.coverUrl = COVER_IMAGE_URL
params.coverImage = DATA
params.operatorUserIds = OPERATOR_USER_IDS
params.customType = CUSTOM_TYPE
OpenChannel.createChannel(params: params) { channel, e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```

#### Group channel

To get a group channel's custom type, read the `groupChannel.customType` property.  

```swift
let users = ["Tyler", "Nathan"]
var params = GroupChannelCreateParams()
    params.userIds = users
    params.name = NAME
    params.customType = CUSTOM_TYPE
GroupChannel.createChannel(params: params) { channel, e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```

## Managing channels

## Delete a channel

Only [channel operators](?tab=t.idg08sfacq58) are allowed to delete a channel. To delete a channel, follow the code below.

### Open channel

```swift
openChannel.delete { e in
    if (e != nil) {
        // Handle error.
    }

    // The channel is successfully deleted from the client app.
}
```

#### Group channel

```swift
groupChannel.delete { e in
    if (e != nil) {
        // Handle error.
        return
    }
    // The channel is successfully deleted from the client app.
}
```

## Hide or archive a group channel

You can hide or archive a specific group channel from the channel list UI by following the code below.  

```swift
// Hide or archive a group channel.
groupChannel.hide(hidePreviousMessages, allowAutoUnhide) { e in
    if (e != nil) {
        // Handle error.
    }

    // The channel is successfully hidden from the list.
    // The current user's channel view should be refreshed to reflect the change.
    // ...
}

// Unhide a group channel.
groupChannel.unhide { e in
    if (e != nil) {
        // Handle error.
    }

    // The channel is successfully unhidden from the list.
    // The current user's channel view should be refreshed to reflect the change.
}
```

### List of parameters

| Parameter name | Type | Description |
| :---- | :---- | :---- |
| `hidePreviousMessages` | Bool | Determines whether to show the messages sent and received before hiding or archiving the channel on the channel list. If set to `true`, previous messages aren't displayed in the channel. (Default: `false`) |
| `allowAutoUnhide` | Bool | Determines the state and operating behavior of a channel. If set to `true`, the channel is hidden from the channel list, but when a new message arrives, the hidden channel will show up again on the channel list. If set to `false`, the channel is archived and stays hidden from the channel list unless the `unhide()` method is called. |

You can check the channel state on the channel list by using the `hiddenState` property of a `GroupChannel` object.  

```swift
switch (groupChannel.hiddenState) {
    case .unhidden:
        // Show the channel in the list.
    case .hiddenAllowAutoUnhide:
        // Hide the channel from the list, and get it appeared back on condition.
    case .hiddenPreventAutoUnhide:
        // Archive the channel, and get it appeared back in the list only when unhide() is called.
}
```

You can also filter channels by their state by following the code below.  

```swift
let query = GroupChannel.createMyGroupChannelListQuery(
    GroupChannelListQueryParams(builder: { params in
        params.includeEmpty = true
        // The filter options are ALL, UNHIDDEN, HIDDEN, HIDDEN_ALLOW_AUTO_UNHIDE, and HIDDEN_PREVENT_AUTO_UNHIDE.
        // If set to HIDDEN, hidden and archived channels are returned.
        params.channelHiddenStateFilter = .hiddenAllowAutoUnhide
    }
)
query.next { channels, e in
        // Only archived channels are returned in a result list
        // through the channels parameter of the callback handler.
}
```

## Refresh all data related to a group channel

When a user is online, all data associated with the group channels they are a member of are automatically updated. However, when a user is disconnected from the Vyin Chat server and reconnects later, the `refresh()` method should be called to update the channels with the latest information.  

```swift
groupChannel.refresh { e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```

Note: If you want to provide the updated channels with the latest information when your user's app is in the foreground, call `refresh()` in the `onReconnectSucceeded()` method which receives a callback from the Vyin Chat server when successfully reconnected.

## Moderating a channel

## Freeze and unfreeze a channel

Open channels and group channels can be freezed and unfreezed. When a channel is freezed, only the operators can send messages to the channel and users aren't allowed to chat.

An open channel can be freezed only by using Chat API or on Vyin Chat Dashboard. To freeze an open channel, go to Chat \> Open channels on the dashboard, select an open channel you want to freeze, and click the Freeze icon at the upper right corner. To unfreeze, select the frozen channel and click the Freeze icon again.

Group channels can be freezed by operators using the Chat SDK as shown in the code below.  

```swift
// Freezes a channel.
groupChannel.freeze { e in
    if (e != nil) {
        // Handle error.
    }

    // The channel is frozen.
    // You can send a message to announce that
    // chatting in the channel is unavailable,
    // or do something in response to a successful operation.
}

// Unfreezes a channel.
groupChannel.unfreeze { e in
    if (e != nil) {
        // Handle error.
    }

    // The channel is unfreezed.
    // You can send a message to announce that
    // chatting in the channel is available,
    // or do something in response to a successful operation.
}
```

## Managing channel metadata

## Manage channel metadata

You can store additional information to channels such as background color or channel description with channel metadata, which can be fetched or rendered into the UI. Channel metadata is `Map<String, String>` and it can be stored into a `Channel` object.

To receive and retrieve information about events happening in channels from the Vyin Chat server, you can add a channel event handler. For more information on channel event handlers, see [Channel event types](?tab=t.ihxhjtpz6j4i).  

---

### Create metadata

To store channel metadata into a `Channel` object, create a new object of key-value items in which the data type of the key and value is `Map<String, String>`. Then, pass the object as an argument to a parameter when calling the `createMetaData()` method. You can put multiple key-value items in the dictionary.  

```swift
let data: [String: String] = ["key1": "value1", "key2": "value2"]
channel.createMetaData(data) { map, e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```
---

### Update metadata

The process of updating channel metadata is the same as creating one. Values of existing keys can be updated and values of new keys can be added by calling the `updateMetaData()` method.  

```swift
let data = [
    "key1": "valueToUpdate1", // Update an existing item with a new value.
    "key2": "valueToUpdate2", // Update an existing item with a new value.
    "key3":"valueToAdd3"]     // Add a new key-value item.
)
channel.updateMetaData(data) { map, e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```
---

### Retrieve metadata

You can retrieve channel metadata by creating an `List` of keys to retrieve and passing it as an argument to a parameter in the `getMetaData()` method. A `Map<String, String>` collection will return through the `MetaDataHandler` callback function with corresponding key-value items.  

```swift
let keys = ["key1", "key2"]
channel.getMetaData(keys) { map, e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```
---

### Retrieve cached metadata

When Vyin Chat SDK detects any of the `create`, `read`, `update`, and `delete` operations on the channel metadata, the SDK caches the metadata. The cached metadata is also updated whenever a channel list is fetched.

You can retrieve the cached metadata through the `getCachedMetaData()` method without having to query the server.  

```swift
let cachedMetaData = channel.getCachedMetaData()
let value = cachedMetaData[key] // The key should be a String.
```
---

### Delete metadata

You can delete channel metadata by calling the `deleteMetaData()` method.  

```swift
channel.deleteMetaData("key1") { e in
    if (e != nil) {
        // Handle error.
    }

    // ...
}
```

