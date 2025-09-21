
# Activity 4a: Sorting II

Video explanation: 

## Task 1: "Proof that, under the average-case scenario, the insertion sort has a time complexity of O(N<sup>2</sup>). Draw a clear figure and show all the operations clearly."

To understand the average case, let's compare it to the best case and worst case. Consider the best case. 

<img width="1032" height="615" alt="Untitled" src="https://github.com/user-attachments/assets/79831346-1e58-4a98-9fad-587b2a753d15" />

So for each element of N elements, we just compare it, see that it doesn't need to be swapped, and continue. So for each N, we always do one thing. <img width="217" height="170" alt="image" src="https://github.com/user-attachments/assets/312c4f6d-6a52-497f-a691-63c3a805865e" />




## Task 2: "At the start of the insertion sort, the index of the inspected value is set to 1. Change the index of the inspected value and verify that the total number of operations equals 20. Consider the worst-case scenario. Use N=5, where N is the number of elements."

Let's say we have an array in worst case [5, 4, 3, 2, 1]. 

Every time we make a comparison, that's a step. Every time we make a swap, that's a step.

Let's skip the first element, 5, assuming that it's in order. Let's begin with the second element 4. Now we can compare it to 5 (FIRST STEP). We see that it is in fact less than 5, so we swap it (SECOND STEP).

Now, we are at [4,5,3,2,1]. So we move on to the third element, 3. We first compare it to 5 (THIRD STEP). It's less than 5, so we swap it (FOURTH STEP). Next, we compare it to 4 (FIFTH STEP). It's less than 4, so we swap it (SIXTH STEP).

Now we have [3,4,5,2,1]. So we check the fourth element, which is 2. We compare it to 5 (SEVENTH STEP). It's less than 5 so we swap (EIGHTH STEP). Then we compare it to 4 (NINTH STEP). It's less than 4, so we swap it (TENTH STEP). Then we compare it to 3 (ELEVENTH STEP). It's less than 3 so we swap it (TWELFTH STEP). 

Now, we are at [2,3,4,5,1]. We check the fifth element, which is 1. We compare it to 5 (THIRTEENTH STEP). It's less than 5, so we swap it (FOURTEENTH STEP). We compare it to 4 (FIFTEENTH STEP). It's less than 4, so we swap it (SIXTEENTH STEP). We compare it to 3 (SEVENTEENTH STEP). It's less than 3 so swap it (EIGHTEENTH STEP). Next we compare it to 2 (NINETEENTH STEP). It's less than 2, so we swap it (TWENTIETH STEP).

In 20 steps, we sorted it to [1,2,3,4,5]. 


## Task 3: "The following function returns whether or not a capital “X” is present within a string. (a) What is this function’s time complexity regarding Big O Notation? (b) Then, modify the code to improve the algorithm’s efficiency for best- and average-case scenarios..
```c++
function containsX(string) {
	foundX = false;
	for(let i = 0; i < string.length; i++) { 
		if (string[i] === "X") {
			foundX = true; 
		}
	}
	return foundX; 
}
```
Time complexity is O(N). It goes through and compares each and every thing to look for X. In the best case, we find X right away. We should make it so that as soon as we've found it, we exit. In the average case, we still don't need to go through the whole thing, because we will find it closer to the middle of the array rather than the end or not there at all. What we should do is return as soon as we find it. We can also just assume we'll find X somewhere. That way, if we are optimizing for the average and best case (where X is at the beginning or middle respectively), we can just return the location as soon as we get it. We don't need to update the variable to true, because it's already set to true. This might not be massive, but I think it makes things a bit neater. 
```c++
function containsX(string) {
	foundX = true; 
	for(let i = 0; i < string.length; i++) { 
		if (string[i] === "X") {
            return foundX; 
		}
	}
	foundX = false;
	return foundX; 
}
```


