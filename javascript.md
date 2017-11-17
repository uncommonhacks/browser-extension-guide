# JavaScript {#javascript}

The primary programming language used for browser extensions is JavaScript. If you want to read more about JavaScript in general, there is some good info and tutorials [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript). The rest of this page is a very quick intro into the basic syntax of JavaScript.

If you've used C or a similar language before, and the example below makes sense to you, you can head to the page on [Asynchronous JavaScript](async-js.md), which will probably be new to you, and read that in depth.

If not, you might want to read this page in a bit more carefully and follow the links, but don't get too bogged down in the details of syntax—once you start writing code things will start to make sense.

## JavaScript Example

Here is a basic example of JavaScript code:

```js

function printFruits(fruits) {
	for (let i = 0; i < fruits.length; i++) {
		console.log(fruits[i], "are a fruit");
	}
}

const fruits = ["Apples", "Bananas", "Oranges"];
printFruits(fruits);

/* prints:
Apples are a fruit
Bananas are a fruit
Oranges are a fruit
*/
```


## Basic Components

## Variables

JavaScript is dyamically typed, interpreted language, meaning that as a programmer you don't have to assign types (such as "string", "integer", or "array") to variables when you declare them. Additionally, you don't have to compile JavaScript—you just can run the uncompiled source. Lots of words have been written about the advantages and disadvantages of this, but it ends up making some things easier for smaller projects.

You can declare a variable using the `let` keyword, such as `let x = 42;` You also can use the `const` keyword to declare variables you don't intend to change, and there also is the `var` keyword which has slightly different rules in terms of lexical scoping. Don't get hung up on the differences between `const`, `let`, and `var`, it doesn't *really* matter what you use, especially for small projects, but `let` is a good default choice.

**More info:**
- [Grammar and types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types)
- [Let and var](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/statements/let)

## Conditionals and Loops

`if` statements, and `for` and `while` loops look like they do in C:

```js
if (x < y) {
	stuff;
}

for (let i = 0; i < n; i++) {
	stuff;
}

while(x < y) {
	x++;
}
```

**More info:**
- [Control flow and error handling](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)
- [Loops and iteration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration)

## Functions

Functions are declared using the `function` keyword, followed by the name of the function, and the arguments in parentheses:

```js
function square(x) {
  return x * x;
}
```

You can call functions the same way you do in C or similar languages:

```js
square(5); // 25
```

**More info:**
- [Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)

## Printing

`console.log` is your print statement:

```js
console.log("Hello world!");
```

## More info

See the [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) for more information on the different components of JavaScript. There's also another tutorial [here](https://developer.mozilla.org/en-US/docs/Learn/JavaScript) that doesn't assume as much prior programming experience.