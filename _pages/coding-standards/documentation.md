---
permalink: coding-standards/documentation/
layout: bootstrap-post
title:  "Backdrop CMS Code Documentation Standards"
section: dev
---
{% include setup.liquid %}

# Code Documentation Standards

This page covers how to write inline documentation within your code so that it can be parsed and display in the public developer documentation site. This page is separated into two major sections, a general overview of how to document your code and a complete tag reference.

- **Notes and Standards**
  - [About Backdrop documentation](#about)
  - [General guidelines](#general)
  - [Backdrop standards for in-line code comments](#inline)
  - [Function docblocks](#functions)
  - [Theme template file docblocks](#templates)
  - [Class docblocks](#classes)
  - [Order of documentation sections](#order)
  - [Making lists in documentation](#lists)
  - [Indicating data types in documentation](#types)
- **Tag Reference**
  - [@code and @endcode: Code samples](#code)
  - [@defgroup, @addtogroup, @ingroup, @{, @}: Groups and topics](#groups)
  - [@deprecated: Indicating deprecated functionality](#deprecated)
  - [@file: Documenting files ](#file)
  - [{@inheritdoc}: Overridden method documentation inheritance](#inheritdoc)
  - [@link and endlink: HTML links](#link)
  - [@mainpage: Main documentation for a branch](#mainpage)
  - [@param: Function parameters](#param)
  - [@ref: References to sections](#ref)
  - [@return: Function return values](#return)
  - [@section and @subsection: Documentation sections](#section)
  - [@see: See Also references](#see)
  - [@throws: Thrown exceptions](#throws)
  - [@todo: To Do notes](#todo)
  - [@var: Class property data types](#var)

##Notes and Standards

###About Backdrop documentation <a id="about"></a>[#](#about)

The API module parses documentation and code in PHP files, and it expects documentation to be in a format similar to other code/documentation parsing systems such as PHPDoc, JavaDoc, etc. It was originally based on [Doxygen](http://www.stack.nl/~dimitri/doxygen/manual/index.html), but it has evolved into something that has its own set of tags and a lot of Backdrop-specific functionality.

The API module parses documentation that is in special documentation blocks (known as "docblocks" in the rest of this document).

Syntax example:

```
/**
 * Sample summary line.
 *
 * Next paragraph. Etc.
 */
(code being documented goes here, such as a function declaration, class, etc.)
```

General notes on the API module and the documentation it will parse:

- The API module treats files with the following extensions as PHP: .php, .module, .inc, .install, .engine., .theme, .profile, and .test.
- When parsing a PHP file, the API module parses both documentation and PHP code. PHP files with syntax errors could cause problems.
- The special documentation tags described in this document are only recognized within special PHP comment blocks that start with `/**`. These are known in this document as <em>docblocks</em>.
- In-code comment lines starting with `//` and comment blocks starting with `/*` are not recognized as docblocks.
- Docblocks normally have `*` at the beginning of each line, and the API module strips these out when formatting the documentation (see example above).
- To make a paragraph break in a docblock, leave a blank line (see example above). Some tags also trigger paragraph breaks: `@param`, `@return`, `@see`, `@var`. The API module does not support blank lines within a single tag's documentation (for example, within a single parameter's documentation with `@param`.
- The API module itself does not care about line length (Backdrop documentation standards do, however, see below).
- The first paragraph of a docblock is known as the <em>summary</em>.
- To document a function, class, etc., the docblock must appear directly before the item being documented, with no blank line in between (see example above). There are also a few "free-standing" docblocks (for documenting files and making topics -- see tag reference for details).
- The API module automatically turns the names of functions, classes, etc. that it recognizes in documentation text into links to the documentation of those items.


###General guidelines <a id="general"></a>[#](#general)

In the individual tag references, there are notes about Backdrop standards that pertain to that particular tag. This section contains standards related to all API docblocks.

Syntax example:

```
/**
 * Sample summary line.
 *
 * Next paragraph. Etc.
 */
(code being documented goes here, such as a function declaration, class, etc.)
```

Backdrop standards notes:

- All documentation and comments should form proper sentences,  use proper grammar and punctuation.
- Sentences should be separated by single spaces.
- Comments and variable names should be in English, and use US English spelling (e.g., "color" not "colour").
- To refer to a module or theme, use wording and capitalization like "the Foo Bar module" (in other words, the module/theme name is a proper noun, not including the word "module" or "theme" in the proper noun).
- All caps are used in comments only when referencing constants, for example TRUE.
- Docblocks must have `*` at the beginning of each line (see sample above).
- Lines containing comments (including docblocks) must wrap as close to 80 characters as possible without going over, with a few exceptions (noted in the Tag Reference below).
- Lines after tags such as `@param`, `@return`, etc. that contain documentation connected with that tag are indented two characters. See syntax examples for each tag.
- Every function, constant, class, interface, class member (function, property, constant), and file must be documented, even private class members.
- All summaries (first lines of docblocks) must be under 80 characters, start with a capital letter, and end with a period (.). They must provide a brief description of what a function does, what a class does, what a file contains, etc.
- When implementing a hook, use a short summary of the form `Implements hook_menu().`. Omit the parameter and return value documentation in this case.


###Backdrop standards for in-line code comments <a id="inline"></a>[#](#inline)

Non-header or in-line comments are strongly encouraged. A general rule of thumb is that if you look at a section of code and think "Wow, I don't want to try and describe that", you need to comment it before you forget how it works. Comments should be on a separate line immediately before the code line or block they reference. For example:

```
  // Unselect all other contact categories.
  db_query("UPDATE {contact} SET selected = 0");
```

If each line of a list needs a separate comment, the comments may be given on the same line and may be formatted to a uniform indent for readability.

C style comments (`/* */`) and standard C++ comments (`//`) are both fine, though the former is discouraged within functions (even for multiple lines, repeat the `//` single-line comment). Use of Perl/shell style comments (`#`) is discouraged.

`@todo` statements in inline comments follow the same rules as [using the @todo tag in docblocks](#todo). An example:

```
  // Some other comment here.
  // @todo Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam
  //   nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat,
  //   sed diam voluptua.
  // We explicitly delete all comments.
  comment_delete($cid);
```

###Function docblocks <a id="functions"></a>[#](#functions)

Documenting functions is somewhat more complex than documenting other types of PHP language constructs, so they have their own coding standards:

- Each parameter of a function must be documented with a `@param` tag (see exceptions below).
- If a function has a return value, it must be documented with a `@return` tag (see exceptions below).
- If there is no return value for a function, there must not be a `@return` tag.
- For **most functions** (see exceptions below), summary lines must start with a third person singular present tense verb, and they must explain what the function does. Example: "Calculates the maximum weight for a list."
- Functions that are easily described in one line may be documented by providing the function summary line only (omitting all parameters and return value).

In addition, there are standards for documenting specific special types of functions:


**Hook definition**

The summary starts with an imperative verb that tells why a module would want to implement the hook. Example: "Respond to node deletion." Hook definitions are placed in a `.api.php` file, which is not directly loaded by Backdrop. Each is a fake function whose name starts with "hook", and whose function body is a sample implementation. Parameters and return value that the implementer must use need to be documented.

**Hook implementation**

Documentation uses a short format:

```
/**
 * Implements hook_help().
 */
function blog_help($section) {
```

If appropriate, you can add additional details pertinent to the particular hook implementation in a separate paragraph after the summary. Do not document paramters and return value though -- these are documented on the hook definition referenced in the summary line. And if the hook name has a variable portion:

```
/**
 * Implements hook_form_FORM_ID_alter() for node_type_form().
 */
function mymodule_form_node_type_form_alter(&$form, &$form_state) {
```

**Update functions (implementations of hook_update_N())**

These use a different syntax, because the documentation summary (first line) is displayed to someone running update.php to tell them which updates need to run. So they are documented like this:

```
/**
 * Add several fields to the {node_revision} table.
 *
 * Longer description can go here, if necessary.
 */
function  module_name_update_7002() {
```

Note that the first line should start with an imperative verb, so it will make sense to people running update.php.

**Callbacks for built-in PHP functions**

Many PHP functions, such as uasort() etc., require the caller to pass in a [callback function](http://us.php.net/manual/en/language.pseudo-types.php#language.types.callback). When documenting a function that is intended to be used as a callback in this way, start with a standard function summary line. After the summary, the next paragraph should indicate what function the callback is passed to, and which Backdrop function makes use of it (which could be a list of functions). Example:

```
/**
 * [standard first line]
 *
 * Callback for uasort() within system_themes_page().
```

**Callback definitions and implementations**

Like standard PHP functions, sometimes the Backdrop API also needs callbacks to operate. In this context, a "callback" is a function that a module needs to define in conjunction with a hook implementation, batch set, plugins, or other Backdrop functionality. 

In order to document the parameters and return value of callbacks, make a callback function definition, with a documentation block, in a *.api.php file, similar to hook definition documentation (i.e., they are "dummy" functions in files that are not actually loaded, but they can be referenced in other documentation). Then, in the hook or other function that requires the implementer/caller to return a callback function name, refer to this callback definition function by name.

Notes:

- Callback definition function names in the *.api.php file should start with "callback_", followed by a descriptive name.
- Callback definition function bodies should be an example implementation (like hook definition function bodies).
- The one-line documentation summary should describe what the callback does. The summary starts with an imperative verb that tells why a module would want to define this callback.
- The next line should start with "Callback for ___:" (replace ___ with the name of the hook, function, or other component that uses the callback).
- They should be added to the "@ingroup callbacks" topic.
- The hook or other function that uses the callback should refer to the callback definition function by name in its documentation.
- An actual callback function in module code should have as its one-line documentation summary "Implements callback_NAME().", so that its documentation links to the callback documentation.


Example 1:

```
// In *.api.php file:

/**
 * Perform tasks when a batch is complete.
 *
 * Callback for batch_set().
 *
 * @param bool $success
 *   A boolean indicating whether the batch operation successfully concluded.
 * @param int $results
 *   The results from the batch process.
 * @param array $operations
 *   The batch operations that remained unprocessed. Only relevant if $success
 *   is FALSE.
 *
 * @ingroup callbacks
 */
function callback_batch_finished(success, $results, $operations) {
  // Sample code here
}

// Documentation for batch_set():

 *   - finished: Name of an implementation of callback_batch_finished(). This is
 *     executed after the batch has
 *     completed. This should be used to perform any result massaging that may
 *     be needed, and possibly save data in $_SESSION for display after final
 *     page redirection.

// Note: especially in Backdrop 8, these callback implementations may be
// class methods rather than functions, which are passed in as PHP
// callables rather than strings. So an alternate wording that
// implies that this is also acceptable would be:

 *   - finished: A callable that implements callback_batch_finished().

// Actual callback: _node_mass_update_batch_finished() function.

/**
 * Implements callback_batch_finished().
 */
function _node_mass_update_batch_finished($success, $results, $operations) {
  // ...
}
```

Example 2 (entity URI callback):

```
// In *.api.php file:

/**
 * Return the URI for an entity.
 *
 * Callback for hook_entity_info().
 *
 * @param $entity
 *  The entity to return the URI for.
 *
 * @return
 *  An associative array with element 'path' giving the URL path, and 'options' giving options for
 *  the url() function.
 *
 * @ingroup callbacks
 */
function callback_entity_info_uri($entity) {
  return array(
    'path' => 'node/' . $entity->nid,
  );
}

// Documentation for hook_entity_info() in the @return section:

 *   - uri callback: The name of an implementation of callback_entity_info_uri().

// or

 *   - uri callback: A callable implementing callback_entity_info_uri().

// Actual callback: node_uri() function.

/**
 * Implements callback_entity_info_uri().
 */
function node_uri($node) {
  return array(
    'path' => 'node/' . $node->nid,
  );
}
```

**Form-generating functions**

The form constructor, validation handler(s), and submission handler(s) form a set and are documented as in this sample:

```
/**
 * Form constructor for the user login form.
 *
 * @param string $message
 *   The message to display.
 *
 * @see user_login_form_validate()
 * @see user_login_form_submit()
 *
 * @ingroup forms
 */
function user_login_form($form, &$form_state, $message = '') {
   ...
}

/**
 * Form validation handler for user_login_form().
 *
 * @see user_login_form_submit()
 */
function user_login_form_validate($form, &$form_state) {
   ...
}

/**
 * Form submission handler for user_login_form().
 *
 * @see user_login_form_validate()
 */
function user_login_form_submit($form, &$form_state) {
   ...
}
```

Notes:

- Omit `@param` and `@return` documentation for the standard parameters and return value (such as `$form` and `$form_state`.
- Document parameters specific to this form constructor.


**hook_menu() page router callback functions**

Note: This standard was adopted for Backdrop version 8.x, which is a bit ironic since Backdrop 8.x subsequently did away with hook_menu() itself. The standard still applies to the callbacks used by the router, although they are not technically hook_menu() callbacks any more.

Sample syntax:

```
/**
 * Page callback: Displays a list of content.
 *
 * Longer description can go here.
 *
 * @param string $foo
 *   Description of a parameter for this page.
 *
 * @return array
 *   A render array for a page containing a list of content.
 *
 * @see node_menu()
 */
(function here)
```

Backdrop standards notes:

- In the first line, declare what type of callback it is (Page callback, Access callback, etc.), and give a short description of what it does (like you would for any function). Total line length: 80 characters, ending in "."
- At the end, include an @see line that points to the hook_menu() implementation where this callback is registered. If multiple hook implementations use the callback, include one @see line for each.
- Form constructors that are used as page callbacks should have a first line like "Page callback: Constructs a form for ...".


**Render API callback functions**

The Render API uses various callback functions, which you can assign to a render element when setting up a render array (or a form). Generally, they are documented without parameters or return value documentation, since the parameters and return value are standard for that type of callback. The summary line should be "Render API callback:" followed by a description of what the function does, conforming to the usual function summary standards. In addition, somewhere in the function, there should be a paragraph saying where the callback is assigned and what particular Render API callback it is being used for.

Syntax example:

```
/**
 * Render API callback: Validates the maximum upload size field.
 *
 * Ensures that a size has been entered and that it can be parsed by
 * parse_size().
 *
 * This function is assigned as an #element_validate callback in
 * file_field_instance_settings_form().
 */
function _file_generic_settings_max_filesize($element, &$form_state, $form) {
```

**Themeable functions**

Themeable functions (`theme_foo()`, for example -- functions meant to be called via `theme()` and overrideable by the theme) are documented using these standards:

- The first line should be "Returns HTML for a ..."
- All components of the `$variables` array (or other arguments) need to be documented.
- They should have `@ingroup themeable` in their documentation.

Syntax example:

```
/**
 * Returns HTML for a foo.
 *
 * @param array $variables
 *   An associative array containing:
 *   - foo: The foo object that is being formatted.
 *   - show_bar: TRUE to show the bar component, FALSE to omit it.
 *
 * @ingroup themeable
 */
function theme_foo($variables) {
```

**Template preprocess functions**

Template preprocess functions (template_preprocess_container(), for example) are documented using these standards:

- The first line should be "Prepares variables for [description of foo] templates."
- The second line should be "Default template: foo-bar.tpl.php" where foo-bar.tpl.php is the filename of the default template file.
- All components of the $variables array need to be documented.

Syntax example:

```
/**
 * Prepares variables for container templates.
 *
 * Default template: container.tpl.php.
 *
 * @param array $variables
 *   An associative array containing:
 *   - element: An associative array containing the properties of the element.
 *     Properties used: #id, #attributes, #children.
 */
function template_preprocess_container(&$variables) {
```

###Theme template file docblocks <a id="templates"></a>[#](#templates)

The default implementations for PHP theme template files (*.tpl.php) start with a documentation block that follows the generic standards for [@file](#file) blocks.

Syntax example:

```
/**
 * @file
 * Displays a list of forums.
 *
 * Available variables:
 * - $forums: An array of forums to display. Each $forum in $forums contains:
 *   - $forum->is_container: Is TRUE if the forum can contain other forums. Is
 *     FALSE if the forum can contain only topics.
 *   - $forum->depth: How deep the forum is in the current hierarchy.
 *   - $forum->name: The name of the forum.
 *   - $forum->link: The URL to link to this forum.
 *
 * @see template_preprocess_forum_list()
 *
 * @ingroup themeable
 */
(template markup/code starts here)
```

Backdrop standards notes:

- The variables that are set up in the template_preprocess function should be documented.
- Do not include `@ingroup themeable` in overridden template files in a theme -- only in the base version. Otherwise, theme-specific versions are documented the same way.
- If any variable contains data that has not been sanitized, this should be noted.
- Always include an @see reference to the preprocess function.

###Class docblocks <a id="classes"></a>[#](#classes)

The following standards apply to documenting classes and using namespaces in Backdrop API documentation:

- All classes and all of their methods (including private methods) must be documented.
- If a class has a method that is overriding a method from a parent class/interface, and the documentation is identical, use this short form for the documentation:

```
/**
 * {@inheritdoc}
 */
public function ...
```

- Use a third person verb to start the summary of a class, interface, or method. For example: "Represents a ..." or "Provides...".
- Document exceptions with [@throws](#throws).
- If you use a namespace in documentation, always make sure it is a fully-qualified namespace (beginning with a backslash).
- Other namespace-related standards for Backdrop are under discussion and have not yet been adopted permanently.

###Order of documentation sections <a id="order"></a>[#](#order)

Docblocks for functions, classes, and other PHP items should be written in the following order to comply with Backdrop coding standards. Omit sections that don't apply (for instance, `@param` only applies to functions). Separate different-type sections by a blank line (for instance, all the `@param` documentation goes together, with a blank line before the first parameter and a blank line after the last parameter before the `@return` section starts). Here's the order:

- One-line summary, ending in a period (.).
- Additional paragraph(s) of explanation.
- [@var](#var)
- [@param](#param)
- [@return](#return)
- [@throws](#throws)
- [@ingroup](#defgroup)
- [@deprecated](#deprecated)
- [@see](#see)
- [@todo](#todo)
- [@Plugin](#Plugin) and other annotations

###Making lists in documentation <a id="lists"></a>[#](#lists)

The API module creates nested bullet lists from lines starting with `-` in docblocks.

Syntax example:

```
 * Lists are usually preceded by a line ending in a colon:
 * - Item in the list.
 * - Another item.
 *   - key: Sub-list with keys, first item.
 *   - key2: (optional) Second item with a key.
 * - Back to the outer list. Sometimes list items are quite long, in which case
 *   you can wrap the text like this.
 * - Last item in the outer list.
 * Text that is outside of the list continues here.
```

Syntax notes:

- A hyphen as the first character on the line (except the `*`) indicates a list item.
- All list items at the same nesting level must be indented the same amount.
- There must not be blank lines between items in the same list.
- If a list has "keys" (for instance, a list describing an associative array, or the options for a parameter), use a colon between the key and its description. The API module will format keys with a `strong` tag.

Backdrop standards notes:

- The line before a list or before a new level of nesting must end with a colon (:).
- The hyphen aligns with the start of the text containing that level of list, which means that each level of nesting indents by two spaces.
- Array keys are not put in quotes; keys describing literal strings (such as parameter values) should be put in quotes.
- Indicate optional list items with `(optional)` and default list items with `(default)`. With keys, these words are placed just after the colon.

###Indicating data types in documentation<a id="types"></a>[#](#types)

Certain tags can include data types (`@param`, `@return`, etc.).

Syntax examples:

```
int
string|bool
\Backdrop\Core\Database\StatementInterface
```

Syntax notes:

- Data types can be primitive types (int, string, etc.), complex PHP built-in types (array, object), or PHP classes.
- If only one data type is possible, just use its name.
- If multiple types are possible, separate them by a vertical bar ("|").

Backdrop standards notes:

- Use interface names if possible, or the most general possible class, in place of a specific class.
- Use fully-qualified namespaces on class names if using namespaces.
- For the PHP built-in types, use the following names:

- array (NOT "Array")
- bool (NOT "boolean" or "Boolean"). If only TRUE or only FALSE is a possible value, rather than either one being possible, use true or false instead of bool.
- false (NOT "FALSE", see bool)
- float
- int (NOT "integer")
- null (NOT "NULL")
- object (NOT "stdClass")
- string
- true (NOT "TRUE", see bool)

##Tag Reference

###@code: Code samples <a id="code"></a>[#](#code)

The `@code ... @endcode` tag is used to embed code samples in docblock text. The API module will attempt to format the contents as PHP code when displaying the documentation.

Syntax example:

```
 * Example usage:
 * @code
 * mymodule_print('Hello World!');
 * @endcode
 * Text to immediately follow the code block.
```

Backdrop standards: Put the `@code` and `@endcode` tags on their own lines. Do not use a blank line between the text that explains the code sample and the code sample itself.

###@defgroup, @addtogroup, @ingroup, @{, @}: Groups and topics <a id="groups"></a>[#](#groups)

The `@defgroup` tag is used to define a "group" (in Doxygen terms), which the API module displays as a Topic page. A `@defgroup` tag needs to be in its own docblock (not inside a file description docblock, function docblock, class docblock, etc.). A group defined by `@defgroup` has an identifier (starting with a letter, and composed of numbers, letters, underscores, periods, and hyphens), a title, a summary, and documentation. In addition, individual "items" (files, functions, classes, and other things that are documented with docblocks in the API module) can be designated as being "in" the group. The API module makes a page for each group/topic, and on the page, it lists all of the items that are part of the group.

The examples below show how to define a group using the `@defgroup` tag, and three different ways to define the members of the group, using tags `@{`, `@}`, `@addtogroup`, and `@ingroup`.

Syntax example 1 for defining a group:

```
/**
 * @defgroup group_identifier Topic title goes here
 * @{
 * Summary line for the group/topic goes here.
 *
 * Additional documentation paragraphs go here.
 * @}
 */
```

Syntax example 2 for defining a group and adding members to it at the same time:

```
/**
 * @defgroup group_identifier Topic title goes here
 * @{
 * Summary line for the group/topic goes here.
 *
 * Additional documentation paragraphs go here.
 */

(functions, classes, etc. that belong as members of the group go here)

/**
 * @} End of "defgroup group_identifier".
 */
```

Syntax example for adding several items as members of an existing group:

```
/**
 * @addtogroup group_identifier
 * @{
 */

(functions, classes, etc. that belong as members of the group go here)

/**
 * @} End of "addtogroup group_identifier".
 */
```

Syntax example for adding one item as a member of a group:

```
 * (inside the docblock for this item)
 *
 * @ingroup group_identifier
```

Syntax notes:

- Only define each group identifier once with a `@defgroup` tag. To add more functions to the group that are in a separate file or location, use either `@addtogroup` or `@ingroup`.
- Every docblock within `@{ ... @}` is included in the group. This means that if a class is within these tags, not only the class itself, but also its members will be included in the group. You probably don't want that -- so use `@ingroup` to add a class to a group (putting the tag in the class's docblock).
- `@defgroup` needs to be its own docblock (do not include it in a docblock for a function, file description, class, etc.).


Backdrop standards notes:

- Use groups sparingly. Each group makes a topic page. Only use groups if there is some reference information you need to impart, related to several different functions or classes or constants. If information pertains to only one function, for instance, just put the information in that one function's docblock.

###@deprecated: Indicating deprecated functionality <a id="deprecated"></a>[#](#deprecated)

The `@deprecated` tag is placed in a documentation block to indicate that a function, method, or class has been deprecated and should not be used, but has not yet been removed.

Syntax:

```
 * @deprecated DESCRIPTION
```

The prose description SHOULD be one or more complete sentences, and SHOULD include:

- The version in which the function was first deprecated (or, if added in a dev version, the next expected stable tag).
- When the function will be removed outright, if known.
- An explanation of why the function was deprecated.
- What new mechanism to use to achieve the previous functionality or note that the functionality is slated for outright removal.

A [@see](#see) directive to a direct replacement MAY be included as well.

###@file: Documenting files <a id="file"></a>[#](#file)

The `@file` tag is placed in a docblock to indicate it is documentation for the file as a whole. The API module will only recognize one file docblock per file (the first one; there shouldn't be more than one).

Syntax example:

```
/**
 * @file
 * The theme system, which controls the output of Backdrop.
 *
 * The theme system allows for nearly all output of the Backdrop system to be
 * customized by user themes.
 */
```

Syntax notes:
As in other docblocks, the first paragraph in the file docblock is used as a summary, and the rest is additional documentation. The docblock must be followed by a blank line, and must not contain `@defgroup` or `@mainpage` tags.

Backdrop standards: If a file docblock summary begins with a verb, it should be in third person singular present tense, such as "Handles file uploads." The summary should be one line of up to 80 characters ending in ".".

Backdrop sample for module .install files:

```
/**
 * @file
 * Install, update, and uninstall functions for the Foo Bar module.
 */
```

Backdrop sample for files containing one class/interface:

```
/**
 * @file
 * Contains \Fully\Qualified\Namespace\And\NameOfTheClass.
 */
```

###{@inheritdoc}: Overridden method documentation inheritance <a id="inheritdoc"></a>[#](#inheritdoc)

In a class, if you are overriding or implementing a method from a base class or interface, and the documentation should be exactly the same as the base class/interface method, use the following docblock syntax:

```
  /**
   * {@inheritdoc}
   */
  public function getValue() {
    return $this->value;
  }
```

Note the {} around the @inheritdoc tag. This must be the only line in the docblock.

###@link: HTML links <a id="link"></a>[#](#link)

The `@link ... @endlink` tag is used to make HTML links in docblock text.

Syntax example:

```
 * See also @link topic_foo Foo topic, @endlink and the
 * @link http://example.com web page explaining it further. @endlink
```

Syntax notes:
`@link` is followed by a space; followed by the name of a function, name of a class, file name, [group identifier](#defgroup), or URL; followed by a space; followed by the link text; followed by `@endlink`. The entire `@link ... @endlink` must be on the same line, and must be preceded and followed by whitespace or newlines. It is not necessary to use `@link ... @endlink` to make a link to a function, class, or file if the link text would be the same as the function name, class name, or file name (those will turn into links in text without using  `@link ... @endlink`.

Backdrop standards: The  `@link ... @endlink` line is allowed to exceed 80 characters. If it will fit on the preceding or following line along with additional text in the same paragraph that precedes or follows it without exceeding 80 characters, it can be placed on a line with other text. However, if putting it with other text would make the line exceed 80 characters, place it on its own line.

###@mainpage: Main documentation for a branch <a id="mainpage"></a>[#](#mainpage)

The `@mainpage` tag is used to define the main documentation page for a "branch" (a set of code, such as all the code for Backdrop 1.x, or all the code for the "foo" module) in the API module. This tag must appear in its own docblock. If a branch contains a file with a mainpage docblock, this documentation is used as the landing page for the branch in the API module. If there is no mainpage docblock, the API module constructs a default landing page.

Syntax example:

```
/**
 * @mainpage
 * Welcome to the Backdrop developer's documentation. (further text here)
 */
```


###@param: Function parameters <a id="param"></a>[#](#param)

The `@param` tag is used to document a parameter within a function docblock.

Syntax examples:

```
 * @param string $mail
 *   The email address. The description can be longer if necessary, and if so,
 *   you can wrap it to another line.
 * @param string $from
 *   (optional) The email address to send the mail from, if different from
 *   the site-wide address.   
```

Syntax notes:
The `@param` tag is followed by an optional [data type indicator](#types), then the variable name of the parameter, and then a newline. The following paragraph is considered by the API module to be documentation. The API module formats the information on the same line as the `@param` using a `strong` HTML tag.

Backdrop standards: The documentation is indented two lines (see example). Data types are required to be included as of Backdrop 8.x. Optional parameters are indicated by (optional); include information about the default value only if it is not obvious from the function signature.

###@ref: References to sections <a id="ref"></a>[#](#ref)

Note: These tags are available in API module versions 7.x-1.1 and later.

The `@ref` tag is used to make in-page links to sections and sub-sections defined with the [`@section/@subsection`](#section) tags.

Syntax example:

```
 * This is a link to @ref sec_one right here in the text.
 *
 * @section sec_one Section 1
 * One or more paragraphs for section 1 go here.
```

Syntax note: To make an in-page link to a section or sub-section, insert the `@ref` tag in the text, followed by one space (NOT a newline), followed by the section or sub-section identifier. The link text will be the title defined when you declare the section or sub-section ("Section 1" in this example).


###@return: Function return values <a id="return"></a>[#](#return)

The `@return` tag is used to document the return value within a function docblock.

Syntax example:

```
 * @return string
 *   The email address. The description can be longer if necessary, and if so,
 *   you can wrap it to another line.
```

Syntax notes:
The `@return` tag is followed by an optional [data type indicator](#types), and then a newline. The following paragraph is considered by the API module to be documentation.

Backdrop standards: The documentation is indented two lines (see example). Data types are required to be included as of Backdrop 8.x. Functions without return values must not have @return documentation.

###@section, @subsection: Documentation sections <a id="section"></a>[#](#section)

Note: These tags are available in API module versions 7.x-1.1 and later.

The `@section` tag is used to make section headings within a docblock, and the `@subsection` tag is used to make sub-sections. The API module renders sections as `h3` HTML tags, and sub-sections as `h4` tags. You can also use the [`@ref`](#ref) tag to make in-page links to sections and sub-sections.

Syntax example:

```
 * @section sec_one Section 1
 * One or more paragraphs for section 1 go here.
 *
 * @subsection sub_a Sub-section A
 * One or more paragraphs for sub-section A go here.
```

Syntax notes:

- Each `@section` and `@sub-section` declaration must be on its own line.
- A declaration consists of the `@section/@subsection` tag, followed by one space, then an identifier (letters, numbers, underscores, and hyphens only), then one space, and then the section/sub-section title.


###@see: See Also references <a id="see"></a>[#](#see)

The `@see` tag is used in a docblock to indicate a "See Also" reference. The API module collects these and formats them into a single See Also section on the documentation page.

Syntax example:

```
 * @see foo_bar()
 * @see ajax.inc
 * @see \My\Namespace\MyModuleClass
 * @see \My\Namespace\MyClass::myMethod()
 * @see groupname
 * @see http://backdropcms.org/example
```

Syntax notes:
Follow the `@see` tag by a space, and then the item or URL you want to reference. An item can be a function name, class name, method name (with the class), constant name, file name, [group identifier](#defgroup), or a URL. The API module will format each item it recognizes as a link, and all of the text it finds between `@see` and the next documentation tag or blank line in its own paragraph.

Backdrop standards: Each `@see` reference is on its own line, with no additional text beyond the item being referenced. To provide more explanation, just use a regular documentation paragraph.

###@throws: Thrown exceptions <a id="throws"></a>[#](#throws)

The `@throws` tag is used to document that a function or method throws an exception, and if a function/method has this tag, the API module will create a section listing the exceptions thrown on the function/method page.

Syntax example:

```
(docblock)
 * @throws \My\Namespace\MyFooUndefinedException
 *   Optional description paragraph here, indented by 2 spaces as usual.
(rest of docblock)
```

Backdrop standards notes: The name of an exception class should appear on the same line as the `@throws` tag. Put explanations in starting on the next line, but only if they tell you something useful that is not conveyed by the name of the class. If you include an explanation, like other documentation use complete sentences that stand alone, without assuming that "Throws [exception name]" is part of the sentence automatically, since Throws will be a heading when it's displayed on API sites.

###@todo: To Do notes <a id="todo"></a>[#](#todo)

The `@todo` tag is used to place To Do notes in documentation. At this time, the API module does not do anything special with these notes, so they are simply displayed as paragraphs of text with the `@todo` tag left in.

Syntax example:

```
 * @todo Remove this in 2.0.
 * @todo Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam
 *   nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed
 *   diam voluptua.
```

###@var: Class property data types <a id="var"></a>[#](#var)

The `@var` tag is used to document the data type of a class property.

Syntax example:

```
class FooBar {

  /**
   * The database connection object for this statement's DatabaseConnection.
   *
   * @var \Backdrop\Core\Database\Connection
   */
  public $dbh;

}
```

Syntax notes: The `@var` tag is followed by a space and then a [data type specification](#types).

<small>Portions of this page based on [Drupal API documentation and comment standards](https://drupal.org/node/1354).</small>
