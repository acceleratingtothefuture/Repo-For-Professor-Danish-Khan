# Array data structure assignment 
## Task 1: "Explain how to create an array of 100 elements. You can choose any data type of your choice."

We can create arrays in C++ by specifying the data type and size in square brackets.
```c++

#include <iostream>
using namespace std;

int main() {
    // Create an array of 5 integers and assign values manually
    int numbers[5] = {10, 20, 30, 40, 50};

    // Print elements
    for (int i = 0; i < 5; i++) {
        cout << "numbers[" << i << "] = " << numbers[i] << endl;
    }

    return 0;
}

```
The above array has 5 elements, but the same could be done for 100 elements. 


For big arrays, it's likely that we should just make the array all at once. 

```c++
#include <iostream>
using namespace std;
int main() {
    // Create an array of 100 integers
    int bigArray[100];

    // Print first 10 elements before initialization
    cout << "Before initialization:" << endl;
    for (int i = 0; i < 10; i++) {
        cout << "bigArray[" << i << "] = " << bigArray[i] << endl;
    }

    // Initialize elements with their index value
    for (int i = 0; i < 100; i++) {
        bigArray[i] = i;
    }

    // Print first 10 elements after initialization
    cout << "\nAfter initialization:" << endl;
    for (int i = 0; i < 10; i++) {
        cout << "bigArray[" << i << "] = " << bigArray[i] << endl;
    }

    return 0;
}

```

## Task 2: "What will be the size of each element of an array?"

In C++, the `sizeof()` operator tells us how many bytes each element uses in memory.  
The size depends on the data type of the array elements.

```c++
#include <iostream>
using namespace std;

int main() {
    int intArray[100];
    cout << "int: " << sizeof(intArray[0]) << " bytes" << endl;

    short shortArray[100];
    cout << "short: " << sizeof(shortArray[0]) << " bytes" << endl;

    long longArray[100];
    cout << "long: " << sizeof(longArray[0]) << " bytes" << endl;

    long long longLongArray[100];
    cout << "long long: " << sizeof(longLongArray[0]) << " bytes" << endl;

    bool boolArray[100];
    cout << "bool: " << sizeof(boolArray[0]) << " byte(s)" << endl;

    char charArray[100];
    cout << "char: " << sizeof(charArray[0]) << " byte" << endl;

    double doubleArray[100];
    cout << "double: " << sizeof(doubleArray[0]) << " bytes" << endl;

    float floatArray[100];
    cout << "float: " << sizeof(floatArray[0]) << " bytes" << endl;

    size_t sizeTArray[100];
    cout << "size_t: " << sizeof(sizeTArray[0]) << " bytes" << endl;

    return 0;
}

```

## Task 3: For an array containing 100 elements, provide the number of steps the following operations would take:
Reading

Searching for a value not contained within the array

Insertion at the beginning of the array

Insertion at the end of the array

Deletion at the beginning of the array

Deletion at the end of the array

Let's briefly review the idea of an array. Basically, order of element is dependant on address. They don't point to another location in memory. Rather their addresses themselves imply what is before and after. Searching simply involves checking each address, whereas changing it would require pushing everything later in the list because there is now an abscence in the pattern.

## 3.1: Searching for a value not contained within the array
This would take 100 steps. The way search works, the computer just looks at each value until it finds the right one. So it's just going to go through each one, and each time say "nope, not it" and try the next one until it is out of options.

## 3.2: Insertion at the beginning of the array
101 steps. The beginning of an array is at index 0. And again, the whole order is determined by what is next in the address. If you want to insert something in the beginning, you have to push everything to the right (raise their position each by one), 100 steps, and then your insertion. so 101 steps. 

## 3.3: Insertion at the end of the array
1 step. All we have to do is tack on a new memory address to the end of the array, which is just one step. 

## 3.4: Deletion at the beginning of the array
100 steps. Rather than pushing everything to the left to make room for a new element 0, we're now getting rid of element 0 and pushing each element back 1 position lower. So we delete which is one step, and then move 99 to the left (lower their position each by one), which is 100 steps. 

## 3.5: Deletion at the end of the array
1 step. You don't need to change the addresses of anything. You just delete the one on the end which is one step. 


<img width="429" height="195" alt="image" src="https://github.com/user-attachments/assets/a6faebae-c731-4f64-b97b-7246fba5218b" />
image credit: https://media.geeksforgeeks.org/wp-content/uploads/array-2.png


## Task 4: Normally the search operation in an array looks for the first instance of a given value. But sometimes we may want to look for every instance of a given value. For example, say we want to count how many times the value “apple” is found inside an array. How many steps would it take to find all the “apples”? Give your answer in terms of N.
It would take N steps. You'd have to individually check each one for being apple or not. N steps no matter what.  


## Task 5: Research how to find the memory address of an array. You can use any programming language of your choice. 
```C++
#include <iostream>
using namespace std;

int main() {
    int arr[5] = {1, 2, 3, 4, 5};

    cout << "arr: " << arr << endl;       // prints address of first element
    cout << "&arr: " << &arr << endl;     // technically same address for whole array
    cout << "&arr[0]: " << &arr[0] << endl; // explicitly first element

    return 0;
}


```
