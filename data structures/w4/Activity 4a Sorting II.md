
# Activity 4a: Sorting II

Video explanation: 

## Task 1: "Proof that, under the average-case scenario, the insertion sort has a time complexity of O(N<sup>2</sup>). Draw a clear figure and show all the operations clearly."

To understand the average case, let's compare it to the best case and worst case. Consider the best case. Notice how each comparison gives i > (i - 1) and no further swapping or comparison is needed. 


<img width="1022" height="901" alt="Untitled" src="https://github.com/user-attachments/assets/cb479b20-f242-4312-b148-7d1bc2d2792d" />

Let's calculate the time complexity of the best case. 

So for each element of N elements, we just compare it, see that it doesn't need to be swapped, and continue. So for each N, we always do one step.

<img width="108" height="85" alt="image" src="https://github.com/user-attachments/assets/312c4f6d-6a52-497f-a691-63c3a805865e" />

Except we don't need to do the very first one.

<img width="257" height="86" alt="image" src="https://github.com/user-attachments/assets/5c222664-9f9c-4a29-8f23-34f7cb376500" /> 

So it's N - 1. The best case time complexity is therefore O(N). 



Before we look at the worst case scenario, let's recall the sum of natural numbers from 1 to N. 

<img width="2044" height="1996" alt="Untitled" src="https://github.com/user-attachments/assets/3b820332-fc47-4a68-9220-0504abecc88a" />


## Worst Case Scenario

In the worst case, it will be totally out of order and each element will need to be compared and swapped all the way back to the beginning. 


<img width="838" height="737" alt="image" src="https://github.com/user-attachments/assets/c8b73412-d313-4b89-b0e3-8fdbc7f3ca63" />


Let's calculate the time complexity of the average case. 

At element i, we have to do a compare and a switch for all the elements before it. So two steps, (i - 1) times. 

<img width="260" height="153" alt="image" src="https://github.com/user-attachments/assets/c4118672-d468-4dae-b69e-bf006c37d450" />

Let's factor out the 2. We can multiply it out at the end. 

<img width="317" height="176" alt="image" src="https://github.com/user-attachments/assets/6178be71-a22c-4377-a630-8036c88a253d" />

So for each i, we add itself, then we subtract one. So its really a difference of two summations: 
1) the summation of every natural number 1 to N we discussed earlier.
2) adding 1 for every number from 1 to N.

<img width="488" height="162" alt="image" src="https://github.com/user-attachments/assets/fab23e72-bc4f-4101-ac00-0c99569a04c6" />

We already said that 

<img width="610" height="162" alt="image" src="https://github.com/user-attachments/assets/86aac5f5-c8a3-44af-a40f-56747ddf36b2" />

So

<img width="565" height="400" alt="image" src="https://github.com/user-attachments/assets/0795cc81-8744-4c6a-8358-cab6ff5d043f" />


Multiply the 2 out. 

<img width="497" height="132" alt="image" src="https://github.com/user-attachments/assets/7fdf4daf-1c11-492c-a0eb-6d724001babd" />


Mulitply the N out. 

<img width="431" height="117" alt="image" src="https://github.com/user-attachments/assets/f86ba7c2-79d8-48eb-aec3-8be8a70d9495" />


Subtract.

<img width="238" height="108" alt="image" src="https://github.com/user-attachments/assets/6c070326-41bf-4f0f-98f4-2e61eb0b408d" />

The degree is 2. So according to the rules of big O notation, we only care about the highest degree, therefore the worst case time complexity is O(N<sup>2</sup>). 


## Average Case Scenario

<img width="1022" height="1039" alt="Untitled" src="https://github.com/user-attachments/assets/4a6cd9d9-f78e-463a-8f16-3f7df0a7133d" />

Let's calculate the time complexity of the worst case. 


So each element i probably averages out to about half of its worst case number of steps. 

<img width="466" height="225" alt="image" src="https://github.com/user-attachments/assets/f5daf2ae-7eb8-46f3-92f8-53c6059f57fb" />


Instead of cancelling out the 1/2 and the 2, we can factor out the 1/2. 

<img width="438" height="227" alt="image" src="https://github.com/user-attachments/assets/1f4f2f9b-ec22-434b-9b6e-26125627012e" />


We already found what everything to the right of 1/2 was (the worst case scenario.)

<img width="238" height="108" alt="image" src="https://github.com/user-attachments/assets/6c070326-41bf-4f0f-98f4-2e61eb0b408d" />


So it's just half of that.

<img width="377" height="142" alt="image" src="https://github.com/user-attachments/assets/72d9239c-d939-4cd3-96e6-9afd3ea2b565" />


We can distribute the 1/2 

<img width="342" height="122" alt="image" src="https://github.com/user-attachments/assets/c6f11c3f-d70c-4e16-998d-7ae06fae755e" />

But in big O, we don't care about coeffecients or lesser degrees. We care about the N<sup>2</sup>. Therefore, average case time complexity is O(N<sup>2</sup>). 


## Task 2: "At the start of the insertion sort, the index of the inspected value is set to 1. Change the index of the inspected value and verify that the total number of operations equals 20. Consider the worst-case scenario. Use N=5, where N is the number of elements."

Let's say we have an array in worst case [5, 4, 3, 2, 1]. 

Every time we make a comparison, that's a step. Every time we make a swap, that's a step.


As mentioned above, skip the first element, 5. We just assume that element 0 is in order. 

### Index = 1
Let's begin with the second element 4. Now we can compare it to 5 (FIRST STEP). We see that it is in fact less than 5, so we swap it (SECOND STEP).

### Index = 2
Now, we are at [4,5,3,2,1]. So we move on to the third element, 3. We first compare it to 5 (THIRD STEP). It's less than 5, so we swap it (FOURTH STEP). Next, we compare it to 4 (FIFTH STEP). It's less than 4, so we swap it (SIXTH STEP).

### Index = 3
Now we have [3,4,5,2,1]. So we check the fourth element, which is 2. We compare it to 5 (SEVENTH STEP). It's less than 5 so we swap (EIGHTH STEP). Then we compare it to 4 (NINTH STEP). It's less than 4, so we swap it (TENTH STEP). Then we compare it to 3 (ELEVENTH STEP). It's less than 3 so we swap it (TWELFTH STEP). 

### Index = 4
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
Time complexity is O(N). It goes through and compares each and every thing to look for X. Each step is just a comparison to X per element in the array of N elements. In the best case, we find X right away. We should make it so that as soon as we've found it, we exit. We don't need to keep searching. What we should do is return as soon as we find it. That way, if we are optimizing for the average and best case (where X is at the beginning or around the middle respectively, i.e we don't need to look through each one). We can just return the location as soon as find X. 

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

We don't need to update the variable to true, because it's already set to true. This might not be massive, but I think initalizing the boolean as true makes it better for scenarios where we do find X (not the worst case). 
