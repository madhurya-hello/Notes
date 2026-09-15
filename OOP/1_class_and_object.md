## Problem with Procedural Programming

- any function could alter any variable at any time, causing a bug in one function to silently corrupt data across the system

- example: 
    - in procedural code, tracking a car meant maintaining loose variables like `speed` and `engineStatus` alongside separate functions like `startEngine()` and `accelerate()`

    - if tracking 50 cars, managing arrays became messy, and an unrelated function like `calculateTax()` could accidentally overwrite `speed`

- why Object-Oriented Programming (OOP)?

    - OOP solves this by binding data (variables) and functions (methods) into self-contained units (objects)


## Core Concepts
### Class
- acts as a blueprint or template
- occupies 1 bytes of memory when defined
### Object
- a physical instance of a class
- memory is allocated on the Stack or Heap based on initialization


```cpp
#include <iostream>
#include <string>
using namespace std;

class Car {
public: // allows access from outside the class
    string brand;
    int year;

    void startEngine() {
        cout << brand << " engine started!" << endl;
    }
};

int main() {

    Car car1;
    car1.brand = "Toyota";
    car1.year = 2022;

    Car car2;
    car2.brand = "Tesla";
    car2.year = 2024;

    cout << "Car 1: " << car1.brand << " (" << car1.year << ")" << endl;
    car1.startEngine();

    cout << "Car 2: " << car2.brand << " (" << car2.year << ")" << endl;
    car2.startEngine();

    return 0;
}
```

## Technical Highlights

### Struct vs Classes
- struct members are public by default
- class members are private by default
### Size of an Empty Class
- an empty class in C++ has a size of 1 byte. 
- this ensures that even two empty objects would have unique memory addresses
### Memory Storage Location
```cpp
// Stack
Car myCar;

// Heap
Car* myCar = new Car();
```