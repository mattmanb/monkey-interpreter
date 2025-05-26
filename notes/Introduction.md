 Many different types of interpreters
	- Brainfuck interpreters are very simple
	- Some may compile the code into bytecode and evaluate that
	- JIT interpreters compile the input "just-in-time" into the machine code which gets executed.
- Tree-walking interpreter
	- interpreters which parse the source code
	- build an abstract syntax tree (AST)
	- evaluate the tree
- Monkey programming language
	- Language made for the sole purpose of building an interpreter for it
	- i.e.
	let age = 1;
	let name = "Monkey";
	let result = 10 * (20 / 2);
	- It has arrays and hashes
	let myArray = [1, 2, 3, 4, 5];
	let thorsten = {"name": "Thorsten", "age": 28};
	- Accessing the elements of arrays and hashes is similar to python
	myArray[0] // => 1
	thorsten["name"] // => "Thorsten"
	- let statements can bind functions to names
	let add = fn(a, b) { return a + b; };
	- Implicit return value 
	et add = fn(a, b) { a + b; };
	- Higher order functions supported (passing in a function as a parameter)
	- "First class functions" - functions are values like ints or strings
	- There is more, just look at the book for the syntax
- Go
	- C-like language
	- Easy to understand
	- No complex OOP things