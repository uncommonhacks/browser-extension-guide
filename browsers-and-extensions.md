---
title: Browsers and Extensions
has_children: false
nav_order: 2
---

# Background: Web Browsers and Browser Extensions

Extensions are bits of code that modify the functionality of a web browser. They are written using standard web technologies - JavaScript, HTML, and CSS - plus some dedicated JavaScript APIs. Among other things, extensions can add new features to the browser or change the appearance or content of particular websites.

Browser extensions are one of the few things in the tech world that are _close_ to being cross-platform. Most of the major browsers (Chrome, Firefox, Microsoft Edge, Opera)[^1] have standardized (more or less) on the same toolset, which is based on the framework and APIs[^2] first implemented in Google Chrome. Firefox historically had an entirely different framework for add-ons, but recently implemented the toolset used by Chrome and phased out the other add-on formats. Microsoft and Opera have likewise made similar moves in recent years.

With some exceptions, most extensions written originally for Chrome will run unmodified on Firefox, Edge, and Opera. All of the browsers implement (more or less) the same APIs, though there are some differences between the browsers in terms of conventions about how to deal with asynchronous JavaScript. There are various names for what this toolset is called, including “Chrome extensions”, “WebExtensions”, and “browser extensions” that can be used interchangeably. For reasons that we’ll get into later, the conventions used by Firefox WebExtensions[^3] are a bit easier to work with, and it’s possible to do things that way and have your extension also work with Chrome and other browsers without any modification.

[^1]: You might notice that Safari is missing from this list. Safari has its own extension toolset, but it is not as widely used and there aren’t many extensions outside the most popular ones.

[^2]: Application Programming Interface, basically the set of standards for things like “this is how you ask the browser what the currently open tabs are”

[^3]: In one sentence, it’s “use `browser.*` instead of `chrome.*`, and use async/await instead of promises or callbacks.” But if you don’t know what any of those words mean, don’t worry about that!