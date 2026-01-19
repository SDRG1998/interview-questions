## **1. What is pointer, What happens when you dereference a null pointer?**

-A pointer in C is a variable that stores the memory address of another variable. 

-Instead of holding a value like 10 or 3.14, a pointer holds an address where a value is stored in memory.

    int x = 10;
    int *p = &x;

Dereferencing a pointer:

-Dereferencing means accessing the value stored at the memory address the pointer points to.

-The dereference operator is *.

    printf("%d", *p);  // prints 10

NULL pointer: 

-A NULL pointer is a pointer that does not point to any valid memory location.

    int *p = NULL;

-NULL means “points to nothing

-Dereferencing a NULL pointer is undefined behavior

-It usually causes a program crash (segmentation fault)

    *p = 5;  // ❌ ERROR: dereferencing NULL pointer

correct Statement

-A pointer in C is a variable that stores the address of another variable.

-A NULL pointer cannot be dereferenced because it does not point to any valid memory location, and dereferencing it may cause a program crash.


## __2. Difference between const int *ptr, int *const ptr, const int*const ptr?__

| Declaration                             | Meaning                                      | Can change value (`*ptr`)? | Can change pointer (`ptr`)? | Explanation                                                                                  |
| --------------------------------------- | -------------------------------------------- | -------------------------- | --------------------------- | -------------------------------------------------------------------------------------------- |
| `const int *ptr`<br>or `int const *ptr` | Pointer to **constant integer**              | ❌ No                       | ✅ Yes                       | The data pointed to is read-only through `ptr`, but the pointer can point to another address |
| `int *const ptr`                        | **Constant pointer** to integer              | ✅ Yes                      | ❌ No                        | The pointer address is fixed, but the value at that address can be modified                  |
| `const int *const ptr`                  | **Constant pointer** to **constant integer** | ❌ No                       | ❌ No                        | Neither the pointer nor the value it points to can be changed                                |

Example:

int x = 10;
int y = 20;

1️⃣ const int *ptr

    const int *ptr = &x;
    
    *ptr = 15;   // ❌ ERROR (value is const)
    ptr = &y;    // ✅ OK

2️⃣ int *const ptr

    int *const ptr = &x;
    
    *ptr = 15;   // ✅ OK
    ptr = &y;    // ❌ ERROR (pointer is const)

3️⃣ const int *const ptr

    const int *const ptr = &x;
    
    *ptr = 15;   // ❌ ERROR
    ptr = &y;    // ❌ ERROR

One-line interview summary

    const int *ptr → value constant
    
    int *const ptr → pointer constant
    
    const int *const ptr → both constant

## __3. Memory Layout in C?__ 

+-------------------------+ <- High memory addresses
|        Stack            |  <- Local (automatic) variables, function calls
|                         |
|                         |
+-------------------------+
|        Heap             |  <- Dynamically allocated memory (malloc, calloc)
|                         |
+-------------------------+
|   Uninitialized Data    |  <- .bss segment: global/static variables with no initial value
|   (globals/statics)     |
+-------------------------+
|   Initialized Data      |  <- .data segment: global/static variables with initial values
|   (globals/statics)     |
+-------------------------+
|   Read-Only Data (RO)   |  <- .rodata segment: constants, string literals, const globals/statics
|   (const, literals)     |
+-------------------------+
|        Text             |  <- .text segment: program code
+-------------------------+ <- Low memory addresses

**Text Segment**

-The text segment (or code segment) stores the executable code of the program like program’s functions and instructions.

-The segment is usually read-only to prevent accidental modification during execution.

-It is typically stored in the lower part of memory.

-The size of the text segment depends on the number of instructions and the program’s complexity.

**Data Segment**

-The data segment stores global and static variables of the program.

-Variables in this segment retain their values throughout program execution.

-The size of the data segment depends on the number and type of global/static variables.

-It is divided into initialized and uninitialized (BSS) sections.

**Heap Segment**

-The heap segment is used for dynamic memory allocation.

-It starts at the end of the BSS segment and grows towards higher memory addresses.

-Memory in the heap is managed using functions like malloc(), realloc(), and free().

-The heap is shared by all shared libraries and dynamically loaded modules in a process.

**Stack Segment**

-The stack stores local variables, function parameters, and return addresses for each function call.

-Each function call creates a stack frame in this segment.

-The stack is usually at higher memory addresses and grows opposite to the heap.

-When the stack and heap meet, the program’s free memory is exhausted.

## _4. How does const differ from #define in C? __

**1️⃣ Nature**

<img width="629" height="145" alt="image" src="https://github.com/user-attachments/assets/c4b41a70-701a-4420-99ab-13435e2e3bfe" />

**2️⃣ Data type and type safety**

    #define PI 3.14       // no type checking
    const float pi = 3.14f; // type-safe

<img width="629" height="97" alt="image" src="https://github.com/user-attachments/assets/a1fa0615-6656-4649-aecc-24e454436453" />

**3️⃣ Memory allocation**

<img width="629" height="97" alt="image" src="https://github.com/user-attachments/assets/78cc9154-7000-4f79-bc16-264bb9939b1d" />

**4️⃣ Scope rules**

<img width="629" height="73" alt="image" src="https://github.com/user-attachments/assets/be18e499-28eb-4c77-a307-51bd162051b4" />

**5️⃣ Debugging**

-const variables are visible in a debugger

-#define macros disappear after preprocessing, so not visible

**6️⃣ Error detection**

-const variables are checked by the compiler

-Macros can cause subtle bugs:

    #define SQR(x) x*x
    int a = SQR(2+3); // Expands to 2+3*2+3 = 11 ❌

Interview wordings

-#define is a preprocessor macro that performs textual substitution without type or memory, whereas const is a compiler-enforced typed constant stored in memory, following C scoping and enabling type safety and debugging.

## __5. Explain the concept of memory alignment and padding in C. __

**1️⃣ Memory Alignment in C**

-Memory alignment is the requirement that data types be stored at memory addresses that are multiples of a certain number, usually the size of the data 

type or the CPU word.

    Example:
    
    char → 1 byte → can start at any address
    
    short → 2 bytes → address must be multiple of 2
    
    int → 4 bytes → address must be multiple of 4
    
    double → 8 bytes → address must be multiple of 8

-Reason:

    -Modern CPUs read memory in word-sized chunks. If a variable is not aligned, the CPU may need multiple memory cycles to fetch it, wasting time.

    -Aligned: CPU reads in 1 cycle

    -Misaligned: CPU reads in 2+ cycles, may require extra operations

**2️⃣ Memory Padding**

Definition:

-Padding is extra unused bytes inserted by the compiler to ensure correct alignment of data members in struct

-Padding ensures that each member starts at the correct aligned address.

      struct S {
        char c1;   // 1 byte
        char c2;   // 1 byte
        int i;     // 4 bytes
    };

<img width="811" height="121" alt="image" src="https://github.com/user-attachments/assets/98b5c36d-c02f-41e1-abcd-6af4cca7aefa" />
   
**3️⃣ Why alignment matters**

Performance: Misaligned data can cause CPU to perform extra memory reads, slowing execution.

Correctness: Some CPUs (like older ARM, SPARC) cannot access misaligned addresses and will crash.

Memory layout predictability: Makes struct offsets predictable, useful in hardware interfaces or network protocols.

**4️⃣ Rules to remember**

Each data type has natural alignment (usually its size or CPU word size).

Struct members are aligned according to their natural alignment.

Padding may be added between members or at the end of struct (struct size rounded to largest alignment).

Interview line: 

“Alignment means storing variables at addresses that are multiples of their size so the CPU can fetch them in one access. Padding is unused memory inserted by the compiler to maintain alignment in structs.”,

## __6. Explain the concept of a dangling pointer. How can you avoid it in your programs? __

-A dangling pointer is a pointer that points to memory that has already been freed or deallocated, or that no longer exists, but you still try to use it

-Using it leads to undefined behavior, including crashes, data corruption, or security issues.

    #include <stdlib.h>
    #include <stdio.h>
    
    int main() {
        int *ptr = (int *)malloc(sizeof(int));
        *ptr = 42;
    
        free(ptr);       // memory is deallocated
        printf("%d\n", *ptr); // ❌ Dangling pointer! Undefined behavior
        return 0;
    }

-How to avoid dangling pointers

    -Set pointer to NULL after freeing:

    -Avoid returning addresses of local variables:

Interview line:

-Definition: “A dangling pointer is a pointer that points to memory that has been deallocated or gone out of scope.”

-Danger: “Dereferencing it causes undefined behavior.”

-Avoidance: “Set pointers to NULL after free, don’t return addresses of local variables, and manage pointer lifetime carefully.”

## __7. What is the difference between int *p[10] and int (*p)[10]? Provide examples. __

int *p[10] -> p is an array of 10 pointers to int, Each element p[i] is a pointer to an int.

    int *p[10];
          ↑
         * → each element is a pointer
    p[10] → array of 10 elements

    #include <stdio.h>
    
    int main() {
        int a = 5, b = 10;
        int *p[10];   // array of 10 int pointers
    
        p[0] = &a;
        p[1] = &b;
    
        printf("%d %d\n", *p[0], *p[1]);  // prints 5 10
        return 0;
    }

    p[0] → &a
    p[1] → &b
    p[2] → NULL (or garbage)
    ...
    p[9] → ?

int (*p)[10] — Pointer to an array of 10 ints, *p gives the entire array of 10 integers.

    int (*p)[10];
           ↑
          * → p is a pointer
    (*p)[10] → points to an array of 10 ints

    #include <stdio.h>
    
    int main() {
        int arr[10] = {0,1,2,3,4,5,6,7,8,9};
        int (*p)[10] = &arr;   // pointer to the array
    
        printf("%d\n", (*p)[0]);  // prints 0
        printf("%d\n", (*p)[5]);  // prints 5
    
        return 0;
    }

    p → points to arr[0..9]
    *p → entire array arr
    (*p)[i] → accesses element i of array

## __8. What are the limitations of parameterized macros compared to functions? Provide examples. __

<img width="618" height="529" alt="image" src="https://github.com/user-attachments/assets/9fb77d34-d82f-4326-b8df-21a9941dc797" />

## __9. What is the purpose of volatile keyword in C? Provide an example. __

The volatile keyword tells the compiler that a variable's value can be changed at any time by external factors outside the program's control (e.g., hardware, interrupts, or other threads). It prevents the compiler from optimizing code that assumes the variable's value does not change unexpectedly.

	Key Use Cases:
	Memory-Mapped Hardware Registers: Variables representing hardware registers may change independently of the program.
	Shared Variables in Multithreading: Variables shared between threads or modified by an interrupt handler.
	Signal Handlers: Variables modified in signal or interrupt handlers."

## __10. Pointer Arithmetics __

"#include <stdio.h>
int main() 
{
    int arr[5] = {10, 20, 30, 40, 50};
    int *ptr = arr;
    int Var2 = *ptr;
    printf(""Step 1: %p, %d, %d\n"", ptr, *ptr, Var2);
    Var2 = *ptr++;
    printf(""Step 2: %p, %d, %d\n"", ptr, *ptr, Var2);
    Var2 = (*ptr)++;
    printf(""Step 3: %p, %d, %d\n"", ptr, *ptr, Var2);
    Var2 = ++(*ptr);
    printf(""Step 4: %p, %d, %d\n"", ptr, *ptr, Var2);
    Var2 = *(++ptr);
    printf(""Step 5: %p, %d, %d\n"", ptr, *ptr, Var2);
    Var2 = *(ptr++);
    printf(""Step 6: %p, %d, %d\n"", ptr, *ptr, Var2);
    return 0;
}"

## __11. How do double pointers (int **ptr) work? Write a program to dynamically allocate a 2D array using double pointers. __

-A double pointer (int **ptr) stores the address of another pointer.

    int x = 10;   // normal int
    int *p = &x;  // pointer to int → stores address of x
    int **ptr = &p; // double pointer → stores address of p

2D array

    #include <stdio.h>
    #include <stdlib.h>
    
    int main() {
        int m = 3, n = 4;   // 3 rows, 4 columns
        int **arr;
    
        // Step 1: allocate array of row pointers
        arr = (int **)malloc(m * sizeof(int *));
        if (arr == NULL) {
            printf("Memory allocation failed\n");
            return 1;
        }
    
        // Step 2: allocate each row
        for (int i = 0; i < m; i++) {
            arr[i] = (int *)malloc(n * sizeof(int));
            if (arr[i] == NULL) {
                printf("Memory allocation failed\n");
                return 1;
            }
        }
    
        // Step 3: initialize and print array
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                arr[i][j] = i * n + j;  // just sample data
                printf("%d ", arr[i][j]);
            }
            printf("\n");
        }
    
        // Step 4: free memory
        for (int i = 0; i < m; i++) {
            free(arr[i]);  // free each row
        }
        free(arr);        // free array of pointers
    
        return 0;
    }

## __12. Explain the difference between void * and char *? When would you use void *? ~__

<img width="618" height="265" alt="image" src="https://github.com/user-attachments/assets/b5fcf3aa-432e-4d9b-9a0d-0964283d41a7" />

<img width="752" height="81" alt="image" src="https://github.com/user-attachments/assets/3bf4bf66-4286-4854-910e-f7f8be05e414" />

char * → pointer to a character, known type, can dereference and do arithmetic.

void * → generic pointer, unknown type, cannot dereference or do arithmetic without casting, useful for generic functions and memory management.

<img width="738" height="50" alt="image" src="https://github.com/user-attachments/assets/0cd43fdb-a369-4269-864c-91bc7a052d0c" />

## __13. What is self-referential structure? __

A self-referential structure is a struct that contains a pointer to itself (i.e., a pointer to the same type of structure inside it). used in LL etc..

    struct Node {
        int data;
        struct Node *next; // pointer to same type
    };

## __14. Write a program to check whether nth bit in the number is set or not. If not set the bit. __

    #include <stdio.h>
    
    int main() {
        int num = 5, k = 2; // 5 = 0101 in binary
    
        // Check if k-th bit is set
        if (num & (1 << k)) {
            printf("Already Set\n");
        } else {
            // Toggle k-th bit
            num = num ^ (1 << k);
            printf("Bit was not set. New number = %d\n", num);
        }
    
        return 0;
    }

## __14. What is the difference between sizeof(char) and sizeof(char *)? Why might they differ on different platforms? __

sizeof(char) -> 1 byte
sizeof(char *) -> 4 byte (32-bit) or 8 byte (64-bit)

## __15. Write a program to calculate the sizeof an array without using sizeof. __

	#include <stdio.h>
	
	// Macro to calculate number of elements in a static array
	#define ARRAY_LENGTH(arr) ((char *)(&arr + 1) - (char *)arr) / ((char *)(&arr[0] + 1) - (char *)(&arr[0]))
	
	int main() {
	    int arr1[] = {1, 2, 3, 4, 5};
	    char arr2[] = {'a', 'b', 'c'};
	    double arr3[] = {1.1, 2.2, 3.3, 4.4};
	
	    printf("Number of elements in arr1 = %d\n", ARRAY_LENGTH(arr1));
	    printf("Number of elements in arr2 = %d\n", ARRAY_LENGTH(arr2));
	    printf("Number of elements in arr3 = %d\n", ARRAY_LENGTH(arr3));
	
	    return 0;
	}

## __16. What is dynamic memory allocation. How free() works __

-Dynamic memory allocation in C/C++ is the process of allocating memory at runtime rather than at compile time.

<img width="333" height="241" alt="image" src="https://github.com/user-attachments/assets/624d5270-d445-4ef6-be4b-d46d6421ab21" />

1️⃣ What free() actually does

When you allocate memory dynamically using malloc(), calloc(), or realloc(), the memory comes from the heap, which is managed by the memory allocator provided by the C runtime (usually part of the OS).

free(ptr) does NOT zero out memory or physically delete it. Instead:

-It marks the memory block pointed to by ptr as available in the heap.

-It updates internal data structures used by the memory allocator to track free blocks.

-The memory can now be reused by future allocations (malloc, calloc, realloc).

2️⃣ How free() works internally (simplified)

Every allocated block has a header (hidden metadata) that stores:

Size of the block

Status (allocated/free)

Pointers to next/previous blocks (in some allocators)

When you call free(ptr):

The allocator reads the header of ptr to find its size.

Marks the block as free.

Optionally merges adjacent free blocks (coalescing) to avoid fragmentation.

Adds the block back to the free list, so future allocations can reuse it.

## __17. What is the difference between #define macros and inline functions? When would you use one over the other? __

Macros are generally used to define constant values that are being used repeatedly in programs. Macros can even accept arguments and such macros are known as function-like macros.

MACRO with function code

	#include <stdio.h>
	
	// Macro function to find maximum
	#define MAX(x, y) ((x) > (y) ? (x) : (y))
	
	int main() {
	    int a = 10, b = 20;
	
	    printf("Maximum = %d\n", MAX(a, b));
	    printf("Maximum = %d\n", MAX(a + 5, b - 5)); // works correctly
	
	    return 0;
	}

-Inline functions are those functions whose definition is small and can be substituted at the place where its function call is made.

-Usually, people say that having an inline function increases performance by saving time of the function call overhead (i.e. passing arguments variables, return address, return value, stack mantle, and its dismantling, etc.)

but whether an inline function serves your purpose in a positive or negatively depends purely on your code design and is largely debatable.

-The compiler does inlining for performing optimizations. If compiler optimization has been disabled, then inline functions would not serve their purpose and their function call would not be replaced by their function definition.

-To have GCC inline your function regardless of optimization level, declare the function with the “always_inline” attribute:

Inline function code

	#include <stdio.h>
	
	// Inline function
	inline int square(int x) {
	    return x * x;
	}
	
	int main() {
	    int a = 5;
	    int b = 3;
	
	    printf("Square of %d = %d\n", a, square(a));
	    printf("Square of %d = %d\n", b, square(b));
	    printf("Square of %d+1 = %d\n", a, square(a + 1)); // works safely
	
	    return 0;
	}

Inline functions advantages over macros

-Since they are functions, the type of arguments is checked by the compiler whether they are correct or not.

-There is no risk if called multiple times. But there is risk in macros which can be dangerous when the argument is an expression.

-They can include multiple lines of code without trailing backslashes.

-Inline functions have their own scope for variables and they can return a value.

-Debugging code is easy in the case of Inline functions as compared to macros.

-Remember, inlining is only a request to the compiler, not a command. The compiler can ignore the request for inlining. The compiler may not perform inlining in such circumstances as:

	-If a function contains a loop. (for, while, do-while)
	-If a function contains static variables.
	-If a function is recursive.
	-If a function return type is other than void, and the return statement doesn’t exist in the function body.
	-If a function contains switch or goto statement.

<img width="867" height="586" alt="image" src="https://github.com/user-attachments/assets/1e7ac596-5061-4d2a-a312-d34efe764786" />

## __18. What is the difference between static and dynamic library.__

Static Lib:

-A static library is a collection of object files (.o) packaged into a single file (.a) using ar command.

-File extension: .a (archived file)

-Linking: Linked at compile time. The code from the library is copied into the executable.

	gcc main.c -L. -lmylib -o myprogram

	Here, -L. points to the directory containing libmylib.a.

Advantages:

-No dependency on the library at runtime.

-Faster execution because all code is already part of the executable.	

Disadvantages:

-Executable size increases.

-Updating library requires recompiling all executables using it.

Dynamic Library (Shared Library):

-Definition: A dynamic library is also a collection of object files but is loaded at runtime rather than compile time.

-File extension: .so (shared object)

-Linking: Linked at runtime (though compiler needs it at compile/link time for references).

	gcc main.c -L. -lmylib -o myprogram
	export LD_LIBRARY_PATH=.:$LD_LIBRARY_PATH
	./myprogram

	Here, libmylib.so is loaded when myprogram runs.

Advantages:

-Smaller executable size.

-Multiple programs can share the same library in memory.

-Updating the library doesn’t require recompiling executables.	

Disadvantages:

-Slight runtime overhead due to dynamic linking.

-Executable won’t run if the shared library is missing or incompatible (dependency issues).

**Creation** 

Static

Compile to object file

	gcc -c mathlib.c -o mathlib.o

Create static library

	ar rcs libmathlib.a mathlib.o

		ar = archive tool
	
		r = replace/add object files

		c = create if it doesn’t exist

		s = create index (important for linker)

Dynamic Lib

Compile to position-independent object file

	gcc -fPIC -c mathlib.c -o mathlib.o

	-fPIC = Position Independent Code (required for shared libraries).

Create shared library
	
	gcc -shared -o libmathlib.so mathlib.o

	-shared tells gcc to make a dynamic library.

	Output: libmathlib.so

Use the dynamic library

	gcc main.c -L. -lmathlib -o main

Run the program:

	export LD_LIBRARY_PATH=.:$LD_LIBRARY_PATH  # Tell Linux where to find .so
	./main	

## __19. How select() API works in networking. __
