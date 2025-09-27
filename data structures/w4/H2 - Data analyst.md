# H2 - Data analyst

You are a data analyst for a company that sells residential computers. The manager shared with you a dataset of the year 2023 sales.

<img width="342" height="462" alt="image" src="https://github.com/user-attachments/assets/9c24edd4-3933-44a8-8f10-c60c0d7187d8" />


A manager is planning an advertising campaign and needs to identify the stretch of consecutive months with the highest total sales. The goal is to target ads immediately before and after that peak run of months.


# a. part 1 Approach (no code or pseudocode):
"Describe, in plain language, how you would determine which consecutive months produce the highest total sales. Clearly state the inputs, the desired output (start month, end month, and total)."

We need an algorithim that traverses an array of sales and makes new calculations with each element. This will be the fastest. By doing a live algorithim to find the best possible subarray based on what we've seen each time, we can come to a conclusion through only one traversal through the array, hopefully giving it O(N) run time. 

An effecient and popular algorithim like this is one that compares the current value to the current subarray one is on. If the new value in itself is greater than its sum with the current subarray, then the current subarray is very negative and should be discarded. It makes the new current subarray starting with this new element. Otherwise, if the current subarray is increaed by adding this element, it just adds the element to the current subarray. Once it is on a new subarray, it compares it to the highest valued subarray seen so far. If this one is the greatest, it sets it as that. 

Let's see the idea for this with an example

<img width="564" height="345" alt="image" src="https://github.com/user-attachments/assets/8ccbc939-8336-4ba3-9c0d-62b3d961a7cd" />

credit: https://media.geeksforgeeks.org/wp-content/cdn-uploads/kadane-Algorithm.png



Here is the algorithim's thought process for each element:


Element: 0. Value: -2. This is the only element we've seen so far. So by default, we will set it as the current subarray and the max subarray. 

Element: 1. Value: -3. -3 by itself is better than -2 and -3. If we find better numbers, we want to discard that -2 from the beginning of it because an an extra negative is useless at the start. So our new current subarray will be -3. 

Element: 2. Value: 4. 4 by itself is much better than the sum of 4 and -3. So our new current, and our new best is now just 4.

Element 3: Value -1. -1 by itself is not better than the sum of 4 and -1. So we'll hold onto it in hopes of getting something better with out 4. The sub array is now [4, -1]. But our best is still [4]. 

Element 4: Value: -2. -2 by itself is not better than 4 - 1 - 2 = 1. In other words, the 4 is so good that even if we subtract 3 from it, we still have a 1 so we'll hold on to the -2. Our current sub array is now [4, -1 , -2]. Our best subarray is [4]. 

Element: 5. Value: 1. 1 is not better than 4 - 2 - 1 + 1. so we'll tack it on Our current sub array is now [4, -2, -1, 1]. Our best sub array is [4].

Element: 6. Value: 5. The big payoff! 5 is not better than 4 - 2 - 1 + 1 + 5 so we'll tack it on. Our current sub array is now [4, -2, -1, 1, 5]. This is our best sub array, with a sum of 7! 

Element: 7. Value: -3. -3 is certainly not better than 4 - 2 - 1 + 1 + 5 - 3. So our current sub array is [4, -2, -1, 1, 5, -3]. But this -3 makes it worse than our best, which is [4, -2, -1, 1, 5]. Therefore [4, -2, -1, 1, 5] is the max sub array with a sum of 7.

## b. Implementation (C++):

Let's code this algorithim into some C++ that takes each month's data as input, and returns the max sum array as output. 

```c++
#include <iostream>
#include <string>
using namespace std;

// Function to find max subarray sum and its start/end indices
void maxSubarray(int nums[], int size, int& maxSum, int& start, int& end) {
    maxSum = nums[0]; // Start with first element as max sum
    int currentSum = nums[0]; // Current sum being calculated
    start = 0; // Start index of max subarray
    end = 0; // End index of max subarray
    int tempStart = 0; // Temporary start index for current subarray

    for (int i = 1; i < size; i++) {
        // If current element is larger than sum so far, start new subarray
        if (nums[i] > currentSum + nums[i]) {
            currentSum = nums[i];
            tempStart = i;
        } else {
            currentSum = currentSum + nums[i];
        }

        // Update max sum and indices if current sum is larger
        if (currentSum > maxSum) {
            maxSum = currentSum;
            start = tempStart;
            end = i;
        }
    }
}

int main() {
    // Array for 12 months of data
    int data[12];
    // Array for month names
    string months[12] = {"January", "February", "March", "April", "May", "June",
                         "July", "August", "September", "October", "November", "December"};

    // Get input for each month
    cout << "Enter integer data for each month:" << endl;
    for (int i = 0; i < 12; i++) {
        cout << months[i] << ": ";
        cin >> data[i];
    }

    // Find max subarray sum and indices
    int maxSum, startMonth, endMonth;
    maxSubarray(data, 12, maxSum, startMonth, endMonth);

    // Print results
    cout << "\nMaximum subarray sum: " << maxSum << endl;
    cout << "From " << months[startMonth] << " to " << months[endMonth] << endl;

    return 0;
}
```

If you haven't noticed, the main problem with this algorithim is that it is basically just looking for the patches of positive numbers that can best balance out the negative numbers between them (i.e the highest positive numbers separated by the least amount of negativity i.e as few elements of negative numbers as close to zero as possible). 

In other words, this algorithim expects a relative even distribution, perhaps uniform or normal, of positive and negative numbers. This is terrible for our sales data because we only have 2 and 3 digit positive numbers. IF we try this right now, it will just give us all the numbers, because its looking for the biggest sum you can get from a subarray of numbers. The maximum subarray. But in our case, the maximum subarray is the entire array, because there are no negative numbers to avoid. Each month, even the months with poor sales, add to the total number of sales. 

If we want this algorithim to work, a simple way to make it so that our data set of 2 and 3 figure positive numbers is now relatively similar amount of positive and negative numbers. A quick and dirty way to do this is to find the mean, and subtract the mean from each number. Everything less than the mean goes negative, everything greater than the mean goes positive. 

```c++
#include <iostream>
#include <string>
using namespace std;

// Function to find max subarray sum and its start/end indices
void maxSubarray(int nums[], int size, int& maxSum, int& start, int& end) {
    maxSum = nums[0]; // Start with first element as max sum
    int currentSum = nums[0]; // Current sum being calculated
    start = 0; // Start index of max subarray
    end = 0; // End index of max subarray
    int tempStart = 0; // Temporary start index for current subarray

    for (int i = 1; i < size; i++) {
        // If current element is larger than sum so far, start new subarray
        if (nums[i] > currentSum + nums[i]) {
            currentSum = nums[i];
            tempStart = i;
        } else {
            currentSum = currentSum + nums[i];
        }

        // Update max sum and indices if current sum is larger
        if (currentSum > maxSum) {
            maxSum = currentSum;
            start = tempStart;
            end = i;
        }
    }
}

int main() {
    // Array for 12 months of data
    int data[12];
    // Array for month names
    string months[12] = {"January", "February", "March", "April", "May", "June",
                         "July", "August", "September", "October", "November", "December"};

    // Get input for each month
    cout << "Enter integer data for each month:" << endl;
    for (int i = 0; i < 12; i++) {
        cout << months[i] << ": ";
        cin >> data[i];
    }

    // Calculate average //NEW!
    double sum = 0; //NEW!
    for (int i = 0; i < 12; i++) { //NEW!
        sum = sum + data[i]; //NEW!
    } //NEW!
    double average = sum / 12; //NEW!

    // Transform data by subtracting average //NEW!
    int transformed[12]; //NEW!
    for (int i = 0; i < 12; i++) { //NEW!
        transformed[i] = (int)(data[i] - average); //NEW! // Convert to int
    } //NEW!

    // Find max subarray sum and indices
    int maxSum, startMonth, endMonth;
    maxSubarray(transformed, 12, maxSum, startMonth, endMonth); //NEW! (modified to use transformed data)

    // Print results
    cout << "\nAverage of input data: " << average << endl; //NEW!
    cout << "Maximum subarray sum (on transformed data): " << maxSum << endl; //NEW! (modified to indicate transformed data)
    cout << "From " << months[startMonth] << " to " << months[endMonth] << endl;

    return 0;
}
```


This gives us an improvement on simply outputting all the months. However, let's remember that the algorithim is biased towards finding some not-so-bad negatives separating really high positives, while shaving off any really bad negatives on either end of the subarray. The result? We kick off the last 2 elements, which are especially low and on the end. But we have two high patches of numbers between. It's safe to say that our sales team doesn't want to know that there's a slow seaon to avoid. They don't just want to avoid the worst case, they want it filtered down to only the best few months for an ad campaign. So we have to really punish any subarray that has anything but great numbers. The quick and dirty way to do this is instead of subtracting the average, we can subtract the midpoint of the average and the maximum. 

```C++
#include <iostream>
#include <string>
using namespace std;

// Function to find max subarray sum and its start/end indices
void maxSubarray(int nums[], int size, int& maxSum, int& start, int& end) {
    maxSum = nums[0]; // Start with first element as max sum
    int currentSum = nums[0]; // Current sum being calculated
    start = 0; // Start index of max subarray
    end = 0; // End index of max subarray
    int tempStart = 0; // Temporary start index for current subarray

    for (int i = 1; i < size; i++) {
        // If current element is larger than sum so far, start new subarray
        if (nums[i] > currentSum + nums[i]) {
            currentSum = nums[i];
            tempStart = i;
        } else {
            currentSum = currentSum + nums[i];
        }

        // Update max sum and indices if current sum is larger
        if (currentSum > maxSum) {
            maxSum = currentSum;
            start = tempStart;
            end = i;
        }
    }
}

int main() {
    // Array for 12 months of data
    int data[12];
    // Array for month names
    string months[12] = {"January", "February", "March", "April", "May", "June",
                         "July", "August", "September", "October", "November", "December"};

    // Get input for each month
    cout << "Enter integer data for each month:" << endl;
    for (int i = 0; i < 12; i++) {
        cout << months[i] << ": ";
        cin >> data[i];
    }

    // Calculate average
    double sum = 0;
    for (int i = 0; i < 12; i++) {
        sum = sum + data[i];
    }
    double average = sum / 12;

    // Find maximum value //NEW!
    int maxValue = data[0]; //NEW!
    for (int i = 0; i < 12; i++) { //NEW!
        if (data[i] > maxValue) { //NEW!
            maxValue = data[i]; //NEW!
        } //NEW!
    } //NEW!

    // Calculate midpoint //NEW!
    double midpoint = (average + maxValue) / 2; //NEW!

    // Transform data by subtracting midpoint //NEW!
    int transformed[12]; //NEW!
    for (int i = 0; i < 12; i++) { //NEW!
        transformed[i] = (int)(data[i] - midpoint); //NEW! // Convert to int
    } //NEW!

    // Find max subarray sum and indices
    int maxSum, startMonth, endMonth;
    maxSubarray(transformed, 12, maxSum, startMonth, endMonth);

    // Print results
    cout << "\nAverage of input data: " << average << endl;
    cout << "Maximum value of input data: " << maxValue << endl; //NEW!
    cout << "Midpoint (avg + max)/2: " << midpoint << endl; //NEW!
    cout << "Maximum subarray sum (on transformed data): " << maxSum << endl;
    cout << "From " << months[startMonth] << " to " << months[endMonth] << endl;

    return 0;
}
```


## a. part 2. "how you’ll handle ties, all-negative values, or multiple peak segments."

Ties/multiple peaks: The algorithim cuts off any bad values on the ends of the peak. So if two numbers are both high maximums, it will continue to move out from them until there is no longer a tie. If there are two perfectly symmetrical peaks, it WILL grab all of them. This is a feature, not a bug. It tells our salesteam that there are two seasons that are identically advantageous. 

All-negative values: a typical algorithim would just output the number closest to zero. But if we're in sales, zero is a minimum. If we assume sales can't be negative and that the sales are distributed around a positive number, we will always have a relatively even ratio of negatives to positive. However, if we did have all negatives for whatever reason (maybe they wanted to look at profit instead of sales), this algorithim would still. If production cost was constant, this algorithim would output the same months as it would when only looking at the non-negative sales numbers. 


## c. Pseudocode + Complexity:
Provide pseudocode for your algorithm and analyze its time and space complexity using Big-O notation. Target an O(N) time solution.

Pseudocode:
months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
N = 12
data = array of size N
for i from 0 to N-1:
read data[i] for months[i]
sum = 0
for each x in data:
sum += x
avg = sum / N
maxVal = data[0]
for each x in data:
if x > maxVal:
maxVal = x
midpoint = (avg + maxVal) / 2
transformed = array of size N
for i from 0 to N-1:
transformed[i] = floor(data[i] - midpoint)  // or integer cast
// Kadane's algorithm
current_max = transformed[0]
global_max = transformed[0]
start = 0
end = 0
temp_start = 0
for i from 1 to N-1:
if transformed[i] > current_max + transformed[i]:
current_max = transformed[i]
temp_start = i
else:
current_max += transformed[i]
if current_max > global_max:
global_max = current_max
start = temp_start
end = i
output avg
output maxVal
output midpoint
output global_max as max subarray sum
output months[start] to months[end]
Time Complexity: O(N) - Each step (input, sum/avg calculation, max finding, transformation, and Kadane's algorithm) involves a single pass over the array of size N.
Space Complexity: O(N) - Requires O(N) space for the data array, transformed array, and months list (though months is fixed-size).


3d. Limitations:
Discuss limitations of your approach (e.g., sensitivity to noise/outliers, tie-breaking choices, single peak assumption, lack of seasonality handling, etc.) and when a different method might be preferable.



```c++
#include <iostream>
#include <vector>
#include <string>
using namespace std;

// Kadane’s Algorithm to find max subarray sum and indices
pair<int, pair<int,int>> maxSubarray(const vector<int>& nums) {
    int n = nums.size();
    int current_max = nums[0];
    int global_max = nums[0];
    int start = 0, end = 0, temp_start = 0;

    for (int i = 1; i < n; i++) {
        if (nums[i] > current_max + nums[i]) {
            current_max = nums[i];
            temp_start = i;
        } else {
            current_max += nums[i];
        }

        if (current_max > global_max) {
            global_max = current_max;
            start = temp_start;
            end = i;
        }
    }
    return {global_max, {start, end}};
}

int main() {
    vector<int> data(12);
    vector<string> months = {"January","February","March","April","May","June",
                             "July","August","September","October","November","December"};

    cout << "Enter integer data for each month:\n";
    for (int i = 0; i < 12; i++) {
        cout << months[i] << ": ";
        cin >> data[i];
    }

    // Compute average
    double sum = 0;
    for (int x : data) sum += x;
    double avg = sum / data.size();

    // Find maximum value
    int maxVal = data[0];
    for (int x : data) {
        if (x > maxVal) maxVal = x;
    }

    // Midpoint between average and maximum
    double midpoint = (avg + maxVal) / 2.0;

    // Transform each element into (element - midpoint)
    vector<int> transformed(12);
    for (int i = 0; i < 12; i++) {
        transformed[i] = static_cast<int>(data[i] - midpoint); 
    }

    // Run Kadane on transformed data
    auto result = maxSubarray(transformed);
    int maxSum = result.first;
    int startMonth = result.second.first;
    int endMonth = result.second.second;

    cout << "\nAverage of input data: " << avg << endl;
    cout << "Maximum value of input data: " << maxVal << endl;
    cout << "Midpoint (avg + max)/2: " << midpoint << endl;
    cout << "Maximum subarray sum (on transformed data): " << maxSum << endl;
    cout << "From " << months[startMonth] << " to " << months[endMonth] << endl;

    return 0;
}


```
