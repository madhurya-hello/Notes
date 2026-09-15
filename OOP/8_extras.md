## Struct vs Class in C++
- `struct`: members are **public** by default
- `class`: members are **private** by default
```cpp
// Struct
struct TreeNode {
    int data;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int val) : data(val), left(nullptr), right(nullptr) {}
};
// Class
class TreeNode {
public: // must explicitly add the 'public'
    int data;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int val) : data(val), left(nullptr), right(nullptr) {}
};
```

## Object Allocation
### Stack Allocation
- **Syntax**: `TreeNode node(5);`
- **Management**: automatic memory management
### Heap Allocation
- **Syntax**: `TreeNode* node = new TreeNode(5);`
- **Management**: requires manual cleanup using `delete node;`
- **Note**: Binary Trees and Linked Lists always use this method
```cpp
int main() {
    // Stack Object
    TreeNode stackNode(10);
    cout << "Stack Node Data: " << stackNode.data << "\n";

    // Heap Object
    TreeNode* root = new TreeNode(1);
    TreeNode* leftChild = new TreeNode(2);
    
    root->left = leftChild;

    delete leftChild;
    delete root;
}
```

## RAM Architecture
```text
+-------------------------------------------------------------+
| STACK MEMORY   [Function Frames: local variables]           | <-- Grows Downward v
+-------------------------------------------------------------+
|                      (Unused RAM Space)                     |
+-------------------------------------------------------------+
| HEAP MEMORY    [Dynamic Tree Nodes, Large Arrays]           | <-- Grows Upward ^
+-------------------------------------------------------------+
```
### The Stack
- operates on a **LIFO** basis
- calling a function allocates a **Stack Frame** at the top
- **Stack Pointer** slides forward to allocate space and slides back when the function finishes
- variables are destroyed automatically when a function finishes
### The Heap
- a massive, unorganized warehouse floor
- we can use the `new` keyword to ask the OS for space, the OS marks a slot as "occupied" and returns a **pointer**
- we need to explicitly free the space using `delete` or else memory-leak