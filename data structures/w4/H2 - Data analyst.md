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
