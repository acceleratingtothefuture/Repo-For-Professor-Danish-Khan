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

#include <iostream>
#include <vector>
#include <string>
#include <unordered_set>

using namespace std;

struct Player {
    string first_name;
    string last_name;
    string team;
};

vector<string> playersInBothSports(
    const vector<Player>& basketball_players,
    const vector<Player>& football_players
) {
    unordered_set<string> seen;
    vector<string> result;

    for (const auto& p : basketball_players) {
        seen.insert(p.first_name + " " + p.last_name);
    }

    for (const auto& p : football_players) {
        string full_name = p.first_name + " " + p.last_name;
        if (seen.count(full_name)) {
            result.push_back(full_name);
        }
    }

    return result;
}

int main() {
    vector<Player> basketball_players = {
        {"Jill", "Huang", "Gators"},
        {"Janko", "Barton", "Sharks"},
        {"Wanda", "Vakulskas", "Sharks"},
        {"Jill", "Moloney", "Gators"},
        {"Luuk", "Watkins", "Gators"}
    };

    vector<Player> football_players = {
        {"Hanzla", "Radosti", "32ers"},
        {"Tina", "Watkins", "Barleycorns"},
        {"Alex", "Patel", "32ers"},
        {"Jill", "Huang", "Barleycorns"},
        {"Wanda", "Vakulskas", "Barleycorns"}
    };

    vector<string> both = playersInBothSports(basketball_players, football_players);

    for (const auto& name : both) {
        cout << name << endl;
    }

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
