[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

# ACNS JavaScript Style Guide() {

*Good codestyle gives you superpower!*


## Table of Contents

  1. [Variables](#variables)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Properties](#properties)
  1. [Operators](#operators)
  1. [Blocks](#blocks)
  1. [Conditional Statements](#conditional-statements)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Constructors](#constructors)
  1. [Events](#events)
  1. [jQuery](#jquery)
  1. [NS specific](#specific)
      - [File Naming Conventions](#file-naming-conventions)
      - [Backbone Coding Standards](#backbone-coding-standards)
      - [NS Best Practices](#ns-best-practices)
  1. [License](#license)
  

## Variables

  - Always use `var` to declare variables. Not doing so will result in global variables. **Polluting the global namespace reduces your testosterone level.**

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - Use one `var` declaration per variable.
    It's easier to add new variable declarations this way, and you never have
    to worry about swapping out a `;` for a `,` or introducing punctuation-only
    diffs.

    ```javascript
    // bad
    var avengersList = getAvengers(),
        superhuman = true,
        name = 'Iron Man';

    // bad
    // (compare to above, and try to spot the mistake)
    var avengersList = getAvengers(),
        superhuman = true;
        name = 'Iron Man';

    // good
    var avengersList = getAvengers();
    var superhuman = true;
    var name = 'Iron Man';
    ```

  - Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    var i, len, avengerName,
        avengersList = getAvengers(),
        superhuman = true;

    // bad
    var i;
    var avengersList = getAvengers();
    var avengerName;
    var superhuman = true;
    var len;

    // good
    var avengersList = getAvengers();
    var superhuman = true;
    var avengerName;
    var length;
    var i;
    ```

  - Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

    ```javascript
    // bad
    function() {
      test();
      console.log('Age of Ultron');

      //..other stuff..

      var avengerName = getAvengerName();

      if (avengerName === 'Captain America') {
        return false;
      }

      return avengerName;
    }

    // good
    function() {
      var avengerName = getAvengerName();

      test();
      console.log('Age of Ultron');

      //..other stuff..

      if (avengerName === 'Captain America') {
        return false;
      }

      return name;
    }

    ```

**[⬆ back to top](#table-of-contents)**



## Objects

  - Use the literal syntax for object creation.

    ```javascript
    // bad
    var avenger = new Object();

    // good
    var avenger = {};
    ```

  - Don't use [reserved words](http://es5.github.io/#x7.6.1) as keys. It won't work in IE8. [More info](https://github.com/airbnb/javascript/issues/61).

    ```javascript
    // bad
    var hulk = {
      default: { name: 'Dr. Bruce Banner' },
      private: true
    };

    // good
    var hulk = {
      defaults: { name: 'Dr. Bruce Banner' },
      hidden: true
    };
    ```

  - Use readable synonyms in place of reserved words.

    ```javascript
    // bad
    var hulk = {
      class: 'creature'
    };

    // bad
    var hulk = {
      klass: 'creature'
    };

    // good
    var hulk = {
      type: 'creature'
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  - Use the literal syntax for array creation.

    ```javascript
    // bad
    var avengers = new Array();

    // good
    var avengers = [];
    ```

  - Use Array#push instead of direct assignment to add items to an array.

    ```javascript
    var avengersTeam = [];


    // bad
    avengersTeam[avengersTeam.length] = 'Thor';

    // good
    avengersTeam.push('Thor');
    ```

  - When you need to copy an array use Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = avengersTeam.length;
    var avengersTeamCopy = [];
    var i;

    // bad
    for (i = 0; i < len; i++) {
      avengersTeamCopy[i] = avengersTeam[i];
    }

    // good
    avengersTeamCopy = avengersTeam.slice();
    ```

  - To convert an array-like object to an array, use Array#slice.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Strings

  - Use single quotes `''` for strings.

    ```javascript
    // bad
    var name = "Anthony Edward Stark";

    // good
    var name = 'Anthony Edward Stark';
    ```
    
**[⬆ back to top](#table-of-contents)**


## Functions

  - Function expressions:

    ```javascript
    // anonymous function expression
    var anonymous = function() {
      return true;
    };

    // named function expression
    var named = function named() {
      return true;
    };

    // immediately-invoked function expression (IIFE)
    (function() {
      console.log('Welcome to the age of Ultron!');
    })();
    ```

  - Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.

**[⬆ back to top](#table-of-contents)**



## Properties

  - Use dot notation when accessing properties.

    ```javascript
    var thor = {
      useHammer: true,
      realName: 'Thor Odinson'
    };

    // bad
    var useHammer = thor['useHammer'];

    // good
    var useHammer = thor.useHammer;
    ```

  - Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    var thor = {
      useHammer: true,
      realName: 'Thor Odinson'
    };
    
    var prop = 'useHammer'
    var useHammer = thor[prop];
    ```

**[⬆ back to top](#table-of-contents)**

## Operators

  - Use `===` and `!==` over `==` and `!=`.
  - Comparison operators are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - Use shortcuts.

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (marvelCollection.length > 0) {
      // ...stuff...
    }

    // good
    if (marvelCollection.length) {
      // ...stuff...
    }
    ```
  - The `with` operator should not be used.
  
  - The ternary operator should be written as in the examples:
  
  ```javascript
  var x = a ? b : c;
  
  var y = a ?
      longButSimpleOperandB : longButSimpleOperandC;
  
  var z = a ?
      moreComplicatedB :
      moreComplicatedC;
  ```

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ back to top](#table-of-contents)**


## Blocks

  - The opening curly brace should be on the same line and separated with one space character:

    ```javascript
    // bad
    if (test){
    // ...
    }
    
    // bad
    function foo() 
    {
        // ...
    }
    
    // good
    if (test) {
        // ...
    }
    
    // good
    function foo() {
        // ...
    }
    
    ```

  - Use braces with all multi-line blocks.

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function() { return false; }

    // good
    function() {
      return false;
    }
    ```

  - If you're using multi-line blocks with `if` and `else`, put `else` on the same line as your
    `if` block's closing brace.

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


**[⬆ back to top](#table-of-contents)**

## Conditional Statements

  - The `else` keyword should be on the same line as the closing brace of the if-part of the statement:

    ```javascript
        
    //bad
    if (test) {
    // ...
    } 
    else {
        // ...
    }
    
    //good
    if (test) {
        // ...
    } else {
        // ...
    }

    ```
  - Condition statements should not contain assignment operations:
    
    ```javascript
    //bad
    var foo;
    if ((foo = bar()) > 0) {
        // ...
    }
    
    //good
    var foo = bar();
    if (foo > 0) {
        // ...
    }

    ```
    
  - Conditions longer than the [maximum line length](#general) should be divided as in the example:
    
    ```javascript
    if (longCondition ||
        anotherLongCondition &&
        yetAnotherLongCondition
    ) {
        // ...
    }
    ```
    
  - [Yoda conditions](http://en.wikipedia.org/wiki/Yoda_conditions) should not be used:
    
    ```javascript
    //bad
    if ('driving' === getType()) {
    
    }
    
    //good
    if (getType() === 'driving') {
    
    }
    ```

  -  The switch statement should be written as in the example:
    
    ```javascript
    switch (value) {
        case 1:
            // ...
            break;
    
        case 2:
            // ...
            break;
    
        default:
            // ...
            // no break keyword on the last case
    }
    ```
    


**[⬆ back to top](#table-of-contents)**

## Comments

  - Use `/** ... */` for multi-line comments. Include a description, specify types and values for all parameters and return values.

    ```javascript
    // bad
    // newMarvelHero() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function newMarvelHero(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * newMarvelHero() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function newMarvelHero(tag) {

      // ...stuff...

      return element;
    }
    ```

  - Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment.

    ```javascript
    // bad
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME -- need to figure this out` or `TODO -- need to implement`.

  - Use `// FIXME:` to annotate problems.

    ```javascript
    function saveEarth() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - Use `// TODO:` to annotate solutions to problems.

    ```javascript
    function saveEarth() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
  ```

**[⬆ back to top](#table-of-contents)**


## Whitespace

  - Use soft tabs set to 4 spaces.

    ```javascript
    // bad
    function() {
    ∙∙var name;
    }

    // bad
    function() {
    ∙var name;
    }

    // good
    function() {
    ∙∙∙∙var name;
    }
    ```

  - Place 1 space before the leading brace.

    ```javascript
    // bad
    function test(){
      console.log('Marvel');
    }

    // good
    function test() {
      console.log('Marvel');
    }

    // bad
    avenger.set('attr',{
      age: '38',
      name: 'Robert Bruce Banner'
    });

    // good
    avenger.set('attr', {
      age: '38',
      name: 'Robert Bruce Banner'
    });
    ```

  - Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space before the argument list in function calls and declarations.

    ```javascript
    // bad
    if(isHulk) {
      fight ();
    }

    // good
    if (isHulk) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - Set off operators with spaces.

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - End files with a single newline character.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - Leave a blank line after blocks and before the next statement

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    var obj = {
      foo: function() {
      },
      bar: function() {
      }
    };
    return obj;

    // good
    var obj = {
      foo: function() {
      },

      bar: function() {
      }
    };

    return obj;
    ```


**[⬆ back to top](#table-of-contents)**

## Commas

  - Leading commas: **Nope.**

    ```javascript
    // bad
    var story = [
        once
      , upon
      , aTime
    ];

    // good
    var story = [
      once,
      upon,
      aTime
    ];

    // bad
    var avenger = {
        firstName: 'Clinton'
      , lastName: 'Barton'
      , heroName: 'Hawkeye'
      , superPower: 'strength'
    };

    // good
    var avenger = {
      firstName: 'Clinton',
      lastName: 'Barton',
      heroName: 'Hawkeye',
      superPower: 'strength'
    };
    ```

  - Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in ES5 ([source](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // bad
    var avenger = {
      firstName: 'Clinton',
      lastName: 'Barton',
    };

    var avengers = [
      'Ironman',
      'Thor',
    ];

    // good
    var avenger = {
      firstName: 'Clinton',
      lastName: 'Barton'
    };

    var heroes = [
      'Ironman',
      'Thor'
    ];
    ```

**[⬆ back to top](#table-of-contents)**


## Semicolons

  - **Statements should always end with a semicolon.**

    ```javascript
    // bad
    (function() {
      var name = 'Stark'
      return name
    })()

    // good
    (function() {
      var name = 'Stark';
      return name;
    })();

    // good (guards against the function becoming an argument when two files with IIFEs are concatenated)
    ;(function() {
      var name = 'Stark';
      return name;
    })();
    ```

    [Read more](http://stackoverflow.com/a/7365214/1712802).

**[⬆ back to top](#table-of-contents)**


## Type Casting & Coercion

  - Use `parseInt` for Numbers and always with a radix for type casting.

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = Number(inputValue);

    // good
    var val = parseInt(inputValue, 10);
    ```
    
  - Booleans:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**


## Naming Conventions

  - Avoid single letter names. Be descriptive with your naming.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - Use camelCase when naming objects, functions, and instances.

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var o = {};
    function c() {}

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - Use PascalCase when naming constructors or classes.

    ```javascript
    // bad
    function avenger(options) {
      this.name = options.name;
    }

    var bad = new avenger({
      name: 'nope'
    });

    // good
    function Avenger(options) {
      this.name = options.name;
    }

    var good = new Avenger({
      name: 'Tony Stark'
    });
    ```

  - Use a leading underscore `_` when naming private properties.

    ```javascript
    // bad
    this.__firstName__ = 'Tony';
    this.firstName_ = 'Tony';

    // good
    this._firstName = 'Tony';
    ```

  - When saving a reference to `this` use `self`.

    ```javascript
    // bad
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }

    // bad
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }
    ```

  - Name your functions. This is helpful for stack traces.

    ```javascript
    // bad
    var log = function(msg) {
      console.log(msg);
    };

    // good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

**[⬆ back to top](#table-of-contents)**


## Constructors

  - Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Hulk() {
      console.log('new hulk');
    }

    // bad
    Hulk.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // good
    Hulk.prototype.fight = function fight() {
      console.log('fighting');
    };

    Hulk.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Hulk.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Hulk.prototype.setHeight = function(height) {
      this.height = height;
    };

    var hulk = new Hulk();
    hulk.jump(); // => true
    hulk.setHeight(20); // => undefined

    // good
    Hulk.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Hulk.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var hulk = new Hulk();

    hulk.jump()
      .setHeight(20);
    ```


**[⬆ back to top](#table-of-contents)**


## Events

  - When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

    ```js
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefer:

    ```js
    // good
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ back to top](#table-of-contents)**

## jQuery

  - Use "jQuery" instead "$"
  ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = jQuery('.sidebar');
    ```

  - Prefix jQuery object variables with a `$`.

    ```javascript
    // bad
    var sidebar = jQuery('.sidebar');

    // good
    var $sidebar = jQuery('.sidebar');
    ```

  - Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
      jQuery('.sidebar').hide();

      // ...stuff...

     jQuery('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      var $sidebar = jQuery('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - For DOM queries use Cascading `jQuery('.sidebar ul')` or parent > child `jQuery('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Use `find` with scoped jQuery object queries.

    ```javascript
    // bad
    jQuery('ul', '.sidebar').hide();

    // bad
    jQuery('.sidebar').find('ul').hide();

    // good
    jQuery('.sidebar ul').hide();

    // good
    jQuery('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ back to top](#table-of-contents)**

##NS specific

###File Naming Conventions

   * Models: nameLikeThis.Model.js
   * Views: nameLikeThis.View.js
   * Collections: nameLikeThis.Collection.js
   * Routers: nameLikeThis.Router.js
   * Templates: name_like_this.txt
   * Services:  name-like-this.ss
   * Skins:  name-like-this.css
   * SSP_libraries: NameLikeThis.js
   * Suitelets:  moduleName_suitescripttype_suitescriptName.js
   * SSP files: name-like-this.ssp
   
**[⬆ back to top](#table-of-contents)**

###Backbone Coding Standards
  - Use less logic in the templates.
  - Minimize manual jQuery DOM manipulation, the DOM fragments should be rendered through the View objects.
  - All dependencies must go through RequireJS require() or define() functions. No global state.
  
**[⬆ back to top](#table-of-contents)**

###NS Best Practices
  - Use utf-8 for templates.
  
  - Require module exports
  ```
  //Bad
SC.Application('Shopping').on('beforeStart', function() {require(['Facets.Router', 'Facets.Views', 'Facets.Helper'], function(FacetsRouter, FacetsViews, FacetsHelper) {
    
  //Good
SC.Application('Shopping').on('beforeStart', function() {
    require(['Facets.Router', 'Facets.Views', 'Facets.Helper'], 
        function(FacetsRouter, FacetsViews, FacetsHelper) {
  ```
  
  - Require module exports naming
    - Function arguments for module value should have the same name (without dots)
    ```
  //Bad
SC.Application('Shopping').on('beforeStart', function() {
    require(['Facets.Router', 'Facets.Views', 'Facets.Helper'], 
        function(Router, View, Helper) {
    
  //Good
SC.Application('Shopping').on('beforeStart', function() {
    require(['Facets.Router', 'Facets.Views', 'Facets.Helper'], 
        function(FacetsRouter, FacetsViews, FacetsHelper) {
  ```
  
**[⬆ back to top](#table-of-contents)**

## License

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**

# };
