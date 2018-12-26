---
title: Running and Debugging
has_children: false
nav_order: 7
---

# Running and Debugging

Your extension should run without modification in both Chrome and Firefox.

## Installing

### Chrome

 Visit `chrome://extensions` in your browser \(or open up the Chrome menu by clicking the icon to the far right of the window, and select **Extensions** under the **More Tools** menu to get to the same place\).

1.  Ensure that the **Developer mode** checkbox in the top right-hand corner is checked.

2.  Click **Load unpacked extension…** to pop up a file-selection dialog.

3.  Navigate to the directory in which your extension files live, and select it.

 Alternatively, you can drag and drop the directory where your extension files live onto `chrome://extensions` in your browser to load it.

### Firefox

Open `about:debugging` in Firefox, click **Load Temporary Add-on** and select any file in your extension's directory:

The extension will now be installed, and will stay until you restart Firefox.

Alternatively, you can run the extension from the command line using the [web-ext](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Getting_started_with_web-ext) tool \(which can be handy as it launches a temporary, clean browser profile\)

## Debugging

Both Chrome and Firefox also include handy debuggers to help you debug your code. 

When you're writing code, you can include `console.log("hello!")` statements in your code \(like print statements in any other language\). These statements are printed in the browser's debugger—see [Chrome](https://developer.chrome.com/extensions/tut_debugging) and [Firefox](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Debugging) documentation. There also is a view to step through your extensions code ans set breakpoints, much like GDB, if you've used that before for a class.



