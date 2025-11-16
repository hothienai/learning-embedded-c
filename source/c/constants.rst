Constants in C
=========================

Introduction
-------------------------

Constants are fixed values that do not change during program execution. Unlike variables, 
once a constant is defined, its value remains the same throughout the program. Constants 
make code more readable, maintainable, and less error-prone by replacing magic numbers 
with meaningful names.

Benefits of using constants:
- Improved code readability
- Easier maintenance (change value in one place)
- Prevention of accidental modifications
- Self-documenting code

Types of Constants in C
-------------------------

C provides several ways to define constants:

1. Literal constants
2. Symbolic constants using ``#define``
3. Constant variables using ``const``
4. Enumeration constants using ``enum``

Literal Constants
-------------------------

Literal constants are fixed values written directly in the code.

Integer Literals
~~~~~~~~~~~~~~~~

.. code-block:: c

   int decimal = 42;            // Decimal literal
   int octal = 052;             // Octal literal (starts with 0)
   int hexadecimal = 0x2A;      // Hexadecimal literal (starts with 0x)
   int binary = 0b101010;       // Binary literal (C23 - starts with 0b)
   
   // Integer suffixes
   long lnum = 42L;             // Long literal
   unsigned int unum = 42U;     // Unsigned literal
   unsigned long ulnum = 42UL;  // Unsigned long literal
   long long llnum = 42LL;      // Long long literal

Floating-Point Literals
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   float f1 = 3.14f;            // Float literal (f suffix)
   double d1 = 3.14;            // Double literal (default)
   double d2 = 3.14e2;          // Scientific notation (314.0)
   double d3 = 3.14E-2;         // Scientific notation (0.0314)
   long double ld = 3.14L;      // Long double literal (L suffix)

Character Literals
~~~~~~~~~~~~~~~~~~

.. code-block:: c

   char ch = 'A';               // Single character
   char newline = '\n';         // Escape sequence
   char tab = '\t';             // Tab character
   char quote = '\'';           // Single quote
   char backslash = '\\';       // Backslash

Common escape sequences:

.. code-block:: text

   \n   - Newline
   \t   - Horizontal tab
   \r   - Carriage return
   \b   - Backspace
   \f   - Form feed
   \'   - Single quote
   \"   - Double quote
   \\   - Backslash
   \0   - Null character
   \a   - Alert (bell)
   \v   - Vertical tab

String Literals
~~~~~~~~~~~~~~~

.. code-block:: c

   char str1[] = "Hello, World!";
   char *str2 = "C Programming";
   
   // Multi-line string literals
   char long_str[] = "This is a very long string that "
                     "spans multiple lines for better "
                     "readability in source code.";

Symbolic Constants (#define)
-------------------------

The ``#define`` preprocessor directive creates symbolic constants that are replaced 
by the preprocessor before compilation.

Syntax
~~~~~~

.. code-block:: c

   #define CONSTANT_NAME value

Examples
~~~~~~~~

.. code-block:: c

   #define PI 3.14159
   #define MAX_SIZE 100
   #define BUFFER_LENGTH 256
   #define TRUE 1
   #define FALSE 0
   #define NEWLINE '\n'
   #define MESSAGE "Hello, World!"

Using #define Constants
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   #define PI 3.14159
   #define RADIUS 5.0
   
   int main() {
       double area = PI * RADIUS * RADIUS;
       printf("Area: %.2f\n", area);
       return 0;
   }

Advantages
~~~~~~~~~~

- No memory allocation (text replacement)
- Can be used in array size declarations
- Can be undefined and redefined

.. code-block:: c

   #define MAX 100
   int array[MAX];              // Valid - can use in array size
   
   #undef MAX                   // Undefine
   #define MAX 200              // Redefine

Disadvantages
~~~~~~~~~~~~~

- No type checking
- Not visible in debugger
- Can cause unexpected behavior if not parenthesized

.. code-block:: c

   #define SQUARE(x) x * x      // Dangerous!
   int result = SQUARE(2 + 3);  // Expands to: 2 + 3 * 2 + 3 = 11 (not 25!)
   
   #define SQUARE(x) ((x) * (x))  // Safe version
   int result = SQUARE(2 + 3);    // Expands to: ((2 + 3) * (2 + 3)) = 25

const Keyword
-------------------------

The ``const`` keyword creates type-safe constant variables that cannot be modified 
after initialization.

Syntax
~~~~~~

.. code-block:: c

   const data_type constant_name = value;

Examples
~~~~~~~~

.. code-block:: c

   const int MAX_STUDENTS = 50;
   const float PI = 3.14159f;
   const double GRAVITY = 9.81;
   const char GRADE = 'A';

Using const Variables
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       const int MAX_TEMP = 100;
       const float TAX_RATE = 0.08f;
       
       float price = 50.0f;
       float total = price + (price * TAX_RATE);
       
       printf("Total: $%.2f\n", total);
       
       // MAX_TEMP = 200;       // Error! Cannot modify const variable
       
       return 0;
   }

Advantages
~~~~~~~~~~

- Type safety (type checking at compile time)
- Visible in debugger
- Follows scoping rules like normal variables
- Can have pointers to const

Disadvantages
~~~~~~~~~~~~~

- Allocates memory (unlike #define)
- Cannot be used in array size declarations (in older C standards)
- Cannot be used in case labels

.. code-block:: c

   const int SIZE = 10;
   int arr[SIZE];               // Error in C89/C90, OK in C99+
   
   const int VALUE = 5;
   switch (x) {
       case VALUE:              // Error! Must be a literal constant
           break;
   }

const with Pointers
-------------------------

The ``const`` keyword can be used with pointers in different ways.

Pointer to Constant
~~~~~~~~~~~~~~~~~~~

Cannot change the value pointed to, but can change the pointer itself.

.. code-block:: c

   int value1 = 10;
   int value2 = 20;
   
   const int *ptr = &value1;    // Pointer to constant int
   // *ptr = 15;                // Error! Cannot modify value through ptr
   ptr = &value2;               // OK - can change pointer itself
   printf("%d\n", *ptr);        // Prints: 20

Constant Pointer
~~~~~~~~~~~~~~~~

Cannot change the pointer, but can change the value pointed to.

.. code-block:: c

   int value = 10;
   
   int *const ptr = &value;     // Constant pointer to int
   *ptr = 20;                   // OK - can modify value
   // ptr = &other;             // Error! Cannot change pointer
   printf("%d\n", *ptr);        // Prints: 20

Constant Pointer to Constant
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Cannot change either the pointer or the value pointed to.

.. code-block:: c

   int value = 10;
   
   const int *const ptr = &value;  // Constant pointer to constant int
   // *ptr = 20;                   // Error! Cannot modify value
   // ptr = &other;                // Error! Cannot change pointer
   printf("%d\n", *ptr);            // Prints: 10

Reading Pointer Declarations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Rule:** Read from right to left.

.. code-block:: c

   const int *ptr;              // ptr is a pointer to a constant int
   int const *ptr;              // Same as above (alternative syntax)
   int *const ptr;              // ptr is a constant pointer to an int
   const int *const ptr;        // ptr is a constant pointer to a constant int

Enumeration Constants
-------------------------

The ``enum`` keyword defines a set of named integer constants.

Syntax
~~~~~~

.. code-block:: c

   enum enum_name {
       CONSTANT1,
       CONSTANT2,
       CONSTANT3
   };

Basic Example
~~~~~~~~~~~~~

.. code-block:: c

   enum Color {
       RED,                     // 0
       GREEN,                   // 1
       BLUE                     // 2
   };
   
   enum Color myColor = RED;
   printf("%d\n", myColor);     // Prints: 0

Custom Values
~~~~~~~~~~~~~

.. code-block:: c

   enum Status {
       SUCCESS = 0,
       ERROR = -1,
       PENDING = 1,
       TIMEOUT = 100
   };
   
   enum Status result = SUCCESS;

Sequential Values
~~~~~~~~~~~~~~~~~

.. code-block:: c

   enum Month {
       JAN = 1,                 // 1
       FEB,                     // 2 (auto-increments)
       MAR,                     // 3
       APR,                     // 4
       MAY,                     // 5
       JUN,                     // 6
       JUL,                     // 7
       AUG,                     // 8
       SEP,                     // 9
       OCT,                     // 10
       NOV,                     // 11
       DEC                      // 12
   };

Using Enumerations
~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   enum Day {
       SUNDAY,
       MONDAY,
       TUESDAY,
       WEDNESDAY,
       THURSDAY,
       FRIDAY,
       SATURDAY
   };
   
   int main() {
       enum Day today = WEDNESDAY;
       
       if (today == WEDNESDAY) {
           printf("It's midweek!\n");
       }
       
       switch (today) {
           case MONDAY:
               printf("Start of work week\n");
               break;
           case FRIDAY:
               printf("TGIF!\n");
               break;
           case SATURDAY:
           case SUNDAY:
               printf("Weekend!\n");
               break;
           default:
               printf("Midweek day\n");
       }
       
       return 0;
   }

#define vs const vs enum
-------------------------

Comparison Table
~~~~~~~~~~~~~~~~

.. list-table:: Comparison of Constants
   :header-rows: 1
   :widths: 20 25 25 30

   * - Feature
     - #define
     - const
     - enum
   * - Type safety
     - No
     - Yes
     - Yes (integer only)
   * - Memory
     - No allocation
     - Allocates memory
     - Typically no allocation
   * - Debugger
     - Not visible
     - Visible
     - Visible
   * - Scope
     - Global (file)
     - Follows scope rules
     - Follows scope rules
   * - Array size
     - Yes
     - C99+ only
     - Yes
   * - Case label
     - Yes
     - No
     - Yes
   * - Grouping
     - No
     - No
     - Yes

When to Use What
~~~~~~~~~~~~~~~~

**Use #define when:**
- Need to use in case labels
- Need to use in array sizes (pre-C99)
- Creating simple constants without type
- Conditional compilation needed

**Use const when:**
- Type safety is important
- Need to pass constant by reference
- Want to use pointers to constants
- Debugging visibility is needed

**Use enum when:**
- Defining a set of related integer constants
- Need sequential values
- Want improved code readability for states/options
- Using in switch statements

Best Practices
-------------------------

1. **Use uppercase names for constants**

   .. code-block:: c
   
      #define MAX_SIZE 100
      const int BUFFER_LENGTH = 256;
      enum { TRUE = 1, FALSE = 0 };

2. **Group related constants**

   .. code-block:: c
   
      // Using enum for related constants
      enum ErrorCode {
          ERR_NONE,
          ERR_FILE_NOT_FOUND,
          ERR_PERMISSION_DENIED,
          ERR_OUT_OF_MEMORY
      };

3. **Use const for type-safe constants**

   .. code-block:: c
   
      const double PI = 3.141592653589793;
      const float EPSILON = 1e-6f;

4. **Parenthesize #define macros**

   .. code-block:: c
   
      #define SQUARE(x) ((x) * (x))  // Safe
      #define MAX(a,b) ((a) > (b) ? (a) : (b))

5. **Use meaningful names**

   .. code-block:: c
   
      const int MAX_STUDENTS = 50;     // Good
      const int N = 50;                // Poor

Common Use Cases
-------------------------

Mathematical Constants
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #define PI 3.14159265358979323846
   #define E 2.71828182845904523536
   const double EULER = 0.5772156649;
   const double PHI = 1.618033988749895;  // Golden ratio

Buffer Sizes
~~~~~~~~~~~~

.. code-block:: c

   #define BUFFER_SIZE 1024
   #define MAX_PATH 260
   #define MAX_USERNAME 32

Configuration Values
~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   const int MAX_CONNECTIONS = 100;
   const float TIMEOUT_SECONDS = 30.0f;
   const char DEFAULT_PORT[] = "8080";

State Machines
~~~~~~~~~~~~~~

.. code-block:: c

   enum State {
       STATE_IDLE,
       STATE_RUNNING,
       STATE_PAUSED,
       STATE_STOPPED,
       STATE_ERROR
   };

Error Codes
~~~~~~~~~~~

.. code-block:: c

   enum ErrorCode {
       SUCCESS = 0,
       ERR_INVALID_INPUT = -1,
       ERR_OUT_OF_MEMORY = -2,
       ERR_FILE_NOT_FOUND = -3,
       ERR_PERMISSION_DENIED = -4
   };

Practice Examples
-------------------------

Example 1: Using Various Constant Types
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   #define PI 3.14159
   #define MAX_SIZE 100
   
   const int MIN_VALUE = 0;
   const float TAX_RATE = 0.08f;
   
   enum Status {
       INACTIVE,
       ACTIVE,
       SUSPENDED
   };
   
   int main() {
       double radius = 5.0;
       double area = PI * radius * radius;
       
       int array[MAX_SIZE];
       
       float price = 100.0f;
       float tax = price * TAX_RATE;
       
       enum Status userStatus = ACTIVE;
       
       printf("Area: %.2f\n", area);
       printf("Array size: %d\n", MAX_SIZE);
       printf("Tax: $%.2f\n", tax);
       printf("Status: %d\n", userStatus);
       
       return 0;
   }

Example 2: Constant Pointers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       int x = 10;
       int y = 20;
       
       // Pointer to constant
       const int *ptr1 = &x;
       printf("Value: %d\n", *ptr1);
       ptr1 = &y;               // OK
       printf("Value: %d\n", *ptr1);
       // *ptr1 = 30;           // Error!
       
       // Constant pointer
       int *const ptr2 = &x;
       *ptr2 = 30;              // OK
       printf("Value: %d\n", *ptr2);
       // ptr2 = &y;            // Error!
       
       return 0;
   }

Example 3: Enumeration for Days
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   enum Weekday {
       MONDAY = 1,
       TUESDAY,
       WEDNESDAY,
       THURSDAY,
       FRIDAY,
       SATURDAY,
       SUNDAY
   };
   
   const char* getDayName(enum Weekday day) {
       switch (day) {
           case MONDAY: return "Monday";
           case TUESDAY: return "Tuesday";
           case WEDNESDAY: return "Wednesday";
           case THURSDAY: return "Thursday";
           case FRIDAY: return "Friday";
           case SATURDAY: return "Saturday";
           case SUNDAY: return "Sunday";
           default: return "Unknown";
       }
   }
   
   int main() {
       enum Weekday today = FRIDAY;
       printf("Today is: %s\n", getDayName(today));
       
       return 0;
   }

Summary
-------------------------

- Constants are fixed values that don't change during execution
- Use ``#define`` for simple text replacement constants
- Use ``const`` for type-safe constant variables
- Use ``enum`` for related integer constants
- Constant pointers can be: pointer to const, const pointer, or both
- Choose the appropriate constant type based on your needs
- Use uppercase naming convention for constants
- Constants improve code readability and maintainability
