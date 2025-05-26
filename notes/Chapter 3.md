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
