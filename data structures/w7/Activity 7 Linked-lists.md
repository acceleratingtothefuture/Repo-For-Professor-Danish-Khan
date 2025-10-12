# Linked Lists

## Create a linked list in C++, add nodes and delete nodes at the start of the list.

```c++
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

Node* head = NULL;  // start of the linked list (the "head")

// Insert a new node at the start of the list
void insertAtStart(int value) {
    Node* newNode = new Node;  // create a new node
    newNode->data = value;     // store the data
    newNode->next = head;      // link new node to the old head
    head = newNode;            // move head to point to the new node
    cout << "Inserted " << value << " at start.\n";
}

// Delete a node from the start of the list
void deleteAtStart() {
    if (head == NULL) {
        cout << "List is empty. Nothing to delete.\n";
        return;
    }
    Node* temp = head;     // store current head
    head = head->next;     // move head to next node
    cout << "Deleted " << temp->data << " from start.\n";
    delete temp;           // free memory
}

// Display the linked list
void displayList() {
    Node* temp = head;
    cout << "List: ";
    while (temp != NULL) {
        cout << temp->data << " ";
        temp = temp->next;
    }
    cout << "\n";
}

int main() {
    insertAtStart(10);
    insertAtStart(20);
    insertAtStart(30);

    displayList();     // shows 30 20 10

    deleteAtStart();   // deletes 30
    displayList();     // shows 20 10

    deleteAtStart();   // deletes 20
    displayList();     // shows 10

    deleteAtStart();   // deletes 10
    displayList();     // list empty

    return 0;
}

```
