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
