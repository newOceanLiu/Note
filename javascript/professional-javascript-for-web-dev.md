# Char 1

* Javascript consist of ECMAScript, DOM and BOM (browser object model)

### ECMAScript

* provide the core functionality
* browser is only a hosting environment for ECMAScript, node.js being the server hosting environment

### DOM

* application programming interface for HTML
* maps out an entire page as a hierarchy of nodes, each part of an HTML is a type of node containing different kinds of data
* give developer control over its content and structure
* DOM level 1 was to map the structure of a doc
* DOM Level 2 extend DOM level 1 and support:
  * DOM Views
  * DOM events
  * DOM style
  * DOM traversal and range
* DOM level 3 further extend DOM with methods to load and save doc in uniform way, method to validate a doc, and export all XML 1.0

# Char 2

### \<SCRIPT\> element 

* attributes:
  * `async`: download but not prevent
  * `defer`: defer the script execution, also signal that script won't change the changing the structure of the page 
  * `src`: indicate external file contains the code to be executed
  * `type`: 'application/javascript'
* two way to interpret it 
  * inline: block the rest of the load
  * external: require `src` attribute, generally considered best practice
    * maintainablility
    * caching: to avoid download the same file twice
* tag placement
  * traditionally it is included in the header
  * but for large js files, puting it in header means it has to be downloaded/parsed/interpreted before rendering page, which can cause delay
  * modern web app put all js reference in \<body\> element, after the page content
* defered scripts
  * even with `defer` it is still best to put the script at the bottom of the page

# Char 3

* can `"use strict"`(anywhere) to avoid error-prone syntax

### Variables

* loosely typed, var can hold any type of data

* `var message = "hi"` doesn't mark the variable as being string type, you can change the value and type of the value at anytime (not recommended)

  ```javascript
  var message = "hi";
  message = 100
  ```

* using `var` operator makes the variable local to the scope in which it is defined

* possible to define a variable globally by omitting the `var` operator (not recommended)

  ```javascript
  message = "hi"
  ```

### Data types

* five simple data types: `Undefined, Null, Boolean, Number, String`
* one complex data type: `Object` which is an unordered list of name-variable pairs
* `typeof` operator:
  * return datatype or function
  * `typeof null` would return `Object`
  * is a operator, not function, so no parenthese `typeof message`
* `Undefined` type
  * var declared without initialization, it is assigned the value of `undefined`
  * `typeof` would return `undefined` value for both declared but uninitialized value as well as undeclared value
* `Null` type
  * when defining a variable that is meant to later hold a object, it is recommended to init the value with `null`
  * empty object pointer
* `Boolean` type
  * `if (Object)` will automatically perform `Boolean()` function on the object, and will return different value based on the type
* `Number` type
  * default int
  * use `0x[something]` as hex
  * js will always try to convert a float to int if it finds it is a whole number
  * float is less accurate than int and check equation may fail
  * `isNaN("10") == false`
  * number in string format can convert to number with `Number()`
* `String` type
  * '' and "" is the same in js
  * once created, the value of string cannot be changed

### Operators

* `add` : if either operand is an object, number or boolean, the `toString()` method is called: `5 + "5" = "55"`
* relational operators:
  * if compare string, compare each char code: `"23" < "3" == true`  
* equality
  * normall equal `==` , will call `valueOf` on object if one operant is an object and the other not
  * Identically equal: do not covert operant before testing for equality `===`, which check on data type equality first (recommended)

### Statements

* `label`: can be used with nested loops, label a for loop, can be used with break/continue to control outer/inner loop to act on 
* `with`: set the scope of the code within a particular object, convenience for times when a single object was being coded over and over again (not allowed in strict mode)

### Functions

* no need to specify return type, function can return any value at any time
* argument: it is represented as an array, so can be accessed by either argument name or `arguments[index]`
* **No Overloading** : since arguments does not check on length or type of the argument list

# Char 4

### Variable

* primitive value: atomic pieces of data
* reference value: object are accessed by reference
  * can add, change, delete properties and method at any time
  * copy a object is actually copy the pointer to the same object
* all function arguments are passed by **value**
* use `typeof` on primitive type, use `instanceof` on reference type

### Execution context and scope

* scope chain: provide ordered access to all variable/functions that an execution context has access to 
* the front is always the variable object
* next is the containing context
* the last is always the global context
* always search from the begining of the scope chain to the last for the identifier
* object may **not** be destroyed after `if/for` block, causing confusion
* if an object is initialized without first being declared, it gets added to global context **automatically**


### Garbage collection:

* Managing memory: 
  * keep only the data that is necessary
  * when data is no longer necessary, set the value to null, "dereference"

# Char 5



