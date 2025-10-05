 # Activity 6: Stacks and queues


## Task 1: Using Figure 17 as a model, in the book Data Structures in C++, illustrate the result of each operation in the sequence, PUSH(S,4), PUSH(S,1), PUSH(S,3), POP(S), PUSH(S,8), and POP(S) on an initially empty stack S  stored in array S[1...6]. ode is not required. 3 pts

Stacks are last in, first out. So to push things we just drop it in the top of the stack. To pop, we just take out from the top of the stack.

<img width="2044" height="3266" alt="Untitled" src="https://github.com/user-attachments/assets/4610f6e9-d266-41b3-a431-bda66289cfa5" />


## Task 2: Using Figure 18 as a model, in the book Data Structures in C++, illustrate the result of each operation in the sequence ENQUEUE(Q,4), ENQUEUE(Q,1), ENQUEUE(Q,3), DEQUEUE(Q), ENQUEUE(Q,8), and DEQUEUE(Q) on an initially empty queue Q stored in array Q[1...6]  Code is not required. 3 pts

Our head is basically the "first person in line". And the tail where the next person would go in the back of the line. When we start, the head and tail are the same. There is no person to stand behind to go to the back of the line. As elements line up after the head, we push the tail of the queue back. As the element at the head is dequeued, the element that was dequeued imemdiately after them (i.e the next element after the head) now becomes the head of the queue, just like in a regular line. 

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
<img width="2044" height="1829" alt="Untitled" src="https://github.com/user-attachments/assets/6db00e45-1eba-427e-8aee-e04611de072d" />

As we can see, an overflow either means that head is at position 1 and tail is equal to length. Or it means that tail has wrapped arround and gone back to right below the head, so head == tail + 1. We can check for those. 

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

This is our current pseudocode for DEQUEUE. 

```psuedocode
x = Q[Q.head]
if Q.head == Q.length
    Q.head = 1
else Q.head = Q.head + 1
return x
```
<img width="2044" height="1411" alt="Untitled" src="https://github.com/user-attachments/assets/16021f7e-cbda-4b3b-af5f-00b50baa0e73" />

So we can update our pseudocode to check for head == tail. 

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

1. INSERT-FRONT. Someone cutting in front of the line. 

Check for overflow (the deque is full if advancing either end would collide).

Decrement head (wrapping to the end if head == 1).

Place the new element at Q[head].


2. INSERT-BACK. Someone getting to the back of the line, like we've already seen in a normal queue. 

Check for overflow.

Place the new element at Q[tail].

Increment tail (wrapping to 1 if tail == Q.length).


3. DELETE-FRONT. The person at the front of the line is ready. This is how we've already seen a dequeue like a normal line works. 

Check for underflow (empty if head == tail).

Retrieve Q[head].

Increment head (wrapping to 1 if head == Q.length).


4. DELETE-BACK. This is when the last person in line leaves first, just like popping from a stack. 

Check for underflow.

Decrement tail (wrapping to Q.length if tail == 1).

Retrieve Q[tail].


How can we be sure that this is O(1)? As we've seen before, checking overflow and underflow always just involves looking at the index of head and the index of tail. If there are 10 elements, there will be one head and one tail. If there are 10,000 ekements, there will be one head and one tail. Similarly, any insertion or deletion just invovles accessing an element of the array at what is currently pointed to by the head or by the tail. We don't need to shift all the elements. We don't need to loop through anything. We access the head, of which there is always one and only one. We access the tail, of which there is always one and only one. 
