## 3.1
The 'E' in REPL stands for **Evaluation**. Instead of characters and tokens, the input code of the Monkey language will finally have meaning to it. Expression like 1 + 2 will evaluate to 3, 5 < 1 will be false, and much much more.
- Not everything is cut and dry
- There will be some choices that have to be made about the interpreter, such as the order of how parameter expressions get evaluated, and probably a lot more. 

## 3.2
Compilers and interpreters alike form an AST (the tree the expression is stored in) but the decision of what to do with that expression is up to the evaluator.
- A compiler creates an executable artifact based off the AST 
- An interpreter runs the code based on the AST, with no artifacts
- Java creates bytecode from an AST
Just in Time (JIT) interpreters/compilers might take the AST, compile it into bytecode, then using a VM the bytecode is compiled into machine code as it is needed during execution, hence the JIT aspect.

## 3.3 A Tree-Walking Interpreter
This implementation of a Monkey interpreter will be an AST "on-the-fly" tree-walking interpreter. There is a sacrifice in performance for this type, but its much more simple to implement.

Here is the pseudocode for the eval function:
```Go
function eval(astNode) {
	if (astNode is integerliteral) {
		return astNode.integerValue
	} else if (astNode is booleanLiteral) {
		return astNode.booleanValue
	} else if (astNode is infixExpression) {
		leftEvaluated = eval(astNode.Left)
		rightEvaluated = eval(astNode.Right)
		if astNode.Operator == "+" {
			return leftEvaluated + rightEvaluated
		} else if ast.Operator == "-" {
			return leftEvaluated - rightEvaluated
		}
	}
}
```

## 3.4 Representing Objects
There are a large variety of ways to store and represent variables later on in the code. It often depends on the language being used for the interpreter. 
- Will the data just be stored as primitive datatype (int, float, char, bool)
- Will the language have more complex data structures? (stack, queue, vector, array, list)
- How does the source language of the interpreter affect how values get stored?
In this monkey interpreter, values will be represented with an Object that will interface nicely with the design of the interpreter.

Each type (so far - integers, booleans, null) needs to be implemented from the `object` interface to get the type and 'inspect' the value.

## 3.5 Evaluating Expressions

The `eval` function is the most important. It should use recursion to call itself until the expression can be boiled down to literals and single operations. 
- Added integer and boolean literals
	- For booleans, there are only ever two objects that will ever be created, so to save space we just create the two objects for later use
	- The same goes for `null` 

#### Infix evaluation
Two categories
	- Returns a boolean ( <, >, &&, || )
	- Returns a number ( +, -, \*, / )

Added support for infix mathematical operations as well as boolean infix operators. Needed support for `leftVal` and `rightVal` being integers, `leftVal/rightVal` being bool and the opposite being int, and two booleans.

When **infix expressions** get evaluated, the double integer expressions must get evaluated first. The boolean comparisons are made super simple if they are handled after double integer, since we just compare the string literals to each other in any cases where integers aren't on both sides of the infix operator.

i.e.
``` Go
func evalInfixExpression(
	operator string,
	left, right object.Object,
	) object.Object {
		switch {
		case left.Type() == object.INTEGER_OBJ && right.Type() == object.INTEGER_OBJ:
		return evalIntegerInfixExpression(operator, left, right)
		case operator == "==":
			return nativeBoolToBooleanObject(left == right)
		case operator == "!=":
			return nativeBoolToBooleanObject(left != right)
		default:
			return NULL
		}
}
```
- Quick note on '>' '<' - I think it would be cool to make true/false evaluate to 0 or 1 when using  >< to compare two booleans/numbers
## 3.6 Conditionals
Pretty simple implementation for conditionals. We add some handling for Block Statements by calling `evalStatements`, then add a case for `*ast.IfExpression` in our `Eval` function. The `evalIfExpression` function simply evaluates the internal expression of the if statement, and returns the result if its 'truthy', otherwise it returns NULL. 
- In our case, `false` and `null` are 'truthy', but return false
- `True` is obviously truthy, and numbers are also truthy (i.e. `5` will return true from the `isTruthy` function)

## 3.7 Return Statements
Return statements stop the processing within a function, and should evaluate to the expression provided in the return statement.

Having an issue with the return statement tests. `return 5` returns \<nil>, but RETURN or any other version of `return` with at least 1 uppercase letter works fine... The parser tests aren't passing under `TestReturnStatements` and `TestReturnStatement`...
- Left off at the top of page 130