# JavaScript Coding Guidelines

This is a set of coding conventions and rules for use in JavaScript programming. It is inspired by the Sun document Code Conventions for the Java Programming Language. It is heavily modified of course because JavaScript is not Java.

The long-term value of software to an organization is in direct proportion to the quality of the codebase. Over its lifetime, a program will be handled by many pairs of hands and eyes. If a program is able to clearly communicate its structure and characteristics, it is less likely that it will break when modified in the never-too-distant future.

Code conventions can help in reducing the brittleness of programs.

All of our JavaScript code is sent directly to the public. It should always be of publication quality.

Neatness counts.

## JavaScript Files

JavaScript programs should be stored in and delivered as .js files.

JavaScript code should not be embedded in HTML files unless the code is specific to a single session. Code in HTML adds significantly to pageweight with no opportunity for mitigation by caching and compression.

`<script src=filename.js>` tags should be placed as late in the body as possible. This reduces the effects of delays imposed by script loading on other page components. There is no need to use the language or type attributes. It is the server, not the script tag, that determines the MIME type.

## Indentation

The unit of indentation is four spaces. Use of tabs should be avoided because (as of this writing in the 21st Century) there still is not a standard for the placement of tabstops. The use of spaces can produce a larger filesize, but the size is not significant over local networks, and the difference is eliminated by minification.

## Nesting

Avoid deep nesting as much as possible, best practice is no more than three deep. If you find yourself nesting too deeply, try separate it into multiple functions.

``` js
// Pyramid of Doom!
if ... {
    for ... {
        while ... {
            if ... {
            }
        }
    }
}
```

## Line Length

Avoid lines longer than 80 characters. When a statement will not fit on a single line, it may be necessary to break it. Place the break after an operator, ideally after a comma. A break after an operator decreases the likelihood that a copy-paste error will be masked by semicolon insertion. The next line should be indented 8 spaces.

## Comments

It is useful to leave comments that will be read at a later time by people (possibly yourself) who will need to understand what you have done. The comments should be well-written and clear, just like the code they are annotating. An occasional nugget of humor might be appreciated. Frustrations and resentments will not.

It is important that comments be kept up-to-date. Erroneous comments can make programs even harder to read and understand.

Make comments meaningful. Focus on what is not immediately visible. Don't waste the reader's time with stuff like

``` js
i = 0; // Set i to zero.
```

Generally use line comments. Save block comments for formal documentation and for commenting out.

``` js
  /**
   * Reduces a sequence of names to initials.
   * @param  {String} name  Space Delimited sequence of names.
   * @param  {String} sep   A period separating the initials.
   * @param  {String} trail A period ending the initials.
   * @param  {String} hyph  A hypen separating double names.
   * @return {String}       Properly formatted initials.
   */
  function makeInits(name, sep, trail, hyph) {
    function splitBySpace(nm) {
      return nm.trim().split(/\s+/).map(function(x) {return x[0]}).join(sep).toUpperCase();
    }
    return name.split(hyph).map(splitBySpace).join(hyph) + trail;
  }
  /**
   * Reduces a sequence of names to initials.
   * @param  {String} name Space delimited sequence of names.
   * @return {String}      Properly formatted initials.
   */
  function makeInitials(name) {
    return makeInits(name, '.', '.', '-');
  }
```

## Variable Declarations

All variables should be declared before used. JavaScript does not require this, but doing so makes the program easier to read and makes it easier to detect undeclared variables that may become implied globals. Implied global variables should never be used.

The var statements should be the first statements in the function body.

It is preferred that each variable be given its own line. They should be listed in alphabetical order.


``` js
var currentEntry, // currently selected table entry
    level = 0,    // indentation level
    size;         // size of table
```

JavaScript does not have block scope, so defining variables in blocks can confuse programmers who are experienced with other C family languages. Define all variables at the top of the function.

Use of global variables should be minimized. Implied global variables should never be used.

## Function Declarations

All functions should be declared before they are used. Inner functions should follow the var statement. This helps make it clear what variables are included in its scope.

There should be no space between the name of a function and the ( (left parenthesis) of its parameter list. There should be one space between the ) (right parenthesis) and the { (left curly brace) that begins the statement body. The body itself is indented four spaces. The } (right curly brace) is aligned with the line containing the beginning of the declaration of the function.

Naming conventions should follow camelCase, with the first letter being lowercase. Use descriptive enough method names that reading documentation is as optional as it needs to be. Go over the line limit if necessary

``` js
function outer(c, d) {
    var e = c * d;

    function inner(a, b) {
        return (e * a) + b;
    }

    return inner(0, 1);
}
```

This convention works well with JavaScript because in JavaScript, functions and object literals can be placed anywhere that an expression is allowed. It provides the best readability with inline functions and complex structures.

``` js
function getElementsByClassName(className) {
    var results = [];
    walkTheDOM(document.body, function (node) {
        var a;                  // array of class names
        var c = node.className; // the node's classname
        var i;                  // loop counter

       // If the node has a class name, then split it into a list of simple names.
       // If any of them match the requested name, then append the node to the set of results.
        if (c) {
            a = c.split(' ');
            for (i = 0; i < a.length; i += 1) {
                if (a[i] === className) {
                    results.push(node);
                    break;
                }
            }
        }
    });
    return results;
}
```

If a function literal is anonymous, there should be one space between the word function and the ( (left parenthesis). If the space is omited, then it can appear that the function's name is function, which is an incorrect reading.

``` js
div.onclick = function (e) {
    return false;
};

that = {
    method: function () {
        return this.datum;
    },
    datum: 0
};
```

Use of global functions should be minimized.

When a function is to be invoked immediately, the entire invocation expression should be wrapped in parents so that it is clear that the value being produced is the result of the function and not the function itself.

``` js
var collection = (function () {
    var keys = [], values = [];

    return {
        get: function (key) {
            var at = keys.indexOf(key);
            if (at >= 0) {
                return values[at];
            }
        },
        set: function (key, value) {
            var at = keys.indexOf(key);
            if (at < 0) {
                at = keys.length;
            }
            keys[at] = key;
            values[at] = value;
        },
        remove: function (key) {
            var at = keys.indexOf(key);
            if (at >= 0) {
                keys.splice(at, 1);
                values.splice(at, 1);
            }
        }
    };
}());
```

Curly braces in-line

## Names

Names should be formed from the 26 upper and lower case letters (A .. Z, a .. z), the 10 digits (0 .. 9), and _ (underbar). Avoid use of international characters because they may not read well or be understood everywhere. Do not use $ (dollar sign) or \ (backslash) in names.

Do not use _ (underbar) as the first character of a name. It is sometimes used to indicate privacy, but it does not actually provide privacy. If privacy is important, use the forms that provide private members. Avoid conventions that demonstrate a lack of competence.

Naming conventions should follow camelCase, with the first letter being lowercase in most cases. Use descriptive enough variable names that comments are not necessary. Go over the line limit if necessary

Constructor functions which must be used with the new prefix should start with a capital letter. JavaScript issues neither a compile-time warning nor a run-time warning if a required new is omitted. Bad things can happen if new is not used, so the capitalization convention is the only defense we have.

Global variables should be in all caps. (JavaScript does not have macros or constants, so there isn't much point in using all caps to signify features that JavaScript doesn't have.)

## Statements

### Simple Statements

Each line should contain at most one statement. Put a ; (semicolon) at the end of every simple statement. Note that an assignment statement which is assigning a function literal or object literal is still an assignment statement and must end with a semicolon.

JavaScript allows any expression to be used as a statement. This can mask some errors, particularly in the presence of semicolon insertion. The only expressions that should be used as statements are assignments and invocations.

### Compound Statements

Compound statements are statements that contain lists of statements enclosed in { } (curly braces).

The enclosed statements should be indented four more spaces.
The { (left curly brace) should be at the end of the line that begins the compound statement.
The } (right curly brace) should begin a line and be indented to align with the beginning of the line containing the matching { (left curly brace).
Braces should be used around all statements, even single statements, when they are part of a control structure, such as an if or for statement. This makes it easier to add statements without accidentally introducing bugs.
Labels

Statement labels are optional. Only these statements should be labeled: while, do, for, switch.

### return Statement

A return statement with a value should not use ( ) (parentheses) around the value. The return value expression must start on the same line as the return keyword in order to avoid semicolon insertion.

### if Statement

The if class of statements should have the following form:

``` js
if (condition) {
    statements
}

if (condition) {
    statements
} else {
    statements
}

if (condition) {
    statements
} else if (condition) {
    statements
} else {
    statements
}
```

### for Statement

A for class of statements should have the following form:

``` js
for (initialization; condition; update) {
    statements
}

for (variable in object) {
    if (filter) {
        statements
    } 
}
```

The first form should be used with arrays and with loops of a predeterminable number of iterations.

The second form should be used with objects. Be aware that members that are added to the prototype of the object will be included in the enumeration. It is wise to program defensively by using the hasOwnProperty method to distinguish the true members of the object:

``` js
for (variable in object) {
    if (object.hasOwnProperty(variable)) {
        statements
    } 
}
```

### while Statement

A while statement should have the following form:

``` js
while (condition) {
    statements
}
```

### do Statement

A do statement should have the following form:

``` js
do {
    statements
} while (condition);
```

Unlike the other compound statements, the do statement always ends with a ; (semicolon).

### switch Statement

A switch statement should have the following form:

``` js
switch (expression) {
case expression:
    statements
default:
    statements
}
```

Each case is aligned with the switch. This avoids over-indentation.

Each group of statements (except the default) should end with break, return, or throw. Do not fall through.

### try Statement

The try class of statements should have the following form:

``` js
try {
    statements
} catch (variable) {
    statements
}

try {
    statements
} catch (variable) {
    statements
} finally {
    statements
}
```

### continue Statement

Avoid use of the continue statement. It tends to obscure the control flow of the function.

### with Statement

The with statement should not be used.

## Whitespace

Blank lines improve readability by setting off sections of code that are logically related.

Blank spaces should be used in the following circumstances:

A keyword followed by ( (left parenthesis) should be separated by a space.

Never mix spaces and tabs.

``` js
while (true) {
```

A blank space should not be used between a function value and its ( (left parenthesis). This helps to distinguish between keywords and function invocations.
All binary operators except . (period) and ( (left parenthesis) and [ (left bracket) should be separated from their operands by a space.
No space should separate a unary operator and its operand except when the operator is a word such as typeof.
Each ; (semicolon) in the control part of a for statement should be followed with a space.
Whitespace should follow every , (comma).
Bonus Suggestions

## {} and []

Use {} instead of new Object(). Use [] instead of new Array().

Use arrays when the member names would be sequential integers. Use objects when the member names are arbitrary strings or names.

## , (comma) Operator

Avoid the use of the comma operator except for very disciplined use in the control part of for statements. (This does not apply to the comma separator, which is used in object literals, array literals, var statements, and parameter lists.)

End each element in array with a comma especially if each element is on its own line

## Block Scope

In JavaScript blocks do not have scope. Only functions have scope. Do not use blocks except as required by the compound statements.

## Assignment Expressions

Avoid doing assignments in the condition part of if and while statements.

Is

``` js
if (a = b) {
```

a correct statement? Or was

``` js
if (a == b) {
```

intended? Avoid constructs that cannot easily be determined to be correct.

## === and !== Operators.

It is almost always better to use the === and !== operators. The == and != operators do type coercion. In particular, do not use == to compare against falsy values.

## Confusing Pluses and Minuses

Be careful to not follow a + with + or ++. This pattern can be confusing. Insert parents between them to make your intention clear.

``` js
total = subtotal + +myInput.value;
```

is better written as

``` js
total = subtotal + (+myInput.value);
```

so that the + + is not misread as ++.

## eval is Evil

The eval function is the most misused feature of JavaScript. Avoid it.

eval has aliases. Do not use the Function constructor. Do not pass strings to setTimeout or setInterval.

## Spacing for operations

Spacing should be used between operands and operators. For example, for a line where simple arithmetic is used, instead of typing; "val x=a+b", rather type; "val x = a + b".

This improves the readability of the code and reduces ambiguity when attempting to understand what the code is meant to do.

## Parentheses for operations

Parentheses should be used wherever compound statements are being used. For example, in a compound arithmetic operation such as: "val hexArea = (3 * Math.sqrt(3) * s * s / 2);", this could lead to ambiguous understanding of what the written code does, and could potentially lead to issues or erroneous results in some cases.

The correct form of how the above code should be written is as follows: "val hexArea = ( (3 * Math.sqrt(3)) * (s * s) / 2);"

## {} and Scope

In the case of functions, loops and conditional statements, the "{" should be placed on the same line as the declaration of the statements required, this removes unnecessary lines and makes the code neater and more compact.

## Repetition

In the case of code that needs to be reused in multiple areas, rather create a function which performs the functionality of the code that needs to be reused. This makes it easily readable as readers will not wonder if the different code does the same thing or not as they both use the same function.

This can also, in some cases, make it easier for updating or revision of the code, as you will only need to edit the code in one place.

(forked from http://javascript.crockford.com/code.html)


## Code Structure

End code lines with semi-colon

Space between functions' blocks

## Visual Studio Code Extensions

Prettier as a common code formatter

## Modularize — one function per task
This is a general programming best practice — making sure that you create functions that fulfill one job at a time makes it easy for other developers to debug and change your code without having to scan through all the code to work out what code block performs what function.

This also applies to creating helper functions for common tasks. If you find yourself doing the same thing in several different functions then it is a good idea to create a more generic helper function instead, and reuse that functionality where it is needed.

