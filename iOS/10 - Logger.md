# Vyin Chat SDK for iOS - Logger

## Logger

Vyin Chat SDK for iOS offers a logging system that allows you to keep track of a number of events and activities including data flow, error, and information while running your app. You can closely monitor the operation of the Vyin Chat SDK and improve debug efficiency using our logging system.  

---

### Log levels

Vyin Chat SDK's logger is based on the log levels classified by iOS. Log levels can be used to categorize and control log outputs. There are a total of six log levels, which are `ALL` (verbose), `DEBUG`, `INFO`, `WARN`, `ERROR`, and `NONE`. Apart from `NONE`, the other five log levels take precedence over the other in the following order: `ALL` (verbose) \> `DEBUG` \> `INFO` \> `WARN` \> `ERROR`. If you would rather not use the logger, you can select `NONE` to opt out of recording `GIM` logs.

| Level | Description |
| :---- | :---- |
| `NONE` | No logs recorded. |
| `ALL` | Logs detailed information about all events and activities, including the log messages in `DEBUG`, `INFO`, `WARN`, and `ERROR`. |
| `DEBUG` | Logs what's happening inside the SDK that could be helpful to debug unexpected behaviors, as well as the log messages in `INFO`, `WARN`, and `ERROR`. |
| `INFO` | Logs the general events that take place in the Vyin Chat SDK, as well as the log messages in `WARN` and `ERROR`. |
| `WARN` | Logs unexpected events which wouldn’t affect the operation of Vyin Chat SDK but might cause other issues. This log level also shows the log messages in `ERROR`. |
| `ERROR` | Logs things that have caused failures in specific events, but not an SDK-wide failure. |

Note: If an unexpected behavior is caused by a user's mistake or environmental factors, the behavior is classified as `WARN`. If it is due to the Vyin Chat SDK or Vyin Chat internal operations, it is classified as `ERROR`.  

---

### How to configure the log level

The default log level set for Vyin Chat SDK for iOS is `LogLevel.WARN`, which means that Vyin Chat SDK keeps logs of both errors and warning messages. You can change the settings through the `logLevel` property in the `InitParams` class and pass it on when initializing the SDK as follows.  

```swift
let params = InitParams(applicationId: applicationId, isLocalCachingEnabled: true, logLevel: .verbose)
GIMChat.initialize(params: params)
```

| Parameter | Type | Description |
| :---- | :---- | :---- |
| `logLevel` | LogLevel | Specifies the severity level of the log to retrieve. One takes precedence over the other in the order of `VERBOSE`, `DEBUG`, `INFO`, `WARN`, and `ERROR`. You can also use `NONE` to stop recording `GIM` logs. |

---

### Log filtering

All Vyin Chat SDK log messages start with `GIM`. If you want to see the log messages from Vyin Chat SDK, search for messages with the keyword `GIM`.















