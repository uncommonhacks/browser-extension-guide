# Messaging

_or "how to get different parts of your browser extension to talk to each other"_

One aspect of browser extension that can be challenging is getting the different components of your extension to talk to each other. A popup might need to access data set by a background script, or a content script might need to access information that it is not able to access directly due to browser security restrictions. In those cases, you will need to use the **messaging** APIs to send "messages" between the different components of your extension.

## Communication between background and popup

A_lso applies to bundled web pages, options pages, and sidebar \(Firefox\)_

Popup and other bundled web pages in your extensions run in a priviledged context, so you can access the full set of browser APIs directly. So, for example, you can call `browser.storage.local.set()` in your background script, and `brower.storage.local.get()` in a popup, and both scripts will be accessing the same datastore. You generally shouldn't need to have the popup page and background script directly communicate with each other, but if the need arises you can use [`runtime.getBackgroundPage()`](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API/runtime/getBackgroundPage), which lets you access data and call functions defined in your background script directly from the popup. You can also use the messaging APIs described in the next section, but that is generally more difficult to use.



## Messaging between background and content scripts

Content scripts, which run directly in the pages the user visits, however, are not considered priviledged by the browser. This means that they cannot call most browser APIs, so if, for example, you need your content script to know the current tabs open, you can't just call `browser.tabs.query()` directly. However, you can send a message to the background script, have the background script access the information, and then send a response back. You can do this using [`browser.runtime.sendMessage()`](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API/runtime/sendMessage) and [`browser.runtime.onMessage()`](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API/runtime/onMessage). Here are a couple of examples:

#### **Basic example: sending a message from content script, waiting for response**

We have a content script that sends a message to the background script when something on the page is clicked:

```js
// content-script.js

function notifyBackgroundPage(e) {
  let message = await browser.runtime.sendMessage({
    greeting: "Greeting from the content script"
  });
  console.log("Message from the background script:", message);
}

window.addEventListener("click", notifyBackgroundPage);
```

In the background script, we define a listener function that is called when a message is recieved. Note that we are given a **callback** to send a response back.

```js
// background-script.js

function handleMessage(request, sender, sendResponse) {
  console.log("Message from the content script: " +
    request.greeting);
  sendResponse({response: "Response from background script"});
}

browser.runtime.onMessage.addListener(handleMessage);
```

#### **Advanced example: sending responses asynchronously**

If you don't need to send responses back to the content script, or the responses you're sending are determined synchronously \(i.e. there aren't any `await` or `.then` in your handleMessage function, the code above should work. If you _do_ need to do asynchronous operations in your `handleMessage` function, then things get more complicated—one of the authors of this guide has personally struggled with getting this to work. Specifically, using async/await is error prone, and the documentation about how to handle sending the message asynchrounously is confusing. The following code, however, seems to work:

```js
// background-script.js

function handleMessage(message, sender, sendResponse) {
  if (message.type === "queryDatabase") {
    query = queryDatabase(message.query); // async function, returns a promise
    query.then(res => { // cannot use async/await
      sendResponse(res);
    })
    return true; // this tells browser that we will call sendResponse asynchronously
  }
}

browser.runtime.onMessage.addListener(handleMessage);
```

Notably, the following aspects of this code are important:

* No async/await in `handleMessage`, must use promises directly
* In `promise.then()`, call `sendResponse()`
* Outside of promise, return true from `handleMessage` function to tell browser that we will call `sendResponse` asynchronously

## Alternative messaging methods

There also is another way to send and recieve messages: you can open a "port" between two components with [`browser.runtime.connect()`](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API/runtime/connect), and then send and recieve messages through that port. You may find code online that uses it, but it can be difficult to work with and unless you have some need that the above methods don't satisfy I wouldn't reccomend using it.

