## Constructor - The Birth of an Object

- shares the **exact same name** as the class
- has **no return type** (not even void)

## What was the need? (Garbage Value Era)

- memory allocated for an object was filled with uninitialized garbage values
- developers had to manually call initialization functions like `initCharacter()`
- constructors solved this by making object creation and initialization a single event

```cpp
class GameCharacter {
public:
    int health;
    int ammo;
    // Constructor 
    GameCharacter() {
        health = 100;
        ammo = 50;
    }
};

int main() {
    
    GameCharacter player1; // automatically calls GameCharacter()
    // player1.health is guaranteed to be 100, preventing garbage values!

}
```

## Types of Constructors

- **Default Constructor**: takes no parameters and initializes objects with default values
- **Parameterized Constructor**: accepts arguments to initialize objects with custom values
- **Copy Constructor**: creates a new object by copying data from an existing object

```cpp
class Hero {
public:
    string name;
    int health;

    // Default Constructor
    Hero() {
        name = "Unknown"; health = 100;
    }

    // Parameterized Constructor
    Hero(string n, int h) {
        name = n; health = h;
    }

    // Copy Constructor
    Hero(const Hero &old_obj) {
        name = old_obj.name;
        health = old_obj.health;
    }
};
```

## Overloading vs. Overriding

### Overloading
- same method name in the same class but with **different parameters**
- gets decided during **compile-time**

### Overriding
- child class provides a **new implementation** for a method in the parent class
- must have the **same name** and **same parameters**
- requires `virtual` in the method of the parent class
- gets decided during **runtime**


```cpp
class Calculator {
public:
    int add(int a, int b) {
        return a + b;
    }
    // Overloaded
    int add(int a, int b, int c) {
        return a + b + c;
    } 
};

class Animal {
public:
    virtual void speak() { // 'virtual' enables late binding
        cout << "Generic noise\n";
    } 
};

class Dog : public Animal {
public:
    // Overridden
    void speak() override {
        cout << "Bark!\n";
    }
};
```

## Chains & Orders

### Constructor Execution:
- runs **top to bottom** (parent to child) 
- you cannot build the 2nd floor of a house until the foundation is built

### Destructor Execution:
- runs **bottom to top** (child to parent)

### Method Lookup:
- compiler checks the child class first
- if found, it runs it
- otherwise, it moves up to parent and grandparent in a sequential manner
- **note:** in case of multiple inheritance, i.e. multiple parents, the compiler will check all the parents parallelly

```cpp
class Grandparent {
public:
    Grandparent() {
        cout << "Grandparent Constructor\n";
    }
    virtual void sayHello() {
        cout << "Hello from Grandparent\n";
    }
};

class Parent : public Grandparent {
public:
    Parent() {
        cout << "Parent Constructor\n";
    }
    // Overridden
    void sayHello() override { 
        cout << "Hello from Parent\n";
    }
};

class Child : public Parent {
public:
    Child() {
        cout << "Child Constructor\n";
    }
    // Overridden
    void sayHello() override {
        cout << "Hello from Child\n";
    }
};

int main() {
    Child obj; // Output: Grandparent Constructor -> Parent Constructor -> Child Constructor
    obj.sayHello(); // Output: "Hello from Child" (most recent version)
    obj.Parent::sayHello(); // Output: "Hello from Parent" (using Scope Resolution Operator)
}
```

Note: Once the `sayHello` function was declared virtual in **Grandparent** class, it remained implicitly virtual across all derived classes down the inheritance chain and hence in the **Parent** we don't have to write `virtual void sayHello() override {` and instead we simply write `void sayHello() override {`

## Interview Questions

**Que:** What happens if a Child class doesn't define a constructor?  
**Ans:** C++ automatically generates a default constructor for the Child
```cpp
class Parent {
public:
    Parent() {
        cout << "Parent default constructor called.\n";
    }
};

class Child : public Parent {
    // No constructor defined here. 
    // The compiler generates a default constructor that automatically calls Parent().
};

int main() {
    Child c; // Output: Parent default constructor called.
    return 0;
}
```

**Que:** What if the Parent class has a Parameterized Constructor but no Default Constructor?  
**Ans:** The Child class will fail to compile unless you explicitly pass arguments to the Parent Constructor
```cpp
class Parent {
public:
    // Parameterized Constructor
    Parent(int x) {
        cout << "Parent initialized with: " << x << "\n";
    }
};

class Child : public Parent {
public:
    // We MUST explicitly pass arguments to the Parent constructor in the initializer list
    Child(int x, int y) : Parent(x) {
        cout << "Child initialized with: " << y << "\n";
    }
         
    // This would FAIL to compile because there is no Parent() to call:
    Child(int x, int y) { ... } 
    
};

int main() {
    Child c(10, 20);
    return 0;
}
```

**Que:** Can you override static methods?  
**Ans:** No we cannot. Defining a static method with the same name in a Child class results into **method hiding**, not overriding
```cpp
class Parent {
public:
    virtual void overridingExample() {
        cout << "OVERRIDING: Parent's virtual method\n";
    }
    void hidingExample() {
        cout << "HIDING: Parent's non-virtual method\n";
    }
};

class Child : public Parent {
public:
    void overridingExample() override {
        cout << "OVERRIDING: Child's implementation\n";
    }
    void hidingExample() {
        cout << "HIDING: Child's implementation\n";
    }
};

int main() {
    
    Parent* p = new Child();

    // C++ looks at the actual object type at RUNTIME.
    p->overridingExample(); // Output: OVERRIDING: Child's implementation
    
    // C++ looks at the pointer type at COMPILE-TIME.
    p->hidingExample(); // Output: HIDING: Parent's non-virtual method

    Child c;
    c.overridingExample(); // Output: OVERRIDING: Child's implementation
    c.hidingExample();     // Output: HIDING: Child's implementation

    delete p;
    return 0;
}
```

**Que:** How do you prevent inheritance or method overriding?  
**Ans:** By using the `final` keyword, e.g. `class CoreEngine final { ... }` and `void calculate() override final { ... }` 
```cpp
// Preventing Inheritance
class CoreEngine final {};

// This would cause a compile error
class ModifiedEngine : public CoreEngine {}; 


class Base {
public:
    virtual void calculate() {
        cout << "Base calculation\n";
    }
};

class Derived : public Base {
public:
    // Preventing Method Overriding
    void calculate() override final {
        cout << "Derived calculation (final)\n";
    }
};

class Leaf : public Derived {
public:
    // This would cause a compile error
    void calculate() override {
        out << "Leaf calculation\n";
    }
};

int main() {
    return 0;
}
```

**Que:**  What is the "Ambiguity Trap" in C++?  
**Ans:** If a Child class inherits from two Parent classes that both share a method with the same name, then calling that method by a Child instance would causes an Ambiguity Error (compile-time)
```cpp
class ParentA {
public: 
    void talk() {}
};

class ParentB {
public: 
    void talk(int x) {}
};

class Child : public ParentA, public ParentB {};

int main() {
    Child obj;
    obj.talk(); // ERROR: Ambiguous call!
    obj.ParentA::talk(); // FIX: Use scope resolution operator
}
```