# Activity 8 Binary search trees

Video explanation: 

## Task 1: Imagine you were to take an empty binary search tree and insert the following sequence of numbers in this order: [1, 5, 9, 2, 4, 10, 6, 3, 8]. Draw a diagram showing what the binary search tree would look like. Remember, the numbers are being inserted in the order presented here.

<img width="2531" height="1996" alt="Untitled" src="https://github.com/user-attachments/assets/6f5b9576-d440-460c-aa0b-091df8e2e5ec" />

## Task 2: If a well-balanced binary search tree contains 1,000 values, what is the maximum number of steps it would take to search for a value within it?

What is the least amount of layers that a binary search tree can have to contain 1,000 values? If we answer this question, we know the maximum amount of layers it would need to traverse in a well balanced tree. 

Level 1: 1 node (root)
Total = 1

Level 2: 2 nodes
1 + 2 = 3

Level 3: 4 nodes
3 + 4 = 7

Level 4: 8 nodes
7 + 8 = 15

Level 5: 16 nodes
15 + 16 = 31

Level 6: 32 nodes
31 + 32 = 63

Level 7: 64 nodes
63 + 64 = 127

Level 8: 128 nodes
127 + 128 = 255

Level 9: 256 nodes
255 + 256 = 511

Level 10: 512 nodes
511 + 512 = 1023

So a balanced binary search tree with 1000 values would be full up through level 10. That means the maximum path length, how many steps to reach the deepest leaf, is 10.

## Task 3: Write an algorithm that finds the greatest value within a binary search tree. 

1. Start at the root node.
2. If the tree is empty, there is no greatest value. Stop.
3. While the current node has a right child, move to the right child.
4. When you reach a node that has no right child, stop.
5. The value at that node is the greatest value in the tree.

## Task 4: 

```c++
#include <iostream>        
using namespace std;       


struct Node {
    int val;              
    Node* left;           // pointer to the left child (smaller values go here)
    Node* right;          // pointer to the right child (larger values go here)
};

// this function inserts a new value into the binary search tree
// it returns the (possibly new) root of the tree after insertion
Node* insert(Node* root, int value) {

    // if there is no node here yet, create one
    if (root == NULL) {
        Node* newNode = new Node;   // dynamically allocate a new node on the heap
        newNode->val = value;       // store the given value in the new node
        newNode->left = NULL;       // no left child yet
        newNode->right = NULL;      // no right child yet
        return newNode;             // this new node becomes the root (or child)
    }

    // if the value is smaller than the current node, go left
    if (value < root->val) {
        root->left = insert(root->left, value);   // insert into the left subtree
    } 
    // otherwise, it’s larger, so go right
    else {
        root->right = insert(root->right, value); // insert into the right subtree
    }

    // return the (unchanged) root pointer to keep the tree linked correctly
    return root;
}

// this function prints all nodes by their level (root = level 1, children = level 2, etc.)
void printByLevel(Node* root, int level = 1) {

    // if the tree (or subtree) is empty, just stop
    if (root == NULL) return;

    // print the current node’s value and which level it’s on
    cout << "Level " << level << ": " << root->val << endl;

    // recursively print the left and right children, each one level deeper
    if (root->left != NULL)
        printByLevel(root->left, level + 1);

    if (root->right != NULL)
        printByLevel(root->right, level + 1);
}

int main() {

    // define an array of numbers we want to insert into our tree
    int arr[] = {1, 5, 9, 2, 4, 10, 6, 3, 8};
    int n = 9;   // number of values in the array

    Node* root = NULL;    // start with an empty tree (no root yet)

    // insert each number from the array into the tree
    for (int i = 0; i < n; i++) {
        root = insert(root, arr[i]);   // add one value at a time
    }

    // print the contents of the tree level by level
    cout << "Binary Search Tree (by level):" << endl;
    printByLevel(root);

    return 0;   // end of program
}


```
