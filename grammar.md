Copyright (c) the Selfie Project authors. All rights reserved. Please see the AUTHORS file for details. Use of this source code is governed by a BSD license that can be found in the LICENSE file.

Selfie is a project of the Computational Systems Group at the Department of Computer Sciences of the University of Salzburg in Austria. For further information and code please refer to:

selfie.cs.uni-salzburg.at

This is the grammar of the C Star (C\*) programming language.

C\* is a tiny subset of the programming language C. C\* features global variable declarations with optional initialization as well as procedures with parameters and local variables. C\* has five statements (assignment, while loop, if-then-else, procedure call, and return) and standard arithmetic (`+`, `-`, `*`, `/`, `%`) and comparison (`==`, `!=`, `<`, `>`, `<=`, `>=`) operators over variables and procedure calls as well as integer, character, and string literals. C\* includes the unary `*` operator for dereferencing pointers hence the name but excludes data types other than `uint64_t` and `uint64_t*`, bitwise and Boolean operators, and many other features. The C\* grammar is LL(1) with 7 keywords and 22 symbols. Whitespace as well as single-line (`//`) and multi-line (`/*` to `*/`) comments are ignored.

C\* Keywords: `uint64_t`, `void`, `sizeof`, `if`, `else`, `while`, `return`

C\* Symbols: `integer`, `character`, `string`, `identifier`, `,`, `;`, `(`, `)`, `{`, `}`, `+`, `-`, `*`, `/`, `%`, `=`, `==`, `!=`, `<`, `>`, `<=`, `>=`, `...`, `<<`, `>>`, `&`, `|`, `~`, `&&`, `||`, `!`, `[`, `]`

with:

```
integer    = digit { digit } | hex_digit .

character  = "'" printable_character "'" .

string     = """ { printable_character } """ .

identifier = letter { letter | digit | "_" } .
```

and:

```
digit  = "0" | ... | "9" .

letter = "a" | ... | "z" | "A" | ... | "Z" .

hex_digit = digit | "a" | ... | "f" | "A" | ... | "F" .
```

C\* Grammar:

```
cstar      = { variable [ initialize ] | array | struct ";" | procedure } .

variable   = type identifier .

array      = variable { "[" integer "]" } . 

struct     = "struct" identifier "{" { variable ";" } "};" . 

type       = "uint64_t" [ "*" ] .

initialize = "=" [ cast ] [ "-" ] value .

cast       = "(" type ")" .

value      = integer | character .

statement  = assignment ";" | if | while | for | call ";" | return ";" .

assignment = ( [ "*" ] identifier | "*" "(" logical_or ")" ) "=" logical_or .

logical_or = logical_and [ "||" logical_and ] . 

logical_and  = expression [ "&&" expression ] . 

expression = shift [ ( "==" | "!=" | "<" | ">" | "<=" | ">=" ) shift ] .

shift      = arithmetic [ ("<<" | ">>") arithmetic] .

arithmetic = term { ( "+" | "-" ) term } .

term       = factor { ( "*" | "/" | "%" ) factor } .

factor     = [ cast ] [ "-" ] [ "*" ] ["!"]
             ( "sizeof" "(" type ")" | literal | identifier | call | "(" logical_or ")" ) .

literal    = value | string .

if         = "if" "(" logical_or ")"
               ( statement | "{" { statement } "}" )
             [ "else"
               ( statement | "{" { statement } "}" ) ] .

while      = "while" "(" logical_or ")"
               ( statement | "{" { statement } "}" ) .

for        = "for" "(" assignment ";" logical_or ";" assignment ")"
               ( statement | "{" { statement } "}" ) .

procedure  = ( type | "void" ) identifier "(" [ variable | array { "," variable } [ "," "..." ] ] ")"
             ( ";" | "{" { variable | array ";" } { statement } "}" ) .

call       = identifier "(" [ logical_or { "," logical_or } ] ")" .

return     = "return" [ logical_or ] .
```