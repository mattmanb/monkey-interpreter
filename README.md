# Monkey Interpreter (Following *Writing an Interpreter in Go*)

This repository is a personal project where I follow along with the book *Writing an Interpreter in Go* by Thorsten Ball. The book walks through the process of building a programming language (called Monkey) from scratch using the Go programming language.

## 📘 About the Book

*Writing an Interpreter in Go* is a hands-on introduction to compiler and interpreter design. It walks through the implementation of a small, interpreted, dynamically typed language. This repository contains the code I write as I work through the book, chapter by chapter.

## 🧠 What You’ll Find Here

- ✅ My implementation of the Monkey interpreter
- 🗂️ Chapter-by-chapter progress tracking
- 🧪 Unit tests for various components (lexer, parser, evaluator, etc.)
- ✏️ Notes, comments, and improvements where I felt inspired to experiment

## 📂 Project Structure

```bash
.
├── ast/           # Abstract Syntax Tree definitions
├── evaluator/     # Evaluates parsed AST into runtime values
├── lexer/         # Tokenizer for the Monkey language
├── object/        # Object system (e.g., strings, integers, booleans)
├── parser/        # Recursive descent parser
├── repl/          # Read-Eval-Print Loop (interactive shell)
├── token/         # Token types used by the lexer
├── main.go        # Entry point to the Monkey REPL
└── README.md      # This file
