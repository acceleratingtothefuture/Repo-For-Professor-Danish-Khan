# Activity 12: Recursions
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

## 2. My kid was playing with my computer and changed my factorial function so that it computes factorial based on (n - 2) instead of (n - 1). Predict what will happen when we run factorial(10) using this function:

```pseudocode
def factorial(n)
    return 1 if n == 1
    return n * factorial(n - 2)
end
```

It will overflow and run infinitely. The code will multiply every other number from 10, so 10 then 8 then 4, then 2, then skipping over 1 and go to 0 then -2, then -4 never reaching a base case. 

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

