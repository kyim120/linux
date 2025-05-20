
### **Comparison Table**

| Feature            | **Shallow Copy**                              | **Deep Copy**                                      | **Dynamic Memory**                         |
|-------------------|------------------------------------|--------------------------------------|--------------------------------|
| **Definition**   | Copies an object including pointer addresses, leading to shared memory. | Creates a new object with its own copy of dynamically allocated memory. | Allocates memory at runtime using pointers. |
| **Memory Handling** | Does not allocate new memory, shares existing memory. | Allocates separate memory to avoid shared resources. | Uses heap memory allocation. |
| **Usage** | Default behavior in C++ (copy constructor and assignment). | Requires explicit implementation using copy constructor or assignment operator. | Used for flexible memory allocation. |
| **Impact on Pointers** | Multiple objects point to the same memory, leading to potential issues. | Each object has an independent copy of memory. | Objects dynamically control memory during runtime. |
| **Performance** | Faster but riskier (due to shared memory issues). | Slightly slower but safer (avoids memory conflicts). | Flexible allocation but requires careful management. |
| **Memory Management** | No additional allocation; deletion affects all references. | Memory safely allocated and deallocated per object. | Requires manual `new` and `delete` operations. |
| **Example** | Default copy constructor, simple structure copying. | User-defined copy constructor for deep duplication. | `new` and `delete` operations for dynamic memory. |

---

### **Theoretical Explanation**
1. **Shallow Copy**  
   Shallow copy occurs when a copy constructor or assignment operator copies pointer addresses instead of duplicating the actual data. This means multiple objects end up sharing the same memory block. If one object modifies or deallocates that memory, other objects using it may encounter undefined behavior.

2. **Deep Copy**  
   Deep copy explicitly creates new memory for an object's attributes rather than sharing the original memory. This prevents accidental modifications from affecting multiple objects. Deep copying is commonly implemented via a custom copy constructor or overloaded assignment operator.

3. **Dynamic Memory**  
   Dynamic memory allows objects and arrays to allocate memory at runtime, typically using `new` and `delete`. This is useful when the memory requirement isn’t known in advance. Unlike static memory, dynamic memory allocation provides flexibility but must be carefully managed to avoid leaks or dangling pointers.

---
Some Code Example
---

### **1. Shallow Copy Code Structure**
Shallow copy occurs when the default copy constructor or assignment operator copies only pointer addresses rather than duplicating the allocated memory.

```cpp
class ShallowCopy {
private:
    int* data;

public:
    // Constructor
    ShallowCopy(int value) {
        data = new int(value);
    }

    // Default Copy Constructor (Shallow Copy)
    ShallowCopy(const ShallowCopy& other) {
        data = other.data; // Copies pointer, not the actual data
    }

    // Destructor
    ~ShallowCopy() {
        delete data;
    }

    void showData() {
        std::cout << "Data: " << *data << std::endl;
    }
};
```

💡 **Issue:** If one instance deletes the dynamically allocated memory, the other instance is left with a dangling pointer, leading to undefined behavior.

---

### **2. Deep Copy Code Structure**
Deep copy manually allocates new memory and copies the actual data rather than just copying the pointer.

```cpp
class DeepCopy {
private:
    int* data;

public:
    // Constructor
    DeepCopy(int value) {
        data = new int(value);
    }

    // Deep Copy Constructor
    DeepCopy(const DeepCopy& other) {
        data = new int(*other.data); // Allocates new memory and copies data
    }

    // Destructor
    ~DeepCopy() {
        delete data;
    }

    void showData() {
        std::cout << "Data: " << *data << std::endl;
    }
};
```

🔹 **Advantage:** Each object manages its own memory, preventing unwanted data corruption.

---

### **3. Dynamic Memory Code Structure**
Dynamic memory allocation allows flexible memory management using `new` and `delete`.

```cpp
class DynamicMemory {
private:
    int* data;

public:
    // Constructor (Allocates memory dynamically)
    DynamicMemory(int value) {
        data = new int(value);
    }

    // Destructor (Releases allocated memory)
    ~DynamicMemory() {
        delete data;
    }

    void showData() {
        std::cout << "Data: " << *data << std::endl;
    }
};

int main() {
    DynamicMemory obj(100);
    obj.showData();

    return 0;
}
```

⚡ **Key Points:**  
- **Memory is allocated dynamically** (`new int(value)`).
- **Destructor ensures memory is properly released** (`delete data`).

---

### **Conclusion**
- **Shallow Copy:** Shares memory → Risk of dangling pointers.
- **Deep Copy:** Allocates new memory → Prevents unintended data modifications.
- **Dynamic Memory:** Uses heap memory → Offers flexibility but requires careful management.
