# Activity 9 Binary heaps

## 1: Draw what the following heap would look like after we insert the value 11 into it:


<img width="817" height="550" alt="image" src="https://github.com/user-attachments/assets/dfa532c3-4347-4ee0-8dfa-9064baf9997d" />


<img width="818" height="548" alt="image" src="https://github.com/user-attachments/assets/a2666ddf-b99f-43ca-b195-108ab9369293" />


## 2. Draw what the previous heap would look like after we delete the root node.

<img width="817" height="550" alt="image" src="https://github.com/user-attachments/assets/8dee6597-a831-40a9-a027-fa10df148228" />

## 3. Imagine you’ve built a brand-new heap by inserting the following numbers into the heap in this particular order: 55, 22, 34, 10, 2, 99, 68. If you then pop them from the heap one at a time and insert the numbers into a new array, in what order would the numbers now appear?

It's just in descending order {99, 68, 55, 34, 22, 10, 2}.

Consider two facts:

1. The delete function always grabs the root node.
2. The root node must always be the node of highest value.

Therfore, successive deletion will delete items in descending order. 
