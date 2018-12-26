---
title: JavaScript
has_children: true
nav_order: 9
permalink: /advanced-topics
---

# Advanced Topics

## External Dependencies

If you’re including one or two external dependencies, such as JQuery or Bootstrap, it's easiest just to download the JavaScript files for them and directly include them in your extension. If you're including more dependencies, especially if you have more web development/JavaScript experience and want to install modules using npm, there's a way to bundle those dependencies using something called [Webpack](https://webpack.js.org/). See an example extension [here](https://github.com/mdn/webextensions-examples/tree/master/react-es6-popup) that uses Webpack to bundle React. But this can take a long time to set up and get working, so I wouldn't generally recommend going down this route for a small project.

