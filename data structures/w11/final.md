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

vector<string> playersInBothSports(const vector<Player>& basketball_players,
                                   const vector<Player>& football_players) {
    vector<string> result;

    // Put all basketball full names in a set
    unordered_set<string> basketballNames;
    for (int i = 0; i < basketball_players.size(); i++) {
        string fullName = basketball_players[i].first_name + " " +
                          basketball_players[i].last_name;
        basketballNames.insert(fullName);
    }

    // Check each football player against the set
    unordered_set<string> added;  // to avoid duplicates in result
    for (int j = 0; j < football_players.size(); j++) {
        string fullName = football_players[j].first_name + " " +
                          football_players[j].last_name;

        // If the name is in the basketball set and not yet in result
        if (basketballNames.find(fullName) != basketballNames.end() &&
            added.find(fullName) == added.end()) {
            result.push_back(fullName);
            added.insert(fullName);
        }
    }

    return result;
}

int main() {
    vector<Player> basketball_players;
    basketball_players.push_back(Player{"Jill", "Huang", "Gators"});
    basketball_players.push_back(Player{"Janko", "Barton", "Sharks"});
    basketball_players.push_back(Player{"Wanda", "Vakulskas", "Sharks"});
    basketball_players.push_back(Player{"Jill", "Moloney", "Gators"});
    basketball_players.push_back(Player{"Luuk", "Watkins", "Gators"});

    vector<Player> football_players;
    football_players.push_back(Player{"Hanzla", "Radosti", "32ers"});
    football_players.push_back(Player{"Tina", "Watkins", "Barleycorns"});
    football_players.push_back(Player{"Alex", "Patel", "32ers"});
    football_players.push_back(Player{"Jill", "Huang", "Barleycorns"});
    football_players.push_back(Player{"Wanda", "Vakulskas", "Barleycorns"});

    vector<string> both = playersInBothSports(basketball_players, football_players);

    cout << "Players in both sports:" << endl;
    for (int i = 0; i < both.size(); i++) {
        cout << both[i] << endl;
    }

    return 0;
}


```

We go through the basketball list first and for each player we build a full name string with first and last name and put that full name in a hash set so we can check it fast later. Then we go through the football list and again build the full name string for each player. For each football full name we check if that name is in the basketball set, which tells us this person plays both sports, and we also check a second hash set so we do not add the same full name to the result more than once. If both checks pass we push the full name into the result list and record it in the added set. In the end the result list holds all players who appear in both original lists, and since we only walk each list once and all set operations are constant time on average, the whole thing runs in time proportional to the total number of players in both lists.

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
#include <iostream>   // for cout
#include <vector>     // for vector

using namespace std;

// This function finds the missing number in the array.
// The array contains numbers from 0 to N, but one number is missing.
int findMissingNumber(const vector<int>& nums) {
    int n = nums.size();
    // If the array length is N, then the numbers should be 0 through N.
    // The sum of numbers from 0 to N is: N * (N + 1) / 2
    int expectedSum = n * (n + 1) / 2;

    int actualSum = 0;

    // Loop through the array and add up all the values
    for (int i = 0; i < n; i++) {
        actualSum += nums[i];
    }

    // The missing number is the difference between what the sum
    // should be and what it actually is
    return expectedSum - actualSum;
}

int main() {
    // Example 1: numbers from 0 to 6, missing 4
    vector<int> a = {2, 3, 0, 6, 1, 5};

    // Example 2: numbers from 0 to 9, missing 1
    vector<int> b = {8, 2, 3, 9, 4, 7, 5, 0, 6};

    // Call the function and print the results
    cout << findMissingNumber(a) << endl; // should print 4
    cout << findMissingNumber(b) << endl; // should print 1

    return 0;
}


```

The code works by using math instead of checking every number against every other number. It knows that if the array has length N, then the numbers are supposed to be all the integers from 0 up to N, with exactly one missing. First, it calculates what the sum of all numbers from 0 to N should be using the formula N times N plus 1 divided by 2. Then it loops through the array once and adds up all the numbers that are actually there. Since exactly one number is missing, the difference between the expected sum and the actual sum is that missing number. The function returns this difference, and because it only loops through the array one time and uses just a few variables, it runs in O(N) time and uses constant extra space.


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

// This function takes an array of stock prices and the size of the array.
// It returns the maximum profit you can make by buying once and selling once
// in the future (sell day must be after buy day).
int maxProfit(int prices[], int size) {
    // If there are fewer than 2 prices, you cannot make any transaction.
    // In a real program you might throw an error or handle this differently,
    // but here we just return 0.
    if (size < 2) {
        return 0;
    }

    // Start by assuming the first price is the minimum price we have seen so far.
    int minPrice = prices[0];

    // Initialize maxProfit as the profit from buying on day 0 and selling on day 1.
    // This makes sure we compare real buy-sell pairs.
    int maxProfit = prices[1] - prices[0];

    // Loop over the array starting at index 1, because index 0 is already in minPrice.
    for (int i = 1; i < size; i++) {
        int currentPrice = prices[i];

        // Pretend we sell on this day at currentPrice.
        // The best possible profit for selling today is currentPrice - minPrice,
        // where minPrice is the lowest price we have seen before today.
        int profitIfSoldToday = currentPrice - minPrice;

        // If this profit is bigger than any profit we have seen so far,
        // update maxProfit.
        if (profitIfSoldToday > maxProfit) {
            maxProfit = profitIfSoldToday;
        }

        // Now update minPrice if today's price is lower than any previous price.
        // This makes it a better buying price for future days.
        if (currentPrice < minPrice) {
            minPrice = currentPrice;
        }
    }

    // After the loop, maxProfit holds the best profit from one buy and one sell.
    return maxProfit;
}

int main() {
    // Example from the problem statement.
    // Day 1: 10
    // Day 2: 7
    // Day 3: 5
    // Day 4: 8
    // Day 5: 11
    // Day 6: 2
    // Day 7: 6
    int prices[] = {10, 7, 5, 8, 11, 2, 6};

    // Size of the array (7 days).
    int size = 7;

    // Call the function to compute the maximum profit.
    int bestProfit = maxProfit(prices, size);

    // Print the result to standard output using cout.
    // For this example, the correct answer is 6:
    // Buy at 5, sell at 11.
    cout << "Maximum profit from one buy and one sell is: " << bestProfit << endl;

    return 0;
}

```
We go through the list of prices one time and keep track of two things as we go. First, it remembers the lowest price it has seen so far, which represents the best day to buy before the current day. Second, for each new day, it pretends to sell on that day and calculates the profit by subtracting the lowest earlier price from today’s price. If this profit is bigger than the best profit seen so far, it saves it. After checking the profit, it updates the lowest price if today’s price is lower than any previous one. By doing this in a single loop, the code checks every possible buy and sell pair in an efficient way, without nested loops, and ends with the highest possible profit from one buy followed by one sell.

## Task 4

You’re writing a function that accepts an array of numbers and computes the highest product of any two numbers in the array. At first glance, this is easy, as we can just find the two greatest numbers and multiply them. However, our array can contain negative numbers and look like this:

```pseudocode
[5, -10, -6, 9, 4]
```

We could use nested loops to multiply every possible pair of numbers, but this would take $O(N^2)$ time. **Your job is to optimize the function so that it’s a speedy $O(N)$.**


```c++
#include <iostream>
using namespace std;

// This function finds the highest product of any two numbers in the array
int maxProductOfTwo(int arr[], int n) {
    // Start by assuming the first two numbers are the max and min
    int max1 = arr[0];
    int max2 = arr[1];

    int min1 = arr[0];
    int min2 = arr[1];

    // Make sure max1 >= max2 at the start
    if (max2 > max1) {
        int temp = max1;
        max1 = max2;
        max2 = temp;
    }

    // Make sure min1 <= min2 at the start
    if (min2 < min1) {
        int temp = min1;
        min1 = min2;
        min2 = temp;
    }

    // Loop through the rest of the array once
    for (int i = 2; i < n; i++) {
        int x = arr[i];

        // Update the two largest numbers
        if (x > max1) {
            max2 = max1;
            max1 = x;
        } else if (x > max2) {
            max2 = x;
        }

        // Update the two smallest numbers
        if (x < min1) {
            min2 = min1;
            min1 = x;
        } else if (x < min2) {
            min2 = x;
        }
    }

    // Product of the two largest numbers
    int productMax = max1 * max2;

    // Product of the two smallest numbers
    // This matters because two negatives make a positive
    int productMin = min1 * min2;

    // Return the larger of the two products
    if (productMax > productMin) {
        return productMax;
    } else {
        return productMin;
    }
}

int main() {
    // Example array with both positive and negative numbers
    int arr[] = {5, -10, -6, 9, 4};

    // Calculate number of elements in the array
    int n = sizeof(arr) / sizeof(arr[0]);

    // Call the function
    int result = maxProductOfTwo(arr, n);

    // Print the result
    cout << "Highest product of two numbers: " << result << endl;

    return 0;
}


```
We scan the array one time while keeping track of the two largest values and the two smallest values seen so far. It needs the two largest numbers because their product could be the biggest if they are both positive, and it also needs the two smallest numbers because if they are both negative, multiplying them gives a large positive result. As the loop runs, each new number is compared against the current largest and smallest values and placed in the correct spot by shifting the old values down. After the loop finishes, the code computes the product of the two largest numbers and the product of the two smallest numbers and returns whichever product is bigger, which correctly handles negative values while only using a single pass through the array.
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
First we set up a counting array where each spot represents one possible temperature. Then we go through the list of readings and, for each one, figure out which slot it belongs to and bump up the count there. After that, we walk through the counting array from lowest to highest and print each temperature the number of times it was seen. Since we only scan the input once and then scan this small fixed range, the whole thing runs in linear time instead of doing slower comparison-based sorting.

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
#include <unordered_set>
using namespace std;

// Returns length of the longest consecutive sequence
int longestConsecutive(int arr[], int n) {
    if (n == 0) {
        return 0;
    }

    // Put all numbers in a set for O(1) lookups
    unordered_set<int> nums;
    for (int i = 0; i < n; i++) {
        nums.insert(arr[i]);
    }

    int best = 1;

    // For each number, see if it is the start of a sequence
    for (int i = 0; i < n; i++) {
        int current = arr[i];

        // Only start counting if current - 1 is not in the set
        if (nums.find(current - 1) == nums.end()) {
            int length = 1;
            int nextVal = current + 1;

            // Count how long the sequence goes
            while (nums.find(nextVal) != nums.end()) {
                length++;
                nextVal++;
            }

            if (length > best) {
                best = length;
            }
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

    cout << "Result for first array: " << result1 << endl;   // 4
    cout << "Result for second array: " << result2 << endl;  // 5

    return 0;
}

```
We first put every number from the array into a hash set so we can check if a value exists in constant time. Then we loop through the original array and for each number we only start a sequence if there is no number just before it (so if x is in the array but x − 1 is not, x is the start of a run). From that start value we keep checking x + 1, x + 2, x + 3, and so on in the set, counting how long this chain of consecutive numbers goes. Each time we finish a chain, we compare its length to the best length we have seen so far and update the best if this one is longer. In the end we return that best length. Building the set takes linear time, and the loops together are also linear because each integer is only walked forward through a sequence once, so the total runtime is O(N).
