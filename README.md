
# Tiny Language Compiler

A compiler for the **Tiny Language** – a simple procedural programming language used for educational and demonstration purposes. This compiler implements scanning (lexical analysis) and parsing (syntactic analysis) based on formal specifications.

## 📚 Table of Contents

- [Language Description](#-language-description)
- [Lexical Specifications (Regular Expressions)](#-lexical-specifications-regular-expressions)
	- [DFA for Number](#dfa-for-number)
	- [DFA for String](#dfa-for-string)
	- [DFA for Identifier](#dfa-for-identifier)
	- [DFA for Comment](#dfa-for-comment)
	- [DFAs for Operators](#dfas-for-operators)
	- [DFAs for Reserved Keywords](#dfas-for-reserved-keywords)
- [Grammar Rules (CFG)](#-grammar-rules-cfg)
  - [Program Structure](#program-structure)
  - [Function Declarations](#function-declarations)
  - [Function Body](#function-body)
  - [Statement Types](#statement-types)
  - [Expressions](#expressions)
  - [I/O and Variables](#io-and-variables)
  - [Conditions and Control Flow](#conditions-and-control-flow)
  - [Control Structures](#control-structures)
- [Components](#-components)
- [Example Code](#-example-code)

---

## 📜 Language Description

Tiny Language supports a structure similar to C-like procedural languages, including:

- Functions (user-defined and a required `main`)
- Variable declarations (`int`, `float`, `string`)
- Input/Output: `read`, `write`
- Control Flow: `if`, `elseif`, `else`, `repeat`...`until`
- Expressions and arithmetic
- Comments using `/*...*/`

---

## 🔍 Lexical Specifications (Regular Expressions)

**Character Classes:**
- `Digit`: `[0-9]`
- `Letter`: `[a-zA-Z]`
- `Operator`: `| \ / * ( ) + - := ; , <> < > = && || { }`

**Tokens:**
- **Number**: `(+|-)? Digit+ (“.” Digit*)? (E(+|-)? Digit)?`
- **String**: `" (Letter | Digit | Operator)* "`
- **Identifier**: `Letter+ (Letter | Digit)*`
- **Comment**: `(" / ")+ "*" (Digit | Letter | Operator)* "*" ("/")+`

### DFA for Number:
![Screenshot_2025-06-01_052850](https://github.com/user-attachments/assets/ad564cc8-9264-4feb-82f0-1abc3b4db270)

### DFA for String:
![Screenshot_2025-06-01_052808](https://github.com/user-attachments/assets/6eb3ce69-5e13-4fcb-9e96-f64eb66db27e)


### DFA for Identifier:

![Screenshot_2025-06-01_052812](https://github.com/user-attachments/assets/da6309f9-44d0-4c9b-8c43-1db06ffbfe54)

### DFA for Comment:
![Screenshot_2025-06-01_052819](https://github.com/user-attachments/assets/fc936c7a-ba93-4584-a3bf-544b195ac516)


### DFAs for Operators:
![Screenshot_2025-06-01_052829](https://github.com/user-attachments/assets/69fb60ca-57c9-41d0-bd43-96f54e6c8902)

### DFAs for Reserved Keywords:

![Screenshot_2025-06-01_052838](https://github.com/user-attachments/assets/1ba43266-97d7-4c70-8712-88205913f661)


**Reserved Keywords**:
```
int, float, string, read, write, repeat, until, if, elseif, else,
then, return, endl, main
```

---

## 🧠 Grammar Rules (CFG)

### Program Structure:
```
Program → Progfunction MainFunction
Progfunction → Function | ε
MainFunction → Datatype main () FunctionBody
```

### Function Declarations:
```
Function → FunctionDeclaration FunctionBody
FunctionDeclaration → Datatype identifier (Parameter)
Parameter → Datatype identifier Parameter' | ε
Parameter' → , Datatype identifier Parameter' | ε
```

### Function Body:
```
FunctionBody → { Statements ReturnStatement }
Statements → Statement Statements'
Statements' → ; Statement Statements' | ε
```

### Statement Types:
```
Statement → CommentStatement | AssignmentStatement | DeclarationStatement
          | WriteStatement | ReadStatement | ReturnStatement
          | ConditionStatement | IfStatement | ElseIfStatement
          | ElseStatement | RepeatStatement
```

### Expressions:
```
Expression → number | string | Operation
Operation → OperationBegin Op identifier
OperationBegin → Operation | identifier
Op → + | - | / | *
```

### I/O and Variables:
```
AssignmentStatement → identifier := Expression
DeclarationStatement → Datatype Declist | ε
Declist → identifier Declist' | AssignmentStatement Declist'
Declist' → , identifier Declist' | , AssignmentStatement Declist' | ε
WriteStatement → write Expression ; | write endl ;
ReadStatement → read identifier ;
```

### Conditions and Control Flow:
```
ConditionStatement → Condition ConditionStatement'
ConditionStatement' → BooleanOperator Condition ConditionStatement' | ε
Condition → identifier ConditionOperator Term
ConditionOperator → < | > | = | <>
BooleanOperator → && | ||
Term → number | identifier | FunctionCall
```

### Control Structures:
```
IfStatement → if ConditionStatement then Statements IfTail end
IfTail → ElseIfStatement | ElseStatement | ε
ElseIfStatement → elseif ConditionStatement then Statements IfTail
ElseStatement → else ConditionStatement Statements
RepeatStatement → repeat Statements until ConditionStatement
```

---

## 🔧 Components

- **Scanner (Lexer)**: Uses DFA based on the REs to tokenize input.
- **Parser**: Validates token streams against CFG rules.
- **Sample Programs**: Demonstrate syntax and compiler handling.

---

## 📂 Example Code

```c
int sum(int a, int b) {
    return a + b;
}

int main() {
    int val, counter;
    read val;
    counter := 0;
    repeat
        val := val - 1;
        write "Iteration number [";
        write counter;
        write "] the value of x = ";
        write val;
        write endl;
        counter := counter + 1;
    until val = 1
    write endl;
    string s := "number of Iterations = ";
    write s;
    counter := counter - 1;
    write counter;

    float z1 := 3*2*(2+1)/2-5.3;
    z1 := z1 + sum(1, y);
    if z1 > 5 || z1 < counter && z1 = 1 then
        write z1;
    elseif z1 < 5 then
        z1 := 5;
    else
        z1 := counter;
    end
    return 0;
}
```
