## 2.1
Parsers
- A parser is a part of the interpreter (or just a function of a language) that takes input, then formats the input into a data structure
- The most common data structure seen from parsers is a "Syntax Tree" or "Abstract Syntax Tree"
- A **Syntax Tree** contains each part of the input, whether it be a JSON file, or source code for our `monkey` programming language
- The **Abstract Syntax Tree** leaves parts of the input out. These parts if the source code is input would be semicolons, parentheses, etc. These parts of the input would be used to help format the **AST** instead of actually being included 
- A parser is also known as something that performs "Syntactic Analysis", which ensures that the correct syntax is used and the code is formatted in an appropriate way (no infinite loops, else statements without if statements, stuff like that as far as I can tell)
## 2.2
Parser generators
- Parser Generators use something called *CFGs* as input - Context-free Grammar
- This grammar is what the parser generator uses to create code which can be compiled and run to interpret the code defined by the CFG
- Its hard to understand how these parser generators work without having made a parser yourself, so that's what I'll be doing!
## 2.3
Parser for the Monkey Language
- Its important to understand that there are different types of parsers. Most generally, there are two types:
	1. Top-down Parsing
	2. Bottom-up Parsing
- Broadly speaking, top-down parsing starts with a root node which the rest of the input is built off of. Bottom-up parsing starts with the lowest level of the input and build up from that
- Both methods have different approaches and variations
- For our approach, we're using a **top-down parsing** method known as **Recursive Descent Parsing**
- This applies the top-down approach and recursion to iterate through the input of the parser to make sense of it
- Next section starts by learning more of the **Pratt Parser** (other name for Recursive Descent) and applying some of the concepts into parsing `let` and `return` statements
## 2.4
Starting the parser
- Started this section by implementing the structure of our `AST` - this includes the root node, and creates structures for other nodes that will be added like Let statement 
- The let statement is the main focus of this section, as that is mainly the thing we implemented
- The parser includes a `ParseStatement` function which switches through the different possible types of nodes/statements we can have
- Started by implementing the `ParseLetStatement` function which ensures that the statement starts with 'let' followed by an identifier, then an equal sign, then some kind of expression (we don't read in the expression right now) until a semicolon appears.
- Also added a simple test of 3 `let` statements
- Error catching is also added whenever `expectToken` is called. If the expected token is not the same as the actual next token, we call `peekError` and add a string error to a list of strings in the parser. This way all the errors are found in one run instead of exiting as soon as a single error happens.

## 2.5
Adding `return` statements to the parser
- return statements are similar to let statements, but even simpler. 
- only a 'return' token followed by an expression with a semicolon at the end
- added the ReturnStatement struct to our AST
- Created a test return statements function in `parser_test.go` - looks pretty much identical to return let statements except an addition `testReturnStatement` wasn't needed (because return statements are much simpler than let statements I think)
- Then finally added a parser case to the switch statement in `parseStatement` function in `parser.go`

## 2.6
Parsing Expressions
#### Expressions in Monkey
- Prefix operators:
	- -5
	- !true
	- !false
- Postfix operator:
	- foobar++
	- **NOT IN MONKEY!**
- Binary operators (aka **infix operators**)
	- 5 + 5
	- 5 - 5
	- 5 \* 5
	- 5 / 5
- Comparison operators
	- foo == bar
	- foo != bar
	- foo < bar
	- foo > bar
- Group expression operators
	- 5 \* (5 + 5)
	- ((5+5 ) \* 5) \* 5
- Call expressions
	- add(2,3)
	- add(add(2,3), add(5, 10))
	- max(5, add(5, (5 \* 5)))
- Identifiers are expressions
	- food \* bar / foobar
	- add(foo, bar)
- Function literal in place of an identifier (and `if-else` statement)
```monkey
fn(x, y) { return x + y }(5, 5);
(fn(x) {return x}(5) + 10) * 10;

// if/else
let result = if (10 > 5) { true } else { false };
result // => true
```
#### Top Down Operator Precedence (Pratt Parsing)
**Really long chapter**
- Added a string function to the interface in our AST, which lets the nodes get printed (usually just the literal)
- Defined integer literals and identifiers in the AST
- Implemented parse functions for integer literals and identifiers
- The parser contains maps for prefix and infix parser functions
- The map uses a specific function based on the tokenType (defined in the maps `prefixParseFns` and `infixParseFns`)
-  Parsing of prefix operations
	- Lots of recursion, which does make the code a lot more condensed
- The only prefix operators in Monkey are `!` and `-` 
- **By the way, testing functions are all implemented as well**
	- `ast_test.go`
	- `parser_test.go`
- Starting **infix operators**
- `parseExpression` adds a for loops that implements infix parsing, but *I am still confused how Pratt parsing actually works*
- 
![[Screenshot 2024-06-08 at 1.10.59 PM.png | 400]]
This is the structure of an example AST that sets and identifier to a value.

## 2.7
**How Pratt Parsing works**

- In Pratt's original paper, some of the vocab was different
	- prefixParseFns are 'nuds' (null denotation)
	- infixParseFns are 'leds' (left denotation)


![[Screenshot 2024-06-14 at 5.35.10 PM.png | 400]]
This is the structure of the example AST of the expression 
`1 + 2 + 3;`

The Pratt parsing part of the parser is the real meat of the sandwich for the parser. It allows us to parse through expression and turn them into an **Abstract Syntax Tree (AST)**, which accounts for operator precedence. We use the variable `precedence` when determining the AST, but in theory the term "**right-binding power**." The right-binding power determines how many operators to the right of the current operator will be "sucked in" to this expression. The "**left-binding power**" is determined by `peekPrecedence()` and determines if this expression will be pulled into the *next* one. 

The big determination is if the right-binding power or left-binding power is greater. If the left binding power of the next operand is greater than the current right-binding power, then everything parsed so far becomes the left part of another expression. 

Another example is the expression `!1 + 2`. Obviously we want the string to look something like (-1) + 2. In our Pratt parsing implementation, precedence is the precedence of the PREFIX operator. Since PREFIX is so high in the list, almost nothing can make the `1` of the expression turn into the 'left-arm' of another expression since the actual GO expression `precedence < p.peekPrecedence()` will never be true (unless `peakPrecedence()` is a function call)

## 2.8 Expanding the Parser
- Added a `testIdentifier` helper function, and added testing for boolean expressions.
- Grouped expressions (parenthesis-surrounded expressions) were easy to implement with the functions we already have
	- Added some tests for parenthesis functionality
	- added the prefix parse fn for a parenthesis (`parseGroupedExpression`)
	- implemented the `parseGroupedExpression` function, which basically parses until a right parenthesis is found
- if expressions
	- requires `blockStatement` type since an if expression can have several expressions
		- as can an else statement
	- `parseIfExpression` can optionally have an else expression as well. peek token is used a lot to confirm correct use of parenthesis and curly braces for conditions and block statements respectively
- function literals
	- added a prefix parse function for parsing function declarations
	- make sure there is a left parenthesis after the `fn` keyword, check for parameters, make sure there is a right parenthesis
	- then parse the block statement within the curly braces
	- added some tests for parsing a function and for parsing just the parameters
- function calling
	- we need to register left parenthesis as an infix operator so we can parse the arguments (or expressions) correctly
- LPAREN precedence (the 'P' in PEMDAS)
	- made it the highest precedence there is
	- only linked to parseCallExpression right now
- Wrapping up 'TODOS'
	- added parse expression to let statement and return statement parsing
	- added a monkey ascii art that gets printed whenever errors happen
