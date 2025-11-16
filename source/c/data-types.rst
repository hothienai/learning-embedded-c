Data Types in C
=========================

Introduction
-------------------------

Data types are fundamental building blocks in C programming that define the type of data 
a variable can hold and the operations that can be performed on it. Understanding data types 
is crucial for efficient memory usage, proper data manipulation, and avoiding common programming errors.

In C, every variable must be declared with a specific data type before it can be used. The data type 
determines:

- The amount of memory allocated for the variable
- The range of values it can store
- The operations that can be performed on it
- How the data is interpreted by the compiler

Basic Data Types
-------------------------

C provides several basic (primitive) data types that can be categorized into:

1. Integer types
2. Floating-point types
3. Character type
4. Void type

Integer Data Types
-------------------------

Integer types store whole numbers (positive, negative, or zero) without decimal points.

char
~~~~~

The ``char`` type is primarily used to store single characters but is technically an integer type.

**Size:** 1 byte (8 bits)

**Range:** 
- Signed: -128 to 127
- Unsigned: 0 to 255

.. code-block:: c

   char letter = 'A';           // Character literal
   char digit = '9';            // Character digit
   signed char temp = -50;      // Explicitly signed
   unsigned char byte = 200;    // Unsigned char

**Key Points:**

- Characters are stored as ASCII values
- Can perform arithmetic operations on char
- ``'A'`` has ASCII value 65, ``'a'`` has value 97

int
~~~~

The ``int`` type is the most commonly used integer type.

**Size:** Typically 4 bytes (32 bits) on modern systems, but can be 2 bytes on some platforms

**Range (32-bit):**
- Signed: -2,147,483,648 to 2,147,483,647
- Unsigned: 0 to 4,294,967,295

.. code-block:: c

   int age = 25;
   int temperature = -10;
   unsigned int count = 1000;
   
   // Using integer suffixes
   int large = 100000;
   unsigned int uid = 50000U;  // U suffix for unsigned

**Key Points:**

- Default integer type when no type is specified
- Signed by default (can be negative)
- Size depends on the platform (16-bit, 32-bit, or 64-bit)

short
~~~~~

The ``short`` type uses less memory than ``int``.

**Size:** 2 bytes (16 bits)

**Range:**
- Signed: -32,768 to 32,767
- Unsigned: 0 to 65,535

.. code-block:: c

   short s = 1000;
   short int si = 2000;        // 'int' is optional
   unsigned short us = 50000;

**Key Points:**

- More memory-efficient for small values
- Keyword ``int`` can be omitted (``short`` is equivalent to ``short int``)

long
~~~~

The ``long`` type provides extended range for larger values.

**Size:** 
- Typically 4 bytes on 32-bit systems
- 8 bytes on 64-bit systems (on many platforms)

**Range (32-bit):**
- Signed: -2,147,483,648 to 2,147,483,647
- Unsigned: 0 to 4,294,967,295

.. code-block:: c

   long population = 7800000000L;  // L suffix for long
   long int distance = 384400L;
   unsigned long ul = 4000000000UL;

long long
~~~~~~~~~

The ``long long`` type (introduced in C99) provides even larger range.

**Size:** 8 bytes (64 bits)

**Range:**
- Signed: -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807
- Unsigned: 0 to 18,446,744,073,709,551,615

.. code-block:: c

   long long bignum = 9000000000000LL;  // LL suffix
   unsigned long long huge = 18000000000000000ULL;

Floating-Point Data Types
-------------------------

Floating-point types store numbers with decimal points and exponential values.

float
~~~~~

Single-precision floating-point type.

**Size:** 4 bytes (32 bits)

**Precision:** ~6-7 decimal digits

**Range:** Approximately ±3.4E-38 to ±3.4E+38

.. code-block:: c

   float pi = 3.14159f;         // f suffix for float
   float temp = 36.6f;
   float scientific = 1.23e-4f; // Scientific notation

**Key Points:**

- Use ``f`` or ``F`` suffix to specify float literals
- Without suffix, decimal literals are treated as ``double``
- Less precise but uses less memory than ``double``

double
~~~~~~

Double-precision floating-point type.

**Size:** 8 bytes (64 bits)

**Precision:** ~15-16 decimal digits

**Range:** Approximately ±1.7E-308 to ±1.7E+308

.. code-block:: c

   double precise_pi = 3.141592653589793;
   double distance = 384400.0;
   double small = 2.998e8;      // Speed of light in m/s

**Key Points:**

- Default type for floating-point literals
- More precise than ``float`` but uses more memory
- Preferred for scientific calculations

long double
~~~~~~~~~~~

Extended-precision floating-point type.

**Size:** Platform-dependent (typically 10, 12, or 16 bytes)

**Precision:** Platform-dependent (typically 18-20 decimal digits)

.. code-block:: c

   long double very_precise = 3.14159265358979323846L;

**Key Points:**

- Provides highest precision
- Implementation varies by compiler and platform
- Use ``L`` suffix for literals

Void Type
-------------------------

The ``void`` type represents the absence of a type.

**Common Uses:**

1. **Function return type** - indicates function returns no value
2. **Function parameters** - indicates function takes no parameters
3. **Generic pointers** - ``void*`` can point to any data type

.. code-block:: c

   void printMessage(void) {
       printf("Hello, World!\n");
       // No return statement needed
   }
   
   void* generic_pointer;  // Can point to any type

Type Modifiers
-------------------------

Type modifiers alter the properties of basic data types:

signed
~~~~~~

Explicitly specifies that a type can hold negative values (default for ``int``).

.. code-block:: c

   signed int value = -100;
   signed char sc = -50;

unsigned
~~~~~~~~

Specifies that a type can only hold non-negative values, effectively doubling the positive range.

.. code-block:: c

   unsigned int count = 4000000000U;
   unsigned char byte = 255;

short and long
~~~~~~~~~~~~~~

Modify the size and range of integer types.

.. code-block:: c

   short int small = 100;
   long int large = 1000000L;
   long long int huge = 9000000000000LL;

Size of Data Types
-------------------------

The ``sizeof`` operator returns the size in bytes of a data type or variable.

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       printf("Size of char: %zu bytes\n", sizeof(char));
       printf("Size of int: %zu bytes\n", sizeof(int));
       printf("Size of short: %zu bytes\n", sizeof(short));
       printf("Size of long: %zu bytes\n", sizeof(long));
       printf("Size of long long: %zu bytes\n", sizeof(long long));
       printf("Size of float: %zu bytes\n", sizeof(float));
       printf("Size of double: %zu bytes\n", sizeof(double));
       printf("Size of long double: %zu bytes\n", sizeof(long double));
       
       return 0;
   }

**Output (typical on 64-bit system):**

.. code-block:: text

   Size of char: 1 bytes
   Size of int: 4 bytes
   Size of short: 2 bytes
   Size of long: 8 bytes
   Size of long long: 8 bytes
   Size of float: 4 bytes
   Size of double: 8 bytes
   Size of long double: 16 bytes

Type Ranges Summary
-------------------------

.. list-table:: Integer Type Ranges
   :header-rows: 1
   :widths: 25 15 30 30

   * - Type
     - Size
     - Signed Range
     - Unsigned Range
   * - char
     - 1 byte
     - -128 to 127
     - 0 to 255
   * - short
     - 2 bytes
     - -32,768 to 32,767
     - 0 to 65,535
   * - int
     - 4 bytes
     - -2,147,483,648 to 2,147,483,647
     - 0 to 4,294,967,295
   * - long
     - 4/8 bytes
     - Platform-dependent
     - Platform-dependent
   * - long long
     - 8 bytes
     - -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807
     - 0 to 18,446,744,073,709,551,615

Type Conversion
-------------------------

Implicit Conversion (Type Coercion)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

C automatically converts between compatible types in certain situations.

.. code-block:: c

   int i = 10;
   float f = i;        // int to float (implicit)
   
   float x = 3.14f;
   int y = x;          // float to int (loses decimal part)
   
   char c = 'A';
   int ascii = c;      // char to int

**Rules:**

- Smaller types promoted to larger types
- ``float`` promoted to ``double`` in expressions
- Integer types promoted to ``float``/``double`` when mixed
- Decimal part truncated when converting float to int

Explicit Conversion (Type Casting)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use cast operator to explicitly convert types.

.. code-block:: c

   float pi = 3.14159f;
   int truncated = (int)pi;        // 3
   
   int a = 5, b = 2;
   float result = (float)a / b;    // 2.5, not 2
   
   double d = 3.99;
   int i = (int)d;                 // 3

Best Practices
-------------------------

1. **Choose appropriate type for your data**

   .. code-block:: c
   
      unsigned int age;           // Age is never negative
      float temperature;          // Decimal values needed
      char grade;                 // Single character

2. **Use fixed-width types for portability** (from ``<stdint.h>``)

   .. code-block:: c
   
      #include <stdint.h>
      
      uint8_t  byte;             // Exactly 8 bits
      int16_t  word;             // Exactly 16 bits
      uint32_t dword;            // Exactly 32 bits
      int64_t  qword;            // Exactly 64 bits

3. **Be aware of type limits**

   .. code-block:: c
   
      #include <limits.h>
      
      printf("INT_MAX: %d\n", INT_MAX);
      printf("INT_MIN: %d\n", INT_MIN);
      printf("UINT_MAX: %u\n", UINT_MAX);

4. **Avoid implicit conversions that lose precision**

   .. code-block:: c
   
      double precise = 3.14159265358979;
      float less_precise = precise;     // Loses precision
      
      long long big = 9000000000LL;
      int small = big;                  // Potential data loss

5. **Use sizeof for portable code**

   .. code-block:: c
   
      // Don't assume int is 4 bytes
      int arr[100];
      size_t arr_size = sizeof(arr) / sizeof(arr[0]);

Common Pitfalls
-------------------------

1. **Integer Overflow**

   .. code-block:: c
   
      unsigned char c = 255;
      c = c + 1;                 // Wraps to 0

2. **Unexpected Type Promotion**

   .. code-block:: c
   
      unsigned char a = 255;
      unsigned char b = 1;
      int result = a + b;        // 256, promoted to int

3. **Division Truncation**

   .. code-block:: c
   
      int a = 5, b = 2;
      int result = a / b;        // 2, not 2.5
      float correct = (float)a / b;  // 2.5

4. **Comparing Signed and Unsigned**

   .. code-block:: c
   
      int signed_val = -1;
      unsigned int unsigned_val = 1;
      if (signed_val < unsigned_val)  // May not work as expected!
          printf("Unexpected behavior\n");

5. **Float Precision Issues**

   .. code-block:: c
   
      float f1 = 0.1f;
      float f2 = 0.2f;
      float sum = f1 + f2;
      if (sum == 0.3f)           // May be false due to precision!
          printf("Equal\n");

Practice Examples
-------------------------

Example 1: Variable Declaration and Initialization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       // Integer types
       char letter = 'X';
       short small_num = 1000;
       int count = 42;
       long population = 7800000000L;
       long long huge = 9223372036854775807LL;
       
       // Floating-point types
       float temperature = 36.5f;
       double precise_value = 3.141592653589793;
       
       // Display values
       printf("Character: %c\n", letter);
       printf("Small number: %hd\n", small_num);
       printf("Count: %d\n", count);
       printf("Population: %ld\n", population);
       printf("Huge: %lld\n", huge);
       printf("Temperature: %.1f\n", temperature);
       printf("Precise: %.15f\n", precise_value);
       
       return 0;
   }

Example 2: Type Conversion
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int main() {
       int num = 5;
       float result;
       
       // Integer division
       result = num / 2;          // 2.0 (truncated)
       printf("Integer division: %.1f\n", result);
       
       // Float division
       result = (float)num / 2;   // 2.5
       printf("Float division: %.1f\n", result);
       
       // Character to integer
       char ch = 'A';
       int ascii = ch;
       printf("ASCII of %c: %d\n", ch, ascii);
       
       return 0;
   }

Example 3: Checking Type Sizes and Limits
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   #include <limits.h>
   #include <float.h>
   
   int main() {
       printf("=== Integer Limits ===\n");
       printf("CHAR_MIN: %d, CHAR_MAX: %d\n", CHAR_MIN, CHAR_MAX);
       printf("SHRT_MIN: %d, SHRT_MAX: %d\n", SHRT_MIN, SHRT_MAX);
       printf("INT_MIN: %d, INT_MAX: %d\n", INT_MIN, INT_MAX);
       printf("LONG_MIN: %ld, LONG_MAX: %ld\n", LONG_MIN, LONG_MAX);
       
       printf("\n=== Unsigned Limits ===\n");
       printf("UCHAR_MAX: %u\n", UCHAR_MAX);
       printf("USHRT_MAX: %u\n", USHRT_MAX);
       printf("UINT_MAX: %u\n", UINT_MAX);
       printf("ULONG_MAX: %lu\n", ULONG_MAX);
       
       printf("\n=== Float Limits ===\n");
       printf("FLT_MIN: %e, FLT_MAX: %e\n", FLT_MIN, FLT_MAX);
       printf("DBL_MIN: %e, DBL_MAX: %e\n", DBL_MIN, DBL_MAX);
       
       return 0;
   }

Summary
-------------------------

- C provides several basic data types: integers, floating-point, character, and void
- Integer types: ``char``, ``short``, ``int``, ``long``, ``long long``
- Floating-point types: ``float``, ``double``, ``long double``
- Type modifiers: ``signed``, ``unsigned``, ``short``, ``long``
- Use ``sizeof`` to determine type sizes
- Be careful with type conversions to avoid data loss
- Consider using fixed-width types (``int32_t``, etc.) for portability
- Always be aware of type ranges and limits to avoid overflow/underflow
