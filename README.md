# orelang

> 日本語のREADMEはこちらです: [README.ja.md](README.ja.md)

orelang is a simple programming language in JSON. (JSONで記述するシンプルなプログラミング言語、orelang)

## Features

-   **S-expression Syntax**: All operations are expressed in JSON arrays, like Lisp.
-   **Core Constructs**: Variables (`set`, `get`), sequential execution (`step`), and console output (`print`).
-   **Control Flow**: `if`, `while`, `until`, and `break`.
-   **Functions**: Define functions with `func`, call them with `call`, and use `return`. Modules can be loaded from URLs or local paths using `import`.
-   **Stack-based Arguments**: Functions receive arguments via a global stack using `push` and `pop`.
-   **Standard Library**: Includes basic operators, along with `String`, `Math`, and `Array` utilities.

## Usage

### Command Line (with Deno)

You can execute `.orelang` files directly from the command line using the `ore.js` helper script.

```sh
# Run a demo program that uses the geo3x3.orelang module
cd demo
deno run -A ../ore.js ./simple_geo3x3.orelang
```

### As a JavaScript Module

Import `orelang.js` to run programs within your Deno or browser-based JavaScript code.

```javascript
import { orelang } from "https://code4fukui.github.io/orelang/orelang.js";

// Calls a remote .orelang file and passes 10 and 20 as arguments
const result = await orelang.run(["call", "https://code4fukui.github.io/orelang/demo/sum.orelang", 10, 20]);

console.log(result);
```

## Language Reference

All operations are JSON arrays where the first element is the operator name.

### Core Syntax

-   `["step", expression1, expression2, ...]`
    Evaluates all expressions in order and returns the result of the last one.

-   `["set", "variableName", expression]`
    Assigns the result of `expression` to a variable.

-   `["get", "variableName"]`
    Retrieves the value of a variable.

-   `["print", expression1, expression2, ...]`
    Prints the evaluated expressions to the console.

### Operators

-   `["+", a, b, ...]`, `["-", a, b, ...]`, `["*", a, b, ...]`, `["/", a, b]`, `["%", a, b]`
-   `["=", a, b]`, `[">", a, b]`, `["<", a, b]`, `[">=", a, b]`, `["<=", a, b]`
-   `["&&", a, b, ...]`, `["||", a, b, ...]`
-   `["!", a]`

### Control Flow

-   `["if", condition, then_expression, else_expression]`
    If `condition` is true, evaluates `then_expression`, otherwise evaluates `else_expression`.

-   `["while", condition, expression]`
    Evaluates `expression` repeatedly as long as `condition` is true.

-   `["until", condition, expression]`
    Evaluates `expression` repeatedly until `condition` is true.

-   `["break"]` or `["break", value]`
    Exits a `while` or `until` loop, optionally returning a value.

### Functions & Modules

Function arguments are not passed directly; they are pushed onto a global stack and retrieved within the function using `pop`.

-   `["func", "functionName", expression]`
    Defines a function.

-   `["call", "functionName", arg1, arg2, ...]`
    Pushes `arg1`, `arg2`, etc., to the stack and then executes the function. Can also call a URL directly: `["call", "path/to/file.orelang", arg1, ...]`

-   `["return", expression]`
    Returns a value from a function.

-   `["import", "functionName", "URL_or_path"]`
    Loads an external `.orelang` file and assigns it to `functionName`.

### Stack Operations

-   `["push", value1, value2, ...]`
    Pushes one or more values onto the global stack.

-   `["pop"]`
    Pops and returns the top value from the global stack.

### Standard Library

#### String
-   `["String.charAt", string, index]`
-   `["String.indexOf", string, substring]`
-   `["String.length", string]`
-   `["String.parseInt", string]`
-   `["String.parseFloat", string]`

#### Math
-   `["Math.floor", number]`

#### Array
-   `["Array", item1, item2, ...]`
-   `["Array.length", array]`
-   `["Array.getAt", array, index]`
-   `["Array.setAt", array, index, value]`

## reference

-   [orelang](https://qiita.com/shuetsu@github/items/ac21e597265d6bb906dc)