# Vyin Chat SDK for iOS - Error Codes

## Error codes

Vyin Chat SDK for iOS has two types of error codes.

* Client error codes: These errors are caused by the client app side, such as entering an incorrect or invalid parameter, or sending a request when disconnected from the Vyin Chat server.  
* Server error codes: These errors are caused by the Vyin Chat server side.

---

### Client error codes

The following are errors labeled with six-digit integers beginning with 800 that are defined in `GIMError`.

| Error | Code | Description |
| :---- | :---- | :---- |
| `ERR_INVALID_INITIALIZATION` | 800100 | This error occurs when the GIMChat initialization fails . This could be due to an invalid APP\_ID or repeated initialization attempts. |
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
| `ERR_WEBSOCKET_CONNECTION_FAILED` | 800210 | The websocket connection to the server failed to establish. |
| `ERR_REQUEST_FAILED` | 800220 | The server failed to process the request due to an internal reason. |
| `ERR_FILE_UPLOAD_CANCEL_FAILED` | 800230 | The request to cancel file upload failed due to an unexpected error. |
| `ERR_FILE_UPLOAD_CANCELED` | 800240 | The file upload request is canceled. |
| `ERR_INITIALIZATION_CANCELED` | 800103 | This error is triggered if the SDK initialization exceeds five seconds. It's a safeguard to avoid app unresponsiveness (ANR). Retrying initialization might work. |
| `ERR_DATABASE_ERROR` | 800700 | Something went wrong with the database. While the SDK continues to operate, local caching is disabled. |
| `ERR_DATABASE_ERROR_ENCRYPTION` | 800701 | This relates to sqlcipher issues, possibly due to an undeclared sqlcipher dependency or an erroneous encryption key. If the password is forgotten, clearing the data with `GIMChat.clearCachedData` is advised. |

---

### Server error codes

The following errors are six-digit integers beginning with 400, 500, and 900\.

| Code | Description |
| :---- | :---- |
| `400100(bad request)` | UNEXPECTED\_PARAMETER\_TYPEThe request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `String`. |
| `400101(bad request)` | UNEXPECTED\_PARAMETER\_TYPEThe request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `number`. |
| `400102(bad request)` | UNEXPECTED\_PARAMETER\_TYPEThe request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `list`. |
| `400103(bad request)` | UNEXPECTED\_PARAMETER\_TYPEThe request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `JSON`. |
| `400104(bad request)` | UNEXPECTED\_PARAMETER\_TYPEThe request specifies one or more parameters in an unexpected data type. The data type of the parameters should be `Bool`. |
| `400105(bad request)` | MISSING\_REQUIRED\_PARAMETERSThe request is missing one or more required parameters. |
| `400106(bad request)` | NEGATIVE\_NUMBER\_NOT\_ALLOWEDThe parameter specifies a negative number. Its value should be a positive number. |
| `400108(bad request)` | UNAUTHORIZED\_REQUESTThe request isn't authorized and can't access the requested resource. |
| `400109(bad request)` | EXPIRED\_PAGE\_TOKENThe value of the `token` parameter for pagination has expired. |
| `400110(bad request)` | PARAMETER\_VALUE\_LENGTH\_EXCEEDEDThe length of the parameter value is too long. |
| `400111(bad request)` | INVALID\_VALUEThe request specifies an invalid value. |
| `400112(bad request)` | INCOMPATIBLE\_VALUESThe two parameters of the request, which should have unique values, specify the same value. |
| `400113(bad request)` | PARAMETER\_VALUE\_OUT\_OF\_RANGEThe request specifies one or more parameters which are outside the allowed value range. |
| `400114(bad request)` | INVALID\_URL\_OF\_RESOURCEThe resource identified with the URL in the request can't be found. |
| `400151(bad request)` | NOT\_ALLOWED\_CHARACTERThe request specifies an illegal value containing special character, empty String, or white space. |
| `400201(bad request)` | RESOURCE\_NOT\_FOUNDThe resource identified with the request's `resourceId` parameter can't be found. |
| `400202(bad request)` | RESOURCE\_ALREADY\_EXISTSThe resource identified with the request's `resourceId` parameter already exists. |
| `400203(bad request)` | TOO\_MANY\_ITEMS\_IN\_PARAMETERThe parameter specifies more items than allowed. |
| `400300(bad request)` | DEACTIVATED\_USER\_NOT\_ACCESSIBLEThe request can't retrieve the deactivated user data. |
| `400301(bad request)` | USER\_NOT\_FOUNDThe user identified with the request's `USER_ID` can't be found because either the user doesn't exist or has been deleted. |
| `400302(bad request)` | INVALID\_ACCESS\_TOKENThe access token provided for the request specifies an invalid value. |
| `400303(bad request)` | INVALID\_SESSION\_KEY\_VALUEThe session key provided for the request specifies an invalid value. |
| `400304(bad request)` | APPLICATION\_NOT\_FOUNDThe application identified with the request can't be found. |
| `400305(bad request)` | USER\_ID\_LENGTH\_EXCEEDEDThe length of the `USER_ID` is too long. |
| `400306(bad request)` | PAID\_QUOTA\_EXCEEDEDThe request can't be completed because you have exceeded your paid quota. |
| `400307(bad request)` | DOMAIN\_NOT\_ALLOWEDThe request can't be completed because it came from the restricted domain. |
| `400401(bad request)` | INVALID\_API\_TOKENThe API token provided for the request specifies an invalid value. |
| `400402(bad request)` | MISSING\_SOME\_PARAMETERSThe request is missing one or more necessary parameters. |
| `400403(bad request)` | INVALID\_JSON\_REQUEST\_BODYThe request body is an invalid JSON. |
| `400404(bad request)` | INVALID\_REQUEST\_URLThe request specifies an invalid HTTP request URL that can't be accessed. |
| `400500(bad request)` | TOO\_MANY\_USER\_WEBSOCKET\_CONNECTIONSThe number of the user's websocket connections exceeds the allowed amount. |
| `400501(bad request)` | TOO\_MANY\_APPLICATION\_WEBSOCKET\_CONNECTIONSThe number of the application's websocket connections exceeds the allowed amount. |
| `400700(bad request)` | BLOCKED\_USER.SEND\_MESSAGE\_NOT\_ALLOWEDThe request can't be completed due to one of the following reasons: The sender is blocked by the recipient or has been deactivated, the message is longer than the maximum message length, or the message contains texts or URLs blocked by application settings or filters. |
| `400701(bad request)` | BLOCKED\_USER.INVITED\_NOT\_ALLOWEDThe request can't be completed because the blocking user is trying to invite the blocked user to a channel. |
| `400702(bad request)` | BLOCKED\_USER.INVITE\_NOT\_ALLOWEDThe request can't be completed because the blocked user is trying to invite the blocking user to a channel. |
| `400750(bad request)` | BANNED\_USER.ENTER\_CHANNEL\_NOT\_ALLOWEDThe request can't be completed because the user is banned from a channel they're trying to enter. |
| `400751(bad request)` | BANNED\_USER.ENTER\_CUSTOM\_CHANNEL\_NOT\_ALLOWEDThe request can't be completed because the user is banned from a custom channel they're trying to enter. |
| `400920(bad request)` | UNACCEPTABLEThe request failed because the combination of parameter values is invalid. Even if each parameter value is valid, a combination of parameter values becomes invalid when it doesn't follow specific conditions.READ\_ONLY\_MODEWhen the application is in the read-only mode, can't complete API requests except for `GET`.NOT\_ACCESSIBLEThe request `DELETE` fails when the corresponding message has a child message. |
| `400930(bad request)` | INVALID\_ENDPOINTThe request failed because it is sent to an invalid endpoint that no longer exists. |
| `403100(forbidden)` | APPLICATION\_NOT\_AVAILABLEThe application identified with the request isn't available. |
| `500601(internal server error)` | INTERNAL\_ERROR.PUSH\_TOKEN\_NOT\_REGISTEREDThe server encounters an error while trying to register the user's push token. |
| `500602(internal server error)` | INTERNAL\_ERROR.PUSH\_TOKEN\_NOT\_UNREGISTEREDThe server encounters an error while trying to unregister the user's push token. |
| `500901(internal server error)` | INTERNAL\_ERRORThe server encounters an unexpected exception while trying to process the request. |
| `500910(too many request)` | RATE\_LIMIT\_EXCEEDEDThe request can't be completed because you have exceeded your rate limits. |
| `503(service unavailable)` | SERVICE\_UNAVAILABLEThe request failed due to a temporary failure of the server. |
| `900010(request failed)` | USER\_LOGIN\_REQUIREDThe request failed because the user isn't logged in to the server. |
| `900020(request failed)` | USER\_NOT\_MEMBERThe request failed because the user isn't a member of the channel. |
| `900021(request failed)` | USER\_DEACTIVATEDThe request failed because the user is deactivated. |
| `900022(request failed)` | USER\_NOT\_OWNER\_OF\_MESSAGEThe request failed because the user has no permission to edit the other user's message. |
| `900023(request failed)` | PENDING\_USER\_SEND\_MESSAGE\_NOT\_ALLOWEDThe request failed because the user is trying to send the messages in the channel of which they are not the member. |
| `900025(request failed)` | INVALID\_MENTION\_FOR\_MESSAGEThe specified mention type in the request is invalid. |
| `900026(request failed)` | INVALID\_PUSH\_OPTION\_FOR\_MESSAGEThe specified push option in the request is invalid. |
| `900027(request failed)` | TOO\_MANY\_META\_KEY\_FOR\_MESSAGEThe request failed because it specifies more meta data keys for the message than allowed. |
| `900028(request failed)` | TOO\_MANY\_META\_VALUE\_FOR\_MESSAGEThe request failed because it specifies more meta data values for the message than allowed. |
| `900029(request failed)` | INVALID\_META\_ARRAY\_FOR\_MESSAGEThe request failed because it specifies an invalid value in the meta data for the message. |
| `900030(request failed)` | GUEST\_NOT\_ALLOWEDThe request failed because the guest isn't allowed to perform this operation. |
| `900040(request failed)` | MUTED\_USER\_IN\_APPLICATION\_SEND\_MESSAGE\_NOT\_ALLOWEDThe request failed because the user is muted in the application and isn't allowed to send the message. |
| `900041(request failed)` | MUTED\_USER\_IN\_CHANNEL\_SEND\_MESSAGE\_NOT\_ALLOWEDThe request failed because the user is muted in the channel and isn't allowed to send the message. |
| `900050(request failed)` | CHANNEL\_FROZENThe request failed because the channel is frozen and no one can send the message to the channel. |
| `900060(request failed)` | PROFANITY\_MESSAGE\_BLOCKEDThe request failed because it specifies the message containing a profanity word. |
| `900061(request failed)` | BANNED\_URLS\_BLOCKEDThe request failed because it specifies the message containing a URL that isn't allowed. |
| `900065(request failed)` | RESTRICTED\_DOMAIN\_BLOCKEDThe request failed because it comes from the domain that isn't allowed. |
| `900066(request failed)` | MODERATED\_FILE\_BLOCKEDThe request failed because it contains the file violating at least one of the content management policies. |
| `900070(request failed)` | ENTER\_DELETED\_CHANNELThe request failed because the user is trying to enter a deleted channel. |
| `900080(request failed)` | BLOCKED\_USER\_RECEIVE\_MESSAGE\_NOT\_ALLOWEDThe request failed because the blocking user is trying to send the message to the blocked user in a 1-to-1 distinct channel. |
| `900081(request failed)` | DEACTIVATED\_USER\_RECEIVE\_MESSAGE\_NOT\_ALLOWEDThe request failed because the user is trying to send the message to the deactivated user in a 1-to-1 distinct channel. |
| `900090(request failed)` | WRONG\_CHANNEL\_TYPEThe request failed because it specifies a wrong channel type. |
| `900100(request failed)` | BANNED\_USER\_SEND\_MESSAGE\_NOT\_ALLOWEDThe request failed because the user is banned from the channel and isn't allowed to send the message. |
| `900200(request failed)` | TOO\_MANY\_MESSAGESThe number of the sent messages exceeds the allowed amount. |
| `900300(request failed)` | MESSAGE\_NOT\_FOUNDThe request failed because the message to edit can't be retrieved. |
| `900400(request failed)` | TOO\_MANY\_PARTICIPANTSThe number of the channel's participants exceeds the allowed amount. |
| `900500(request failed)` | CHANNEL\_NOT\_FOUNDThe request failed because there is no channel to perform this operation. |

