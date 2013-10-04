---
permalink: coding-standards/js/
layout: bootstrap-post
title: "JavaScript Coding Standards"
section: dev
---
{% include setup.liquid %}

#JavaScript Coding Standards

Backdrop's JavaScript coding standards generally follow [those of Drupal](http://drupal.org/node/172169).

- [Indenting and Whitespace](#indenting)
- [Operators](#operators)
- [Control Structures](#control-structures)
- [Function Calls](#function-calls)
- [Function Declarations](#function-declarations)
- [Variables](#variables)
- [Comments](#comments)
- [jQuery-specific Notes](#jquery)
- [Discouraged Conventions](#discouraged)


##Indenting and Whitespace <a id="indenting">[#](#indenting)</a>

Use an indent of 2 spaces, with no tabs.

Lines should have no trailing whitespace at the end.

Files should be formatted with `\n` as the line ending (Unix line endings), not `\r\n` (Windows line endings).

All text files should end in a single newline (`\n`). This avoids the verbose "\ No newline at end of file" patch warning and makes patches easier to read since it's clearer what is being changed when lines are added to the end of a file.

##Operators <a id="operators"></a>[#](#operators)

All binary operators (operators that come between two values), such as `+`, `-`, `=`, `!=`, `==`, `>`, etc. should have a space before and after the operator, for readability. For example, an assignment should be formatted as `var foo = bar;` rather than `var foo=bar;`. Unary operators (operators that operate on only one value), such as `++`, should not have a space between the operator and the variable or number they are operating on.

```js
var string = 'Foo' + bar;
string = bar + 'foo';
string = bar() + 'foo';
string = 'foo' + 'bar';
```

When using the concatenating assignment operator (`+=`), use a space on each side as with the assignment operator:

```js
var string += 'Foo';
string += bar;
string += baz();
```

The `==` and `!=` operators do type coercion before comparing. This may result in unexpected comparison results, such as `' \t\r\n' == 0` to be true. This can mask type errors. When comparing to any of the following values, use the `===` or `!==` operators, which do not do type coercion.

##Control Structures <a id="control-structures"></a>[#](#control-structures)

These include if, for, while, switch, etc. Here is an example if statement, since it is the most complicated of them:

```js
if (condition1 || condition2) {
  action1();
}
else if (condition3 && condition4) {
  action2();
}
else {
  defaultAction();
}
```

Control statements should have one space between the control keyword and opening parenthesis, to distinguish them from function calls.

You are strongly encouraged to always use curly braces even in situations where they are technically optional. Having them increases readability and decreases the likelihood of logic errors being introduced when new lines are added.

###switch

For switch statements:

```js
switch (condition) {
  case 1:
    action1();
    break;

  case 2:
    action2();
    break;

  default:
    defaultAction();
}
```

###try

The try class of statements should have the following form:

```js
try {
  // Statements...
}
catch (error) {
  // Error handling...
}
finally {
  // Statements...
}
```

###for ... in

The `for ... in` statement allows for looping through the names of all of the properties of an object. However caution should be used when doing this because all of the members which were inherited through the prototype chain will also be included in the loop. Often this results in returning method members when the interest is in data members. To prevent this, the body of every `for ... in` statement should be wrapped in an `if` statement that does filtering. It can select for a particular type or range of values, or it can exclude functions, or it can exclude properties from the prototype. The most common approach looks like this:

```js
for (var variable in object) {
  if (object.hasOwnProperty(variable))
    // Statements...
  }
}
```

and NOT this:

```js
for (var variable in object) {
  // Statements...
}
```

##Function Calls <a id="function-calls"></a> [#](#function-calls)

Functions should be called with no spaces between the function name, the opening parenthesis, and the first parameter; spaces between commas and each parameter, and no space between the last parameter, the closing parenthesis, and the semicolon.

```
var foobar = foo(bar, baz, quux);
```

There should be one space on either side of an equals sign used to assign the return value of a function to a variable. In the case of a block of related assignments, more space may be inserted to promote readability:

```
var short        = foo(bar);
var longVariable = foo(baz);
```

##Function Declarations <a id="function-declarations"></a>[#](#function-declarations)

Functions and methods should be named in lowerCamelCase. Within a scoped file or enclosure, function names do not have to be namespaced with a module name.

```js
function funStuff(field, settings) {
  settings = settings || Drupal.settings;
  alert("This JS file does fun message popups.");
  return field;
}
```

There should be no space between the function name and the following left parenthesis.

When creating a function that is in the global namespace, it should be assigned to the Drupal global or to a namespaced parent variable created by the defining module.

```js
Drupal.behaviors.tableDrag = function (context) {
  ...
  div.onclick = function (e) {
    return false;
  };
  ...
};
```

When defining anonymous functions, there should be no space between the function name and the following left parenthesis.

##Variables <a id="variables"></a> [#](#variables)

All variables should be declared with `var` before they are used and should only be declared once. Doing this makes the program easier to read and makes it easier to detect undeclared variables that may become implied globals.

Variables should not be defined in the global scope; try to define them in a local function scope. All variables should be declared at the beginning of a function.

lowerCamelCasing should be used for pre-defined constants. Unlike the PHP standards, you should use lowercase `true`, `false` and `null` as the uppercase versions are not valid in JS.

Variables added through drupal_add_js() should also be lowerCamelCased, so that they can be consistent with other variables once they are used in JavaScript.

```php
<?php
drupal_add_js(array('myModule' => array('basePath' => base_path())), 'setting');
?>
```

This variable would then be referenced as `Drupal.settings.myModule.basePath;`

###Arrays

Arrays should be formatted with a space separating each element and assignment operator, if applicable:

```js
var someArray = ['hello', 'world'];
```

Note that if the line spans longer than 80 characters (often the case with form and menu declarations), each element should be broken into its own line, and indented one level.

Note there is no comma at the end of the last array element. This is different from the PHP coding standards. Having a comma on the last array element in JS will cause an exception to occur.

###Use literal expressions

Use literal expressions instead of the new operator:

- Instead of `new Array()` use `[]`
- Instead of `new Object()` use `{}`
- Don't use the wrapper forms `new Number`, `new String`, `new Boolean`.

In most cases, the wrapper forms should be the same as the literal expressions. However, this isn't always the case, take the following as an example:

```js
var literalNum = 0;
var objectNum = new Number(0);
if (literalNum) { } // false because 0 is a false value, will not be executed.
if (objectNum) { }  // true because objectNum exists as an object, will be executed.
if (objectNum.valueOf()) { } // false because the value of objectNum is 0.
```

###Variables that contain jQuery objects

Prefix variables that point to jQuery objects with a dollar sign (`$`). In any part of your code it should be easy to understand which variables are jQuery objects and which are not.

Incorrect

```js
var foo = $('.foo');
var object = { bar: $('.bar') };
```

Correct

```js
var $foo = $('.foo');
var object = { $bar: $('.bar') };
```

##Comments <a id="comments"></a>[#](#comments)

Comment standards are discussed on the separate [Doxygen and comment formatting conventions]({{ BASE_PATH }}coding-standards/doxygen).

Non-documentation comments should use capitalized sentences with punctuation. Comments should be on a separate line immediately before the code line or block they reference. For example:

```js
// Unselect all other checkboxes.
```

If each line of a list needs a separate comment, the comments may be given on the same line and may be formatted to a uniform indent for readability.

C style comments (`/* */`) and standard C++ comments (`//`) are both fine.

##jQuery-specific Notes <a id="jquery"></a>[#](#jquery)

Event Delegation

Every event (e.g. click, mouseover, etc.) in JavaScript “bubbles” up the DOM tree to parent elements. This is incredibly useful when you want many elements to call the same function. Instead of binding an event listener function to all of them, you can bind it once to their parent, and have it figure out which node triggered the event.

In examples of jQuery code, you may often see events simply return false; to prevent the default behavior of that event. This type of prevention is often misused and also prevents the "bubbling" propagation of the event. This effect is not always desired. If you wish to either prevent the default behavior or stop propagation, they should be explicitly defined.

Incorrect
$('.item').click(function(event) {
  ...
  // Calls both event.preventDefault() and event.stopPropagation().
  return false;
});

Correct (Drupal 7)
$menus.delegate('.item', 'click', function(event) {
  ...
  // Prevents the default behavior (ie: click).
  event.preventDefault();
  // Prevents propagation (ie: "bubbling").
  event.stopPropagation();
});

Correct (Drupal 8)
$menus.on('click', '.item', function(event) {
  ...
  // Prevents the default event behavior (ie: click).
  event.preventDefault();
  // Prevents the event from propagating (ie: "bubbling").
  event.stopPropagation();
});
Functions

Separate your functions into individual functions and call them when needed.
Incorrect
// Can be called only on click.
$('.btn').click( function() { ... });

Correct
// Can be called anywhere.
function clickFunction() { ... };
$('div').click( function() {clickFunction() });
Context

Always try to give your selectors a context. The default context will search the entire page's DOM. If the context is a cached selection, use find().
Incorrect
// This has no context and is very slow.
var $element = $('.element');

Improved
// Search only in #sidebar.
var $element = $('.element', '#sidebar');

Correct
var $sidebar = $('#sidebar');
var $element = $sidebar.find('.element');
Using #id or .class

Finding elements by the tag ID is much faster (test) then by class name. If your target element appears on the page only once, select it by #id. In case when you have more than one, use class but descend from an #id. See the above context code examples.
jQuery.attr()

This applies only for D6 and D7 because D8 will ship with jQuery 1.9+ which instead uses .prop() for property.
Wrong use of .attr is the cause of the many compatibility problems. Use booleans to set a property, not an empty string. Also, do not assume that property values that are returned are always booleans. .attr('checked') could return either true or 'checked' depending on jQuery version and markup.

Incorrect

```js
$element.attr('disabled', '');
if ($element.attr('checked') === true) { ... }
```

Correct

```js
$element.attr('disabled', false);
$element.attr('disabled', true);
if ($element.attr('checked')) { ... }
jQuery.each()
```

This method is indeed very powerful (and appropriate) when dealing with existing instantiated jQuery objects and the need to iterate on them.

It is often misused when needing to iterate over simple native JavaScript arrays or objects. Using native JavaScript for loops are 300-1000 times faster than jQuery.each() ([test](http://jsperf.com/jquery-each-vs-quickeach/83)).

Incorrect

```js
var array = [ ... ];
$.each(array, function(i, item) { ... });
```

Correct

```js
var array = [ ... ];
for (var i, len = array.length; i < len; i += 1) {
  var element = array[i];
}
```

##Discouraged Conventions <a id="discouraged"></a>[#](#discouraged)

JavaScript's language allows for a lot of flexibility when writing code. Within Backdrop, some less-common patterns are discouraged as they descrease the readability of the code or may be difficult for less-experienced developers to follow.

###"with" statement

The with statement was intended to provide a shorthand for accessing members in deeply nested objects. For example, it is possible to use the following shorthand (but not recommended) to access foo.bar.foobar.abc, etc:

```js
with (foo.bar.foobar) {
  var abc = true;
  var xyz = true;
}
```

However it's impossible to know by looking at the above code which abc and xyz will get modified. Does foo.bar.foobar get modified? Or is it the global variables abc and xyz?

Instead you should use the explicit longer version:

```js
foo.bar.foobar.abc = true;
foo.bar.foobar.xyz = true;
```

Or if you really want to use a shorthand, consider the following alternative method:

```js
var o = foo.bar.foobar;
o.abc = true;
o.xyz = true;
```

###Comma Operator

The comma operator causes the expressions on either side of it to be executed in left-to-right order, and returns the value of the expression on the right, and should be avoided. Example usage is:

```js
var x = (y = 3, z = 9);
```

This sets x to 9. This can be confusing for users not familiar with the syntax and makes the code more difficult to read and understand. So avoid the use of the comma operator except for in the control part of for statements.

###eval calls

`eval()` is evil. It effectively requires the browser to create an entirely new scripting environment (just like creating a new web page), import all variables from the current scope, execute the script, collect the garbage, and export the variables back into the original environment. Additionally, the code cannot be cached for optimization purposes. It is probably the most powerful and most misused method in JavaScript. It also has aliases. So do not use the `Function` constructor and do not pass strings to `setTimeout()` or `setInterval()`.

<small>Portions of this page based on [Drupal JavaScript Coding Standards](http://drupal.org/node/172169).</small>
