## manifest.json {#manifest-json}

This is the only file that must be present in every extension. It contains basic metadata such as its name, version and the permissions it requires. It also provides pointers to other files in the extension.

This manifest can also contain pointers to several other types of files:

*   [Background pages](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Anatomy_of_a_WebExtension#Background_scripts): Implement long-running logic.
*   Icons for the extension and any buttons it might define.
*   [Content scripts](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Anatomy_of_a_WebExtension#Content_scripts): JavaScript included with your extension, that you will inject into web pages.
*   [Popups, sidebars, and options pages](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Anatomy_of_a_WebExtension#Sidebars_popups_options_pages): HTML documents that provide content for various user interface components.