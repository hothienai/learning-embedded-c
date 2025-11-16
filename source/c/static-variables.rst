Static Variables in C
=========================

Introduction
-------------------------

The ``static`` keyword in C is a storage class specifier that fundamentally changes how variables 
behave regarding their scope, lifetime, and visibility. Unlike regular variables, static variables 
persist throughout the entire program execution and are initialized only once.

**Key characteristics of static variables:**
- Initialized only once (at program start or first function call)
- Retain their value between function calls
- Have a lifetime equal to the program's lifetime
- Can limit scope to file or function level

What is a Static Variable?
--------------------------

A static variable is a variable that:

1. **Preserves its value** between function calls (for local static)
2. **Exists for the entire program duration** (not destroyed when out of scope)
3. **Is initialized only once** (not re-initialized on subsequent calls)
4. **Can have file-level scope** (when declared outside functions)

Types of Static Variables
-------------------------

There are two main types of static variables in C:

1. **Local Static Variables** - Declared inside functions
2. **Global Static Variables** - Declared outside all functions

Local Static Variables
-------------------------

Local static variables are declared inside a function with the ``static`` keyword.

Syntax
~~~~~~

.. code-block:: c

   void function_name() {
       static data_type variable_name = initial_value;
   }

Basic Example
~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   void counter() {
       static int count = 0;    // Initialized only once
       count++;
       printf("Count: %d\n", count);
   }
   
   int main() {
       counter();               // Prints: Count: 1
       counter();               // Prints: Count: 2
       counter();               // Prints: Count: 3
       return 0;
   }

**What happens:**
- First call: ``count`` is initialized to 0, incremented to 1
- Second call: ``count`` retains value 1, incremented to 2
- Third call: ``count`` retains value 2, incremented to 3

Characteristics
~~~~~~~~~~~~~~~

1. **Initialization happens only once**

   .. code-block:: c
   
      void test() {
          static int x = 5;     // Initialized only on first call
          x++;
          printf("%d ", x);
      }
      
      int main() {
          test();               // Prints: 6
          test();               // Prints: 7
          test();               // Prints: 8
          return 0;
      }

2. **Scope is limited to the function**

   .. code-block:: c
   
      void function1() {
          static int value = 10;
          // value is accessible here
      }
      
      int main() {
          // value is NOT accessible here
          return 0;
      }

3. **Lifetime is the entire program**

   .. code-block:: c
   
      void process() {
          static int data = 100;
      }
      // data continues to exist even after function returns
      // (but cannot be accessed from outside)

Default Initialization
~~~~~~~~~~~~~~~~~~~~~~

Static variables are automatically initialized to 0 if not explicitly initialized.

.. code-block:: c

   void test() {
       static int x;            // Automatically initialized to 0
       static float f;          // Automatically initialized to 0.0
       static char *ptr;        // Automatically initialized to NULL
       
       printf("x = %d, f = %.1f\n", x, f);
   }

Global Static Variables
-------------------------

Global static variables are declared outside all functions with the ``static`` keyword.

Syntax
~~~~~~

.. code-block:: c

   static data_type variable_name = initial_value;

Purpose
~~~~~~~

Global static variables limit the visibility of a variable to the file in which it is declared. 
This is also called **file scope** or **internal linkage**.

Example
~~~~~~~

**file1.c:**

.. code-block:: c

   #include <stdio.h>
   
   static int file_counter = 0;  // Only accessible in file1.c
   
   void increment_file1() {
       file_counter++;
       printf("File1 counter: %d\n", file_counter);
   }

**file2.c:**

.. code-block:: c

   #include <stdio.h>
   
   static int file_counter = 0;  // Different variable, only in file2.c
   
   void increment_file2() {
       file_counter++;
       printf("File2 counter: %d\n", file_counter);
   }

Both files can have a variable with the same name without conflict because the ``static`` 
keyword limits their scope to their respective files.

Benefits
~~~~~~~~

1. **Prevents naming conflicts** between different source files
2. **Encapsulation** - hides implementation details
3. **Reduces pollution** of the global namespace

Comparison with Non-Static
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   // Without static (external linkage)
   int global_var = 100;        // Accessible from other files via extern
   
   // With static (internal linkage)
   static int file_var = 100;   // Only accessible in this file

Use Cases for Static Variables
------------------------------

1. Function Call Counters
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   void log_message(const char *msg) {
       static int call_count = 0;
       call_count++;
       printf("[%d] %s\n", call_count, msg);
   }
   
   int main() {
       log_message("First message");    // [1] First message
       log_message("Second message");   // [2] Second message
       log_message("Third message");    // [3] Third message
       return 0;
   }

2. Unique ID Generation
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   int generate_id() {
       static int next_id = 1000;
       return next_id++;
   }
   
   int main() {
       int id1 = generate_id();  // 1000
       int id2 = generate_id();  // 1001
       int id3 = generate_id();  // 1002
       return 0;
   }

3. One-Time Initialization
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   void initialize_once() {
       static int initialized = 0;
       
       if (!initialized) {
           printf("Performing initialization...\n");
           // Do expensive initialization
           initialized = 1;
       }
       printf("Function running...\n");
   }
   
   int main() {
       initialize_once();  // Performs initialization
       initialize_once();  // Skips initialization
       initialize_once();  // Skips initialization
       return 0;
   }

4. State Machines
~~~~~~~~~~~~~~~~~

.. code-block:: c

   typedef enum {
       STATE_IDLE,
       STATE_RUNNING,
       STATE_PAUSED,
       STATE_STOPPED
   } State;
   
   void state_machine(int event) {
       static State current_state = STATE_IDLE;
       
       switch (current_state) {
           case STATE_IDLE:
               if (event == START) {
                   current_state = STATE_RUNNING;
               }
               break;
           case STATE_RUNNING:
               if (event == PAUSE) {
                   current_state = STATE_PAUSED;
               }
               break;
           // ... more states
       }
   }

5. Caching/Memoization
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   int fibonacci(int n) {
       static int cache[100] = {0};
       
       if (n <= 1) return n;
       if (cache[n] != 0) return cache[n];  // Return cached value
       
       cache[n] = fibonacci(n-1) + fibonacci(n-2);
       return cache[n];
   }

6. Singleton Pattern
~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   typedef struct {
       int data;
       char name[50];
   } Config;
   
   Config* get_config() {
       static Config instance = {0, "Default"};
       return &instance;
   }
   
   int main() {
       Config *cfg1 = get_config();
       Config *cfg2 = get_config();
       // cfg1 and cfg2 point to the same static instance
       return 0;
   }

Static vs Regular Variables
---------------------------

Comparison Table
~~~~~~~~~~~~~~~~

.. list-table:: Static vs Regular Local Variables
   :header-rows: 1
   :widths: 30 35 35

   * - Feature
     - Regular Variable
     - Static Variable
   * - Initialization
     - Every function call
     - Only once (first call)
   * - Lifetime
     - Function scope
     - Entire program
   * - Scope
     - Function only
     - Function only
   * - Default value
     - Undefined (garbage)
     - Zero
   * - Memory location
     - Stack
     - Data segment
   * - Destroyed when
     - Function returns
     - Program ends

Code Example
~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   void compare_variables() {
       int regular = 0;         // Re-initialized each call
       static int static_var = 0;  // Initialized only once
       
       regular++;
       static_var++;
       
       printf("Regular: %d, Static: %d\n", regular, static_var);
   }
   
   int main() {
       compare_variables();     // Regular: 1, Static: 1
       compare_variables();     // Regular: 1, Static: 2
       compare_variables();     // Regular: 1, Static: 3
       return 0;
   }

Static Functions
-------------------------

The ``static`` keyword can also be applied to functions to limit their visibility to the 
file in which they are defined.

Syntax
~~~~~~

.. code-block:: c

   static return_type function_name(parameters) {
       // function body
   }

Example
~~~~~~~

**file1.c:**

.. code-block:: c

   #include <stdio.h>
   
   static void helper() {       // Only visible in file1.c
       printf("Helper function\n");
   }
   
   void public_function() {     // Visible to other files
       helper();
   }

**file2.c:**

.. code-block:: c

   // void helper();            // Error! Cannot access static function
   void public_function();      // OK - can access non-static function

Benefits
~~~~~~~~

1. **Encapsulation** - Hide implementation details
2. **Prevents name conflicts** - Different files can have functions with the same name
3. **Optimization** - Compiler can better optimize file-local functions

Memory Layout
-------------------------

Static variables are stored in the **data segment** of the program, not on the stack.

Memory Segments
~~~~~~~~~~~~~~~

.. code-block:: text

   ┌─────────────────┐
   │     Stack       │  ← Regular local variables
   ├─────────────────┤
   │     Heap        │  ← Dynamic allocation (malloc)
   ├─────────────────┤
   │  BSS Segment    │  ← Uninitialized static/global variables
   ├─────────────────┤
   │  Data Segment   │  ← Initialized static/global variables
   ├─────────────────┤
   │  Code Segment   │  ← Program code
   └─────────────────┘

Example
~~~~~~~

.. code-block:: c

   int global = 10;             // Data segment
   static int file_static = 20; // Data segment
   
   void function() {
       int local = 30;          // Stack
       static int func_static = 40;  // Data segment
   }

Best Practices
-------------------------

1. **Use static for file-scope variables**

   .. code-block:: c
   
      // In .c file
      static int module_counter = 0;  // Hide from other files

2. **Use static for helper functions**

   .. code-block:: c
   
      static int calculate_helper(int x) {
          return x * 2;
      }

3. **Initialize static variables explicitly**

   .. code-block:: c
   
      static int count = 0;     // Explicit (clearer intent)
      // vs
      static int count;         // Implicit zero initialization

4. **Use static for one-time initialization flags**

   .. code-block:: c
   
      void init_module() {
          static int initialized = 0;
          if (!initialized) {
              // Initialize once
              initialized = 1;
          }
      }

5. **Minimize use of static variables (when possible)**

   Static variables can make code harder to test and debug. Consider:
   - Passing parameters instead
   - Using structures to hold state
   - Limiting static usage to truly necessary cases

Common Pitfalls
-------------------------

1. Thread Safety Issues
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   // NOT thread-safe!
   void counter() {
       static int count = 0;
       count++;                 // Race condition in multi-threaded code
   }
   
   // Solution: Use mutexes or atomic operations

2. Unexpected Persistence
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   void process_data(int reset) {
       static int sum = 0;
       if (reset) {
           sum = 0;             // Manual reset needed
       }
       sum += 10;
   }

3. Initialization Order
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   static int a = 10;
   static int b = a + 5;        // OK in C, but avoid complex initialization

4. Cannot Take Address in Initializer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   void test() {
       static int x = 10;
       // static int *ptr = &x;  // Error! Cannot initialize with non-constant
   }

Practice Examples
-------------------------

Example 1: Function Call Counter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   void print_hello() {
       static int times_called = 0;
       times_called++;
       printf("Hello! (Called %d times)\n", times_called);
   }
   
   int main() {
       print_hello();           // Hello! (Called 1 times)
       print_hello();           // Hello! (Called 2 times)
       print_hello();           // Hello! (Called 3 times)
       return 0;
   }

Example 2: ID Generator
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   int get_unique_id() {
       static int id = 0;
       return ++id;
   }
   
   int main() {
       printf("ID: %d\n", get_unique_id());  // ID: 1
       printf("ID: %d\n", get_unique_id());  // ID: 2
       printf("ID: %d\n", get_unique_id());  // ID: 3
       return 0;
   }

Example 3: Average Calculator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   float running_average(float new_value) {
       static float sum = 0.0;
       static int count = 0;
       
       sum += new_value;
       count++;
       
       return sum / count;
   }
   
   int main() {
       printf("Average: %.2f\n", running_average(10.0));  // 10.00
       printf("Average: %.2f\n", running_average(20.0));  // 15.00
       printf("Average: %.2f\n", running_average(30.0));  // 20.00
       return 0;
   }

Example 4: Toggle Function
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   void toggle_led() {
       static int state = 0;
       state = !state;
       printf("LED is %s\n", state ? "ON" : "OFF");
   }
   
   int main() {
       toggle_led();            // LED is ON
       toggle_led();            // LED is OFF
       toggle_led();            // LED is ON
       toggle_led();            // LED is OFF
       return 0;
   }

Example 5: File-Scope Static
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   static int module_data = 100;  // File scope only
   
   static void private_function() {
       printf("Private: %d\n", module_data);
   }
   
   void public_function() {
       private_function();
       module_data++;
   }
   
   int main() {
       public_function();       // Private: 100
       public_function();       // Private: 101
       // private_function();   // OK - same file
       return 0;
   }

Example 6: State Machine
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdio.h>
   
   typedef enum { OFF, ON, BLINKING } LedState;
   
   void led_state_machine(int button_press) {
       static LedState state = OFF;
       
       if (button_press) {
           switch (state) {
               case OFF:
                   state = ON;
                   printf("LED: ON\n");
                   break;
               case ON:
                   state = BLINKING;
                   printf("LED: BLINKING\n");
                   break;
               case BLINKING:
                   state = OFF;
                   printf("LED: OFF\n");
                   break;
           }
       }
   }
   
   int main() {
       led_state_machine(1);    // LED: ON
       led_state_machine(1);    // LED: BLINKING
       led_state_machine(1);    // LED: OFF
       led_state_machine(1);    // LED: ON
       return 0;
   }

Summary
-------------------------

- ``static`` keyword changes variable scope, lifetime, and visibility
- Local static variables retain their value between function calls
- Static variables are initialized only once
- Default initialization is zero (not garbage)
- Global static variables limit visibility to the current file
- Static variables exist in data segment, not on stack
- Useful for counters, state machines, caching, and singletons
- Can make testing harder, so use judiciously
- Not thread-safe by default - requires synchronization
- Static functions provide file-scope encapsulation
