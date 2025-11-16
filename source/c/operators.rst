Operators in C
=========================

Introduction
-------------------------

Operators are special symbols that perform specific operations on one, two, or three operands, 
and then return a result. They are fundamental building blocks for creating expressions and 
performing computations in C programming.

C provides a rich set of operators categorized into several types:
- Arithmetic operators
- Relational operators
- Logical operators
- Bitwise operators
- Assignment operators
- Increment/Decrement operators
- Conditional (Ternary) operator
- Special operators

Arithmetic Operators
-------------------------

Arithmetic operators perform mathematical operations on numeric operands.

Basic Arithmetic Operators
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. list-table:: Arithmetic Operators
   :header-rows: 1
   :widths: 15 30 25 30

   * - Operator
     - Description
     - Example
     - Result
   * - ``+``
     - Addition
     - ``5 + 3``
     - ``8``
   * - ``-``
     - Subtraction
     - ``5 - 3``
     - ``2``
   * - ``*``
     - Multiplication
     - ``5 * 3``
     - ``15``
   * - ``/``
     - Division
     - ``5 / 2``
     - ``2`` (integer division)
   * - ``%``
     - Modulus (remainder)
     - ``5 % 2``
     - ``1``

Examples
~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       int a = 10, b = 3;
       
       printf("Addition: %d + %d = %d\n", a, b, a + b);        // 13
       printf("Subtraction: %d - %d = %d\n", a, b, a - b);     // 7
       printf("Multiplication: %d * %d = %d\n", a, b, a * b);  // 30
       printf("Division: %d / %d = %d\n", a, b, a / b);        // 3
       printf("Modulus: %d %% %d = %d\n", a, b, a % b);        // 1
       
       return 0;
   }

Integer vs Floating-Point Division
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   int x = 7, y = 2;
   float result1 = x / y;           // 3.0 (integer division, then converted)
   float result2 = (float)x / y;    // 3.5 (floating-point division)
   
   printf("Integer division: %.1f\n", result1);   // 3.0
   printf("Float division: %.1f\n", result2);     // 3.5

Modulus Operator Notes
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   // Modulus only works with integers
   int mod1 = 10 % 3;      // 1
   int mod2 = -10 % 3;     // -1 (sign of dividend)
   int mod3 = 10 % -3;     // 1 (sign of dividend)
   
   // float a = 10.5 % 3.2;  // Error! Cannot use % with floats

Relational (Comparison) Operators
-------------------------

Relational operators compare two operands and return 1 (true) or 0 (false).

Relational Operators Table
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. list-table:: Relational Operators
   :header-rows: 1
   :widths: 15 30 25 30

   * - Operator
     - Description
     - Example
     - Result
   * - ``==``
     - Equal to
     - ``5 == 5``
     - ``1`` (true)
   * - ``!=``
     - Not equal to
     - ``5 != 3``
     - ``1`` (true)
   * - ``>``
     - Greater than
     - ``5 > 3``
     - ``1`` (true)
   * - ``<``
     - Less than
     - ``5 < 3``
     - ``0`` (false)
   * - ``>=``
     - Greater than or equal to
     - ``5 >= 5``
     - ``1`` (true)
   * - ``<=``
     - Less than or equal to
     - ``5 <= 3``
     - ``0`` (false)

Examples
~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       int a = 10, b = 20;
       
       printf("%d == %d is %d\n", a, b, a == b);  // 0 (false)
       printf("%d != %d is %d\n", a, b, a != b);  // 1 (true)
       printf("%d > %d is %d\n", a, b, a > b);    // 0 (false)
       printf("%d < %d is %d\n", a, b, a < b);    // 1 (true)
       printf("%d >= %d is %d\n", a, b, a >= b);  // 0 (false)
       printf("%d <= %d is %d\n", a, b, a <= b);  // 1 (true)
       
       return 0;
   }

Common Mistake: Assignment vs Comparison
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   int x = 5;
   
   if (x = 10) {           // Wrong! This is assignment, not comparison
       printf("True\n");   // Always executes (10 is non-zero)
   }
   
   if (x == 10) {          // Correct - comparison
       printf("x is 10\n");
   }

Logical Operators
-------------------------

Logical operators perform logical operations and return 1 (true) or 0 (false).

Logical Operators Table
~~~~~~~~~~~~~~~~~~~~~~~~

.. list-table:: Logical Operators
   :header-rows: 1
   :widths: 15 30 35 20

   * - Operator
     - Description
     - Example
     - Result
   * - ``&&``
     - Logical AND
     - ``(5 > 3) && (8 > 5)``
     - ``1`` (true)
   * - ``||``
     - Logical OR
     - ``(5 > 3) || (8 < 5)``
     - ``1`` (true)
   * - ``!``
     - Logical NOT
     - ``!(5 > 3)``
     - ``0`` (false)

Truth Tables
~~~~~~~~~~~~

**AND (&&) Operator:**

.. code-block:: text

   A     B     A && B
   0     0       0
   0     1       0
   1     0       0
   1     1       1

**OR (||) Operator:**

.. code-block:: text

   A     B     A || B
   0     0       0
   0     1       1
   1     0       1
   1     1       1

**NOT (!) Operator:**

.. code-block:: text

   A     !A
   0      1
   1      0

Examples
~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       int a = 5, b = 10, c = 15;
       
       // AND operator
       if (a < b && b < c) {
           printf("a < b < c\n");  // Executes
       }
       
       // OR operator
       if (a > 10 || b > 5) {
           printf("At least one condition is true\n");  // Executes
       }
       
       // NOT operator
       if (!(a > b)) {
           printf("a is not greater than b\n");  // Executes
       }
       
       return 0;
   }

Short-Circuit Evaluation
~~~~~~~~~~~~~~~~~~~~~~~~~

Logical operators use short-circuit evaluation:

.. code-block:: c

   int x = 5;
   
   // AND: If first is false, second is not evaluated
   if (x > 10 && ++x > 5) {
       // Second condition not evaluated
   }
   printf("%d\n", x);  // Still 5
   
   // OR: If first is true, second is not evaluated
   if (x < 10 || ++x > 5) {
       // Second condition not evaluated
   }
   printf("%d\n", x);  // Still 5

Bitwise Operators
-------------------------

Bitwise operators perform operations on individual bits of integer operands.

Bitwise Operators Table
~~~~~~~~~~~~~~~~~~~~~~~~

.. list-table:: Bitwise Operators
   :header-rows: 1
   :widths: 15 30 30 25

   * - Operator
     - Description
     - Example
     - Binary Result
   * - ``&``
     - Bitwise AND
     - ``5 & 3``
     - ``0101 & 0011 = 0001`` (1)
   * - ``|``
     - Bitwise OR
     - ``5 | 3``
     - ``0101 | 0011 = 0111`` (7)
   * - ``^``
     - Bitwise XOR
     - ``5 ^ 3``
     - ``0101 ^ 0011 = 0110`` (6)
   * - ``~``
     - Bitwise NOT
     - ``~5``
     - ``~0101 = 1010`` (-6)
   * - ``<<``
     - Left shift
     - ``5 << 1``
     - ``0101 << 1 = 1010`` (10)
   * - ``>>``
     - Right shift
     - ``5 >> 1``
     - ``0101 >> 1 = 0010`` (2)

Examples
~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       unsigned int a = 5;   // 0101 in binary
       unsigned int b = 3;   // 0011 in binary
       
       printf("a & b = %u\n", a & b);   // 1 (0001)
       printf("a | b = %u\n", a | b);   // 7 (0111)
       printf("a ^ b = %u\n", a ^ b);   // 6 (0110)
       printf("~a = %u\n", ~a);         // Large number (inverted bits)
       printf("a << 1 = %u\n", a << 1); // 10 (1010)
       printf("a >> 1 = %u\n", a >> 1); // 2 (0010)
       
       return 0;
   }

Common Bitwise Operations
~~~~~~~~~~~~~~~~~~~~~~~~~~

**Setting a bit:**

.. code-block:: c

   unsigned int flags = 0;
   flags |= (1 << 3);      // Set bit 3

**Clearing a bit:**

.. code-block:: c

   flags &= ~(1 << 3);     // Clear bit 3

**Toggling a bit:**

.. code-block:: c

   flags ^= (1 << 3);      // Toggle bit 3

**Checking a bit:**

.. code-block:: c

   if (flags & (1 << 3)) {
       // Bit 3 is set
   }

Assignment Operators
-------------------------

Assignment operators assign values to variables.

Assignment Operators Table
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. list-table:: Assignment Operators
   :header-rows: 1
   :widths: 15 30 30 25

   * - Operator
     - Description
     - Example
     - Equivalent
   * - ``=``
     - Simple assignment
     - ``x = 5``
     - 
   * - ``+=``
     - Add and assign
     - ``x += 3``
     - ``x = x + 3``
   * - ``-=``
     - Subtract and assign
     - ``x -= 3``
     - ``x = x - 3``
   * - ``*=``
     - Multiply and assign
     - ``x *= 3``
     - ``x = x * 3``
   * - ``/=``
     - Divide and assign
     - ``x /= 3``
     - ``x = x / 3``
   * - ``%=``
     - Modulus and assign
     - ``x %= 3``
     - ``x = x % 3``
   * - ``&=``
     - AND and assign
     - ``x &= 3``
     - ``x = x & 3``
   * - ``|=``
     - OR and assign
     - ``x |= 3``
     - ``x = x | 3``
   * - ``^=``
     - XOR and assign
     - ``x ^= 3``
     - ``x = x ^ 3``
   * - ``<<=``
     - Left shift and assign
     - ``x <<= 2``
     - ``x = x << 2``
   * - ``>>=``
     - Right shift and assign
     - ``x >>= 2``
     - ``x = x >> 2``

Examples
~~~~~~~~

.. code-block:: c

   int x = 10;
   
   x += 5;     // x = 15
   x -= 3;     // x = 12
   x *= 2;     // x = 24
   x /= 4;     // x = 6
   x %= 5;     // x = 1

Increment and Decrement Operators
-------------------------

These operators increase or decrease a variable's value by 1.

Increment/Decrement Table
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. list-table:: Increment/Decrement Operators
   :header-rows: 1
   :widths: 20 40 40

   * - Operator
     - Description
     - Example
   * - ``++var``
     - Pre-increment
     - Increment then use value
   * - ``var++``
     - Post-increment
     - Use value then increment
   * - ``--var``
     - Pre-decrement
     - Decrement then use value
   * - ``var--``
     - Post-decrement
     - Use value then decrement

Pre vs Post Increment
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   int x = 5;
   int y;
   
   // Post-increment
   y = x++;    // y = 5, x = 6 (use then increment)
   
   x = 5;
   // Pre-increment
   y = ++x;    // y = 6, x = 6 (increment then use)

Examples
~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       int a = 5, b = 5;
       
       printf("Post-increment: %d\n", a++);  // Prints 5, a becomes 6
       printf("a is now: %d\n", a);          // Prints 6
       
       printf("Pre-increment: %d\n", ++b);   // Prints 6, b becomes 6
       printf("b is now: %d\n", b);          // Prints 6
       
       return 0;
   }

Conditional (Ternary) Operator
-------------------------

The conditional operator is a shorthand for simple if-else statements.

Syntax
~~~~~~

.. code-block:: c

   condition ? expression_if_true : expression_if_false;

Examples
~~~~~~~~

.. code-block:: c

   int a = 10, b = 20;
   int max;
   
   // Using ternary operator
   max = (a > b) ? a : b;
   printf("Max: %d\n", max);    // 20
   
   // Equivalent if-else
   if (a > b) {
       max = a;
   } else {
       max = b;
   }

Nested Ternary
~~~~~~~~~~~~~~

.. code-block:: c

   int x = 15;
   char *result = (x > 20) ? "Greater than 20" :
                  (x > 10) ? "Greater than 10" :
                             "10 or less";
   printf("%s\n", result);  // "Greater than 10"

Special Operators
-------------------------

sizeof Operator
~~~~~~~~~~~~~~~

Returns the size in bytes of a variable or data type.

.. code-block:: c

   int x;
   printf("Size of int: %zu bytes\n", sizeof(int));
   printf("Size of x: %zu bytes\n", sizeof(x));
   printf("Size of double: %zu bytes\n", sizeof(double));

Comma Operator
~~~~~~~~~~~~~~

Evaluates multiple expressions from left to right, returns the rightmost value.

.. code-block:: c

   int x, y, z;
   z = (x = 5, y = 10, x + y);  // z = 15
   
   // Common use in for loops
   for (int i = 0, j = 10; i < j; i++, j--) {
       printf("%d %d\n", i, j);
   }

Address-of Operator (&)
~~~~~~~~~~~~~~~~~~~~~~~

Returns the memory address of a variable.

.. code-block:: c

   int x = 10;
   printf("Address of x: %p\n", &x);
   
   int *ptr = &x;  // Store address in pointer

Dereference Operator (*)
~~~~~~~~~~~~~~~~~~~~~~~~~

Accesses the value at the address pointed to by a pointer.

.. code-block:: c

   int x = 10;
   int *ptr = &x;
   printf("Value: %d\n", *ptr);  // 10
   *ptr = 20;                     // Modify x through pointer

Member Access Operators
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   struct Point {
       int x;
       int y;
   };
   
   struct Point p = {10, 20};
   struct Point *ptr = &p;
   
   // Dot operator (.)
   printf("%d\n", p.x);      // 10
   
   // Arrow operator (->)
   printf("%d\n", ptr->x);   // 10
   // Equivalent to: (*ptr).x

Cast Operator
~~~~~~~~~~~~~

Explicitly converts one data type to another.

.. code-block:: c

   int x = 10;
   float y = (float)x;       // Cast int to float
   
   int a = 5, b = 2;
   float result = (float)a / b;  // 2.5

Operator Precedence and Associativity
-------------------------

Operator precedence determines the order in which operators are evaluated.

Precedence Table
~~~~~~~~~~~~~~~~

.. list-table:: Operator Precedence (High to Low)
   :header-rows: 1
   :widths: 10 40 25 25

   * - Level
     - Operators
     - Associativity
     - Description
   * - 1
     - ``() [] -> . ++ --``
     - Left to Right
     - Postfix
   * - 2
     - ``++ -- ! ~ + - * & sizeof (type)``
     - Right to Left
     - Unary
   * - 3
     - ``* / %``
     - Left to Right
     - Multiplicative
   * - 4
     - ``+ -``
     - Left to Right
     - Additive
   * - 5
     - ``<< >>``
     - Left to Right
     - Shift
   * - 6
     - ``< <= > >=``
     - Left to Right
     - Relational
   * - 7
     - ``== !=``
     - Left to Right
     - Equality
   * - 8
     - ``&``
     - Left to Right
     - Bitwise AND
   * - 9
     - ``^``
     - Left to Right
     - Bitwise XOR
   * - 10
     - ``|``
     - Left to Right
     - Bitwise OR
   * - 11
     - ``&&``
     - Left to Right
     - Logical AND
   * - 12
     - ``||``
     - Left to Right
     - Logical OR
   * - 13
     - ``?:``
     - Right to Left
     - Conditional
   * - 14
     - ``= += -= *= /= %= &= ^= |= <<= >>=``
     - Right to Left
     - Assignment
   * - 15
     - ``,``
     - Left to Right
     - Comma

Examples
~~~~~~~~

.. code-block:: c

   int result;
   
   result = 2 + 3 * 4;      // 14 (not 20) - * has higher precedence
   result = (2 + 3) * 4;    // 20 - parentheses override precedence
   
   result = 10 - 5 - 2;     // 3 (left to right: (10-5)-2)
   
   int a = 5, b = 10;
   result = a > 3 && b < 15;  // 1 (relational before logical)

Best Practices
-------------------------

1. **Use parentheses for clarity**

   .. code-block:: c
   
      // Less clear
      int x = a + b * c / d;
      
      // More clear
      int x = a + ((b * c) / d);

2. **Avoid complex expressions**

   .. code-block:: c
   
      // Hard to read
      result = a++ + ++b * c-- / ++d;
      
      // Better
      b++;
      d++;
      result = a + (b * c) / d;
      a++;
      c--;

3. **Be careful with side effects**

   .. code-block:: c
   
      int x = 5;
      // Undefined behavior - modifying x multiple times
      int y = x++ + ++x;  // Don't do this!

4. **Use meaningful variable names with operators**

   .. code-block:: c
   
      // Good
      total_price = base_price + tax;
      
      // Poor
      tp = bp + t;

Common Mistakes
-------------------------

1. **Confusing = and ==**

   .. code-block:: c
   
      if (x = 5) { }   // Wrong - assignment
      if (x == 5) { }  // Correct - comparison

2. **Integer division**

   .. code-block:: c
   
      int result = 5 / 2;        // 2, not 2.5
      float result = 5.0 / 2;    // 2.5

3. **Bitwise vs Logical operators**

   .. code-block:: c
   
      if (a & b) { }   // Bitwise AND (wrong for logic)
      if (a && b) { }  // Logical AND (correct)

4. **Operator precedence**

   .. code-block:: c
   
      if (x & FLAG == 0) { }      // Wrong! == has higher precedence
      if ((x & FLAG) == 0) { }    // Correct

Practice Examples
-------------------------

Example 1: Calculator
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       float num1, num2;
       char op;
       
       printf("Enter operation (e.g., 5 + 3): ");
       scanf("%f %c %f", &num1, &op, &num2);
       
       switch (op) {
           case '+':
               printf("Result: %.2f\n", num1 + num2);
               break;
           case '-':
               printf("Result: %.2f\n", num1 - num2);
               break;
           case '*':
               printf("Result: %.2f\n", num1 * num2);
               break;
           case '/':
               if (num2 != 0)
                   printf("Result: %.2f\n", num1 / num2);
               else
                   printf("Error: Division by zero\n");
               break;
           default:
               printf("Invalid operator\n");
       }
       
       return 0;
   }

Example 2: Bit Manipulation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       unsigned int num = 0;
       
       // Set bit 3
       num |= (1 << 3);
       printf("After setting bit 3: %u\n", num);  // 8
       
       // Set bit 5
       num |= (1 << 5);
       printf("After setting bit 5: %u\n", num);  // 40
       
       // Clear bit 3
       num &= ~(1 << 3);
       printf("After clearing bit 3: %u\n", num); // 32
       
       // Toggle bit 5
       num ^= (1 << 5);
       printf("After toggling bit 5: %u\n", num); // 0
       
       return 0;
   }

Example 3: Finding Min/Max
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       int a = 15, b = 25, c = 10;
       int max, min;
       
       // Find max using ternary operator
       max = (a > b) ? a : b;
       max = (max > c) ? max : c;
       
       // Find min
       min = (a < b) ? a : b;
       min = (min < c) ? min : c;
       
       printf("Numbers: %d, %d, %d\n", a, b, c);
       printf("Max: %d\n", max);
       printf("Min: %d\n", min);
       
       return 0;
   }

Summary
-------------------------

- Arithmetic operators: ``+``, ``-``, ``*``, ``/``, ``%``
- Relational operators: ``==``, ``!=``, ``>``, ``<``, ``>=``, ``<=``
- Logical operators: ``&&``, ``||``, ``!``
- Bitwise operators: ``&``, ``|``, ``^``, ``~``, ``<<``, ``>>``
- Assignment operators: ``=``, ``+=``, ``-=``, ``*=``, etc.
- Increment/Decrement: ``++``, ``--`` (pre and post)
- Conditional: ``?:``
- Special: ``sizeof``, ``,``, ``&``, ``*``, ``->``, ``.``, ``()``
- Understand operator precedence and associativity
- Use parentheses for clarity
- Avoid complex expressions with side effects
