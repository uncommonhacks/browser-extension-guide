---
title: Browser Extension Architecture
has_children: false
nav_order: 3
---

# Browser Extension Architecture

An extension consists of a collection of files, packaged for distribution and installation. Browser extensions mainly consist of:

1. A `manifest.json` file that declares the extensions name, permissions, and what other components you've included.
2. A background script (optional) that runs inside the browser and can access information such as web requests.
3. A content script (optional) that runs inside pages that the user loads, and can communicate with the background script.
4. A popup, opened with a toolbar button, that consists of a small web page loaded in the popup.

The rest of this page describes each component in a bit more detail and links to some resources with more. We’ve also made an [example extension](https://github.com/uncommonhacks/webextension-starter) that you can start with that implements these pieces and documents the code.

## `manifest.json`

This is the only file that must be present in every extension. It contains basic metadata such as its name, version and the permissions it requires. It also provides pointers to other files in the extension.

This manifest can also contain pointers to several other types of files:

* [Background pages](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Anatomy_of_a_WebExtension#Background_scripts): Implement long-running logic.
* Icons for the extension and any buttons it might define.
* [Content scripts](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Anatomy_of_a_WebExtension#Content_scripts): JavaScript included with your extension, that you will inject into web pages.
* [Popups, sidebars, and options pages](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Anatomy_of_a_WebExtension#Sidebars_popups_options_pages): HTML documents that provide content for various user interface components.

## Background Scripts

Extensions often need to maintain long-term state or perform long-term operations independently of the lifetime of any particular web page or browser window. That is what background scripts are for. Background scripts are loaded as soon as the extension is loaded and stay loaded until the extension is disabled or uninstalled. You can use any of the [WebExtension APIs](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API) in the script, as long as you have requested the necessary [permissions](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/manifest.json/permissions).

You can include a background script using the `background` key in `manifest.json`. You can specify multiple background scripts: if you do, they run in the same context, just like multiple scripts that are loaded into a single web page.

## Content Scripts

Use content scripts to access and manipulate web pages. Content scripts are loaded into web pages and run in the context of that particular page. With a content script, you can write code that runs inside pages that you visit in the browser, and do things like directly modify elements on the page. You also specify content scripts in `manifest.json`, where you can specify whether the script should run on all pages or only on specific domains.

Because content scripts have direct access to the content of every page you visit, there are more restrictions about what they can do. Content scripts are not able to access most privileged browser APIs. This means that, for example, you can’t get a list of the currently open tabs directly from a content script, but you can get this information through intra-extension messaging APIs—you can write code that sends a message to the background script, the background script can ask the browser for the privileged information, and then send the result back to the content script. Our example extension shows how you can do this.

## Popups, sidebars, options pages

Your extension can include various user interface components whose content is defined using an HTML document:

* a [popup](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/user_interface/Popups) is a dialog that you can display when the user clicks on a [toolbar button](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/user_interface/Browser_action) or [address bar button](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/user_interface/Page_actions)
* a [sidebar](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/user_interface/Sidebars) \(Firefox only, not supported in Chrome\) is a pane that is displayed at the left-hand side of the browser window, next to the web page
* an [options page](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/user_interface/Options_pages) is a page that's shown when the user accesses your add-on's preferences in the browser's native add-ons manager.

For each of these components, you create an HTML file and point to it using a specific property in [manifest.json](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/manifest.json). The HTML file can include CSS and JavaScript files, just like a normal web page.

All of these are a type of [Extension pages](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/user_interface/Extension_pages), and unlike a normal web page, your JavaScript can use all the same privileged WebExtension APIs as your background script. They can even directly access variables in the background page using [`runtime.getBackgroundPage()`](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/runtime/getBackgroundPage).



## Graphical Summary![](/assets/webextension-anatomy.png)

## Acknowledgements

This section is an edited version of a similar page on [MDN](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Anatomy_of_a_WebExtension).

