# Vyin Chat SDK for Android - Error Codes

## Error codes

Vyin Chat SDK for Android has two types of error codes.

- **Client error codes:** These errors are caused by the client app side, such as entering an incorrect or invalid parameter, or sending a request when disconnected from the Vyin Chat server.
- **Server error codes:** These errors are caused by the Vyin Chat server side.

---

## Client error codes

The following are errors labeled with six-digit integers beginning with 800 that are defined in `GIMError.kt`.

| Error | Code | Description |
| :---- | :---- | :---- |
| `ERR_INVALID_INITIALIZATION` | 800100 | This error occurs when the GIMChat initialization fails. This could be due to an invalid APP_ID or repeated initialization attempts. |
| `ERR_CONNECTION_REQUIRED` | 800101 | The request from a client app failed because the device wasn't connected to the server. |
| `ERR_CONNECTION_CANCELED` | 800102 | The connection is canceled or the disconnecting method is called while the `GIMChat` instance is trying to connect to the server. |
| `ERR_INVALID_PARAMETER` | 800110 | The parameter of the method specifies an invalid value. |
| `ERR_NETWORK` | 800120 | The connection failed due to the unstable network or an unexpected error in the Chat SDK network library. |
| `ERR_NETWORK_ROUTING_ERROR` | 800121 | The request routing to the server failed. |
| `ERR_MALFORMED_DATA` | 800130 | The data format of the server response is invalid. |
| `ERR_MALFORMED_ERROR_DATA` | 800140 | The data format of the error message is invalid due to the problem with the request. |
| `ERR_WRONG_CHANNEL_TYPE` | 800150 | The specified channel type in the request is invalid. |
| `ERR_MARK_AS_READ_RATE_LIMIT_EXCEEDED` | 800160 | The interval between the successive requests is less than a second. |
| `ERR_QUERY_IN_PROGRESS` | 800170 | A retrieval request is arriving while the server is still processing the previous retrieval request for channels, messages, or users, and in preparation to send the response. |
| `ERR_ACK_TIMEOUT` | 800180 | The server failed to send a response for the request in 10 seconds. |
| `ERR_LOGIN_TIMEOUT` | 800190 | The server failed to send a response for the `GIMChat` instance's login request in 10 seconds. |
| `ERR_WEBSOCKET_CONNECTION_CLOSED` | 800200 | The request was submitted while disconnected from the server. |
| `ERR_WEBSOCKET_CONNECTION_FAILED` | 800210 | The WebSocket connection to the server failed to establish. |
| `ERR_REQUEST_FAILED` | 800220 | The server failed to process the request due to an internal reason. |
| `ERR_FILE_UPLOAD_CANCEL_FAILED` | 800230 | The request to cancel file upload failed due to an unexpected error. |
| `ERR_FILE_UPLOAD_CANCELED` | 800240 | The file upload request is canceled. |
| `ERR_FILE_UPLOAD_TIMEOUT` | 800250 | The file upload request timed out before the server confirmed completion. Retry the upload or check network stability. |
| `ERR_FILE_SIZE_LIMIT_EXCEEDED` | 800260 | The file size exceeds the maximum allowed limit. Reduce the file size before uploading. |
| `ERR_PENDING` | 800400 | The operation is pending and hasn't been processed yet. |
| `ERR_PARSED_INVALID_ACCESS_TOKEN` | 800500 | The access token could not be parsed because it specifies an invalid value. |
| `ERR_SESSION_KEY_REFRESH_SUCCEEDED` | 800501 | The session key was successfully refreshed. This is an informational code, not an error. |
| `ERR_SESSION_KEY_REFRESH_FAILED` | 800502 | The session key refresh failed. Re-authenticate the user to obtain a new session key. |
| `ERR_COLLECTION_DISPOSED` | 800600 | The collection object has been disposed and can no longer be used. Re-initialize the collection before performing further operations. |
| `ERR_DATABASE_ERROR` | 800700 | Something went wrong with the database. While the SDK continues to operate, local caching is disabled. |

---

## Server error codes

The following errors are six-digit integers beginning with 400, 500, and 900.

| Code | Description |
| :---- | :---- |
| `400100 (bad request)` | UNEXPECTED_PARAMETER_TYPE — The request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `string`. |
| `400101 (bad request)` | UNEXPECTED_PARAMETER_TYPE — The request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `number`. |
| `400102 (bad request)` | UNEXPECTED_PARAMETER_TYPE — The request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `list`. |
| `400103 (bad request)` | UNEXPECTED_PARAMETER_TYPE — The request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `JSON`. |
| `400104 (bad request)` | UNEXPECTED_PARAMETER_TYPE — The request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `boolean`. |
| `400105 (bad request)` | MISSING_REQUIRED_PARAMETERS — The request is missing one or more required parameters. |
| `400106 (bad request)` | NEGATIVE_NUMBER_NOT_ALLOWED — The parameter specifies a negative number. Its value should be a positive number. |
| `400107 (bad request)` | INVALID_PARAMETER_VALUE_NEGATIVE — The parameter specifies a negative number where only positive values are accepted. |
| `400108 (bad request)` | UNAUTHORIZED_REQUEST — The request isn't authorized and can't access the requested resource. |
| `400109 (bad request)` | EXPIRED_PAGE_TOKEN — The value of the `token` parameter for pagination has expired. |
| `400110 (bad request)` | PARAMETER_VALUE_LENGTH_EXCEEDED — The length of the parameter value is too long. |
| `400111 (bad request)` | INVALID_VALUE — The request specifies an invalid value. |
| `400112 (bad request)` | INCOMPATIBLE_VALUES — The two parameters of the request, which should have unique values, specify the same value. |
| `400113 (bad request)` | PARAMETER_VALUE_OUT_OF_RANGE — The request specifies one or more parameters which are outside the allowed value range. |
| `400114 (bad request)` | INVALID_URL_OF_RESOURCE — The resource identified with the URL in the request can't be found. |
| `400151 (bad request)` | NOT_ALLOWED_CHARACTER — The request specifies an illegal value containing special character, empty string, or white space. |
| `400201 (bad request)` | RESOURCE_NOT_FOUND — The resource identified with the request's `resourceId` parameter can't be found. |
| `400202 (bad request)` | RESOURCE_ALREADY_EXISTS — The resource identified with the request's `resourceId` parameter already exists. |
| `400203 (bad request)` | TOO_MANY_ITEMS_IN_PARAMETER — The parameter specifies more items than allowed. |
| `400300 (bad request)` | DEACTIVATED_USER_NOT_ACCESSIBLE — The request can't retrieve the deactivated user data. |
| `400301 (bad request)` | USER_NOT_FOUND — The user identified with the request's `USER_ID` can't be found because either the user doesn't exist or has been deleted. |
| `400302 (bad request)` | INVALID_ACCESS_TOKEN — The access token provided for the request specifies an invalid value. |
| `400303 (bad request)` | INVALID_SESSION_KEY_VALUE — The session key provided for the request specifies an invalid value. |
| `400304 (bad request)` | APPLICATION_NOT_FOUND — The application identified with the request can't be found. |
| `400305 (bad request)` | USER_ID_LENGTH_EXCEEDED — The length of the `USER_ID` is too long. |
| `400309 (bad request)` | SESSION_KEY_EXPIRED — The session key provided for the request has expired. Re-authenticate the user to obtain a new session key. |
| `400310 (bad request)` | SESSION_TOKEN_REVOKED — The session token has been revoked and is no longer valid. Re-authenticate the user to obtain a new token. |
| `400306 (bad request)` | PAID_QUOTA_EXCEEDED — The request can't be completed because you have exceeded your paid quota. |
| `400307 (bad request)` | DOMAIN_NOT_ALLOWED — The request can't be completed because it came from the restricted domain. |
| `400401 (bad request)` | INVALID_API_TOKEN — The API token provided for the request specifies an invalid value. |
| `400402 (bad request)` | MISSING_SOME_PARAMETERS — The request is missing one or more necessary parameters. |
| `400403 (bad request)` | INVALID_JSON_REQUEST_BODY — The request body is an invalid JSON. |
| `400404 (bad request)` | INVALID_REQUEST_URL — The request specifies an invalid HTTP request URL that can't be accessed. |
| `400500 (bad request)` | TOO_MANY_USER_WEBSOCKET_CONNECTIONS — The number of the user's WebSocket connections exceeds the allowed amount. |
| `400501 (bad request)` | TOO_MANY_APPLICATION_WEBSOCKET_CONNECTIONS — The number of the application's WebSocket connections exceeds the allowed amount. |
| `400700 (bad request)` | BLOCKED_USER.SEND_MESSAGE_NOT_ALLOWED — The request can't be completed due to one of the following reasons: The sender is blocked by the recipient or has been deactivated, the message is longer than the maximum message length, or the message contains texts or URLs blocked by application settings or filters. |
| `400701 (bad request)` | BLOCKED_USER.INVITED_NOT_ALLOWED — The request can't be completed because the blocking user is trying to invite the blocked user to a channel. |
| `400702 (bad request)` | BLOCKED_USER.INVITE_NOT_ALLOWED — The request can't be completed because the blocked user is trying to invite the blocking user to a channel. |
| `400750 (bad request)` | BANNED_USER.ENTER_CHANNEL_NOT_ALLOWED — The request can't be completed because the user is banned from a channel they're trying to enter. |
| `400751 (bad request)` | BANNED_USER.ENTER_CUSTOM_CHANNEL_NOT_ALLOWED — The request can't be completed because the user is banned from a custom channel they're trying to enter. |
| `400920 (bad request)` | UNACCEPTABLE — The request failed because the combination of parameter values is invalid. Even if each parameter value is valid, a combination of parameter values becomes invalid when it doesn't follow specific conditions. READ_ONLY_MODE: When the application is in the read-only mode, can't complete API requests except for `GET`. NOT_ACCESSIBLE: The `DELETE` request fails when the corresponding message has a child message. |
| `400930 (bad request)` | INVALID_ENDPOINT — The request failed because it is sent to an invalid endpoint that no longer exists. |
| `403100 (forbidden)` | APPLICATION_NOT_AVAILABLE — The application identified with the request isn't available. |
| `500601 (internal server error)` | INTERNAL_ERROR.PUSH_TOKEN_NOT_REGISTERED — The server encounters an error while trying to register the user's push token. |
| `500602 (internal server error)` | INTERNAL_ERROR.PUSH_TOKEN_NOT_UNREGISTERED — The server encounters an error while trying to unregister the user's push token. |
| `500901 (internal server error)` | INTERNAL_ERROR — The server encounters an unexpected exception while trying to process the request. |
| `500910 (too many requests)` | RATE_LIMIT_EXCEEDED — The request can't be completed because you have exceeded your rate limits. |
| `503 (service unavailable)` | SERVICE_UNAVAILABLE — The request failed due to a temporary failure of the server. |
| `900010 (request failed)` | USER_LOGIN_REQUIRED — The request failed because the user isn't logged in to the server. |
| `900020 (request failed)` | USER_NOT_MEMBER — The request failed because the user isn't a member of the channel. |
| `900021 (request failed)` | USER_DEACTIVATED — The request failed because the user is deactivated. |
| `900022 (request failed)` | USER_NOT_OWNER_OF_MESSAGE — The request failed because the user has no permission to edit the other user's message. |
| `900023 (request failed)` | PENDING_USER_SEND_MESSAGE_NOT_ALLOWED — The request failed because the user is trying to send messages in the channel of which they are not the member. |
| `900025 (request failed)` | INVALID_MENTION_FOR_MESSAGE — The specified mention type in the request is invalid. |
| `900026 (request failed)` | INVALID_PUSH_OPTION_FOR_MESSAGE — The specified push option in the request is invalid. |
| `900027 (request failed)` | TOO_MANY_META_KEY_FOR_MESSAGE — The request failed because it specifies more meta data keys for the message than allowed. |
| `900028 (request failed)` | TOO_MANY_META_VALUE_FOR_MESSAGE — The request failed because it specifies more meta data values for the message than allowed. |
| `900029 (request failed)` | INVALID_META_ARRAY_FOR_MESSAGE — The request failed because it specifies an invalid value in the meta data for the message. |
| `900030 (request failed)` | GUEST_NOT_ALLOWED — The request failed because the guest isn't allowed to perform this operation. |
| `900040 (request failed)` | MUTED_USER_IN_APPLICATION_SEND_MESSAGE_NOT_ALLOWED — The request failed because the user is muted in the application and isn't allowed to send the message. |
| `900041 (request failed)` | MUTED_USER_IN_CHANNEL_SEND_MESSAGE_NOT_ALLOWED — The request failed because the user is muted in the channel and isn't allowed to send the message. |
| `900050 (request failed)` | CHANNEL_FROZEN — The request failed because the channel is frozen and no one can send the message to the channel. |
| `900060 (request failed)` | PROFANITY_MESSAGE_BLOCKED — The request failed because it specifies the message containing a profanity word. |
| `900061 (request failed)` | BANNED_URLS_BLOCKED — The request failed because it specifies the message containing a URL that isn't allowed. |
| `900065 (request failed)` | RESTRICTED_DOMAIN_BLOCKED — The request failed because it comes from the domain that isn't allowed. |
| `900066 (request failed)` | MODERATED_FILE_BLOCKED — The request failed because it contains the file violating at least one of the content management policies. |
| `900070 (request failed)` | ENTER_DELETED_CHANNEL — The request failed because the user is trying to enter a deleted channel. |
| `900080 (request failed)` | BLOCKED_USER_RECEIVE_MESSAGE_NOT_ALLOWED — The request failed because the blocking user is trying to send the message to the blocked user in a 1-to-1 distinct channel. |
| `900081 (request failed)` | DEACTIVATED_USER_RECEIVE_MESSAGE_NOT_ALLOWED — The request failed because the user is trying to send the message to the deactivated user in a 1-to-1 distinct channel. |
| `900090 (request failed)` | WRONG_CHANNEL_TYPE — The request failed because it specifies a wrong channel type. |
| `900100 (request failed)` | BANNED_USER_SEND_MESSAGE_NOT_ALLOWED — The request failed because the user is banned from the channel and isn't allowed to send the message. |
| `900200 (request failed)` | TOO_MANY_MESSAGES — The number of the sent messages exceeds the allowed amount. |
| `900300 (request failed)` | MESSAGE_NOT_FOUND — The request failed because the message to edit can't be retrieved. |
| `900400 (request failed)` | TOO_MANY_PARTICIPANTS — The number of the channel's participants exceeds the allowed amount. |
| `900500 (request failed)` | CHANNEL_NOT_FOUND — The request failed because there is no channel to perform this operation. |
| `900800 (request failed)` | NOT_OPERATOR — The request failed because the user doesn't have operator-level permissions required for this operation. |
