# Activity 5 Hash tables

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
    const char* name = "Austin";
    cout << "hash(\"" << name << "\") = " << hash_function(name) << "\n";
}

```

## With the help of a figure, explain the problem that occured due to introducing a tombstone to mark the deleted cell. 5 pts

Let's understand how a hash function works. Here is how we would see if our array contains the value "Airplane".                                                                                                                                                                                                                                                                                    
<img width="1022" height="998" alt="Untitled" src="https://github.com/user-attachments/assets/717f9578-7da4-41cd-bf9e-edde7bad346a" />
## 
But what if our hash functions map two distinct inputs to the same output? What if we inserted something at a value, but then later we want to insert something with the same hash funciton output? 
<div></div>
<img width="1022" height="998" alt="Untitled" src="https://github.com/user-attachments/assets/84e29952-3715-4f9c-b9e9-656f2a8e4cd6" />
WAIT! There's a problem. We search for something by getting its hash value and seeing if that spot is empty. And we find other data with the same hash value by starting at what's stored at the hash function, and if we don't find the data. We try looking in the next place. But what if the value of the first spot we check is deleted? The algorithim will see "empty" in the first spot and conclude that the value we are looking for isn't there! 
                        
<img width="2044" height="1996" alt="Untitled" src="https://github.com/user-attachments/assets/67cac77e-8d25-47f6-8805-0c2a313bf15f" />



To stop from seeing that the first spot is empty and giving up, we can put a "Tombstone" in place of a value we want to delete. That way, we get rid of the value from the list, but we also don't give the false impression that nothing else with the same hash output is there. 
<img width="2044" height="1996" alt="Untitled" src="https://github.com/user-attachments/assets/56ffe896-eee8-453c-8395-1dd7a84e1724" />  
## 
But tombstones can get messy after awhile. Look at all the tombstones we would have to go through to search for "Frog". 
<img width="2044" height="1996" alt="Untitled" src="https://github.com/user-attachments/assets/23eca625-7c05-4bab-8164-6ad64311788c" />
## 




We can clean up unncessary tombstones. 
<img width="2044" height="1996" alt="Untitled" src="https://github.com/user-attachments/assets/9bc7b712-f65b-4d4f-b451-33d426ec5393" />





## Conclusion on tombstones 


What tombstones prevent

If you mark a deleted slot as truly empty, a later search that relies on probing will stop early at that empty slot and falsely conclude a key isn’t present, even if the key was displaced further along. A tombstone says “this spot used to be occupied; keep probing” which preserves correctness. This matches the chapter’s explanation of search, insert, and delete with linear probing and the need for a special marker instead of a plain empty cell.

What tombstones cause

They accumulate. Searches must step over them, which lengthens probe sequences and slows down lookups and inserts. With more and more insertions/deletions (and therefore, more and more tombstones), average distance from a record’s home position increases and performance degrades until you periodically rehash to clean out tombstones and pack items back into home positions.
