
# Activity 2: Searching 

Video explanation:

## Task 1: "How many steps would it take to perform a linear search for the number 8 in the ordered array, [2, 4, 6, 8, 10, 12, 13]?"

This would take four steps. First, it would check 2, which is not 8. Second, it would look for 4, which is not 8. Third it would look for 6, which is not 8. Fourth, it would look for 8, which is 8. At that point it does not continue since it found 8.

## Task 2: "How many steps would binary search take for the previous example?"

One step. Start (0 + 6) / 2 = 3. Floor of 3 is 3. Check element 3, which is 8. You are already done! 

## Task 3: "What is the maximum number of steps it would take to perform a binary search on an array of size 100,000?"

17 steps. Binary search complexity increases with <img width="43.5" height="23" alt="image" src="https://github.com/user-attachments/assets/9bb5b891-9a2d-47ee-9f66-60508b243bed" /> therefore we take log base 2 of 100,000, which is 16 and change. Therefore, 16 steps would not be enough in all cases. Round up to 17. 2 to the power of 16 is less than 100,000 but 2 to the power of 17 is more than 100,000. So it would take 17 steps at most. 


<img width="366" height="93" alt="image" src="https://github.com/user-attachments/assets/a9e18d4f-fad7-4a63-bf64-25ba9adc7124" />
Credit: https://www.calculator.net/log-calculator.html?xv=100000&base=2&yv=&x=Calculate

## Task 4: "Write a C++ code that implements the linear and binary search algorithms. The algorithm should be able to calculate the number of steps against the given search." 

```c++
#include <iostream>
using namespace std;

/*
    Demo: linear search vs binary search on a sorted array.
    Array A has 32 integers sorted in ascending order.
    We search for target T twice: first linearly, then with binary search.
    Outputs the found index (or -1 if not found) and shows steps and count of steps taken. 
*/

int main() {
    // Number of elements in the array.
    const int N = 32;

    // Sorted array with values 10, 20, ..., 320.
    // Binary search assumes the data is sorted in nondecreasing order.
    const int A[N] = {
        10, 20, 30, 40, 50, 60, 70, 80,
        90, 100, 110, 120, 130, 140, 150, 160,
        170, 180, 190, 200, 210, 220, 230, 240,
        250, 260, 270, 280, 290, 300, 310, 320
    };

    // Target value to search for.
    const int T = 10;

    // Announce the target being searched.
    cout << "Looking for " << T << "\n";

    // -------------------------
    // Linear search
    // -------------------------

    // steps_lin counts how many element checks the linear search performs.
    int steps_lin = 0;

    // idx_lin stores the index where T is found.
    // Initialize to -1 to indicate "not found" until proven otherwise.
    int idx_lin = -1;

    // Loop over all elements from index 0 to N-1.
    for (int i = 0; i < N; ++i) {
        // Increment the step counter because we are about to check A[i].
        ++steps_lin;  // increment

        // Compare the current element to the target.
        if (A[i] == T) {
            // If equal, record the index where T was found.
            idx_lin = i;

            // Exit the loop early because we already found T.
            break;
        }
        // If not equal, the loop continues to the next i automatically.
    }

    // Report the result of linear search: index where T was found and steps taken.
    cout << "[linear] index=" << idx_lin << " steps=" << steps_lin << "\n\n";

    // -------------------------
    // Binary search
    // -------------------------

    cout << "[binary]\n\n";

    // steps_bin counts how many midpoint checks the binary search performs.
    int steps_bin = 0;

    // idx_bin stores the index where T is found.
    // Initialize to -1 to indicate "not found" until proven otherwise.
    int idx_bin = -1;

    // L and U define the current inclusive search bounds: [L, U].
    // Start with the full array: lower bound L at 0 and upper bound U at N-1.
    int L = 0;
    int U = N - 1;

    // Continue as long as there is a valid range to inspect.
    while (L <= U) {
        // Compute midpoint index without risking overflow:
        // m = L + (U - L) / 2
        int m = L + (U - L) / 2;

        // Increment the step counter because we are about to check A[m].
        ++steps_bin;  // increment

        // Print a narration line describing this step.
        cout << "[step " << steps_bin << "] midpoint of [" << L << "," << U
             << "] -> m=" << m << "; A[m]=" << A[m];

        // Compare the midpoint value to the target.
        if (A[m] < T) {
            // Midpoint value is less than T, so T can only be to the right.
            // Narrow the search to the right half by moving L past m.
            L = m + 1;  // set new lower bound to m + 1
            cout << " too low, set L=m+1 -> L=" << L << "\n";
        } else if (A[m] > T) {
            // Midpoint value is greater than T, so T can only be to the left.
            // Narrow the search to the left half by moving U before m.
            U = m - 1;  // set new upper bound to m - 1
            cout << " too high, set U=m-1 -> U=" << U << "\n";
        } else {
            // A[m] equals T, so we found the target at index m.
            idx_bin = m;  // record found index
            cout << " equal to T, found at index " << m << "\n";

            // Exit the loop because the search is complete.
            break;
        }
    }

    // Print a blank line for spacing.
    cout << "\n";

    // Report the result of binary search: index where T was found and steps taken.
    cout << "index=" << idx_bin << " steps=" << steps_bin << "\n";

    // Return 0 to indicate successful program termination.
    return 0;
}

```
