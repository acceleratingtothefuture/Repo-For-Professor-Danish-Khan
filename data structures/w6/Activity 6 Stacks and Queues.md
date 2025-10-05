 # Stacks and queues


## Task 1: Using Figure 17 as a model, in the book Data Structures in C++, illustrate the result of each operation in the sequence, PUSH(S,4), PUSH(S,1), PUSH(S,3), POP(S), PUSH(S,8), and POP(S) on an initially empty stack S  stored in array S[1...6]. ode is not required. 3 pts

 <img width="2044" height="1996" alt="Untitled" src="https://github.com/user-attachments/assets/b7e9ead2-53fd-4c58-83c6-db4c5a7279f9" />

## Task 2: Using Figure 18 as a model, in the book Data Structures in C++, illustrate the result of each operation in the sequence ENQUEUE(Q,4), ENQUEUE(Q,1), ENQUEUE(Q,3), DEQUEUE(Q), ENQUEUE(Q,8), and DEQUEUE(Q) on an initially empty queue Q stored in array Q[1...6]  Code is not required. 3 pts

<img width="2044" height="2244" alt="Untitled" src="https://github.com/user-attachments/assets/4e20e7fe-8128-408f-917e-2ba0e21610e5" />

## Task 3: Rewrite ENQUEUE and DEQUEUE to detect underflow and overflow of a queue. (see Listings 4 & 5 in the book). Code is not required. 1 pt

Let's define overflow and underflow. Overflow refers to the act of performing an ENQUEUE to a full queue. Underflow refers to performing a DEQUEUE to an empty queue. Therefore, an overflow is something that only needs to be addressed in our ENQUEUE code and a an underflow in our DEQUEUE. 

Currently, this is our listing 4, our pseudocode for ENQUEUE. 

```pseudocode
Q[Q.tail] = x
if Q.tail == Q.length
    Q.tail = 1
else Q.tail = Q.tail + 1
```

```pseudocode
if Q.head == Q.tail + 1 or (Q.head == 1 and Q.tail == Q.length)
    report overflow
else
    Q[Q.tail] = x
    if Q.tail == Q.length
        Q.tail = 1
    else
        Q.tail = Q.tail + 1
```

dequeue: 

```psuedocode
x = Q[Q.head]
if Q.head == Q.length
    Q.head = 1
else Q.head = Q.head + 1
return x
```

```pseudocode
if Q.head == Q.tail
    report underflow
else
    x = Q[Q.head]
    if Q.head == Q.length
        Q.head = 1
    else
        Q.head = Q.head + 1
    return x
```

## Task 4 A stack allows insertion and deletion of elements at only end, and a queue allows insertion at one end and deletion at the other end, a deque (double-ended queue) allows insertion and deletion at both ends. Write four O(1) time procedures to insert elements into and delete elements from both ends of a deque implemented by an array. Code is not required. 3 pts

The four procedures are 1) inserting front 2) inserting back 3) deleting front 4) deleting back. 

1. INSERT-FRONT

Check for overflow (the deque is full if advancing either end would collide).

Decrement head (wrapping to the end if head == 1).

Place the new element at Q[head].

2. INSERT-BACK

Check for overflow.

Place the new element at Q[tail].

Increment tail (wrapping to 1 if tail == Q.length).

3. DELETE-FRONT

Check for underflow (empty if head == tail).

Retrieve Q[head].

Increment head (wrapping to 1 if head == Q.length).

4. DELETE-BACK

Check for underflow.

Decrement tail (wrapping to Q.length if tail == 1).

Retrieve Q[tail].

All four take O(1) time because each does only a few pointer updates and at most one array access, no loops or shifting of elements.
