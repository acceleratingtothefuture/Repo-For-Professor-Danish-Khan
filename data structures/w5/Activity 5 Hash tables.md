# Activity 5 Hash tables
Video explanation:

## Assume you have a simple single-dimensional array  = [200, 400, 100, 50, 350] Linear search will take O(N). Write a code in C++/Python to improve the search operation efficiency from O(N) to O(1). **4 pts **

```python

array = [200, 400, 100, 50, 350]  # define our array of values
index_of = {}  # create an empty dictionary (hash table)
i = 0  # counter starts at 0
for v in array:  # loop through each value in the list
    index_of[v] = i  # hash each element of the array into a key, store that element's index as the value. 
    i += 1  # increment counter

# resulting dictionary (hash table):
# {
#     200: 0,
#     400: 1,
#     100: 2,
#     50: 3,
#     350: 4
# }

def contains(x):
    return x in index_of  # O(1) average
    # jump to the inputted bucket. 

def find_index(x):
    return index_of.get(x, -1)  # O(1) average
    # Hashes x, goes to the right bucket, and returns the stored index if present.
    # if the key is missing, return -1 (our chosen default).


# examples
print(contains(100))      # True. Jump to the 100 bucket. See if it has any values. It does because we made a 100 bucket which stores the number 2 since we made our dictionary have the keys be our array's eleements and the values be their respective indices. 
print(find_index(350))    # 4. Jump to the 350 bucket. See if it has any values. It does because we made a 350 bucket which stored 4. Return that value.  
print(find_index(999))    # -1. Jump to our 999 bucket. Oh wait. There isn't one! Nothing to return. So default to -1. 

```

## Use C++, generate hash value of your name. 1 pts

```C++
#include <iostream>
using namespace std;

int main() {
    const char* name = "Austin"; // point the the beginning of my name string
    int hash = 0;
    for (int i = 0; name[i] != '\0'; i++) {
        hash += name[i]; // add up ASCII values of my name 
    }
    cout << "Hash of " << name << " = " << hash << endl;
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
