## Why Polymorphism?
### Analogy
- a smartphone button
- pressing the main button performs completely **different actions depending on which app is currently open**
### Problem
- every function required a unique name for different data types
- e.g. `printInt()`, `printFloat()`, `printString()`
### Solution
- treat different child objects as if they belong to a single, common parent category
- **benefit:** simplifies API design by providing consistent function names

## Types of Polymorphism
### Compile-time Polymorphism
- compiler determines which function to execute while building the code
- multiple functions share the same name but have different parameter lists
### Runtime Polymorphism
- decision of which function body to execute is delayed until the program is running
- achieved using **Inheritance** and **Method Overriding**

## Runtime Polymorphism - Example
```cpp
class Animal {
public:
    // 'virtual' ensures late binding
    virtual void speak() {
        cout << "Animal makes a sound" << endl;
    }
};

class Dog : public Animal {
public:
    void speak() override { // 'override' ensures exact function signature matching
        cout << "Dog barks: Woof! Woof!" << endl;
    }
};

class Cat : public Animal {
public:
    void speak() override {
        cout << "Cat meows: Meow!" << endl;
    }
};

int main() {
    Animal* myAnimal; 
    Dog d;
    Cat c;

    myAnimal = &d;
    myAnimal->speak(); // Output: Dog barks: Woof! Woof! (Dynamic Binding)

    myAnimal = &c;
    myAnimal->speak(); // Output: Cat meows: Meow! (Dynamic Binding)

    return 0;
}
```

## Runtime Polymorphism - Under the Hood

### Two Hidden Components
- **VTable (Virtual Table)**
    - every class which contains virtual functions has a VTable
    - it simply stores the memory addresses of those virtual functions
- **VPTR (Virtual Pointer)**
    - every object instance has this pointer that points directly to its class's VTable

### 3-Step Process at Runtime
- program executes `myAnimal->speak()`
- compiler inspects the actual object sitting in memory, in this case `Dog`
- it follows that object's **VPTR** to the `Dog` **VTable**
- It looks up the memory address for `Dog::speak()` in the table, and then executes it

## Interview Questions

**Que:** What are Pure Virtual Functions & Abstract Classes?  
**Ans:** A Pure Virtual Function has no body and is declared as `virtual void func() = 0;`
```cpp
// Abstract Class (at least one pure virtual function)
class Shape {
public:
    // Pure Virtual Function
    virtual void draw() = 0; 
    
    // Virtual Destructor
    virtual ~Shape() = default; 
};

class Circle : public Shape {
public:
    // The derived class MUST implement this, otherwise it also becomes abstract
    void draw() override {
        cout << "Drawing a Circle.\n";
    }
};

int main() {
    
    // COMPILE ERROR
    Shape s; 

    Shape* myShape = new Circle();
    myShape->draw(); // Output: Drawing a Circle.
    
    delete myShape;
    return 0;
}
```
**Que:** What are Virtual Destructors?  
**Ans:** If we delete a derived child object through a parent pointer and the base destructor is not declared `virtual`, then only the parent destructor runs
```cpp
// --- THE BAD WAY (Memory Leak) ---
class BadParent {
public:
    ~BadParent() {
        cout << "BadParent destroyed.\n";
    }
};

class BadChild : public BadParent {
    int* data;
public:
    BadChild() {
        data = new int[100];
    }
    ~BadChild() { 
        delete[] data; 
        cout << "BadChild data cleaned up.\n"; 
    }
};

// --- THE GOOD WAY (Proper Cleanup) ---
class GoodParent {
public:
    // The virtual keyword ensures the derived destructor is called first!
    virtual ~GoodParent() {
        cout << "GoodParent destroyed.\n";
    }
};

class GoodChild : public GoodParent {
    int* data;
public:
    GoodChild() {
        data = new int[100];
    }
    ~GoodChild() override { 
        delete[] data; 
        cout << "GoodChild data cleaned up.\n"; 
    }
};

int main() {

    BadParent* badPtr = new BadChild();
    delete badPtr; 
    // Output: BadParent destroyed.
    // DANGER: BadChild's destructor was NEVER called. 'data' is leaked!

    GoodParent* goodPtr = new GoodChild();
    delete goodPtr; 
    // Output: GoodChild data cleaned up.
    // Output: GoodParent destroyed.
    // SUCCESS

    return 0;
}
```

**Que:** Can a Constructor be Virtual?   
**Ans:** Nope
```cpp
class Base {
public:
    
    // COMPILE ERROR
    virtual Base() { 
        cout << "Virtual constructor\n"; 
    }    
    
    Base() {
        cout << "Normal Base constructor\n";
    }
    
};
```