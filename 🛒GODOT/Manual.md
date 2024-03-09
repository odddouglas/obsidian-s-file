## Example of GDScript

Some people can learn better by taking a look at the syntax, so here's an example of how GDScript looks.

```python

# Everything after "#" is a comment.
# A file is a class!

# (optional) icon to show in the editor dialogs:
@icon("res://path/to/optional/icon.svg")

# (optional) class definition:
class_name MyClass

# Inheritance:
extends BaseClass


# Member variables.
var a = 5
var s = "Hello"
var arr = [1, 2, 3]
var dict = {"key": "value", 2: 3}
var other_dict = {key = "value", other_key = 2}
var typed_var: int
var inferred_type := "String"

# Constants.
const ANSWER = 42
const THE_NAME = "Charly"

# Enums.
enum {UNIT_NEUTRAL, UNIT_ENEMY, UNIT_ALLY}
enum Named {THING_1, THING_2, ANOTHER_THING = -1}

# Built-in vector types.
var v2 = Vector2(1, 2)
var v3 = Vector3(1, 2, 3)


# Functions.
func some_function(param1, param2, param3):
    const local_const = 5

    if param1 < local_const:
        print(param1)
    elif param2 > 5:
        print(param2)
    else:
        print("Fail!")

    for i in range(20):
        print(i)

    while param2 != 0:
        param2 -= 1

    match param3:
        3:
            print("param3 is 3!")
        _:
            print("param3 is not 3!")

    var local_var = param1 + 3
    return local_var


# Functions override functions with the same name on the base/super class.
# If you still want to call them, use "super":
func something(p1, p2):
    super(p1, p2)


# It's also possible to call another function in the super class:
func other_something(p1, p2):
    super.something(p1, p2)


# Inner class
class Something:
    var a = 10


# Constructor
func _init():
    print("Constructed!")
    var lv = Something.new()
    print(lv.a)
```

If you have previous experience with statically typed languages such as C, C++, or C# but never used a dynamically typed one before, it is advised you read this tutorial: [:ref:`doc_gdscript_more_efficiently`](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#id5).

## Language

In the following, an overview is given to GDScript. Details, such as which methods are available to arrays or other objects, should be looked up in the linked class descriptions.

### [](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#identifiers)Identifiers

Any string that restricts itself to alphabetic characters (`a` to `z` and `A` to `Z`), digits (`0` to `9`) and `_` qualifies as an identifier. Additionally, identifiers must not begin with a digit. Identifiers are case-sensitive (`foo` is different from `FOO`).

Identifiers may also contain most Unicode characters part of [UAX#31](https://www.unicode.org/reports/tr31/). This allows you to use identifier names written in languages other than English. Unicode characters that are considered "confusable" for ASCII characters and emoji are not allowed in identifiers.

### [](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#keywords)Keywords

The following is the list of keywords supported by the language. Since keywords are reserved words (tokens), they can't be used as identifiers. Operators (like `in`, `not`, `and` or `or`) and names of built-in types as listed in the following sections are also reserved.

Keywords are defined in the [GDScript tokenizer](https://github.com/godotengine/godot/blob/master/modules/gdscript/gdscript_tokenizer.cpp) in case you want to take a look under the hood.

|Keyword|Description|
|---|---|
|if|See [if/else/elif](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#if-else-elif).|
|elif|See [if/else/elif](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#if-else-elif).|
|else|See [if/else/elif](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#if-else-elif).|
|for|See [for](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#for).|
|while|See [while](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#while).|
|match|See [match](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#match).|
|break|Exits the execution of the current `for` or `while` loop.|
|continue|Immediately skips to the next iteration of the `for` or `while` loop.|
|pass|Used where a statement is required syntactically but execution of code is undesired, e.g. in empty functions.|
|return|Returns a value from a function.|
|class|Defines a class.|
|class_name|Defines the script as a globally accessible class with the specified name.|
|extends|Defines what class to extend with the current class.|
|is|Tests whether a variable extends a given class, or is of a given built-in type.|
|in|Tests whether a value is within a string, list, range, dictionary, or node. When used with `for`, it iterates through them instead of testing.|
|as|Cast the value to a given type if possible.|
|self|Refers to current class instance.|
|signal|Defines a signal.|
|func|Defines a function.|
|static|Defines a static function or a static member variable.|
|const|Defines a constant.|
|enum|Defines an enum.|
|var|Defines a variable.|
|breakpoint|Editor helper for debugger breakpoints. Unlike breakpoints created by clicking in the gutter, `breakpoint` is stored in the script itself. This makes it persistent across different machines when using version control.|
|preload|Preloads a class or variable. See [Classes as resources](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#classes-as-resources).|
|await|Waits for a signal or a coroutine to finish. See [Awaiting for signals or coroutines](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#awaiting-for-signals-or-coroutines).|
|yield|Previously used for coroutines. Kept as keyword for transition.|
|assert|Asserts a condition, logs error on failure. Ignored in non-debug builds. See [Assert keyword](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#assert-keyword).|
|void|Used to represent that a function does not return any value.|
|PI|PI constant.|
|TAU|TAU constant.|
|INF|Infinity constant. Used for comparisons and as result of calculations.|
|NAN|NAN (not a number) constant. Used as impossible result from calculations.|

### [](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#operators)Operators

The following is the list of supported operators and their precedence.

|**Operator**|**Description**|
|---|---|
|`(` `)`|Grouping (highest priority)<br><br>Parentheses are not really an operator, but allow you to explicitly specify the precedence of an operation.|
|`x[index]`|Subscription|
|`x.attribute`|Attribute reference|
|`foo()`|Function call|
|`await x`|[Awaiting for signals or coroutines](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#awaiting-for-signals-or-coroutines)|
|`x is Node`|Type checking<br><br>See also [:ref:`is_instance_of() <class_@GDScript_method_is_instance_of>`](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#id7) function.|
|`x ** y`|Power<br><br>Multiplies `x` by itself `y` times, similar to calling [:ref:`pow() <class_@GlobalScope_method_pow>`](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#id9) function.<br><br>**Note:** In GDScript, the `**` operator is [left-associative](https://en.wikipedia.org/wiki/Operator_associativity). See a detailed note after the table.|
|`~x`|Bitwise NOT|
|`+x`<br><br>`-x`|Identity / Negation|
|`x * y`<br><br>`x / y`<br><br>`x % y`|Multiplication / Division / Remainder<br><br>The `%` operator is additionally used for [:ref:`format strings <doc_gdscript_printf>`](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#id11).<br><br>**Note:** These operators have the same behavior as C++, which may be unexpected for users coming from Python, JavaScript, etc. See a detailed note after the table.|
|`x + y`<br><br>`x - y`|Addition (or Concatenation) / Subtraction|
|`x << y`<br><br>`x >> y`|Bit shifting|
|`x & y`|Bitwise AND|
|`x ^ y`|Bitwise XOR|
|`x \| y`|Bitwise OR|
|`x == y`<br><br>`x != y`<br><br>`x < y`<br><br>`x > y`<br><br>`x <= y`<br><br>`x >= y`|Comparison<br><br>See a detailed note after the table.|
|`x in y`<br><br>`x not in y`|Inclusion checking<br><br>`in` is also used with the [for](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#for) keyword as part of the syntax.|
|`not x`<br><br>`!x`|Boolean NOT and its [:ref:`unrecommended <boolean_operators>`](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#id13) alias|
|`x and y`<br><br>`x && y`|Boolean AND and its [:ref:`unrecommended <boolean_operators>`](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#id15) alias|
|`x or y`<br><br>`x \| y`|Boolean OR and its [:ref:`unrecommended <boolean_operators>`](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#id17) alias|
|`true_expr if cond else false_expr`|Ternary if/else|
|`x as Node`|[Type casting](https://github.com/godotengine/godot-docs/blob/4.1/tutorials/scripting/gdscript/gdscript_basics.rst#casting)|
|`x = y`<br><br>`x += y`<br><br>`x -= y`<br><br>`x *= y`<br><br>`x /= y`<br><br>`x **= y`<br><br>`x %= y`<br><br>`x &= y`<br><br>`x \|= y`<br><br>`x ^= y`<br><br>`x <<= y`<br><br>`x >>= y`|Assignment (lowest priority)<br><br>You cannot use an assignment operator inside an expression.|

Note
