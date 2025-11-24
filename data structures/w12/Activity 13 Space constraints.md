# Activity 13 Space constraints

Video explanation: https://youtu.be/iDGJi80jsqA

## 1 Following is the 'Word Builder' algorithm. Describe its space complexity in terms of Big O.

```pseudocode
function wordBuilder(array) { 
		let collection = [];
		for(let i = 0; i < array.length; i++) { 
				for(let j = 0; j < array.length; j++) {
						if (i !== j) {
								collection.push(array[i] + array[j]);
						}
				}
		}
		return collection; 
}

```

It goes through every possible combination or basically n x n combinations which is O(n^2). 

## 2 Following is a function that reverses an array. Describe its space complexity in terms of Big O:

```pseudocode
function reverse(array) { 
		let newArray = [];
		for (let i = array.length - 1; i >= 0; i--) { 
				newArray.push(array[i]);
		}
		return newArray;
}
```


The function builds a brand new array and copies every element from the original into it. That output array always holds the same number of items as the input, so you store n extra elements. No nested structure, no extra buffers. Just one new array with n slots.

So the space complexity is O(n).


## 3 Create a new function to reverse an array that takes up just O(1) extra space.

````
function reverseInPlace(arr) {
  let left = 0;
  let right = arr.length - 1;

  while (left < right) {
    const temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;

    left++;
    right--;
  }

  return arr;
}
````

### 4 Following are three different implementations of a function that accepts an array of numbers and returns an array containing those numbers multiplied by 2. For example, if the input is [5, 4, 3, 2, 1], the output will be [10, 8, 6, 4, 2].

```pseudocode
function doubleArray1(array) { 
	let newArray = [];

	for(let i = 0; i < array.length; i++) { 
		newArray.push(array[i] * 2);
	}
	return newArray; 
}


function doubleArray2(array) {
	for(let i = 0; i < array.length; i++) {
  	array[i] *= 2;
  }
	return array; 
}


function doubleArray3(array, index=0) { 
	if (index >= array.length) { return; }
  array[index] *= 2;
  doubleArray3(array, index + 1);
	return array; 
}
```

Fill in the table that follows to describe the efficiency of these three versions in terms of both time and space:


Version 1 walks through the array once and builds a brand new array, so the work grows linearly with n and the extra memory also grows linearly because you store n new numbers. Version 2 also walks through the array once, so the time is linear, but it rewrites each element in the same array, so it only needs a constant amount of extra space no matter how big the input is. Version 3 does the same amount of real work as the others, so the time is linear, but the recursion forces the program to keep one stack frame per element until it reaches the end, so the space grows linearly even though it does not create a new array.
| Version    | Time complexity | Space complexity |
| ---------- | --------------- | ---------------- |
| Version #1 | O(n)              | O(n)                |
| Version #2 | O(n)               | O(1)                |
| Version #3 | O(n)               | O(n)                |

