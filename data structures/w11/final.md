# Final Project
Video explanation: 

## Task 1

You’re working on software that analyzes sports players. Following are two arrays of players of different sports:

```
basketball_players = [
      {first_name: "Jill", last_name: "Huang", team: "Gators"},
      {first_name: "Janko", last_name: "Barton", team: "Sharks"},
      {first_name: "Wanda", last_name: "Vakulskas", team: "Sharks"},
      {first_name: "Jill", last_name: "Moloney", team: "Gators"},
      {first_name: "Luuk", last_name: "Watkins", team: "Gators"}
]

football_players = [
      {first_name: "Hanzla", last_name: "Radosti", team: "32ers"},
      {first_name: "Tina", last_name: "Watkins", team: "Barleycorns"},
      {first_name: "Alex", last_name: "Patel", team: "32ers"},
      {first_name: "Jill", last_name: "Huang", team: "Barleycorns"},
      {first_name: "Wanda", last_name: "Vakulskas", team: "Barleycorns"}
]
```

If you look carefully, you’ll see that some players participate in more than one sport. Jill Huang and Wanda Vakulskas play both basketball *and* football.

You are to write a function that accepts two arrays of players and returns an array of the players who play in *both* sports. In this case, that would be:

`["Jill Huang", "Wanda Vakulskas"]`

While there are players who share first names and players who share last names, we can assume there’s only one person who has a particular *full* name (meaning first *and* last name).

We can use a nested-loops approach, comparing each player from one array against each player from the other array, but this would have a runtime of $O(N * M)$. 

**Your job is to optimize the function so that it can run just $O(N + M)$.**

Idea:

Walk one list and store full names in an unordered_set.

Walk the second list and check membership.

Build result when there’s a match.
```c++

#include <iostream> // For std::cout and std::endl
#include <vector>   // For std::vector
#include <string>   // For std::string

using namespace std;

// This struct represents a single player.
// Each player has a first name, a last name, and a team name.
struct Player {
    string first_name;
    string last_name;
    string team;
};

// This function finds players who are in BOTH sports.
// It takes two vectors of Player objects:
//   - basketball_players: all the basketball players
//   - football_players: all the football players
// It returns a vector of strings, where each string is a full name like "Jill Huang".
vector<string> playersInBothSports(
    const vector<Player>& basketball_players,
    const vector<Player>& football_players
) {
    // This vector will hold the final result.
    // Each entry is the full name of a player who appears in both lists.
    vector<string> result;

    // Loop over every player in the basketball list.
    for (int i = 0; i < basketball_players.size(); i++) {

        // Build the basketball player's full name: "First Last".
        string full_name_basketball =
            basketball_players[i].first_name + " " + basketball_players[i].last_name;

        // We only want to add each common player once.
        // So before we do the nested search, we check if we already
        // added this name to the result vector.
        bool already_in_result = false;

        // Look through the result vector to see if full_name_basketball is already there.
        for (int r = 0; r < result.size(); r++) {
            if (result[r] == full_name_basketball) {
                // If we find it, we mark the flag and break out of this loop.
                already_in_result = true;
                break;
            }
        }

        // If the name is already in the result, we skip the rest of this iteration.
        if (already_in_result) {
            continue; // Go straight to the next basketball player.
        }

        // Now we loop over every player in the football list
        // to see if this basketball player also plays football.
        for (int j = 0; j < football_players.size(); j++) {

            // Build the football player's full name: "First Last".
            string full_name_football =
                football_players[j].first_name + " " + football_players[j].last_name;

            // Compare the full names. If they match, then this person
            // plays both basketball and football.
            if (full_name_basketball == full_name_football) {

                // Add the common player's full name to the result vector.
                result.push_back(full_name_basketball);

                // We found a match for this basketball player,
                // so we can stop checking the remaining football players.
                break;
            }
        }
    }

    // Return the vector of names that are in both sports.
    return result;
}

int main() {
    // Create the list of basketball players.
    // Each entry is a Player struct with first name, last name, and team.
    vector<Player> basketball_players;
    basketball_players.push_back(Player{"Jill", "Huang", "Gators"});
    basketball_players.push_back(Player{"Janko", "Barton", "Sharks"});
    basketball_players.push_back(Player{"Wanda", "Vakulskas", "Sharks"});
    basketball_players.push_back(Player{"Jill", "Moloney", "Gators"});
    basketball_players.push_back(Player{"Luuk", "Watkins", "Gators"});

    // Create the list of football players.
    vector<Player> football_players;
    football_players.push_back(Player{"Hanzla", "Radosti", "32ers"});
    football_players.push_back(Player{"Tina", "Watkins", "Barleycorns"});
    football_players.push_back(Player{"Alex", "Patel", "32ers"});
    football_players.push_back(Player{"Jill", "Huang", "Barleycorns"});
    football_players.push_back(Player{"Wanda", "Vakulskas", "Barleycorns"});

    // Call the function to find players who are in both sports.
    vector<string> both = playersInBothSports(basketball_players, football_players);

    // Print each name on its own line.
    cout << "Players in both sports:" << endl;
    for (int i = 0; i < both.size(); i++) {
        cout << both[i] << endl;
    }

    // Indicate successful program end.
    return 0;
}

```

Time complexity:

Insertions: O(N)

Lookups: O(M)

Total runtime: O(N + M)
Space: O(N)

## Task 2

You’re writing a function that accepts an array of distinct integers from 0, 1, 2, 3...up to N. However, the array will be missing one integer, and your function is to *return the missing one.*

For example, this array has all the integers from 0 to 6, but is missing the 4:

```
[2, 3, 0, 6, 1, 5]
```

Therefore, the function should return 4.

The next example has all the integers from 0 to 9, but is missing the 1:

```
[8, 2, 3, 9, 4, 7, 5, 0, 6]
```

In this case, the function should return the 1.

Using a nested-loops approach would take up to $O(N^2)$. 

**Your job is to optimize the code so that it has a runtime of $O(N)$.**

You have an array of length N containing distinct integers from 0 to N, with one missing. Use the sum formula or XOR. Sum is straightforward:

```c++
#include <iostream>
#include <vector>

int findMissingNumber(const std::vector<int>& nums) {
    int n = nums.size();                // array length is N
    long long expected = 1LL * n * (n + 1) / 2;  // sum 0..N
    long long actual = 0;

    for (int x : nums) {
        actual += x;
    }

    return static_cast<int>(expected - actual);
}

int main() {
    std::vector<int> a = {2, 3, 0, 6, 1, 5};          // missing 4
    std::vector<int> b = {8, 2, 3, 9, 4, 7, 5, 0, 6}; // missing 1

    std::cout << findMissingNumber(a) << "\n"; // 4
    std::cout << findMissingNumber(b) << "\n"; // 1

    return 0;
}

```

This runs in O(N) time and O(1) extra space.

## Task 3

You’re working on some more stock-prediction software. The function you’re writing accepts an array of predicted prices for a particular stock over the course of time.

For example, this array of seven prices:

```
[10, 7, 5, 8, 11, 2, 6]
```

predicts that a given stock will have these prices over the next seven days. (On Day 1, the stock will close at \$10; on Day 2, the stock will close at $7; and so on.)

Your function should calculate the greatest profit that could be made from a single “buy” transaction followed by a single “sell” transaction.

In the previous example, the most money could be made if we bought the stock when it was worth \$5 and sold it when it was worth \$11. This yields a profit of $6 per share.

Note that we could make even more money if we buy and sell multiple times, but for now, this function focuses on the most profit that could be made from just *one* purchase followed by *one* sale.

Now, we could use nested loops to find the profit of every possible buy and sell combination. However, this would be $O(N^2)$ and too slow for our hotshot trading platform. 

**Your job is to optimize the code so that the function clocks in at just $O(N)$.**

Keep track of the lowest price you have seen so far.

For each day after that, pretend you sell that day and calculate the profit.

Remember the biggest profit you ever saw.

```c++
#include <iostream>
using namespace std;

int maxProfit(int prices[], int size) {
    int minPrice = prices[0];
    int maxProfit = prices[1] - prices[0];

    for (int i = 1; i < size; i++) {
        int profit = prices[i] - minPrice;

        if (profit > maxProfit) {
            maxProfit = profit;
        }

        if (prices[i] < minPrice) {
            minPrice = prices[i];
        }
    }

    return maxProfit;
}

int main() {
    int prices[] = {10, 7, 5, 8, 11, 2, 6};
    int size = 7;

    cout << maxProfit(prices, size) << endl;  // prints 6

    return 0;
}
```
Why this is O(N)

The loop runs once through the array.

Each step does a couple of comparisons and assignments.

No nested loops.

## Task 4

You’re writing a function that accepts an array of numbers and computes the highest product of any two numbers in the array. At first glance, this is easy, as we can just find the two greatest numbers and multiply them. However, our array can contain negative numbers and look like this:

```pseudocode
[5, -10, -6, 9, 4]
```

We could use nested loops to multiply every possible pair of numbers, but this would take $O(N^2)$ time. **Your job is to optimize the function so that it’s a speedy $O(N)$.**


```c++
#include <iostream>
using namespace std;

int maxProductOfTwo(int arr[], int n) {
    int max1 = arr[0];
    int max2 = arr[1];
    int min1 = arr[0];
    int min2 = arr[1];

    for (int i = 2; i < n; i++) {
        int x = arr[i];

        if (x > max1) {
            max2 = max1;
            max1 = x;
        } else if (x > max2) {
            max2 = x;
        }

        if (x < min1) {
            min2 = min1;
            min1 = x;
        } else if (x < min2) {
            min2 = x;
        }
    }

    int product1 = max1 * max2;
    int product2 = min1 * min2;

    if (product1 > product2)
        return product1;
    else
        return product2;
}

int main() {
    int arr[] = {5, -10, -6, 9, 4};
    int n = sizeof(arr) / sizeof(arr[0]);

    int result = maxProductOfTwo(arr, n);

    cout << "Highest product of two numbers: " << result << endl;

    return 0;
}

```
## Task 5

You’re creating software that analyzes the data of body temperature readings taken from hundreds of human patients. These readings are taken from healthy people and range from 97.0 degrees Fahrenheit to 99.0 degrees Fahrenheit. An important point: within this application, *the decimal point never goes beyond the tenth place.*

Here’s a sample array of temperature readings:

```
[98.6, 98.0, 97.1, 99.0, 98.9, 97.8, 98.5, 98.2, 98.0, 97.1]
```

You are to write a function that sorts these readings from lowest to highest.

Using a classic sorting algorithm such as Quicksort would take $O(N log N)$. However, in this case, writing a faster sorting algorithm is possible.

Yes, that’s right. Even though you’ve learned that the fastest sorts are $O(N log N)$, this case is different. Why? In this case, there are limited possibilities for the readings. In such a case, we can sort these values in $O(N)$. It may be $N$ multiplied by a constant, but that’s still considered $O(N)$.

```c++
#include <iostream>
using namespace std;

int main() {
    double temps[10] = {98.6, 98.0, 97.1, 99.0, 98.9, 97.8, 98.5, 98.2, 98.0, 97.1};

    int count[21] = {0};

    // Count each temperature
    for (int i = 0; i < 10; i++) {
        int index = (temps[i] - 97.0) * 10;
        count[index]++;
    }

    // Print sorted temperatures
    cout << "Sorted temperatures:\n";

    for (int i = 0; i < 21; i++) {
        while (count[i] > 0) {
            double temp = 97.0 + i * 0.1;
            cout << temp << endl;
            count[i]--;
        }
    }

    return 0;
}


```
## Task 6

You’re writing a function that accepts an array of unsorted integers and returns the length of the *longest consecutive sequence* among them. The sequence is formed by integers that increase by 1. For example, in the array:

```
[10, 5, 12, 3, 55, 30, 4, 11, 2]
```

the longest consecutive sequence is 2-3-4-5. These four integers form an increasing sequence because each integer is one greater than the previous one. While there’s also a sequence of 10-11-12, it’s only a sequence of three integers. In this case, the function should return 4, since that’s the length of the *longest* consecutive sequence that can be formed from this array.

One more example:

```
[19, 13, 15, 12, 18, 14, 17, 11]
```

This array’s longest sequence is 11-12-13-14-15, so the function would return 5.

**Your job is to optimize the function so that it takes $O(N)$ time.**

```c++
#include <iostream>
using namespace std;

// Very simple bubble sort to keep things "bare bones"
void bubbleSort(int arr[], int n) {
    bool swapped;
    for (int i = 0; i < n - 1; ++i) {
        swapped = false;
        for (int j = 0; j < n - 1 - i; ++j) {
            if (arr[j] > arr[j + 1]) {
                int temp   = arr[j];
                arr[j]     = arr[j + 1];
                arr[j + 1] = temp;
                swapped = true;
            }
        }
        if (!swapped) {
            // Array is already sorted
            break;
        }
    }
}

// Returns length of the longest consecutive sequence
int longestConsecutive(int arr[], int n) {
    if (n == 0) {
        return 0;
    }

    // Sort the array first
    bubbleSort(arr, n);

    int best = 1;       // longest sequence found so far
    int current = 1;    // length of the current sequence

    for (int i = 1; i < n; ++i) {
        if (arr[i] == arr[i - 1]) {
            // Same number as before, ignore duplicate
            continue;
        } else if (arr[i] == arr[i - 1] + 1) {
            // Continues the sequence
            current++;
        } else {
            // Break in the sequence, reset current length
            current = 1;
        }

        if (current > best) {
            best = current;
        }
    }

    return best;
}

int main() {
    int a1[] = {10, 5, 12, 3, 55, 30, 4, 11, 2};
    int n1 = sizeof(a1) / sizeof(a1[0]);

    int a2[] = {19, 13, 15, 12, 18, 14, 17, 11};
    int n2 = sizeof(a2) / sizeof(a2[0]);

    int result1 = longestConsecutive(a1, n1);
    int result2 = longestConsecutive(a2, n2);

    cout << "Result for first array: " << result1 << endl;   // should print 4
    cout << "Result for second array: " << result2 << endl;  // should print 5

    return 0;
}


```
