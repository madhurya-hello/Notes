## Main Concept

### Definition
- it is the mechanism by which a *Child* class acquires the properties and behaviors of *Parent* class

### Analogy
- if a parent owns a house, the child will then inherit it 
- the child doesn't need to rebuild a new house from scratch

### Advantage - Code Reusability
- **reuse existing code** from the base class and only **write new code** for specialized features

## Modes of Inheritance
- `public`: **public members** of the parent **stay public** in the child.
- `protected`: **public and protected members** of the parent **become protected** in the child.
- `private`: **all** inherited members **become private** in the child

## Types of Inheritance
- **Single Inheritance**: one parent, one child ($A \rightarrow B$)
- **Multilevel Inheritance**: a child class acts as a parent for another class ($A \rightarrow B \rightarrow C$)
- **Multiple Inheritance**: one child class inherits from two or more parent classes ($A, B \rightarrow C$)
- **Hierarchical Inheritance**: one parent class has multiple child classes ($A \rightarrow B$ and $A \rightarrow C$)
- **Hybrid Inheritance**: a combination of two or more of the above inheritance types

## Example
```cpp
#include <iostream>
#include <string>
using namespace std;

// Parent
class Animal {
protected: 
    string species = "Unknown";
public:
    void eat() {
        cout << "This animal is eating..." << endl;
    }
};

// Child 
class Dog : public Animal {
public:
    void bark() {
        // accessing protected variable
        species = "Canine"; 
        cout << "The " << species << " says: Woof Woof!" << endl;
    }
};

int main() {
    Dog myDog;
    myDog.eat();
    myDog.bark();

    return 0;
}
```
## Class Relationships: Association, Aggregation &amp; Composition
### Association
- two classes know about each other, but there is no ownership
- **analogy:** a doctor and a patient interact during a visit, but both exist independently
```cpp
#include <iostream>
#include <string>

class Patient {
public:
    string name;
    Patient(string n) {
        name = n;
    }
};

class Doctor {
public:
    string name;
    Doctor(string n) {
        name = n;
    }
    // Association
    void consult(Patient* p) {
        cout << "Doctor " << name << " is treating patient " << p->name << endl;
    }
};

int main() {
    Doctor doc("House");
    Patient pat("Wilson");
    // Association
    doc.consult(&pat);

    return 0;
}
```

### Aggregation

- "whole-part" relationship where the part can exist independently of the whole
- **analogy:** if a department shuts down, the professor working in it will still exist and can join another department

```cpp
#include <iostream>
#include <vector>
#include <string>

class Teacher {
public:
    string name;
    Teacher(string n) {
        name = n;
    }
};

class Department {
private:
    // Aggregation (holds pointers)
    vector<Teacher*> teachers;
public:
    void addTeacher(Teacher* t) {
        teachers.push_back(t);
    }
};

int main() {

    // a teacher is created
    Teacher* t1 = new Teacher("Mr. Smith"); 
    
    // a department is created
    {
        Department physics;
        physics.addTeacher(t1);
    } 
    // the department gets destroyed

    // the teacher still exists
    cout << "Teacher " << t1->name << " is still here." << endl;

    delete t1;
    return 0;
}
```

### Composition

- "has-a" relationship where the part cannot exist without the whole
- **analogy:** if you demolish a house, the rooms inside are destroyed as well

```cpp
#include <iostream>

class Room {
public:
    Room() {
        cout << "Room created." << endl;
    }
    ~Room() {
        cout << "Room destroyed." << endl;
    }
};

class House {
private:
    // Composition (room creation is tied strictly to the house lifecycle)
    Room livingRoom;
public:
    House() {
        cout << "House created." << endl;
    }
    ~House() {
        cout << "House destroyed." << endl;
    }
};

int main() {
    
    // both House and Room are created here
    {
        House myHouse; 
    } 
    // both House and Room are destroyed

    return 0;
}
```

