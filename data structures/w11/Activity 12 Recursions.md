# Activity 12: Recursions
Video explanation: https://youtu.be/rr0m6TMHNRo

## 1. The following function prints every other number from a low number to a high number. For example, if low is 0 and high is 10, it would print:

```pseudocode
0
2
4
6
8
10
```

Identify the base case in the function:

```pseudocode
def print_every_other(low, high) 
    return if low > high
    puts low
    print_every_other(low + 2, high)
end
```

The base case is the condition that stops the recursion:

low > high

When that condition is true, the function returns and the recursive calls stop. In this case, its once low = 12. 

```c++
#include <iostream>
using namespace std;

void print_every_other(int low, int high) {
    // base case
    if (low > high) {
        return;
    }

    cout << low << endl;
    print_every_other(low + 2, high);
}

int main() {
    print_every_other(0, 10);
    return 0;
}


```

## 2. My kid was playing with my computer and changed my factorial function so that it computes factorial based on (n - 2) instead of (n - 1). Predict what will happen when we run factorial(10) using this function:

```pseudocode
def factorial(n)
    return 1 if n == 1
    return n * factorial(n - 2)
end
```

It will overflow and run infinitely. The code will multiply every other number from 10, so 10 then 8 then 6 then 4, then 2, then skipping over 1 and go to 0 then -2, then -4 never reaching a base case. 

This code won't print: 

```c++

#include <iostream>
using namespace std;

long long factorial(int n) {
    if (n == 1) {
        return 1;
    }
    return n * factorial(n - 2);
}

int main() {
    cout << factorial(10) << endl;
    return 0;
}

```

## 3. Following is a function in which we pass in two numbers called low and high. The function returns the sum of all the numbers from low to high. For example, if low is 1, and high is 10, the function will return the sum of all numbers from 1 to 10, which is 55. However, our code is missing the base case, and will run indefinitely! Fix the code by adding the correct base case

```pseudocode
def sum(low, high)
    return high + sum(low, high - 1)
end
```
The code continues to lower high down until it gets to the base case. In the above example, the base case is when we finally get down to 1, which is low. So it should be return low if high == low
```pseudocode
def sum(low, high)
    return low if high == low
    return high + sum(low, high - 1)
end
```

```c++
#include <iostream>
using namespace std;

int sum(int low, int high) {
    if (high == low) {
        return low;
    }
    return high + sum(low, high - 1);
}

int main() {
    cout << sum(1, 10) << endl; // prints 55
    return 0;
}

```
## 4. Here is an array containing both numbers as well as other arrays, which in turn contain numbers and arrays:

```pseudocode
array=[ 1, 
        2, 
        3,
        [4, 5, 6],
        7,
        [8,
          [9, 10, 11,
            [12, 13, 14]
          ] 
        ],
        [15, 16, 17, 18, 19,
          [20, 21, 22,
            [23, 24, 25,
              [26, 27, 29]
            ], 30, 31 
          ], 32
        ], 33 
      ]
```
Write a recursive function that prints all the numbers (and just numbers).


```pseudocode
def print_numbers(item)
    if item is a number
        puts item
        return
    end

    # item is an array
    for element in item
        print_numbers(element)
    end
end

# call it like this:
print_numbers(array)
```

```c++
#include <iostream>
#include <vector>
using namespace std;

// An Item represents either:
// 1) a single number
// 2) a list of other Items
struct Item {
    // tells us which kind this Item is
    bool isNumber;

    // valid only if isNumber == true
    int number;

    // valid only if isNumber == false
    vector<Item> list;

    // constructor for a number
    // lets us write: Item x = 5;
    Item(int n) : isNumber(true), number(n) {}

    // constructor for a list
    // lets us write: Item x = {1, 2, {3, 4}};
    Item(initializer_list<Item> l) : isNumber(false), list(l) {}
};

// recursive function that prints all numbers
void print_numbers(const Item& item) {

    // base case:
    // if this Item is a number, print it and stop
    if (item.isNumber) {
        cout << item.number << endl;
        return;
    }

    // recursive case:
    // this Item is a list, so walk through it
    for (const auto& element : item.list) {
        // recurse on each element, which may itself
        // be a number or another list
        print_numbers(element);
    }
}

int main() {

    // build a nested structure that matches the given array
    // numbers and lists are freely mixed
    Item array = {
        1,
        2,
        3,
        {4, 5, 6},
        7,
        {
            8,
            {
                9, 10, 11,
                {12, 13, 14}
            }
        },
        {
            15, 16, 17, 18, 19,
            {
                20, 21, 22,
                {
                    23, 24, 25,
                    {26, 27, 29}
                },
                30, 31
            },
            32
        },
        33
    };

    // start the recursion at the top level
    print_numbers(array);

    return 0;
}


```

