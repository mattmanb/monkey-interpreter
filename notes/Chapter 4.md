# Extending the Interpreter
## Data Types and Functions
So far, only integers and booleans are supported. A language should include addition variables.

Adding them will be must easier because we already created the structure of the interpreter, and Go will handle a lot of the logic between actual data structure functions, we just need to account for it in Monkey (i.e. string concatenation)

## Strings
Pretty much identical to integers, would be harder of Strings weren't inherently supported in Go

## Built in functions
This section essentially adds supports for inherent functions in the Monkey language like 'len' for a string
- Implementation defines the method and creates a new object basically identical to object.(function)
- The check for if the function is defined also looks for built in functions

## Arrays

