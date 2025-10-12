# Linked Lists

## Create a linked list in C++, add nodes and delete nodes at the start of the list.

```c++
#include <iostream>          // bring in input/output stuff like cout and endl
using namespace std;         // so we don’t have to write std:: every time

// define a "Node" structure, which is the basic building block of the linked list
struct Node {
    int data;                // the actual integer value we store in this node
    Node* next;              // a pointer that tells us where the next node is
};

// create a global pointer called "head" to track the first node in the list
// right now, the list is empty, so head points to nothing (NULL)
Node* head = NULL;

// this function inserts a new node at the *beginning* of the linked list
void insertAtStart(int value) {

    Node* newNode = new Node;    // dynamically create a new node on the heap
    newNode->data = value;       // put the value we were given into the new node
    newNode->next = head;        // make this new node point to the old head node
    head = newNode;              // move the head pointer so it now points to this new node

    cout << "Inserted " << value << " at start.\n";  // print confirmation for human eyes
}

// this function deletes the node currently at the *start* of the linked list
void deleteAtStart() {

    // if the list is empty (head is NULL), there’s nothing to delete
    if (head == NULL) {
        cout << "List is empty. Nothing to delete.\n";  // tell the user there’s nothing to remove
        return;                                         // exit the function early
    }

    Node* temp = head;          // temporarily store the current first node so we can delete it later
    head = head->next;          // move the head pointer to the second node (which is now first)
    cout << "Deleted " << temp->data << " from start.\n";  // print what got deleted
    delete temp;                // actually free the memory for the node we just removed
}

// this function prints out all the data values in the linked list
void displayList() {
    Node* temp = head;          // start at the first node

    cout << "List: ";           // start printing the list contents
    while (temp != NULL) {      // keep going until we hit the end (NULL)
        cout << temp->data << " ";  // print the value in the current node
        temp = temp->next;          // move to the next node in the chain
    }
    cout << "\n";               // print a newline for neatness
}

// main function – this is where the program actually runs
int main() {

    // add three numbers to the start of the list (30 ends up first)
    insertAtStart(10);          // list becomes: 10
    insertAtStart(20);          // list becomes: 20 -> 10
    insertAtStart(30);          // list becomes: 30 -> 20 -> 10

    displayList();              // print: 30 20 10

    // now we start deleting from the start
    deleteAtStart();            // deletes 30, list becomes: 20 -> 10
    displayList();              // print: 20 10

    deleteAtStart();            // deletes 20, list becomes: 10
    displayList();              // print: 10

    deleteAtStart();            // deletes 10, list becomes empty
    displayList();              // print: (nothing)

    return 0;                
}


```
