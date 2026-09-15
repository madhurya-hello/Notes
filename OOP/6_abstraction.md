## Core Concepts
### The "Contract" Logic
- abstraction establishes a blueprint or contract. 
- example: any class claiming to be a `Shape` must provide a `draw()` method
### Abstraction vs. Encapsulation
- **encapsulation** focuses on **data security** by hiding data members and preventing unauthorized modification
- **abstraction** focuses on **design efficiency** by hiding structural complexity

## Abstraction - Achieving in C++
### Pure Virtual Function
- **Syntax**: `virtual void display() = 0;`
- forces child classes to provide their own implementation
### Abstract Class
- we **cannot create an object** of an abstract class
- any child class **must override all pure virtual functions**, otherwise the child class also becomes an abstract class
- an abstract class **can have a constructor**, which runs when a child object is created to initialize base class components.

## Abstraction - Example
```cpp
// 1. Abstract Class
class PaymentMethod {
public:
    // pure virtual function
    virtual void makePayment(double amount) = 0;
    // abstract classes can contain normal methods
    void showReceipt(double amount) {
        cout << "Receipt generated for: $" << amount << endl;
    }
};

class UPI : public PaymentMethod {
public:
    void makePayment(double amount) override {
        cout << "Processing $" << amount << " via UPI Transaction..." << endl;
    }
};

class CreditCard : public PaymentMethod {
public:
    void makePayment(double amount) override {
        cout << "Processing $" << amount << " via Secure Credit Card Gateway..." << endl;
    }
};

int main() {

    // ERROR! Cannot instantiate an abstract class
    PaymentMethod p; 

    // Base class pointers are allowed!
    PaymentMethod* myPayment; 

    UPI upiObj;
    myPayment = &upiObj;
    myPayment->makePayment(500); // Calls UPI implementation
    myPayment->showReceipt(500);  // Calls base class concrete function

    return 0;
}
```

## Interview Questions

**Que:** What is the difference between an interface and an abstract?  
**Ans:** In C++, an Interface is simply an abstract class where all methods are pure virtual functions

**Que:** Can an Abstract Class have a constructor?  
**Ans:** Yupp
```cpp
// Abstract Class
class Shape {
protected:
    string color;

public:
    // Constructor
    Shape(string c) {
        color = c; 
        cout << "Shape Constructor called " << color << endl;
    }

    virtual void draw() = 0;
};

// Child Class
class Circle : public Shape {
private:
    double radius;

public:
    // Derived Constructor calls the Base Abstract Constructor
    Circle(string c, double r) : Shape(c) {
        radius = r;
        cout << "Circle Constructor called. Radius: " << radius << endl;
    }

    void draw() override {
        cout << "Drawing a " << color << " circle with radius " << radius << "." << endl;
    }
};

int main() {
    
    Circle myCircle("Red", 5.0);
    // Output: Shape Constructor called
    // Output: Circle Constructor called

    myCircle.draw(); // Output: Drawing a Red circle with radius 5

    return 0;
}
```