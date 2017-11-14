# Asynchronous JavaScript

One unique aspect of JavaScript is the way it handles what is called asynchronous programming.

The core concept behind this is that there are a lot of things related to web programming that might take a long time, like waiting for user input or data over the network. In browser extension development, this usually is related getting information from the browser, which might be busy with other things so it might take some time to, say, get a list of all the currently open tabs. JavaScript is structured in a way that these slow operations won't hold up the rest of your code from running, so code that might take a long time to run is called *asynchronously* and doesn't block the execution of the rest of function.

But the way this is implemented can be kind of confusing. The good thing is that JavaScript as a language is being improved, and there newer, easier to understand ways to write code that deals with asynchronous functions. However, this means that there are 3 different ways to do basically the same thing—the third way (`async/await`) is the easiest to work with, but since you'll run into code online that uses the other two ways, we'll briefly explain all three.

## Three Ways to Write a Function

To do this, we'll explain 3 ways you could write a function in your extension that prints the titles of all the currently open tabs. Effectively, we want to write a function that does something like this:

```js
function printOpenTabs() {
    let tabs = getOpenTabs();
    for (let tab of tabs) {
        console.log(tab.title);
    }
}
```

But since querying for open tabs could potentially be a slow operation,[^1] the browser makers have decided to expose this as an asynchronous API…

[^1]: This may seem like a contrived example—does it really take long enough to query for open tabs that it merits making that an asynchronous function? Maybe, maybe not, but that's how the browser developers have chosen to implement all the browser APIs, so we have to deal with the implications of what this means.

## 1: Callbacks

The first 

```js
function printOpenTabs() {
    chrome.tabs.query({}, function(tabs) {
        for (let tab of tabs) {
            console.log(tab.title);
        }
    });
}
```

## 2: Promises

```js
function printOpenTabs() {
    let query = browser.tabs.query({});
    query.then(tabs ==> {
        for (let tab of tabs) {
            console.log(tab.title);
        }
    });
}
```

## 3: Async/Await

```js
function printOpenTabs() {
    let tabs = await browser.tabs.query({});
    for (let tab of tabs) {
        console.log(tab.title);
    }
}
```

## Summary

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

## TL;DR {#tldr}

* Use async/await!
* If you find an answer on Stack Overflow that does something like `chrome.something(stuff, callback)`, you’ll want to translate that to `await brower.something(stuff);`
* If you’re going to do things this way \(which we would recommend\), you need to include the webextension-polyfill in your extension in order for it to work with Chrome.



