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
