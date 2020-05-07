# Understanding a Linux based OS:  S1E4 - Life of Code
> A computer program is a collection of instructions that can be executed by a computer to perform a specific task. 

Here we shall focus on `C` language, which is a simple, yet powerful system programming language.

`C` is a compiled language. The process of compilation of `C` program looks as follows.

             source.c
                |
                V
         -----------------
         | Preprocessing |
         -----------------
                |
                V
        --------------------
        | Lexical Analysis |
        --------------------
                |
                V
        -------------------
        | Syntax Analysis |
        -------------------
                |
                V
       ---------------------
       | Semantic Analysis |
       ---------------------
                |
                V
        --------------------
        |   Intermediate    |
        |      Code         |
        |   Generation      |
        |       and         |
        |    Optimization   |
        ---------------------
                |
                V
        --------------------
        |     Target        |
        |      Code         |
        |   Generation      |
        |       and         |
        |    Optimization   |
        ---------------------
                 |
                 V
          -----------------
          |    Linking    |
          -----------------
                 |
                 V
               a.out

## Viewing these steps in `clang`

- Preprocessing
```    
    clang -E source.c
```
- Lexical Analysis
```
    clang -fsyntax-only -Xclang -dump-tokens  source.c
```
- Syntax Analysis
```
    clang -Xclang -ast-dump source.c
```
- Intermediate Code Generation
```
    clang -S -emit-llvm source.c 
    # source.ll is intermediate code
```
- Target Code Generation
```
    clang -c source.c
    # generates source.o
    # To view its disassembly
    # objdump -d -s source.o
```
- Linking
```
    clang -o source.o
```
- Execution
```
    ./a.out
```
While executing, some errors like "Segmentation fault" may occur. Although it is possible to monitor the values using `printf()`, some better way is needed.

---------
## `gdb` - The debugger
- Allows to step into the code line by line, print the values of expression, evaluate arbitrary expressions, ...
- We will see its basic functionalities
- Before `gdb` is invoked, we have to add debug symbols to the executable code for the ease of understanding
- `gcc -g source.c`
- And then, `gdb a.out`
- Following table lists some of `gdb` commands along their shorthands and uses

Command | Shorthand | Purpose
--------|-----------|---------
`run`   | `r`       | Runs the executable
`backtrace` | `bt`  | Shows the stack trace in case of error
`break` | `b`       | Add breakpoint at particular function or line
`list` | `l` | Show the code around particular line
`step` | `s` | Execute a line of code
`print` | `p` | Prints the value of an expression
`quit` | `q` | Exit from the debugger

--------
## `valgrind` - The memory inspector
- Programs can dynamically allocate heap memory using `malloc()`
- This memory **has to** be deallocated after the particular task is done using `free()`
- Otherwise, it will lead to a memory leak, where no other program would be able to use a particular portion of memory, even after the program is killed
- `valgrind` executes the programs, and reports memory leaks, if any.
```
    valgrind program
```

---