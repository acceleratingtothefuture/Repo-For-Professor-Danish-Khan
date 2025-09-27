# H2 - Data analyst

Video explanation: https://youtu.be/cwt5s6aWeVk

You are a data analyst for a company that sells residential computers. The manager shared with you a dataset of the year 2023 sales.

<img width="342" height="462" alt="image" src="https://github.com/user-attachments/assets/9c24edd4-3933-44a8-8f10-c60c0d7187d8" />


A manager is planning an advertising campaign and needs to identify the stretch of consecutive months with the highest total sales. The goal is to target ads immediately before and after that peak run of months.


# a. part 1 Approach (no code or pseudocode):
"Describe, in plain language, how you would determine which consecutive months produce the highest total sales. Clearly state the inputs, the desired output (start month, end month, and total)."

We need an algorithim that traverses an array of sales and makes new calculations with each element. This will be the fastest. By doing a live algorithim to find the best possible subarray based on what we've seen each time, we can come to a conclusion through only one traversal through the array, hopefully giving it O(N) run time. 

Kadane's algorithm is a highly efficient and popular solution for this maximum subarray problem, (https://en.wikipedia.org/wiki/Maximum_subarray_problem). The algorithm works by iterating through the array and, at each step, making a simple decision: it compares the current element alone to the sum of the current element plus the existing subarray sum. If the current element by itself is greater, it means the previous subarray sum is dragging it down (probably because it's negative), so the algorithm starts a new subarray from this element. Otherwise, it extends the current subarray by adding the element. After each step, it checks if the current subarray sum is greater than the highest sum found so far and updates the maximum if needed. Because we are constantly deciding on a new current and a new best, we can get the maximum subarray sum in a single pass.

Basically, it looks at each number and says "Am I better off adding this to my current pile, or starting a brand new current pile from just this new number?" And then it asks "which pile was the highest overall?" It outputs that highest pile. 

Let's see the idea for this with an example

<img width="564" height="345" alt="image" src="https://github.com/user-attachments/assets/8ccbc939-8336-4ba3-9c0d-62b3d961a7cd" />

credit: https://media.geeksforgeeks.org/wp-content/cdn-uploads/kadane-Algorithm.png


Here is the algorithim's thought process for each element:


Element: 0. Value: -2. This is the only element we've seen so far. So by default, we will set it as the current subarray and the max subarray. 

Element: 1. Value: -3. -3 by itself is better than -2 and -3. If we find better numbers, we want to discard that -2 from the beginning of it because an an extra negative is useless at the start. So our new current subarray will be -3. But out best sub array was -2 by itself. That's the highest sub array we've seen so far. 

Element: 2. Value: 4. 4 by itself is much better than the sum of 4 and -3. So our new current, and our new best is now just 4. 

Element 3: Value -1. -1 by itself is not better than the sum of 4 and -1. So we'll hold onto it in hopes of getting something better with out 4. The sub array is now [4, -1]. But our best is still [4]. 

Element 4: Value: -2. -2 by itself is not better than 4 - 1 - 2 = 1. In other words, the 4 is so good that even if we subtract 3 from it, we still have a 1 so we'll hold on to the -2. Our current sub array is now [4, -1 , -2]. Our best subarray is [4]. 

Element: 5. Value: 1. 1 is not better than 4 - 2 - 1 + 1 = 2. so we'll tack it on Our current sub array is now [4, -2, -1, 1] which equals 2. Not bad, but our best sub array is [4].

Element: 6. Value: 5. The big payoff! 5 is not better than 4 - 2 - 1 + 1 + 5 = 7. so we'll tack it on. Our current sub array is now [4, -2, -1, 1, 5]. This is our best sub array, with a sum of 7! 

Element: 7. Value: -3. -3 is certainly not better than 4 - 2 - 1 + 1 + 5 - 3 = 4. So our current sub array is [4, -2, -1, 1, 5, -3]. But this -3 makes it worse than our best, which is [4, -2, -1, 1, 5]. Therefore [4, -2, -1, 1, 5] is the max sub array with a sum of 7.

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


<img width="362" height="331" alt="image" src="https://github.com/user-attachments/assets/e474e0c5-e980-4c23-bf67-884055a16b90" />


The problem with this algorithim is that it is basically just looking for the patches of positive numbers that can best balance out the negative numbers between them (i.e the highest positive numbers separated by the least amount of negativity i.e as few elements of negative numbers as close to zero as possible). 

In other words, this algorithim expects a healthy mixture of positive and negative numbers. With our data, it ust outputs the entire array, because its looking for the biggest sum you can get from a consecutive subarray of numbers. Obviously with all positive numbers, every element you include in the sum increases it. Our maximum subarray is the entire array, because there are no negative numbers to avoid. Each month, even the months with poor sales, add to the total number of sales. 

If we want this algorithim to work, we could turn our data set of positive numbers into a mixture of positive and negative numvers A quick and dirty way to do this is to find the mean, and subtract the mean from each number. Everything less than the mean goes negative, everything greater than the mean goes positive. 

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
<img width="458" height="348" alt="image" src="https://github.com/user-attachments/assets/251caed7-ba72-4b57-af12-455d8449a45f" />


This gives us an improvement on simply outputting all the months. However, let's remember that the algorithim will always kick off negative (in our case, below average) numbers at the ends of an array. The result? We kick off the last 2 elements, which are especially low and on the end. Even though we have spots that are clearly higher than others, our algorithim is willing to put up with the moderate numbers, which are still mostly above 100, so that it can grab both of the highest numbers. We want an algorithim that is absolutely addicted to the cream of the crop. We need more bias towards high numbers

It's safe to say that our sales team doesn't just want to know that there's a slow seaon at the beginning or end of the year. So we have to really punish any subarray that has anything but the best numbers. The quick and dirty way to do this is instead of subtracting the average, we can subtract the midpoint of the average and the maximum. 

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
<img width="462" height="397" alt="image" src="https://github.com/user-attachments/assets/5f4097ed-fc0f-41d4-a1c7-ea37c8e17b26" />

Now we have QUALITY output! Our algorithim is seeing anything closer to the average than the maximum as negative, only trying for sub arrays that can stay near the best numbers. 

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

Time Complexity: O(N). Each step (input, sum/avg calculation, max finding, transformation, and Kadane's algorithm) involves a single pass over the array of size N. We NEVER have any inner loops that traverse the other elements for each element. We go through once, doing live calculations and saves. 
Space Complexity: O(N). Requires O(N) space for the data array, transformed array, and months list (though months is fixed-size).


## 3d. Limitations:
Discuss limitations of your approach (e.g., sensitivity to noise/outliers, tie-breaking choices, single peak assumption, lack of seasonality handling, etc.) and when a different method might be preferable.

This algorithim is highly sensitive to outliers because it's based on closeness to the max. If the sales team wants to go broader, we should implement some function that chooses a specific percentile. Even better: a sliding bar that lets them choose from 50th percentile to maximum. They could drag the slider up until they got what they were looking for: as many or few months as they wanted. 

The tie breaking is solid. Because its constantly looking at the previous vs. the latest element, it will break a tie by looking at what comes before and after the tied months. I discussed this in more detail in a. part 2. 
There is nothing here that takes seasonality into consideration, but a simple function could deduct say 20% during holiday shopping season. 
A different method would be preferrable when you want an exact list of the best N months. Since the program is only looking at 12 elements, we could write something which requires much more compute: something that outputs the best 2 in a row, the best 3 in a row, etc. Instead of sliding through once, we would slide through multiple times looking for every combination of 2, 3, 4 etc. elements. 


