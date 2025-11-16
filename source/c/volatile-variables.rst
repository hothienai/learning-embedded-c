Volatile Variables in C
=========================

Introduction
-------------------------

The ``volatile`` keyword is a type qualifier in C that tells the compiler that a variable's 
value may change at any time without any action being taken by the code the compiler finds 
nearby. This prevents the compiler from applying certain optimizations that might produce 
incorrect behavior when dealing with such variables.

**Key Point:** ``volatile`` is about preventing unwanted compiler optimizations, not about 
thread safety or atomic operations.

What Does volatile Mean?
-------------------------

When you declare a variable as ``volatile``, you're instructing the compiler:

1. **Don't optimize away reads** - Always read the variable from memory, not from a cached register
2. **Don't optimize away writes** - Always write the variable to memory, even if it appears unused
3. **Don't reorder operations** - Maintain the order of volatile accesses as written in code

Without volatile
~~~~~~~~~~~~~~~~

.. code-block:: c

   int flag = 0;
   
   while (flag == 0) {
       // Compiler might optimize this to an infinite loop
       // by reading flag only once
   }

With volatile
~~~~~~~~~~~~~

.. code-block:: c

   volatile int flag = 0;
   
   while (flag == 0) {
       // Compiler must read flag from memory each iteration
   }

Syntax
-------------------------

Basic Declaration
~~~~~~~~~~~~~~~~~

.. code-block:: c

   volatile data_type variable_name;

Examples
~~~~~~~~

.. code-block:: c

   volatile int sensor_value;
   volatile unsigned char status_register;
   volatile float temperature;
   volatile uint32_t timer_counter;

Volatile Pointers
~~~~~~~~~~~~~~~~~

There are three variations when combining ``volatile`` with pointers:

1. **Pointer to volatile data:**

   .. code-block:: c
   
      volatile int *ptr;        // Pointer to volatile int
      // Can change pointer, data is volatile

2. **Volatile pointer to data:**

   .. code-block:: c
   
      int *volatile ptr;        // Volatile pointer to int
      // Cannot move pointer, data is not volatile

3. **Volatile pointer to volatile data:**

   .. code-block:: c
   
      volatile int *volatile ptr;  // Both pointer and data are volatile

When to Use volatile
-------------------------

1. Memory-Mapped I/O Registers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Hardware registers mapped into the address space of the processor.

.. code-block:: c

   // ARM Cortex-M microcontroller example
   #define GPIO_ODR  (*((volatile uint32_t *)0x40020014))
   
   volatile uint32_t *gpio_output = (volatile uint32_t *)0x40020014;
   *gpio_output = 0xFF;         // Set GPIO pins

2. Variables Modified by Interrupt Service Routines (ISR)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Variables shared between main code and ISR must be volatile.

.. code-block:: c

   volatile int interrupt_flag = 0;
   
   void ISR_Handler(void) {
       interrupt_flag = 1;      // Set by interrupt
   }
   
   int main(void) {
       while (interrupt_flag == 0) {
           // Wait for interrupt
       }
       // Process interrupt
       return 0;
   }

3. Multi-threaded Applications
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Variables shared between threads (though atomic operations are usually better).

.. code-block:: c

   volatile int shared_data;
   
   // Thread 1
   void thread1(void) {
       shared_data = 100;
   }
   
   // Thread 2
   void thread2(void) {
       int value = shared_data;
   }

**Note:** For proper thread synchronization, use mutexes, semaphores, or atomic operations.

4. Variables Modified by Hardware
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Variables that represent hardware state or are modified by DMA.

.. code-block:: c

   volatile uint8_t dma_buffer[256];
   volatile uint32_t adc_result;

When NOT to Use volatile
-------------------------

1. **Regular Variables**

   .. code-block:: c
   
      // Don't use volatile unnecessarily
      volatile int counter;     // Wrong if just used in normal code
      int counter;              // Correct

2. **Thread Synchronization**

   .. code-block:: c
   
      // Don't rely on volatile for thread safety
      volatile int shared = 0;  // Not thread-safe!
      
      // Use proper synchronization instead
      pthread_mutex_t mutex;
      int shared = 0;

3. **Atomic Operations**

   .. code-block:: c
   
      // volatile doesn't guarantee atomicity
      volatile long long big_value;  // May not be atomic
      
      // Use C11 atomic types instead
      #include <stdatomic.h>
      atomic_llong big_value;

Embedded Systems Examples
-------------------------

Example 1: GPIO Register Access
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   // STM32 microcontroller GPIO example
   typedef struct {
       volatile uint32_t MODER;    // Mode register
       volatile uint32_t OTYPER;   // Output type register
       volatile uint32_t OSPEEDR;  // Output speed register
       volatile uint32_t PUPDR;    // Pull-up/pull-down register
       volatile uint32_t IDR;      // Input data register
       volatile uint32_t ODR;      // Output data register
       volatile uint32_t BSRR;     // Bit set/reset register
       volatile uint32_t LCKR;     // Lock register
   } GPIO_TypeDef;
   
   #define GPIOA ((GPIO_TypeDef *)0x40020000)
   
   void set_pin_high(void) {
       GPIOA->ODR |= (1 << 5);     // Set bit 5
   }

Example 2: Timer Counter
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   // Hardware timer counter register
   #define TIM_CNT (*((volatile uint32_t *)0x40000024))
   
   void delay_microseconds(uint32_t us) {
       volatile uint32_t start = TIM_CNT;
       while ((TIM_CNT - start) < us) {
           // Wait - must read TIM_CNT each iteration
       }
   }

Example 3: Interrupt Flag
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   volatile uint8_t uart_rx_ready = 0;
   volatile uint8_t uart_rx_data;
   
   // Interrupt Service Routine
   void UART_IRQHandler(void) {
       if (UART_Status & RX_COMPLETE) {
           uart_rx_data = UART_Data;
           uart_rx_ready = 1;
       }
   }
   
   // Main code
   int main(void) {
       while (1) {
           if (uart_rx_ready) {
               process_data(uart_rx_data);
               uart_rx_ready = 0;
           }
       }
   }

Example 4: Status Register Polling
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #define STATUS_REG (*((volatile uint8_t *)0x5000))
   #define DATA_READY (1 << 0)
   
   uint8_t wait_for_data(void) {
       // Poll status register
       while (!(STATUS_REG & DATA_READY)) {
           // Compiler must read STATUS_REG each time
       }
       return read_data();
   }

Example 5: Watchdog Timer
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #define WATCHDOG_KICK (*((volatile uint32_t *)0x40003000))
   
   void feed_watchdog(void) {
       WATCHDOG_KICK = 0xCAFE;     // Special value to reset watchdog
   }
   
   int main(void) {
       while (1) {
           do_work();
           feed_watchdog();         // Must actually write each time
       }
   }

volatile with Other Qualifiers
-------------------------------

volatile and const
~~~~~~~~~~~~~~~~~~

A variable can be both ``const`` and ``volatile``.

.. code-block:: c

   // Read-only hardware register that may change
   const volatile uint32_t status_register;
   
   // Can read but cannot write
   uint32_t value = status_register;
   // status_register = 0;      // Error! const prevents writing

volatile and static
~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   static volatile int module_flag;  // File-scope volatile variable

volatile in Function Parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   void process_volatile_data(volatile int *data) {
       int local_copy = *data;   // Read from volatile source
       // Work with local_copy (non-volatile)
       *data = local_copy;       // Write back to volatile
   }

Common Mistakes and Pitfalls
-----------------------------

Mistake 1: Using volatile for Thread Safety
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   // WRONG - volatile doesn't make this thread-safe
   volatile int counter = 0;
   
   void increment(void) {
       counter++;                // Not atomic! Read-modify-write
   }
   
   // CORRECT - use atomic or mutex
   #include <stdatomic.h>
   atomic_int counter = 0;
   
   void increment(void) {
       atomic_fetch_add(&counter, 1);
   }

Mistake 2: Thinking volatile Guarantees Atomicity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   // WRONG - may not be atomic on all platforms
   volatile long long big_value;
   big_value = 0x123456789ABCDEF0LL;  // May be two 32-bit writes
   
   // CORRECT - use appropriate atomic type
   atomic_llong big_value;

Mistake 3: Forgetting volatile for ISR Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   // WRONG - compiler may optimize away the loop
   int flag = 0;
   
   void ISR(void) {
       flag = 1;
   }
   
   void wait(void) {
       while (flag == 0) { }    // May become infinite loop
   }
   
   // CORRECT
   volatile int flag = 0;

Mistake 4: Over-using volatile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   // WRONG - unnecessary volatile
   volatile int temp;
   temp = calculate_value();
   process(temp);
   
   // CORRECT - use volatile only when needed
   int temp = calculate_value();
   process(temp);

Mistake 5: Copying volatile Variables Incorrectly
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   // WRONG - loses volatile qualifier
   volatile uint32_t *hw_reg = (volatile uint32_t *)0x40000000;
   uint32_t *ptr = hw_reg;      // Warning: discards volatile
   
   // CORRECT - preserve volatile
   volatile uint32_t *hw_reg = (volatile uint32_t *)0x40000000;
   volatile uint32_t *ptr = hw_reg;

Compiler Optimization and volatile
-----------------------------------

Without volatile - Compiler Optimizations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   int x = 0;
   x = 10;
   x = 20;
   // Compiler may optimize to just: x = 20;
   
   int y = 0;
   int z = y + y;
   // Compiler may cache y's value in a register

With volatile - No Unwanted Optimizations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   volatile int x = 0;
   x = 10;                      // Must write 10
   x = 20;                      // Must write 20
   // Both writes happen
   
   volatile int y = 0;
   int z = y + y;
   // y is read from memory twice

Memory Barriers and volatile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Note:** ``volatile`` provides a weak form of memory ordering but doesn't provide 
full memory barriers on all architectures.

.. code-block:: c

   // For strict memory ordering, use compiler barriers
   #define barrier() __asm__ __volatile__("": : :"memory")
   
   volatile int flag = 0;
   int data = 0;
   
   void writer(void) {
       data = 42;
       barrier();                // Ensure data is written before flag
       flag = 1;
   }

Best Practices
-------------------------

1. **Use volatile for hardware registers**

   .. code-block:: c
   
      #define UART_DR (*((volatile uint32_t *)0x40013804))

2. **Use volatile for ISR-shared variables**

   .. code-block:: c
   
      volatile uint8_t irq_count = 0;

3. **Don't use volatile for thread synchronization**

   .. code-block:: c
   
      // Use mutexes, atomics, or proper synchronization primitives

4. **Keep volatile scope minimal**

   .. code-block:: c
   
      volatile int sensor_value = read_sensor();
      int local_copy = sensor_value;  // Work with non-volatile copy
      process(local_copy);

5. **Document why volatile is used**

   .. code-block:: c
   
      // Shared with TIMER_IRQ - must be volatile
      volatile uint32_t millisecond_counter;

6. **Use fixed-width types with volatile**

   .. code-block:: c
   
      #include <stdint.h>
      volatile uint32_t hardware_reg;  // Size is explicit

Practice Examples
-------------------------

Example 1: Simple ISR Communication
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdint.h>
   
   volatile uint8_t button_pressed = 0;
   
   void BUTTON_IRQHandler(void) {
       button_pressed = 1;
   }
   
   int main(void) {
       enable_button_interrupt();
       
       while (1) {
           if (button_pressed) {
               printf("Button was pressed!\n");
               button_pressed = 0;
           }
       }
       return 0;
   }

Example 2: Memory-Mapped Register
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdint.h>
   
   #define LED_REG (*((volatile uint32_t *)0x40020014))
   
   void toggle_led(void) {
       static uint32_t state = 0;
       state ^= 1;
       LED_REG = state;         // Write to hardware register
   }

Example 3: Ring Buffer with ISR
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdint.h>
   
   #define BUFFER_SIZE 64
   
   volatile uint8_t rx_buffer[BUFFER_SIZE];
   volatile uint32_t rx_head = 0;
   volatile uint32_t rx_tail = 0;
   
   // Called by interrupt
   void UART_RX_IRQHandler(void) {
       uint8_t data = UART_DATA_REG;
       uint32_t next = (rx_head + 1) % BUFFER_SIZE;
       
       if (next != rx_tail) {    // Not full
           rx_buffer[rx_head] = data;
           rx_head = next;
       }
   }
   
   // Called by main code
   int get_byte(uint8_t *data) {
       if (rx_head == rx_tail) {
           return 0;             // Empty
       }
       
       *data = rx_buffer[rx_tail];
       rx_tail = (rx_tail + 1) % BUFFER_SIZE;
       return 1;
   }

Example 4: Timeout with Hardware Timer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: c

   #include <stdint.h>
   
   #define TIMER_COUNT (*((volatile uint32_t *)0x40000024))
   
   int wait_with_timeout(uint32_t timeout_ms) {
       volatile uint32_t start = TIMER_COUNT;
       
       while (1) {
           if (is_data_ready()) {
               return 1;         // Success
           }
           
           if ((TIMER_COUNT - start) > timeout_ms) {
               return 0;         // Timeout
           }
       }
   }

Summary
-------------------------

- ``volatile`` tells the compiler not to optimize away memory accesses
- Use for hardware registers, ISR-shared variables, and hardware-modified data
- Does NOT provide thread safety or atomic operations
- Does NOT guarantee memory barriers on all platforms
- Essential in embedded systems programming
- Don't overuse - only apply where actually needed
- Combine with other qualifiers (const, static) as appropriate
- Always document why a variable is volatile
- Use fixed-width types (uint32_t, etc.) for clarity
- Consider using proper synchronization primitives for multi-threading
