## The Danger of Open Data
### Problem:
- in an un-encapsulated user profile system, any developer could write `user.age = -50` or `user.password = ""`
- the program would accept these invalid inputs because there was no gatekeeper to stop them
### Solution:
- encapsulation ensures that if any external code wants to view or change data, it must ask permission through official public methods

## Key Benefits
- methods can check if an incoming data is valid or not before making any modification
- to all the variables and methods, we can grant:
    - Read-Only access (only a *Getter*)
    - Write-Only access (only a *Setter*)
    - or No Access at all

## Access Specifiers
- `public`: Members are accessible from outside the class
- `private`: Members are accessible only from within the class
- `protected`: Members are accessible within the class and by derived (child) classes

## Example
```cpp
#include <iostream>
#include <string>
using namespace std;

class BankAccount {
private:
    double balance;
public:
    // Constructor
    BankAccount(double initialBalance) {
        if (initialBalance >= 0) {
            balance = initialBalance;
        } else {
            balance = 0;
        }
    }
    // Setter
    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            cout << "Deposited: " << amount << endl;
        } else {
            cout << "Invalid deposit amount!" << endl;
        }
    }
    // Getter 
    double getBalance() {
        return balance;
    }
};

int main() {
    BankAccount myAccount(1000.0);

    // myAccount.balance = 5000000; // ERROR! 
    
    myAccount.deposit(500.0);   // Output: Deposited: 500
    myAccount.deposit(-200.0);  // Output: Invalid deposit amount!

    cout << "Current Balance: " << myAccount.getBalance() << endl; // Output: 1500

    return 0;
}
```
## Interview Questions

**Que:** How do we achieve Encapsulation in C++?  
**Ans:** By making class variables **private** and providing **public** *Getter* and *Setter* methods to interact with them  
  
**Que:** Why is Encapsulation called a "Black Box"?  
**Ans:** Because users know **what** the class does via public methods, but not **how** it actually does it
