# Asynchronous JavaScript

One unique aspect of JavaScript is the way it handles what is called asynchronous programming.

The core concept behind this is that there are a lot of things related to web programming that might take a long time, like waiting for user input or data over the network. In browser extension development, this usually is related getting information from the browser, which might be busy with other things so it might take some time to, say, get a list of all the currently open tabs. JavaScript is structured in a way that these slow operations won't hold up the rest of your code from running, so code that might take a long time to run is called *asynchronously* and doesn't block the execution of the rest of function.

But the way this is implemented can be kind of confusing. The good thing is that JavaScript as a language is being improved, and there newer, easier to understand ways to write code that deals with asynchronous functions. However, this means that there are 3 different ways to do basically the same thing—the third way (`async/await`) is the easiest to work with, but since you'll run into code online that uses the other two ways, we'll briefly explain all three.

## Three Ways to Write a Function

To do this, we'll walk though how you could write a function in your extension that prints the titles of all the currently open tabs. This will also be an exercise in reading and interpreting API documentation. Effectively, we want to write a function that does something like this:

```js
function printOpenTabs() {
    let tabs = getOpenTabs();
    for (let tab of tabs) {
        console.log(tab.title);
    }
}
```

But since querying for open tabs could potentially be a slow operation,[^1] the browser makers have decided to expose this as an asynchronous API.

[^1]: This may seem like a contrived example—does it really take long enough to query for open tabs that it merits making that an asynchronous function? Maybe, maybe not, but that's how the browser developers have chosen to implement all the browser APIs (the function calls that let you get information from the browser), so we have to deal with the implications regardless.

### 1. Callbacks

If you were to look in the Chrome documentation about how to do this, you'd eventually find [this page](https://developer.chrome.com/extensions/tabs#method-query), which says to use:

```js
chrome.tabs.query(object queryInfo, function callback)
```

So to get the current open tabs, you call the `chrome.tabs.query` function, and give it an argument for the query information, and then give it a function to "call back" with the result. A bit farther down, the docs say that "The callback parameter should be a function that looks like this: `function(array of Tab result) {...};`" Essentially, you ask the browser to query the open tabs, and then give it a function to run when it knows the result. The rest of your code is not blocked waiting for the result, so the line after `chrome.tabs.query` could run before the callback.

Usually people use what are called "anonymous" functions to do this, which results in an example like the following:

```js
function printOpenTabs() {
    chrome.tabs.query({}, function(tabs) {
        for (let tab of tabs) {
            console.log(tab.title);
        }
    });
    console.log("hi!"); // could print before open tab titles
}
```

### 2. Promises

Callbacks work, but they can be difficult to reason about and very messy when you need to nest multiple callbacks. The people who make the JavaScript language realized this, so they made a slightly method: *promises*. Instead of passing a function to call back and not returning any value, `tabs.query` returns a Promise. You can then save that Promise, and when you need to use it provide a function to run using `.then(fn)`.

You'll see most of the Firefox documentation use this method, and implementing our tab display code this way would result in code that looks like:

```js
function printOpenTabs() {
    let queryPromise = browser.tabs.query({});
    queryPromise.then((tabs) ==> {
        for (let tab of tabs) {
            console.log(tab.title);
        }
    });
    console.log("hi!"); // could print before open tab titles
}
```

You might notice a couple of other differences that aren't particularly important right now:
- It's `browser.tabs.query` and not `chrome.tabs.query`, we'll explain this later
- Instead of having the anonymous function written as `function(tabs) {}`, it's `(tabs) => {}`. For our purposes these are essentially the same, but most code out there that uses promises also uses the second method (arrow functions).

This method is *slightly* better, as it is more clear what are arguments to the query and what the function that is called with the result is, and chaining Promises together (when you have multiple dependent promises) is cleaner than nesting callbacks.

### 3. Async/Await

But really all we want to do is get the current tabs, and then print them. The whole concept of asynchronous code execution is nice at times, but for most cases it ends up making life more difficult. The people who make the JavaScript standard eventually realized this, and came up with a new method, called async/await. Implementing our same function using this method results in *much* cleaner code:

```js
async function printOpenTabs() {
    let tabs = await browser.tabs.query({});
    for (let tab of tabs) {
        console.log(tab.title);
    }
    console.log("hi!"); // will print after open tab titles
}
```

What the `await` keyword does is says "wait for the promise set by `browser.tabs.query({})` to resolve (finish/return), and then set `tabs` to the result." Then, whenever you use `await` in a function, you must declare it as an `async function`. If somewhere else in your code you do something that uses the return value for an async function, you must `await` it there.

Overall, for 95% of cases, using async/await is much easier to understand than trying to deal with callbacks or promises. We'll dive into what this means for when you're building your browser extension in the next section.