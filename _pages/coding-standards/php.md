---
permalink: coding-standards/php/
layout: bootstrap-post
title:  "Backdrop CMS PHP Coding Standards"
section: dev
---
{% include setup.liquid %}

# PHP Coding Standards

Backdrop's PHP coding standards mirror [those of Drupal](http://drupal.org/coding-standards).

- [Indenting and Whitespace](#indenting)
- [Operators](#operators)
- [Casting](#casting)
- [Control Structures](#control-structures)
- [Line length and wrapping](#line-length)
- [Function Calls](#function-calls)
- [Function Declarations](#function-declarations)
- [Class Constructor Calls](#constructors)
- [Type Hinting](#type-hinting)
- [Arrays](#array)
- [Quotes](#quotes)
- [String Concatenation](#string-concatenation)
- [Comments](#comments)
- [Including Code](#includes)
- [PHP Code Tags](#php-tags)
- [Semicolons](#semicolons)
- [Naming Conventions](#naming-conventions) (Functions, Constants, Global Variables, Classes, Files)
- [Example URLs](#example-urls)

##Indenting and Whitespace <a id="indenting">[#](#indenting)</a>

Use an indent of 2 spaces, with no tabs.

Lines should have no trailing whitespace at the end.

Files should be formatted with `\n` as the line ending (Unix line endings), not `\r\n` (Windows line endings).

All text files should end in a single newline (`\n`). This avoids the verbose "\ No newline at end of file" patch warning and makes patches easier to read since it's clearer what is being changed when lines are added to the end of a file.

##Operators <a id="operators"></a>[#](#operators)

All binary operators (operators that come between two values), such as `+`, `-`, `=`, `!=`, `==`, `>`, etc. should have a space before and after the operator, for readability. For example, an assignment should be formatted as `$foo = $bar;` rather than `$foo=$bar;`. Unary operators (operators that operate on only one value), such as `++`, should not have a space between the operator and the variable or number they are operating on.

##Casting <a id="casting"></a>[#](#casting)

Put a space between the (type) and the $variable in a cast: `(int) $my_number`.


##Control Structures <a id="control-structures"></a>[#](#control-structures)

Control structures include if, for, while, switch, etc. Here is a sample if statement, since it is the most complicated of them:

```php
if (condition1 || condition2) {
  action1;
}
elseif (condition3 && condition4) {
  action2;
}
else {
  defaultaction;
}
```

(Note: Don't use "else if" -- always use elseif.)

Control statements should have one space between the control keyword and opening parenthesis, to distinguish them from function calls.

Always use curly braces even in situations where they are technically optional. Having them increases readability and decreases the likelihood of logic errors being introduced when new lines are added.

For switch statements:

```php
switch (condition) {
  case 1:
    action1;
    break;

  case 2:
    action2;
    break;

  default:
    defaultaction;
}
```

For do-while statements:

```php
do {
  actions;
} while ($condition);
```

###Alternate control statement syntax for templates

In templates, the alternate control statement syntax using colons instead of brackets is encouraged. Note that there should not be a space between the closing paren after the control keyword, and the colon, and HTML/PHP inside the control structure should be indented. For example:

```php
<?php if (!empty($item)): ?>
  <?php print $item; ?>
<?php endif; ?>

<?php foreach ($items as $item): ?>
  <?php print $item; ?>
<?php endforeach; ?>
```

##Line length and wrapping <a id="line-length"></a>[#](#line-length)

The following rules apply to code. See [Doxygen and comment formatting conventions]({{ BASE_PATH }}coding-standards/doxygen) for rules pertaining to comments.

- In general, all lines of code should not be longer than 80 chars.
- Lines containing longer function names, function/class definitions, variable declarations, etc are allowed to exceed 80 chars.
- Control structure conditions may exceed 80 chars, if they are simple to read and understand:

  ```php
    if ($something['with']['something']['else']['in']['here'] == mymodule_check_something($whatever['else'])) {
      ...
    }
    if (isset($something['what']['ever']) && $something['what']['ever'] > $infinite && user_access('galaxy')) {
      ...
    }
    // Non-obvious conditions of low complexity are also acceptable, but should
    // always be documented, explaining WHY a particular check is done.
    if (preg_match('@(/|\\)(\.\.|~)@', $target) && strpos($target_dir, $repository) !== 0) {
      return FALSE;
    }
  ```

- Conditions should not be wrapped into multiple lines.
- Control structure conditions should also NOT attempt to win the *Most Compact Condition In Least Lines Of Code Award&trade;*:

  ```php
    // DON'T DO THIS!
    if ((isset($key) && !empty($user->uid) && $key == $user->uid) || (isset($user->cache) ? $user->cache : '') == ip_address() || isset($value) && $value >= time())) {
      ...
    }
  ```

  Instead, it is recommended practice to split out and prepare the conditions separately, which also permits documenting the underlying reasons for the conditions:

  ```php
    // Key is only valid if it matches the current user's ID, as otherwise other
    // users could access any user's things.
    $is_valid_user = (isset($key) && !empty($user->uid) && $key == $user->uid);

    // IP must match the cache to prevent session spoofing.
    $is_valid_cache = (isset($user->cache) ? $user->cache == ip_address() : FALSE);

    // Alternatively, if the request query parameter is in the future, then it
    // is always valid, because the galaxy will implode and collapse anyway.
    $is_valid_query = $is_valid_cache || (isset($value) && $value >= time());

    if ($is_valid_user || $is_valid_query) {
      ...
    }
  ```

*Note: This example is still a bit dense. Always consider and decide on your own whether people unfamiliar with your code will be able to make sense of the logic.*

##Function Calls <a id="function-calls"></a> [#](#function-calls)

Functions should be called with no spaces between the function name, the opening parenthesis, and the first parameter; spaces between commas and each parameter, and no space between the last parameter, the closing parenthesis, and the semicolon. Here's an example:

```php
$var = foo($bar, $baz, $quux);
```

As displayed above, there should be one space on either side of an equals sign used to assign the return value of a function to a variable. In the case of a block of related assignments, more space may be inserted to promote readability:

```php
$short         = foo($bar);
$long_variable = foo($baz);
```

##Function Declarations <a id="function-declarations"></a>[#](#function-declarations)

```php
function funstuff_system($field) {
  $system["description"] = t("This module inserts funny text into posts randomly.");
  return $system[$field];
}
```

Arguments with default values go at the end of the argument list. Always attempt to return a meaningful value from a function if one is appropriate.

##Class Constructor Calls <a id="constructors"></a>[#](#constructors)

When calling class constructors with no arguments, always include parentheses:

```php
$foo = new MyClassName();
```

This is to maintain consistency with constructors that have arguments:

```php
$foo = new MyClassName($arg1, $arg2);
```

Note that if the class name is a variable, the variable will be evaluated first to get the class name, and then the constructor will be called. Use the same syntax:

```php
$bar = 'MyClassName';
$foo = new $bar();
$foo = new $bar($arg1, $arg2);
```

##Type Hinting <a id="type-hinting"></a> [#](#type-hinting)

Whenever possible, function and method parameters should specify their data types using [type hinting](http://php.net/manual/en/language.oop5.typehinting.php). Because type hinting cannot be used with scalar types such as `int` or `string`, this only comes into play when arguments are arrays or objects.

```php
class MyClass {
  /**
   * First parameter must be an object implementing the OtherClassInterface.
   */
  public function test(OtherClassInterface $otherClass) {
      echo $otherClass->var;
  }


  /**
   * First parameter must be an array.
   */
  public function test_array(array $input_array) {
    print_r($input_array);
  }
```

When using type hinting on classes, it is recommended to specify the particular interface the class implements rather than the class name itself. This allows other classes that implement the same interface to be passed in, allowing for easier swappability.

##Arrays <a id="arrays"></a> [#](#arrays)

Arrays should be formatted with a space separating each element (after the comma), and spaces around the => key association operator, if applicable:

```php
$some_array = array('hello', 'world', 'foo' => 'bar');
```

Note that if the line declaring an array spans longer than 80 characters (often the case with form and menu declarations), each element should be broken into its own line, and indented one level:

```php
$form['title'] = array(
  '#type' => 'textfield',
  '#title' => t('Title'),
  '#size' => 60,
  '#maxlength' => 128,
  '#description' => t('The title of your node.'),
);
```

Note the comma at the end of the last array element; This is not a typo! It helps prevent parsing errors if another element is placed at the end of the list later.

##Quotes <a id="quotes"></a>[#](#quotes)
Backdrop does not have a hard standard for the use of single quotes vs. double quotes. Where possible, keep consistency within each module, and respect the personal style of other developers.

With that caveat in mind: single quote strings are known to be faster because the parser doesn't have to look for in-line variables. Their use is recommended except in two cases:

1. In-line variable usage, e.g. `"<h2>$header</h2>"`.
2. Translated strings where one can avoid escaping single quotes by enclosing the string in double quotes. One such string would be `"He's a good person."` It would be `'He\'s a good person.'` with single quotes.

##String Concatenations <a id="string-concatenation"></a>[#](#string-concatenation)

Always use a space between the dot and the concatenated parts to improve readability.

```php
$string = 'Foo' . $bar;
$string = $bar . 'foo';
$string = bar() . 'foo';
$string = 'foo' . 'bar';
```

When you concatenate simple variables, you can use double quotes and add the variable inside; otherwise, use single quotes.

```php
$string = "Foo $bar";
```

When using the concatenating assignment operator (`.=`), use a space on each side as with the assignment operator:

```php
$string .= 'Foo';
$string .= $bar;
$string .= baz();
```

##Comments <a id="comments"></a>[#](#comments)

Standards for documenting functions, classes, and methods are discussed on the separate [Doxygen and comment formatting conventions]({{ BASE_PATH }}coding-standards/doxygen).

Comments within functions and methods should use capitalized sentences with punctuation. All caps are used in comments only when referencing constants, e.g., TRUE. Comments should be on a separate line immediately before the code line or block they reference. For example:

```
// Unselect all other checkboxes.
```

If each line of a list needs a separate comment, the comments may be given on the same line and may be formatted to a uniform indent for readability.

C style comments (`/* */`) and standard C++ comments (`//`) are both fine.

##Including Code <a id="includes"></a>[#](#includes)

Anywhere you are unconditionally including a class file, use `require_once()`. Anywhere you are conditionally including a class file (for example, factory methods), use `include_once()`. Either of these will ensure that class files are included only once. They share the same file list, so you don't need to worry about mixing them - a file included with `require_once()` will not be included again by `include_once()`.

*Note: `include_once()` and `require_once()` are statements, not functions. You don't need parentheses around the file name to be included.*

When including code from the same directory or a sub-directory, start the file path with  ".": 
`include_once ./includes/mymodule_formatting.inc`

When including a file relative to the root of the installation (such as a core/includes file), use DRUPAL_ROOT:
`require_once DRUPAL_ROOT . '/core/includes/cache.inc';```

##PHP Code Tags <a id="php-tags"></a>[#](#php-tags)

Always use `<?php ?>` to delimit PHP code, not the shorthand, `<? ?>`. This is the most portable way to include PHP code on differing operating systems and setups.

Note that the `?>` at the end of code files is purposely omitted. This includes for module and include files. The reasons for this can be summarized as:

- Removing it eliminates the possibility for unwanted whitespace at the end of files which can cause "header already sent" errors, XHTML/XML validation issues, and other problems.
- The [closing delimiter at the end of a file is optional](http://www.php.net/basic-syntax.instruction-separation).
- PHP.net itself removes the closing delimiter from the end of its files (example: [prepend.inc](http://www.php.net/source.php?url=/include/prepend.inc)), so this can be seen as a "best practice."

##Semicolons <a id="semicolons"></a>[#](#semicolons)

The PHP language requires semicolons at the end of most lines, but allows them to be omitted at the end of code blocks. Backdrop coding standards require them, even at the end of code blocks. In particular, for one-line PHP blocks:

```php
<?php print $tax; ?> -- YES
<?php print $tax ?> -- NO
```


##Naming Conventions <a id="naming-conventions"></a>[#](#naming-conventions)

###Functions and variables

Functions and variables should be named using lowercase, and words should be separated with an underscore. Functions should in addition have the grouping/module name as a prefix, to avoid name collisions between modules.

```php
function my_module_example_function() {
  $my_variable = 'Some example string';
  return $my_variable;
}
```

###Persistent Variables

Persistent variables (variables/settings defined using Backdrop's <a href="http://api.drupal.org/api/function/variable_get">variable_get()</a>/<a href="http://api.drupal.org/api/function/variable_set">variable_set()</a> functions) should be named using all lowercase letters, and words should be separated with an underscore. They should use the grouping/module name as a prefix, to avoid name collisions between modules.

```php
variable_get('my_module_setting', 'foo');
$value = variable_get('module_module_setting');
```

###Constants

Constants should always be all-uppercase, with underscores to separate words. This includes pre-defined PHP constants like `TRUE`, `FALSE`, and `NULL`. Module-defined constant names should also be prefixed by an uppercase spelling of the module that defines them.

```php
define('MY_MODULE_FLAG_A', 0);
define('MY_MODULE_FLAG_B', 1);
```

###Global Variables

*Any use of custom globals is discouraged*. Instead use a function that wraps a static variable. An alternative to global variables:

```php
function my_module_set_variable($value) {
  static $stored_value = NULL;
  if (isset($value)) {
    $stored_value = $value;
  }
  return $stored_value;
}
function my_module_get_variable() {
  return my_module_set_variable();
}

// When setting the variable:
my_module_set_variable('Some value');

// When getting the variable:
$value = my_module_get_variable();
```

If you absolutely need to define global variables, their name should start with a single underscore followed by the module/theme name and another underscore.

```php
$_GLOBALS['_my_module_variable'] = 'foo';

```

###Classes

Classes should be named using "CamelCase." For example:

```php
abstract class DatabaseConnection extends PDO {
```

Class methods and properties should use "lowerCamelCase":

```
public $lastStatement;

public function getLastStatement() {
  ...
}
```

Class names themselves should adhere to these rules:

1. Class names should not have "Class" in the name.
2. Interfaces should always have the suffix "Interface".
3. Test classes should always have the suffix "Test".

###File names

All documentation files should have the file name extension ".md" to make viewing them on Github easier. Also, the file names for such files should be all-caps (e.g. README.md instead of readme.md) while the extension itself is all-lowercase (i.e. md instead of MD).

Examples: README.md, INSTALL.md, TODO.md, CHANGELOG.md etc.

##Example URLs <a id="example-urls"></a>[#](#example-urls)

Use "example.com" for all example URLs, per [RFC 2606](http://www.rfc-editor.org/rfc/rfc2606.txt).

<small>Portions of this page based on [Drupal PHP Coding Standards](http://drupal.org/coding-standards).</small>
