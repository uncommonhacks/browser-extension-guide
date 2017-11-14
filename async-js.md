# Async JavaScript



## Concept: Asynchronous JavaScript

## Callbacks, Promises, and Async/Await {#callbacks-promises-and-async-await}

```js
chrome.tabs.query(args, function(tab) {
	console.log(tab.id);
});

let query = browser.tabs.query(args);
query.then((tab) => {
	console.log(tab.id);
});

let tab = await browser.tabs.query(args);
console.log(tab.id);
```

## TL;DR {#tl-dr}

* Use async/await!
* If you find an answer on Stack Overflow that does something like `chrome.something(stuff, callback)`, you’ll want to translate that to `await brower.something(stuff);`
* If you’re going to do things this way \(which we would recommend\), you need to include the webextension-polyfill in your extension in order for it to work with Chrome.



