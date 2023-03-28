# MONTY
building a Monty interpreter using Stacks, Queues - LIFO, FIFO.
This is part of the __0x19. C - Stacks, Queues - LIFO, FIFO__ Project.

__RESOURCES TO USE__

    Google
    How do I use extern to share variables between source files in C?
    Stacks and Queues in C
    Stack operations
    Queue operations
    Data structures


__CONCEPTS TO MASTER__

    1. What do LIFO and FIFO mean
    2.0What is a stack, and when to use it
    3. What is a queue, and when to use it
    4. What are the common implementations of stacks and queues
    5. What are the most common use cases of stacks and queues
    6. What is the proper way to use global variables

__REQUIREMENTS__
    ~ Allowed editors: `vi`, `vim`, `emacs`
    ~ All your files will be compiled on Ubuntu 20.04 LTS using `gcc`, using the options `-Wall -Werror -Wextra -pedantic -std=c89`
    ~ All your files should end with a new line
    ~ A README.md file, at the root of the folder of the project is mandatory
    ~ Your code should use the Betty style. It will be checked using `betty-style.pl` and `betty-doc.pl`
    ~ You allowed to use a maximum of one global variable
    ~ No more than 5 functions per file
    ~ You are allowed to use the C standard library
    ~ The prototypes of all your functions should be included in your header file called __monty.h__
    ~ Don’t forget to push your header file
    ~ All your header files should be include guarded
    ~ You are expected to do the tasks in the order shown in the project

__More Info__
## Data structures
Please use the following data structures for this project. Don’t forget to include them in your header file.
`
/**
 * struct stack_s - doubly linked list representation of a stack (or queue)
 * @n: integer
 * @prev: points to the previous element of the stack (or queue)
 * @next: points to the next element of the stack (or queue)
 *
 * Description: doubly linked list node structure
 * for stack, queues, LIFO, FIFO
 */
typedef struct stack_s
{
        int n;
        struct stack_s *prev;
        struct stack_s *next;
} stack_t;
`
`
/**
 * struct instruction_s - opcode and its function
 * @opcode: the opcode
 * @f: function to handle the opcode
 *
 * Description: opcode and its function
 * for stack, queues, LIFO, FIFO
 */
typedef struct instruction_s
{
        char *opcode;
        void (*f)(stack_t **stack, unsigned int line_number);
} instruction_t;
'


__Compilation & Output__
        - Your code will be compiled this way:
    `  gcc -Wall -Werror -Wextra -pedantic -std=c89 *.c -o monty`
        - Any output must be printed on `stdout`
        - Any error message must be printed on `stderr`
        - [Here is a link to a GitHub repository](https://github.com/sickill/stderred) that could help you making sure your errors are printed on `stderr`

* * *
### Tests

We strongly encourage you to work all together on a set of tests
The Monty language

***__Monty 0.98__*** is a scripting language that is first compiled into Monty byte codes (Just like Python). It relies on a unique stack, with specific instructions to manipulate it. The goal of this project is to create an interpreter for Monty ByteCodes files.

__Monty byte code files__

Files containing Monty byte codes usually have the .m extension. Most of the industry uses this standard but it is not required by the specification of the language. There is not more than one instruction per line. There can be any number of spaces before or after the opcode and its argument:
`julien@ubuntu:~/monty$ cat -e bytecodes/000.m
push 0$
push 1$
push 2$
  push 3$
                   pall    $
push 4$
    push 5    $
      push    6        $
pall$
julien@ubuntu:~/monty$
`
Monty byte code files can contain blank lines (empty or made of spaces only, and any additional text after the opcode or its required argument is not taken into account:
`julien@ubuntu:~/monty$ cat -e bytecodes/001.m
push 0 Push 0 onto the stack$
push 1 Push 1 onto the stack$
$
push 2$
  push 3$
                   pall    $
$
$
                           $
push 4$
$
    push 5    $
      push    6        $
$
pall This is the end of our program. Monty is awesome!$
julien@ubuntu:~/monty$`

__The monty program__
- Usage: `monty file
        - where file is the path to the file containing Monty byte code
    - If the user does not give any file or more than one argument to your program, print the error message `USAGE: monty file`, followed by a new line, and exit with the status `EXIT_FAILURE`
    - If, for any reason, it’s not possible to open the file, print the error message Error: Can't open file <file>, followed by a new line, and exit with the status `EXIT_FAILURE`
        - where `<file>` is the name of the file
    - If the file contains an invalid instruction, print the error message `L<line_number>: unknown instruction <opcode>`, followed by a new line, and exit with the status `EXIT_FAILURE`
        - where is the line number where the instruction appears.
        - Line numbers always start at 1
    - The monty program runs the bytecodes line by line and stop if either:
        - it executed properly every line of the file
        - it finds an error in the file
        - an error occured
    - If you can’t malloc anymore, print the error message Error: malloc failed, followed by a new line, and exit with status `EXIT_FAILURE.`
    - You have to use `malloc` and `free` and are not allowed to use any other function from `man malloc` (realloc, calloc, …)

These are valid operation codes:

1. **push**

   Push only valid integers into the doubly linked list. Defaults to a Stack LIFO status. 

   Given:

   ```
   $ cat test.m
   push 1
   push 2
   pall
   ```

   Expected:

   ```
   $ ./monty test.m
   2
   1
   ```

   Failures:

   - Error: usage: push integer      *# if no argument given or invalid argument*

   ​


1. **pall**

   Print the values within the linked list.

   ​

2. **pint**

   Print only the value at the top of the linked list (head of linked list).

   Given:

   ```
   $ cat test.m
   push 1
   push 2
   pint
   ```

   Expected:

   ```
   $ ./monty test.m
   2
   ```

   Failures:

   - Error: can't pint, stack empty      *# If linked list is empty, can't print*

   ​

3. **pop**

   Remove the top element of the linked list.

   Given:

   ```
   $ cat test.m
   push 1
   push 2
   push 3
   pop
   pall
   ```

   Expected:

   ```
   $ ./monty test.m
   2
   1
   ```

   Failures:

   - Error: can't pop an empty stack      *# If linked list is empty*

   ​

4. **swap**

   Swap the top 2 elements of the linked list.

   Given:

   ```
   $ cat test.m
   push 1
   push 2
   swap
   pall
   ```

   Expected:

   ```
   $ ./monty test.m
   1
   2
   ```

   Failures:

   - Error: can't swap, stack too short      *# must have at least 2 elements*

   ​

5. **add**

   Add the top 2 elements of the linked list and store as the new head of the linked list.

   Given:

   ```
   $ cat test.m
   push 1
   push 2
   add
   pall
   ```

   Expected:

   ```
   $ ./monty test.m
   3
   ```

   Failures:

   - Error: can't add, stack too short      *# must have at least 2 elements*

   ​

6. **nop**

   This operation doesn't do anything.

   Given:

   ```
   $ cat test.m
   push 1
   nop
   pall
   ```

   Expected:

   ```
   $ ./monty test.m
   1
   ```

   ​

7. **sub**

   Subtract the top element of the linked list from the second top element of the linked list.

   Given:

   ```
   $ cat test.m
   push 1
   push 2
   sub
   pall
   ```

   Expected:

   ```
   $ ./monty test.m
   -1
   ```

   Failures:

   - Error: can't sub, stack too short      *# must have at least 2 elements*

   ​

8. **div**

   Divide the second top element of the stack by the top element of the stack.

   Given:

   ```
   $ cat test.m
   push 20
   push 10
   div
   pall
   ```

   Expected:

   ```
   $ ./monty test.m
   2
   ```

   Failures:

   - Error: can't div, stack too short      *# must have at least 2 elements*
   - Error: division by zero      *# cannot divide by 0*

   ​

9. **mul**

   Multiply the top 2 elements in the linked list and store as the new head of the linked list.

   Given:

   ```
   $ cat test.m
   push 2
   push 3
   mul
   pall
   ```

   Expected:

   ```
   $ ./monty test.m
   6
   ```

   Failures:

   - Error: can't mul, stack too short      *# must have at least 2 elements*

   ​

10. **mod**

    Uses the modulus of the second top element of the linked list by the top element of the linked list. The new value is stored as the new head.

    Given:

    ```
    $ cat test.m
    push 7
    push 3
    mod
    pall
    ```

    Expected:

    ```
    $ ./monty test.m
    1
    ```

    Failures:

    - Error: can't mod, stack too short      *# must have at least 2 elements*
    - Error: division by zero      *# cannot divide by 0*

    ​

11. **pchar**

    Convert the integer at the top of the linked list into a character using [ASCII](http://www.asciitable.com/).

    Given:

    ```
    $ cat test.m
    push 72
    pchar
    ```

    Expected:

    ```
    $ ./monty test.m
    H
    ```

    Failures:

    - Error: can't pchar, value out of range      *# if value is out of range of ASCII*
    - Error: can't pchar, stack empty      *# if linked list is empty*

    ​

12. **pstr**

    Print the string starting at the top of the linked list, using ASCII values. The string should stop printing if the stack is over, the value of the element is 0, or if the value is not an ASCII value.

    Given:

    ```
    $ cat test.m
    push 1
    push 110
    push 0
    push 110
    push 111
    push 116
    push 114
    push 101
    push 98
    push 108
    push 111
    push 72
    pstr
    ```

    Expected:

    ```
    $ ./monty test.m
    Holberton
    ```

    ​

13. **rotl**

    Rotates the first element of the linked list to the end of the list. The new head is the second top element of the linked list.

    Given:

    ```
    $ cat test.m
    push 1
    push 2
    push 3
    rotl
    pall
    ```

    Expected:

    ```
    $ ./monty test.m
    2
    1
    3
    ```

    ​

14. **rotr**

    Rotates the last element of the linked list to the head, so that it becomes the new head.

    Given:

    ```
    $ cat test.m
    push 1
    push 2
    push 3
    rotr
    pall
    ```

    Expected:

    ```
    $ ./monty test.m
    1
    3
    2
    ```

    ​

15. **stack**

    Mode Change: operations that add or remove from the linked list work using [LIFO](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)). This is the default setting.

    ​

16. **queue**

    Mode Change: operations that add or remove from the linked list work using [FIFO](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics)). 

    Given:

    ```
    $ cat test.m
    queue
    push 1
    push 2
    push 3
    pall
    ```

    Expected:

    ```
    $ ./monty test.m
    1
    2
    3
    ```



## File Descriptions

- `monty.h` - Header file which contains library includes, structs created, and all function prototypes.
- `main.c`  - Main monty file handling the core operations, including reading user input arguments, opening bytecode file, parsing inputs, and calling correct operations.
- `get_opcode_func.c` - Returns the correct operation function.
- `string_helper.c` - Contains tokenizer, comment check, and helper functions.
- `helper.c` - Contains number argument conversion for the push operation, mode switch for queue vs stack, and helper functions for push, rotl, and rotr.
- `failures.c` - Fail checks and exits given certain conditions.
- `stack_funcs.c` - Core functions to manipulate and change the linked list. Other file(s): `stack_funcs2.c`
- `opcode_func.c` - Functions for each operation. Other file(s): `opcode_func2.c`, `opcode_func3.c`, `opcode_func4.c`
- `brainfuck` - Directory with brainfuck files.



## Team
Vashty Kuria [Github](https://github.com/IVashty) || [LinkedIn](https://twitter.com/i-vashty-k)
