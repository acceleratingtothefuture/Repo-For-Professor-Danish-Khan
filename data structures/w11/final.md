# Final Project
Video explanation: https://youtu.be/SSLkVzoxP0I

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

MAIN IDEA: 

Walk one list and put them in a hash table. Walk the second list and see if each one is stored in the table. That way we don't have to compare an element to the whole array each time. We just see if its in the table and we're good. Build result when there’s a match.
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
    unordered_set<string> basketballNames;

    // build hash table from basketball players
    for (int i = 0; i < basketball_players.size(); i++) {
        string fullName = basketball_players[i].first_name + " " +
                          basketball_players[i].last_name;
        basketballNames.insert(fullName);
    }

    // traverse football players and look up in the hash table
    for (int j = 0; j < football_players.size(); j++) {
        string fullName = football_players[j].first_name + " " +
                          football_players[j].last_name;

        if (basketballNames.find(fullName) != basketballNames.end()) {
            result.push_back(fullName);
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

total runtime: O(N + M). First pass: you walk the basketball list once and insert each full name into a hash table. That costs O(N). Second pass: you walk the football list once. For each player you do a hash lookup, which is O(1) average time. Doing that M times costs O(M).
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

Main idea: the least clever N squared nested loop involves traversing the array and marking each number off from a list of what we should find, until we finish the array and our list has only one number remaining. 

However. We can just sum all the numbers of the array up, and compare that to the sum of what we would expect if all the numbers were there. The difference is the missing number. So for the array we just add the natural numbers from 0 to N with the sumamtion formula, then we traverse the array once adding each element and compare. So its O(N). 

```c++
#include <iostream>   
#include <vector>     
using namespace std;

//   finds the missing number in the array.
// The array contains numbers from 0 to N, but one number is missing.
int findMissingNumber(const vector<int>& nums) {
    int n = nums.size();
    // if the array length is N, then the numbers should be 0 through N.
    // The sum of numbers from 0 to N is: N * (N + 1) / 2. Use's Gauss's trick  
    int expectedSum = n * (n + 1) / 2;

    int actualSum = 0;

    //loop through the array and add up all the values
    for (int i = 0; i < n; i++) {
        actualSum += nums[i];
    }

    //  missing number is the difference between what the sum should be and what it actually is
    return expectedSum - actualSum;
}

int main() {
    // first example numbers from 0 to 6, missing 4
    vector<int> a = {2, 3, 0, 6, 1, 5};

    // example 2: numbers from 0 to 9, missing 1
    vector<int> b = {8, 2, 3, 9, 4, 7, 5, 0, 6};

    // call the function and print the results
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

Big idea: If you sell on day i, the best buy day is the cheapest price strictly before day i. So we traverse the array, continously updating the cheapest price as time goes on. On any day you sell, the biggest profit is just "price today - cheapest price we've seen in the past". So we keep track of that difference for each day. The biggest difference is the best combo. When we find the biggest difference, those 2 days that calcuated it are the respective buy and sell days. 

```c++
#include <iostream>
using namespace std;

struct Trade {
    int profit;
    int buyDay;   // 0-based index
    int sellDay;  // 0-based index
};

Trade bestOneTrade(const int prices[], int size) {
    // if fewer than 2 days, no trade
    if (size < 2) return {0, -1, -1};

    int minPrice = prices[0];
    int minDay = 0;

    int bestProfit = prices[1] - prices[0];
    int bestBuy = 0;
    int bestSell = 1;

    for (int day = 1; day < size; day++) {
        int profitIfSellToday = prices[day] - minPrice;

        // if selling today beats our best, record the buy/sell days
        if (profitIfSellToday > bestProfit) {
            bestProfit = profitIfSellToday;
            bestBuy = minDay;
            bestSell = day;
        }

        // update the cheapest price we've ever seen before future days
        if (prices[day] < minPrice) {
            minPrice = prices[day];
            minDay = day;
        }
    }

    return {bestProfit, bestBuy, bestSell};
}

int main() {
    int prices[] = {10, 7, 5, 8, 11, 2, 6};
    int size = sizeof(prices) / sizeof(prices[0]);

    Trade t = bestOneTrade(prices, size);

    cout << "Best profit: " << t.profit << "\n";
    cout << "Buy day: " << (t.buyDay + 1) << " at price " << prices[t.buyDay] << "\n";
    cout << "Sell day: " << (t.sellDay + 1) << " at price " << prices[t.sellDay] << "\n";

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

BIG IDEA: positive numbers arise from positive and positive or negative and negative multiplied. So our biggest number will either be from the two positive numbers with the biggest magnitude, or the two negative numbers with the biggest negative magnitude. So we just traverse the array and look at each number asking "is this the largest magnitude postive number we found? If so, its the new 1st place max and the current 1st place max is demoted to second. If not, is it at least bigger than the current 2nd place? If so update it. If not, move on to the next number." We also do the same check but for first and second place magnitude negative numbers. 

So we can update our winners, traverse once, and then compare the positive * positive product and negative * negative product at the very end. And the winner of those is the highest product. 

```c++
#include <iostream>
using namespace std;

// find  highest product of any two numbers in the array
int maxProductOfTwo(int arr[], int n) {
    // Start by assuming the first two numbers are the max and min
    int max1 = arr[0];
    int max2 = arr[1];

    int min1 = arr[0];
    int min2 = arr[1];

    // make  sure max1 >= max2 at the start
    if (max2 > max1) {
        int temp = max1;
        max1 = max2;
        max2 = temp;
    }

    //make sure min1 <= min2 at the start
    if (min2 < min1) {
        int temp = min1;
        min1 = min2;
        min2 = temp;
    }

    // loop through the rest of the array once
    for (int i = 2; i < n; i++) {
        int x = arr[i];

        //update the two largest numbers
        if (x > max1) {
            max2 = max1;
            max1 = x;
        } else if (x > max2) {
            max2 = x;
        }

        //update the two smallest numbers
        if (x < min1) {
            min2 = min1;
            min1 = x;
        } else if (x < min2) {
            min2 = x;
        }
    }

    //product of the two largest numbers
    int productMax = max1 * max2;

    //product of the two smallest numbers
    // This matters because two negatives make a positive
    int productMin = min1 * min2;

    // return the larger of the two products
    if (productMax > productMin) {
        return productMax;
    } else {
        return productMin;
    }
}

int main() {
    // example array with both positive and negative numbers
    int arr[] = {5, -10, -6, 9, 4};

    //calculate number of elements in the array
    int n = sizeof(arr) / sizeof(arr[0]);

    //call the function
    int result = maxProductOfTwo(arr, n);

    //print the result
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

BIG IDEA: there are only 21 possible values: 97.0, 97.1, 97.2, 97.3, 97.4 etc up to 99.0. So we create an array of 21 elements all initalized to zero. These 21 elements will each hold the count of a temp it represents. the zeroeth element represents 97.0 the first element 97.1 etc. 

So how do we map a temp to the right element in the count array? 

We subtract 97.0 the minimum from a number, then multiple it by 10 and make it an integer. so 97.0 becomes 0. 97.1 becomes 1. 97.2 becomes 2 etc. and as we traverse the unordered temps, we increase the count of the element represented to it. I.e for 97.6, we subtract 97.0 and multiple by 10 to get 6. we increase the count of element 6 in our array to represent another 97.6 found. 

Now we have an array of counts for each temperature. So we can just traverse that array and print each element for its respective counts. for example if the zeroeth element ends with a count of 2, we print 97.0 twice. if the first element has a count of 1, then we print 97.1 once after that etc. 
```c++
#include <iostream>
using namespace std;

// sort and print the temperatures using Counting Sort logic
void sortTemperatures(const double temps[], int n) {
    // We assume the maximum range is 99.0 - 97.0 = 2.0, with 0.1 increments.
    // (2.0 / 0.1) + 1 = 21 possible values (97.0 to 99.0).
    int count[21] = {0};

    //count each temperature
    for (int i = 0; i < n; i++) {
        int index = (int)((temps[i] - 97.0) * 10); //change it relative to minimum 
        count[index]++;
    }

    //print sorted temperatures
    cout << "Sorted temperatures:\n";

    for (int i = 0; i < 21; i++) {
        while (count[i] > 0) {
            double temp = 97.0 + i * 0.1;
            cout << temp << endl;
            count[i]--;
        }
    }
}

int main() {
    double temps[] = {98.6, 98.0, 97.1, 99.0, 98.9, 97.8, 98.5, 98.2, 98.0, 97.1};
    int n = sizeof(temps) / sizeof(temps[0]);

    sortTemperatures(temps, n);

    return 0;
}

```
First we set up a counting array where each spot represents one possible temperature. Then we go through the list of readings and, for each one, figure out which slot it belongs to and bump up the count there. After that, we walk through the counting array from lowest to highest and print each temperature the number of times it was seen. Since we only scan the input once and then scan this small fixed range, the whole thing runs in linear time instead of doing slower comparison based sorting.

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

BIG IDEA: a number can only be the start of a sequence if the number before it doesn't exist. So we traverse each number x and ask "is x - 1 in the sequence?" Of course, in order to be able to quickly see if the whole thing contains a certain previous number, we use a hash table. Anyway, we check each number to be the start of the sequence, and we see how far of a sequence we can get by counting up. if there's no x - 1, is there an x + 1? What about x + 2? We see how high much we can keep getting the next consecutive x + 1,2,3 etc. if the sequence we find is longer than the current record holder, we update it. 

```c++
#include <iostream>
#include <unordered_set>
using namespace std;

// returns length of the longest consecutive sequence
int longestConsecutive(int arr[], int n) {
    if (n == 0) {
        return 0;
    }

    //put all numbers in a set for O(1) lookups so we can just grab it right away
    unordered_set<int> nums;
    for (int i = 0; i < n; i++) {
        nums.insert(arr[i]);
    }

    int best = 1;

    //for each number, see if it is the start of a sequence
    for (int i = 0; i < n; i++) {
        int current = arr[i];

        //only start counting if current - 1 is not in the set
        if (nums.find(current - 1) == nums.end()) {
            int length = 1;
            int nextVal = current + 1;

            // Count how long the sequence goes
            while (nums.find(nextVal) != nums.end()) {
                length++;
                nextVal++;
            }
            //found a new best sequence, update it 
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
