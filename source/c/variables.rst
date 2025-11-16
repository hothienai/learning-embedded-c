Variables in C
=========================

Introduction
-------------------------

A variable is a named storage location in memory that holds a value which can be changed during 
program execution. Variables are fundamental to programming as they allow us to store, manipulate, 
and retrieve data throughout our programs.

In C, every variable must be:
- Declared with a specific data type
- Given a unique name (identifier)
- Allocated memory space before use

What is a Variable?
-------------------------

A variable consists of three main components:

1. **Name (Identifier):** A symbolic name used to reference the variable
2. **Type:** Defines what kind of data the variable can hold
3. **Value:** The actual data stored in memory
4. **Address:** The memory location where the value is stored

.. code-block:: c

   int age = 25;
   // 'int' is the type
   // 'age' is the name (identifier)
   // 25 is the value
   // The address is assigned by the compiler

Variable Declaration
-------------------------

Declaration tells the compiler about the variable's name and type, allocating memory for it.

Syntax
~~~~~~

.. code-block:: c

   data_type variable_name;

Examples
~~~~~~~~

.. code-block:: c

   int count;                    // Declares an integer variable
   float temperature;            // Declares a float variable
   char grade;                   // Declares a character variable
   double price;                 // Declares a double variable

Multiple Variables
~~~~~~~~~~~~~~~~~~

You can declare multiple variables of the same type in one statement:

.. code-block:: c

   int x, y, z;                  // Declares three integers
   float a, b, c;                // Declares three floats
   char first, last, middle;     // Declares three characters

Variable Initialization
-------------------------

Initialization is assigning an initial value to a variable at the time of declaration.

Syntax
~~~~~~

.. code-block:: c

   data_type variable_name = value;

Examples
~~~~~~~~

.. code-block:: c

   int age = 25;                 // Declare and initialize
   float pi = 3.14159f;
   char grade = 'A';
   double price = 99.99;

Multiple Initialization
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   int x = 10, y = 20, z = 30;
   float a = 1.5f, b = 2.5f, c = 3.5f;

Declaration vs Initialization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   // Declaration only (uninitialized)
   int count;                    // Contains garbage value
   
   // Declaration with initialization
   int sum = 0;                  // Contains 0
   
   // Declaration then assignment
   int result;
   result = 42;                  // Assigned later

Variable Naming Rules
-------------------------

Identifiers (variable names) must follow these rules:

Valid Characters
~~~~~~~~~~~~~~~~

- Letters: a-z, A-Z
- Digits: 0-9
- Underscore: _

Rules
~~~~~

1. **Must start with a letter or underscore** (not a digit)

   .. code-block:: c
   
      int age;           // Valid
      int _temp;         // Valid
      int 2count;        // Invalid - starts with digit

2. **Cannot use C keywords** (reserved words)

   .. code-block:: c
   
      int int;           // Invalid - 'int' is a keyword
      int return;        // Invalid - 'return' is a keyword
      int variable;      // Valid

3. **Case-sensitive**

   .. code-block:: c
   
      int age;
      int Age;           // Different from 'age'
      int AGE;           // Different from both above

4. **No spaces or special characters** (except underscore)

   .. code-block:: c
   
      int my_var;        // Valid
      int my var;        // Invalid - contains space
      int my-var;        // Invalid - contains hyphen
      int my$var;        // Invalid - contains $

5. **Length limit** - typically up to 31 characters (compiler-dependent)

C Keywords (Reserved Words)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Cannot be used as variable names:

.. code-block:: text

   auto        break       case        char        const
   continue    default     do          double      else
   enum        extern      float       for         goto
   if          int         long        register    return
   short       signed      sizeof      static      struct
   switch      typedef     union       unsigned    void
   volatile    while
   
   // C99 additions
   _Bool       _Complex    _Imaginary  inline      restrict
   
   // C11 additions
   _Alignas    _Alignof    _Atomic     _Generic    _Noreturn
   _Static_assert          _Thread_local

Naming Conventions
-------------------------

While not enforced by the compiler, following naming conventions improves code readability:

Snake Case (Recommended for C)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   int student_count;
   float average_temperature;
   char first_name;

Camel Case
~~~~~~~~~~

.. code-block:: c

   int studentCount;
   float averageTemperature;
   char firstName;

Descriptive Names
~~~~~~~~~~~~~~~~~

.. code-block:: c

   // Good - descriptive names
   int user_age;
   float product_price;
   char grade_letter;
   
   // Poor - unclear names
   int x;
   float p;
   char g;

Constants in All Caps
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #define MAX_SIZE 100
   #define PI 3.14159
   const int BUFFER_SIZE = 256;

Variable Scope
-------------------------

Scope determines where a variable can be accessed in the program.

Local Variables
~~~~~~~~~~~~~~~

Declared inside a function or block, accessible only within that scope.

.. code-block:: c

   void myFunction() {
       int local_var = 10;      // Local to myFunction
       printf("%d\n", local_var);
   }
   
   int main() {
       int x = 5;               // Local to main
       // printf("%d", local_var);  // Error! local_var not accessible
       return 0;
   }

Global Variables
~~~~~~~~~~~~~~~~

Declared outside all functions, accessible throughout the program.

.. code-block:: c

   int global_var = 100;        // Global variable
   
   void function1() {
       printf("%d\n", global_var);  // Can access global_var
   }
   
   void function2() {
       global_var = 200;        // Can modify global_var
   }
   
   int main() {
       printf("%d\n", global_var);  // Can access global_var
       return 0;
   }

Block Scope
~~~~~~~~~~~

Variables declared within a block (between {}) are only visible in that block.

.. code-block:: c

   int main() {
       int x = 10;
       
       if (x > 5) {
           int y = 20;          // y only exists in this block
           printf("%d %d\n", x, y);
       }
       
       // printf("%d", y);      // Error! y not accessible here
       return 0;
   }

Variable Lifetime
-------------------------

The lifetime of a variable is the duration for which it exists in memory.

Automatic Variables
~~~~~~~~~~~~~~~~~~~

Created when the block is entered, destroyed when the block is exited.

.. code-block:: c

   void myFunction() {
       int auto_var = 10;       // Created when function is called
   }                            // Destroyed when function returns

Storage Classes
-------------------------

Storage classes define the scope, lifetime, and visibility of variables.

auto
~~~~

Default storage class for local variables (rarely used explicitly).

.. code-block:: c

   void myFunction() {
       auto int x = 10;         // Same as: int x = 10;
   }

extern
~~~~~~

Declares a variable that is defined in another file.

File1.c:

.. code-block:: c

   int shared_var = 42;         // Definition

File2.c:

.. code-block:: c

   extern int shared_var;       // Declaration (refers to File1.c)
   
   void display() {
       printf("%d\n", shared_var);
   }

register
~~~~~~~~

Suggests the compiler store the variable in a CPU register for faster access.

.. code-block:: c

   register int counter;        // Hint to compiler (may be ignored)

**Note:** Cannot take the address of a register variable.

const Keyword
-------------------------

The ``const`` keyword makes a variable read-only (cannot be modified after initialization).

.. code-block:: c

   const int MAX_SIZE = 100;
   const float PI = 3.14159f;
   
   // MAX_SIZE = 200;           // Error! Cannot modify const variable

Constant Pointers
~~~~~~~~~~~~~~~~~

.. code-block:: c

   int value = 10;
   
   // Pointer to constant - cannot change value through pointer
   const int *ptr1 = &value;
   // *ptr1 = 20;               // Error!
   ptr1 = &other_value;         // OK - can change pointer itself
   
   // Constant pointer - cannot change pointer
   int *const ptr2 = &value;
   *ptr2 = 20;                  // OK - can change value
   // ptr2 = &other_value;      // Error! Cannot change pointer
   
   // Constant pointer to constant - cannot change either
   const int *const ptr3 = &value;
   // *ptr3 = 20;               // Error!
   // ptr3 = &other_value;      // Error!

Variable Assignment
-------------------------

After declaration, you can assign values to variables using the assignment operator ``=``.

.. code-block:: c

   int x;
   x = 10;                      // Simple assignment
   
   int y = x;                   // Assign value of x to y
   
   int a, b, c;
   a = b = c = 0;              // Chain assignment (right to left)

Compound Assignment
~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   int x = 10;
   
   x += 5;                      // x = x + 5;  (now 15)
   x -= 3;                      // x = x - 3;  (now 12)
   x *= 2;                      // x = x * 2;  (now 24)
   x /= 4;                      // x = x / 4;  (now 6)
   x %= 5;                      // x = x % 5;  (now 1)

Increment and Decrement
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   int count = 5;
   
   count++;                     // Post-increment: count = 6
   ++count;                     // Pre-increment: count = 7
   count--;                     // Post-decrement: count = 6
   --count;                     // Pre-decrement: count = 5

**Difference between pre and post:**

.. code-block:: c

   int x = 5;
   int y = x++;                 // y = 5, x = 6 (use then increment)
   
   int a = 5;
   int b = ++a;                 // b = 6, a = 6 (increment then use)

Best Practices
-------------------------

1. **Always initialize variables**

   .. code-block:: c
   
      int count = 0;            // Good
      int sum;                  // Bad - contains garbage value

2. **Use meaningful names**

   .. code-block:: c
   
      int student_age;          // Good
      int sa;                   // Poor

3. **Declare variables close to their use**

   .. code-block:: c
   
      for (int i = 0; i < 10; i++) {  // Declare i where it's used
          printf("%d ", i);
      }

4. **Minimize global variables**

   .. code-block:: c
   
      // Prefer passing parameters instead of using globals
      int add(int a, int b) {
          return a + b;
      }

5. **Use const for values that shouldn't change**

   .. code-block:: c
   
      const int MAX_STUDENTS = 50;
      const float TAX_RATE = 0.08f;

6. **Initialize pointers**

   .. code-block:: c
   
      int *ptr = NULL;          // Good - initialized to NULL
      // int *ptr;              // Bad - dangling pointer

Common Mistakes
-------------------------

1. **Using uninitialized variables**

   .. code-block:: c
   
      int x;
      printf("%d", x);          // Undefined behavior!

2. **Confusing assignment with comparison**

   .. code-block:: c
   
      int x = 5;
      if (x = 10) {             // Wrong! This is assignment, not comparison
          // Always true (10 is non-zero)
      }
      
      if (x == 10) {            // Correct - comparison
          // This is what you meant
      }

3. **Variable shadowing**

   .. code-block:: c
   
      int x = 10;
      {
          int x = 20;           // Shadows outer x
          printf("%d\n", x);    // Prints 20
      }
      printf("%d\n", x);        // Prints 10

4. **Modifying const variables**

   .. code-block:: c
   
      const int MAX = 100;
      // MAX = 200;             // Error!

5. **Forgetting to declare variables**

   .. code-block:: c
   
      x = 10;                   // Error! x not declared
      int x;                    // Should declare first

Practice Examples
-------------------------

Example 1: Basic Variable Operations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       // Declaration and initialization
       int a = 10;
       int b = 20;
       int sum, diff, prod;
       
       // Operations
       sum = a + b;
       diff = b - a;
       prod = a * b;
       
       // Display results
       printf("a = %d, b = %d\n", a, b);
       printf("Sum = %d\n", sum);
       printf("Difference = %d\n", diff);
       printf("Product = %d\n", prod);
       
       return 0;
   }

Example 2: Variable Scope Demonstration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int global = 100;            // Global variable
   
   void display() {
       int local = 50;           // Local variable
       printf("Global: %d\n", global);
       printf("Local: %d\n", local);
   }
   
   int main() {
       int main_var = 25;        // Local to main
       
       printf("In main - Global: %d\n", global);
       printf("In main - main_var: %d\n", main_var);
       
       display();
       // printf("%d", local);   // Error! local not accessible
       
       return 0;
   }

Example 3: Constant Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       // Constant variable
       const int MAX_SIZE = 100;
       printf("MAX_SIZE: %d\n", MAX_SIZE);
       // MAX_SIZE = 200;        // Error!
       
       return 0;
   }

Summary
-------------------------

- Variables are named storage locations for data
- Must be declared with a type before use
- Should be initialized to avoid undefined behavior
- Follow naming rules and conventions for readability
- Understand scope (local, global, block) and lifetime
- Use storage classes (auto, extern, register) appropriately
- Use ``const`` for read-only variables
- Always initialize variables before use
- Choose meaningful, descriptive names
