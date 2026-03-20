# Vyin Chat Bot Tool SDK Integration (via LicenceId)


## SDK Usage Flow

There are two ways to use the SDK, which will be introduced sequentially below:

1. Manually import the SDK and execute initialization  
2. Add to the website via a Code Snippet

### Manually Importing the SDK

1\. Import the SDK into the desired HTML file

```html
<script src="https://botsdk.vyin.chat/index.umd.js"></script>
```

2\. Call GIMBotTool.init to initialize the SDK

```javascript

GIMBotTool.on('ready', ()=> {
  // Wait for the Chatbot to be ready
  GIMBotTool.call('open')
})

GIMBotTool.init({
  licenseId: 583781098555718802
})
```

**Parameter Description**

| Field Name | Type | Required | Description |
| ----- | :---: | :---: | :---: |
| `licenseId` | string | Y | Used to identify the chatbot's ID |

### **Add to the Website via a Code Snippet**

1\. Log in to the Console Backend  
![][image1]

2\. Select the Organization  
![][image2]

3\. Select the Application  
![][image3]

4\. Navigate to the Side Menu  
![][image4]

5\. Go to Bot Management \> **Bot Profile**  
![][image5]

6\. Click the **Add to website \+** button  
![][image6]

7\. Next, click the **Copy code** button  
![][image7]

8\. Paste the copied code into your website's source code before the \</body\> tag

9\. Once you have successfully added the code to your website, please refresh the page. After the widget loads, you will see it appear in the bottom-right corner of your website.  
![][image8]

# API Interface

The following interfaces are publicly exposed:

* Show Widget  
* Hide Widget  
* Open  
* Minimize  
* Maximize  
* Resize to Default  
* Refresh  
* Destroy  
* Send Message  
* Get State


**Show Widget**

Show Widget

Interface Data

| Parameter | Value | Return Value |
| :---: | ----- | :---: |
| Command Name | show\_widget | void |

   
Example

```javascript
GIMBotTool.call('show_widget')
console.log('show widget!')
```

**Hide Widget**

Hide Widget

Interface Data

| Parameter | Value | Return Value |
| :---: | ----- | :---: |
| Command Name | hide\_widget | void |

Example

```javascript
GIMBotTool.call('hide_widget')
console.log('Widget is invisible now!')
```

**Open**

Open Chat Window

Interface Data

| Parameter | Value | Return Value |
| :---: | ----- | :---: |
| Command Name | open | void |

Example

```javascript
GIMBotTool.call('open')
console.log('Chat window is open now!')
```

**Minimize**

Minimize Chat Window

Interface Data

| Parameter | Value | Return Value |
| :---: | ----- | :---: |
| Command Name | Minimize | void |

Example

```javascript
GIMBotTool.call('minimize')
console.log('Chat window is now minimized!')
```

**Maximize**

Maximize Chat Window

Interface Data

| Parameter | Value | Return Value |
| :---: | ----- | :---: |
| Command Name | Maximize | void |

```javascript
GIMBotTool.call('maximize')
console.log('Chat size is now maximized!')
```

**Resize to Default**

Resize to default Chat Window size

Interface Data

| Parameter | Value | Return Value |
| :---: | ----- | :---: |
| Command Name | resize\_to\_default | void |

```javascript
GIMBotTool.call('resize_to_default')
console.log('Chat size is now reset!')
```

**Refresh**

Refresh Chat Window

Interface Data

| Parameter | Value | Return Value |
| :---: | ----- | :---: |
| Command Name | refresh | void |

Example

```javascript
GIMBotTool.call('refresh')
console.log('Chat window is refreshing!')
```

**Destroy**

Destroys the Widget, which will reset data, remove event listeners, and destroy the Widget DOM Element.

Interface Data

| Parameter | Value | Return Value |
| :---: | ----- | :---: |
| Command Name | destroy | void |

Example

```javascript
GIMBotTool.call('destroy')
console.log('Widget is now destoryed!')
```

## 

**Send Message**

Send Message

Interface Data

| Parameter | Value | Return Value |
| :---: | ----- | :---: |
| Command Name | send\_message | void |
| Data Type | {   messsge: string } |  |

Example

```javascript
GIMBotTool.call('send_message', {
  message: ''
})
```

**Get State**

Get Widget State

Interface Data

| Parameter | Value | Return Value |
| :---: | ----- | ----- |
| Command Name | state | {<br>state: 'uninitialized' \| 'initialized' \| 'ready' \| 'refreshing',<br>visibility: 'maximized' \| 'open' \| 'minimized' \| 'widget_hidden'<br>} |

Example

```javascript
const {visibility} = GIMBotTool.get('state')
console.log(`widget visibility: ${visibility}`)
```

# Event Bundling

The list of available events is as follows:

* **On ready**  
* **On visibility changed**  
* **On error**

**On ready**

Triggered when the Widget has completed initialization and is ready to start a conversation.

```javascript

GIMBotTool.on('ready',()=> {
  console.log('widget ready!')
})
```

**On visibility changed**

Triggered when the Widget's display status changes.

Event Data

| Parameter  | Value | Description |
| :--------: | ----- | :---------: |
| visibility | 'maximized' \| 'open' \| 'minimized' \| 'widget_hidden' | Bot Widget Status |

```javascript

GIMBotTool.on('visibility_changed',(event)=> {
  switch (event.visibility) {
    case "maximized": // Chat window is in maximized display state 
      break;
    case "open": // Chat window is in open display state  
      break;
    case "minimized": // Chat window is in closed/minimized state
      break;
    case "widget_hidden": // Widget is in hidden state 
      break;
  }
})
```

**On error**

This event is triggered when an error occurs.

Event Data

| Parameter | Value | Description |
| ----- | ----- | ----- |
| name | string | Error name — can be used as the main basis for determining the type of error. Please refer to the “Error Name List.” |
| message | string | Error description text |
| createdAt | string | Error occurrence time |
| cause | object | Error source information — used to provide detailed information about the error. This interface is currently in the beta stage, and its specifications or behavior may be subject to change in the future. |
| cause.httpStatusCode | number | HTTP status code (if available) |
| cause.errorCode | number | Error code (if available) |
| cause.rawMessage | string | Original error message (if available) |
| cause.traceId | string | Trace ID (if available) |
| cause.requestUrl | string | Original request URL (if available) |

Error Name List

| Constant | Description |
| ----- | ----- |
| GIMBotTool.ErrorNames.BOT\_LICENSE\_INVALID\_ERROR | Bot License ID is invalid |
| GIMBotTool.ErrorNames.BOT\_LICENSE\_NOT\_FOUND\_ERROR | Corresponding Bot License ID not found |
| GIMBotTool.ErrorNames.BOT\_LICENSE\_UNKNOWN\_ERROR | Bot License error — any Bot License issue that does not fall under the above two categories is classified as this error. |
| GIMBotTool.ErrorNames.BOT\_INFO\_FETCH\_FAILED\_ERROR | Error occurred while fetching Bot information |
| GIMBotTool.ErrorNames.WEBSOCKET\_ERROR | WebSocket connection error |
| GIMBotTool.ErrorNames.NETWORK\_OR\_CORS\_ERROR | Network-related error |

```javascript

GIMBotTool.on('error',(event)=> {
  switch (event.name){
   case GIMBotTool.ErrorNames.BOT_LICENSE_INVALID_ERROR:
   case GIMBotTool.ErrorNames.BOT_LICENSE_NOT_FOUND_ERROR:
   case GIMBotTool.ErrorNames.BOT_LICENSE_UNKNOWN_ERROR:
      console.log('The Bot License ID is incorrect. Please check if the license is valid.');
      break;
   case GIMBotTool.ErrorNames.NETWORK_OR_CORS_ERROR:
      console.log('Please check your network connection.');
      break;
   default:
      console.log('An unknown error occurred：', event);
      break;
  }
})
```

# Code Examples

**Initialize listener**

```javascript

 GIMBotTool.on('ready', ()=> {
   GIMBotTool.call('open')
 })
```

**Send Message**

```javascript

const isChatbotReady = false;

function sendMessage(){
 if (!isChatbotReady) {
  console.log('chat bot is not ready!')
  return
 }

 const { visibility } = GIMBotTool.get('state')

 if (visibility === 'minimized' || visibility === 'widget_hidden') {
   GIMBotTool.call('open')
 }

 GIMBotTool.call('send_message', {
  message: 'Vyin Chat'
 })

 console.log('message sent!')
}

 GIMBotTool.on('ready', ()=> {
   isChatbotReady = true
 })

```

# Troubleshooting

When an error occurs, you can first use the error event to initially troubleshoot the cause.

```javascript
GIMBotTool.on('error',(event)=> {
  switch (event.name){
   case GIMBotTool.ErrorNames.BOT_LICENSE_INVALID_ERROR:
   case GIMBotTool.ErrorNames.BOT_LICENSE_NOT_FOUND_ERROR:
   case GIMBotTool.ErrorNames.BOT_LICENSE_UNKNOWN_ERROR:
      console.log('The Bot License ID is incorrect. Please check if the license is valid.');
      break;
   case GIMBotTool.ErrorNames.NETWORK_OR_CORS_ERROR:
      console.log('Please check your network connection.');
      break;
   default:
      console.log('An unknown error occurred：', event);
      break;
  }
})
```
