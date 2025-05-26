## 1.1
- Changing source code twice: Source Code -> Tokens -> Abstract Tree Syntax
- Source Code to Tokens is called "lexing" or "lexical analysis"
- Example IO for a lexer:
	Source Code: `let x = 5 + 5;`
	Lexer:
	 ```
	[
	 LET,
	 IDENTIFIER("x"),
	 EQUAL_SIGN,
	 INTEGER(5),
	 PLUS_SIGN,
	 INTEGER(5),
	 SEMICOLON
	]
	```

## 1.2
Defining output tokens
- Sample code example we're starting with:
```monkey
let five = 5;
let ten = 10;

let add = fn(x, y) {
	return x + y;
};

let result = add(five, ten);
```
- there is a big difference between `5` and `"5"`
- Variables are called `identifiers` in token terms
- keywords are special words that aren't identifiers
	- `let` and `fn` for example
- every characters must become a token, including semicolon, left paren, right bracket, etc.
- we define the `TokenType` type and `Token` struct in token/token.go
- we also list the TokenType possibilities in a constant

## 1.3
Writing the lexer
- Start by making a `lexer` package
- import libs, then define input and the struct that will contain the source code
- the first test is a simple one: `l := "=+-(){},;"`
- we define each struct in the order to be expected
- to actually read through the input, `readChar`, `nextToken`, and `newToken` are implemented to do the following in order
	- iterate through the input
	- format the token and check if it is valid
	- create the actual token
- the token's structure is like this:
```go
type Token struct {
	Type    TokenType
	Literal string
}
```
- The `TokenType`s are defined as constants, and the literal is the actual character
**Looking back on this section...** 
- so far, only single characters are accounted for in the switch statement within the NextToken function
- to find full words or numbers, a default block is added at the end
- first we check if the next piece of input is a letter, if it is then we read until the input is not a letter any longer
	- Then we compare this string to a hash map of keywords; if the string isn't a keyword, then we say its an identifier
- after checking if its a letter, we check if its a number (reading in until the input is no longer looking at a number)
	- ==In our case, Monkey only supports INTS==
- If the character is not a letter or number, then we assume it is an unknown character and throw an error
- ALSO, we add a `skipwhitespace` function since *Monkey* doesn't account for whitespace

## 1.4
Extending the Lexer and Token class
- Adding a few new tokens:
	- '!'
	- '-'
	- '\*'
	- '\=\='
	- "!="
	- 'IF'
	- 'ELSE'
	- 'RETURN'
	- etc.
- Adding the new keywords and single character tokens was simple since we've done them already
- The two character tokens like `==` and `!=` needed something new
- `peekChar()` is almost identical to `readChar()` except the position doesn't get incremented. This way we can look at the next character in the source code without moving there
- `peekChar` allows us to check for `==` or `!=` in the `=` and `!` cases in our lexer, respectfully
- New test that can be tokenized:
```monkey
let five = 5;
let ten = 10;

let add = fn(x, y) {
	x + y;
};

let result = add(five, ten);

!-/*5;
5 < 10 > 5;

if (5 < 10) {
	return true;
} else {
	return false;
}

10 == 10;
10 != 9;
```

## 1.5 Start of a REPL
**REPL**: Read Eval Print Loop
- part of most interpreters
- reads in line by line of source code
```console
go run main.go
Hello mattbarrington! This is the Monkey programming language!
Feel free to type in commands
>>  let add = fn(x, y) { x + y; };
{Type:LET Literal:let}
{Type:IDENT Literal:add}
{Type:= Literal:=}
{Type:FUNCTION Literal:fn}
{Type:( Literal:(}
{Type:IDENT Literal:x}
{Type:, Literal:,}
{Type:IDENT Literal:y}
{Type:) Literal:)}
{Type:{ Literal:{}
{Type:IDENT Literal:x}
{Type:+ Literal:+}
{Type:IDENT Literal:y}
{Type:; Literal:;}
{Type:} Literal:}}
{Type:; Literal:;}
```
- the `main.go` file reads in the user through the os library, then calls repl.Start which starts reading in Monkey source code
- The lexer is officially complete! Now onto the **Parser**


