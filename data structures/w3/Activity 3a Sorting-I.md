
# Activity 3: Sorting I  

Video explanation: https://www.youtube.com/watch?v=kRqUyfpQdC8

## Task 1: "Use Big O Notation to describe the time complexity of an algorithm that takes 4N + 16 steps"

O(N). "According to the rules of Big O notations, the notation ignores constants and numbers that are not an exponent...if you have a function running time of 5N, we say that this function runs on the order of the big O of N. This is because the constant five no longer matters as N gets large”

credit: https://d-khan.github.io/ds/intro.html

<img width="725" height="390" alt="image" src="https://github.com/user-attachments/assets/1f009985-6746-4815-84bd-053096f7209a" />

If we zoom out to visibly see n and 4n + 16, as both are visible, higher order things like n squared are vertical and lower order things like 1 are flat. 

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/58d2fb3a-0d09-4765-afb1-edb79186967f" />


## Task 2: Use Big O Notation to describe the time complexity of an algorithm that takes 2N<sup>2</sup> 

O(N<sup>2</sup>). Again. We drop the constant and look at the highest order variable. 

<img width="471" height="58" alt="image" src="https://github.com/user-attachments/assets/c8ba0851-96fa-4a75-80f5-f8c9f790f441" />


credit: https://d-khan.github.io/ds/Sorting_algorithms.html

## Task 3: Use Big O Notation to describe the time complexity of the following function, which returns the sum of all numbers of an array after the numbers have been doubled:

```ruby
def double_then_sum(array) 
	doubled_array = []

	array.each do |number| 
		doubled_array << number *= 2
	end

	sum = 0

	doubled_array.each do |number| 
		sum += number
	end
	return sum 
end
```
O(N). For each number in N numbers, it doubles that number and then it adds that number to the accumulated sum. Therefore it's based on 2N. In other words it's directly proportional to N, so it's O(N).


## Task 4: Use Big O Notation to describe the time complexity of the following function, which accepts an array of strings and prints each string in multiple cases: 

```ruby
def multiple_cases(array) 
	array.each do |string|
		puts string.upcase 
		puts string.downcase 
		puts string.capitalize
	end 
end
```
O(N). For each element of the array of strings, it does 3 things so 3N. So it's proportional to N so its O(N). 

## Task 5: The next function iterates over an array of numbers, and for each number whose index is even, it prints the sum of that number plus every number in the array. What is this function’s efficiency in terms of Big O Notation

```ruby
def every_other(array) 
	array.each_with_index do |number, index|
		if index.even?
			array.each do |other_number|
            	puts number + other_number
			end 
		end
	end 
end
```
O(N<sup>2</sup>). The outer loop goes through each number. In itself, that would make it proportional to N. But WITHIN that loop, it has an inner loop for half of the numbers, that adds itself with all the N numbers before it. So for half of the elements in the outer loop of N elements, it goes through ALL the previous N elements. So it's N/2 times N which is N<sup>2</sup>/2. Like we've seen before we drop the 1/2 constant to get O(N<sup>2</sup>).
