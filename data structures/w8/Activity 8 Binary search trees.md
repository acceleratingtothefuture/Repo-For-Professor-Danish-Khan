# Activity 8 Binary search trees

## Task 1: Imagine you were to take an empty binary search tree and insert the following sequence of numbers in this order: [1, 5, 9, 2, 4, 10, 6, 3, 8]. Draw a diagram showing what the binary search tree would look like. Remember, the numbers are being inserted in the order presented here.

<img width="2531" height="1996" alt="Untitled" src="https://github.com/user-attachments/assets/6f5b9576-d440-460c-aa0b-091df8e2e5ec" />

## Task 2: If a well-balanced binary search tree contains 1,000 values, what is the maximum number of steps it would take to search for a value within it?

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
