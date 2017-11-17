# Inferfacing with the Browser

In the last section, we used the `browser.tabs.query()` function to get the currently open tabs. This is one of the many *APIs* provided by the browser. Essentially, the browser makers have defined a set of functions you can call to get information from the browser and set and store data.

JavaScript APIs for WebExtensions can be used inside the extension's [background scripts](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Anatomy_of_a_WebExtension#Background_scripts) and in any other documents bundled with the extension, including [browser action](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Browser_action) or [page action](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Page_actions) popups, [sidebars](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Sidebars), [options pages](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Options_pages), or [new tab pages](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/manifest.json/chrome_url_overrides). A few of these APIs can also be accessed by an extension's [content scripts](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Anatomy_of_a_WebExtension#Content_scripts) (see the [list in the content script guide](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Content_scripts#WebExtension_APIs)).

To use the more powerful APIs you need to [request permission](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/manifest.json/permissions) in your extension's manifest.json.

However, there are some inconsistencies and incompatibilities between browsers. There are some APIs that are only supported in one browser (but this is relatively uncommon issues), but more importantly Firefox and Chrome have different preferences for which asynchronous JavaScript constructs you use.

If you were paying close attention on the last page, you may have noticed that at one point we switched from using `chrome.tabs.query()` to `browser.tabs.query()`. When Firefox copied Chrome's extension framework, they didn't want all the functions to start with "chrome", so they renamed it to "browser". However, they also made another major change—they rewrote the functions to use promises instead of callbacks, so `chrome.tabs.query(args, callback)` became `browser.tabs.query(args)`, which returns a promise (which can then be awaited). Firefox also directly copied all of the `chrome.` functions, so if you write an extension the "Chrome way", it will work unmodified in Firefox.

But if you're going to be starting an extension from scratch, the "Firefox way" (with Promises and async/await) is easier to understand and can also be made to work unmodified in Chrome. To do so, you will need to bundle the [webextension-polyfill](https://github.com/mozilla/webextension-polyfill) with your code, which is a JavaScript file that you include in the extension. Our [example extension](https://github.com/uncommonhacks/webextension-starter) already has the webextension-polyfill included and set up for you.

## Takeaways

* You call specific functions to interact with the browser APIs.
* Firefox's `browser.` syntax is easier to use, and can also work in Chrome.
* If you find an answer on Stack Overflow that does something like `chrome.something(stuff, callback)`, you’ll want to translate that to `await brower.something(stuff);`
* If you’re going to do things this way \(which we would recommend\), you need to include the webextension-polyfill in your extension in order for it to work with Chrome.
