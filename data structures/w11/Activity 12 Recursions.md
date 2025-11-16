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





