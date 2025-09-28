#Activity 5 Hash tables

## Assume you have a simple single-dimensional array array = [200, 400, 100, 50, 350] Linear search will take O(N). Write a code in C++/Python to improve the search operation efficiency from O(N) to O(1). **4 pts **

```python

# Build a value -> index table once, then lookups are O(1)
array = [200, 400, 100, 50, 350]
index_of = {v: i for i, v in enumerate(array)}  # hash table

def contains(x):
    return x in index_of  # O(1) average

def find_index(x):
    return index_of.get(x, -1)  # O(1) average

# examples
print(contains(100))      # True
print(find_index(350))    # 4
print(find_index(999))    # -1

```

## Use C++, generate hash value of your name. 1 pts

```C++
#include <iostream>
#include <cstring>
using namespace std;

#define CAPACITY 50000  // from the tutorial

unsigned long hash_function(const char* str) {
    unsigned long i = 0;
    for (int j = 0; str[j]; j++) i += str[j];
    return i % CAPACITY;
}

int main() {
    const char* name = "YourNameHere";
    cout << "hash(\"" << name << "\") = " << hash_function(name) << "\n";
}

```

## With the help of a figure, explain the problem that occured due to introducing a tombstone to mark the deleted cell. 5 pts

<img width="2044" height="1996" alt="Untitled" src="https://github.com/user-attachments/assets/717f9578-7da4-41cd-bf9e-edde7bad346a" />

<img width="2044" height="1996" alt="Untitled" src="https://github.com/user-attachments/assets/84e29952-3715-4f9c-b9e9-656f2a8e4cd6" />

<img width="2044" height="1996" alt="Untitled" src="https://github.com/user-attachments/assets/67cac77e-8d25-47f6-8805-0c2a313bf15f" />



What tombstones prevent
If you mark a deleted slot as truly empty, a later search that relies on probing will stop early at that empty slot and falsely conclude a key isn’t present, even if the key was displaced further along. A tombstone says “this spot used to be occupied; keep probing” which preserves correctness. This matches the chapter’s explanation of search, insert, and delete with linear probing and the need for a special marker instead of a plain empty cell.

What tombstones cause
They accumulate. Searches must step over them, which lengthens probe sequences and slows down lookups and inserts. The chapter notes that after intermixed inserts and deletes, average distance from a record’s home position increases and performance degrades until you periodically rehash to clean out tombstones and pack items back into home positions.
