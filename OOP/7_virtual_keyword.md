## The 4 Key Use Cases of `virtual`
### Use Case 1: Virtual Functions
```cpp
class Animal {
public:
    // virtual function 
    virtual void sound() {
        cout << "Generic sound";
    }
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Woof!";
    }
};

Animal* pet = new Dog();
pet->sound(); // Output: Woof!
```

### Use Case 2: Pure Virtual Functions
```cpp
class Shape {
public:
    // pure virtual function
    virtual void draw() = 0;
};

class Circle : public Shape {
public:
    void draw() override {
        cout << "Drawing Circle";
    }
};

Shape* a = new Circle();
a->draw(); // Output: Drawing Circle
```

### Use Case 3: Virtual Destructors

- deleting a derived object through a base pointer (`Parent* p = new Child(); delete p;`) without a virtual destructor causes C++ to run only `~Parent()`
- adding `virtual` to the base destructor triggers a **cascade cleanup** - executing the child destructor first, then walking up to the parent destructor
- **Golden Rule:** if a class contains at least one virtual function, then that class's destructor must be virtual
```cpp
class Parent {
public:
    virtual ~Parent() {
        cout << "Parent cleaned up\n";
    }
};

class Child : public Parent {
private:
    int* secretData;
public:
    Child() {
        secretData = new int;
    }
    ~Child() {
        delete[] secretData;
        cout << "Child cleaned up\n";
    }
};

Parent* p = new Child();
delete p;
// Output: Child cleaned up
// Output: Parent cleaned up
```

### Use Case 4: Virtual Inheritance
- **The Diamond Problem:** 
    - class `D` inherits from Parents `B` and `C`
    - both `B` and `C` inherit from Grandparent `A`
    - `D` receives two duplicate copies of `A` 
    - this creates compiler ambiguity errors when accessing grandparent members
- **The Solution:** declaring intermediate inheritance as `virtual public` forces to keep exactly one single instance of `A` across the entire hierarchy
```cpp
class A {
public:
    int gold;
};

// Use 'virtual' to ensure only one shared "gold" instance exists
class B : virtual public A {};
class C : virtual public A {};

class D : public B, public C {
    // 'gold' is now unambiguous!
};
```