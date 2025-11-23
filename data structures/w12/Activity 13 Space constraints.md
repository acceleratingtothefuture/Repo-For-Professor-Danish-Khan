# Activity 13 Space constraints

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
