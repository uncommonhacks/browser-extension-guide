## Popups, sidebars, options pages {#popups-sidebars-options-pages}

Your extension can include various user interface components whose content is defined using an HTML document:

*   a [popup](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/user_interface/Popups) is a dialog that you can display when the user clicks on a [toolbar button](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/user_interface/Browser_action) or [address bar button](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/user_interface/Page_actions)
*   a [sidebar](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/user_interface/Sidebars) (Firefox only, not supported in Chrome) is a pane that is displayed at the left-hand side of the browser window, next to the web page
*   an [options page](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/user_interface/Options_pages) is a page that&#039;s shown when the user accesses your add-on&#039;s preferences in the browser&#039;s native add-ons manager.

For each of these components, you create an HTML file and point to it using a specific property in [manifest.json](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/manifest.json). The HTML file can include CSS and JavaScript files, just like a normal web page.

All of these are a type of [Extension pages](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/user_interface/Extension_pages), and unlike a normal web page, your JavaScript can use all the same privileged WebExtension APIs as your background script. They can even directly access variables in the background page using [`runtime.getBackgroundPage()`](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/runtime/getBackgroundPage).