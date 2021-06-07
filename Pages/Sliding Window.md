# Sliding Window

This technique shows how a nested for loop in some problems can be converted to a single for loop to reduce the time complexity.

- [Maximum sum of subarray of size k](#Maximum-sum-of-subarray-of-size-k)
- [First negative number in every window of size k](#First-negative-number-in-every-window-of-size-k)
- [Count occurence of anagrams](#Count-occurence-of-anagrams)
- [Maximum of all subarray of size k](#Maximum-of-all-subarray-of-size-k)
- [Largest subarray of sum k](#Largest-subarray-of-sum-k)
- [Longest substring with K unique characters](#Longest-substring-with-K-unique-characters)
- [Longest Substring with no repeating characters](#Longest-Substring-with-no-repeating-characters)
- [Pick Toys](#Pick-Toys)
- [Fruits into Baskets](#Fruits-into-Baskets)
- [Longest Substring with Same Letters after Replacement](#Longest-Substring-with-Same-Letters-after-Replacement)
- [Longest Subarray with Ones after Replacement](#Longest-Subarray-with-Ones-after-Replacement)

Template function for most sliding window algorithm:
```python
def function(arr, k):
    # Base condition
    if len(arr) < 2 or k > len(arr):
        return -1
   
    n = len(arr)
    
    # Pointers
    i = 0
    j = 0

    # To store ans or result
    result = []

    # Main Loop
    while j < n:
        # Condition for getting result
        if condition_met:
            do_something

        # Condition for if the window size equals k
        if j-i+1 == k:
            # Check if result is valid

            # Increment i
            i += 1

        # Increment j
        j += 1

    # Return result
    return result
```

## [Maximum sum of subarray of size k](https://www.geeksforgeeks.org/find-maximum-minimum-sum-subarray-size-k/)

Given an array of integers and a number k, find the maximum sum of a subarray of size k. 

#### Input / Output

```
Input  : arr[] = {100, 200, 300, 400}
         k = 2
Output : 700
```


```python
def max_sum_subarray(arr, k):
    if len(arr) < 2 or k > len(arr):
        return -1
    
    max_sum = sum(arr[:k])
    window_sum = max_sum
    
    for i in range(1, len(arr) - k + 1):
        window_sum = window_sum - arr[i-1] + arr[i+k-1]
        if window_sum > max_sum:
            max_sum = window_sum
            
    return max_sum

max_sum_subarray([5 ,2, 7, 2, 8, 1, 0, 4, 6, 3, 9, 4, 1], 5)
```




    26



## [First negative number in every window of size k](https://www.geeksforgeeks.org/first-negative-integer-every-window-size-k/)

Given an array and a positive integer k, find the first negative integer for each window(contiguous subarray) of size k. If a window does not contain a negative integer, then print 0 for that window.

```
Input : {-8, 2, 3, -6, 10}, k = 2
Output : -8 0 -6 -6

First negative integer for each window of size k
{-8, 2} = -8
{2, 3} = 0 (does not contain a negative integer)
{3, -6} = -6
{-6, 10} = -6
```


```python
def first_neg_window(arr, k):
    n = len(arr)
    i = 0
    j = 0
    l = []
    result = []
    
    while j < n:
        if arr[j] < 0:
            l.append(arr[j])
            
        if j - i + 1 == k:
            if len(l) >= 1:
                result.append(l[0])
                if l[0] == arr[i]:
                    l = l[1:]
            else:
                result.append(0)
            i += 1
        j += 1
    
    return result

first_neg_window([12, -1, -7, 8, 9, 10, 11, -4, -5, 2], 3)
```




    [-1, -1, -7, 0, 0, -4, -4, -4]



## [Count occurence of anagrams](https://www.geeksforgeeks.org/count-occurrences-of-anagrams/)

Given a word and a text, return the count of the occurrences of anagrams of the word in the text(For eg: anagrams of word for are for, ofr, rof etc.))

#### Input / Output

```
Input : forxxorfxdofr
        for
Output : 3
Explanation : Anagrams of the word for - for, orf, 
ofr appear in the text and hence the count is 3.
```


```python
def count_ocr_anagram(arr, pattern):
    i = 0
    j = 0
    
    n = len(arr)
    k = len(pattern)
    
    ans = 0
    count = 0
    
    # Count each word in pattern
    word_map = {}
    for w in pattern:
        if w in word_map:
            word_map[w] += 1
        else:
            word_map[w] = 1
            count += 1
    
    while j < n:
        if arr[j] in word_map:
            word_map[arr[j]] -= 1
            if word_map[arr[j]] == 0:
                count -= 1   
        if j - i + 1 == k:
            if count == 0:
                ans += 1
            if arr[i] in word_map:
                word_map[arr[i]] += 1
                # Check if the word is not below 0 otherwise it will make count -ve
                if word_map[arr[i]] > 0:
                    count += 1
            i += 1
        j += 1
    
    return ans

count_ocr_anagram('aabaabaa', 'aaab')
```




    4



## [Maximum of all subarray of size k](https://www.geeksforgeeks.org/sliding-window-maximum-maximum-of-all-subarrays-of-size-k/)

Given an array and an integer K, find the maximum for each and every contiguous subarray of size k.

```
Input: arr[] = {1, 2, 3, 1, 4, 5, 2, 3, 6}, K = 3 
Output: 3 3 4 5 5 5 6
Explanation: 
Maximum of 1, 2, 3 is 3
Maximum of 2, 3, 1 is 3
Maximum of 3, 1, 4 is 4
Maximum of 1, 4, 5 is 5
Maximum of 4, 5, 2 is 5 
Maximum of 5, 2, 3 is 5
Maximum of 2, 3, 6 is 6
```


```python
def max_all_subarr(arr, k):
    i = 0
    j = 0
    n = len(arr)
    
    result = []
    temp_arr = []
    
    while j < n:
        temp_arr.append(arr[j])
        t = 0
        while t < len(temp_arr):
            if temp_arr[t] >= arr[j]:
                temp_arr = temp_arr[t:]
                break
            t += 1
            
        if j - i + 1 == k:
            result.append(temp_arr[0])
            if arr[i] == temp_arr[0]:
                temp_arr = temp_arr[1:]
            i += 1
        j += 1
        
    return result

max_all_subarr([5, 2, 1, -5, 2, 4, 3, 3, -10, 9, 11, 5, 1, -5, 2], 3)
```




    [5, 2, 2, 4, 4, 4, 3, 9, 11, 11, 11, 5, 2]



## Variable size sliding Window

## [Largest subarray of sum k](https://www.geeksforgeeks.org/longest-sub-array-sum-k/)

Given an array arr[] of size n containing integers. The problem is to find the length of the longest sub-array having sum equal to the given value k.

The below algorithm does not work if the array has -ve elements.(Failing on GFG Test Cases).

```
Input : { 10, 5, 2, 7, 1, 9 }, 
            k = 15
Output : 4
The sub-array is {5, 2, 7, 1}
```


```python
def largest_subarr_sum_k(arr, k):
    n = len(arr)
    i = 0
    j = 0
    
    sum_win = 0
    max_win_size = -1
    
    while j < n:
        sum_win += arr[j]
        
        if sum_win > k:
            while sum_win > k:
                sum_win -= arr[i]
                i += 1
        
        if sum_win == k:
            max_win_size = max(max_win_size, j-i+1)
            
        j += 1
        
    return max_win_size

largest_subarr_sum_k([4, 7, 6, 1, 1, 3, 5], 5)
```




    3



## [Longest substring with K unique characters](https://www.geeksforgeeks.org/find-the-longest-substring-with-k-unique-characters-in-a-given-string/)

Given a string you need to print longest possible substring that has exactly M unique characters. If there are more than one substring of longest possible length, then print any one of them.

Example:
```
"aabbcc", k = 1
Max substring can be any one from {"aa" , "bb" , "cc"}.

"aabbcc", k = 2
Max substring can be any one from {"aabb" , "bbcc"}.
```


```python
def longest_substring_with_unique_k_chars(string, k):
    i = 0
    j = 0
    n = len(string)
    
    longest_substring = -1
    unique_chars = {}
    unique_chars_len = 0
    
    while j < n:
        if string[j] not in unique_chars:
            unique_chars[string[j]] = 1
            unique_chars_len += 1
        else:
            unique_chars[string[j]] += 1
        
        if unique_chars_len > k:
            while unique_chars_len > k:
                if unique_chars[string[i]] == 1:
                    del unique_chars[string[i]]
                    unique_chars_len -= 1
                else:
                    unique_chars[string[i]] -= 1
                i += 1
        
        if unique_chars_len == k:
            if j - i + 1 > longest_substring:
                longest_substring = j - i + 1
        j += 1
        
    return longest_substring

longest_substring_with_unique_k_chars('aabacbebebe', 3)
```




    7



## [Longest Substring with no repeating characters](https://www.geeksforgeeks.org/length-of-the-longest-substring-without-repeating-characters/)

Given a string str, find the length of the longest substring without repeating characters. 
```
For “ABDEFGABEF”, the longest substring are “BDEFGA” and “DEFGAB”, with length 6.
For “BBBB” the longest substring is “B”, with length 1.
For “GEEKSFORGEEKS”, there are two longest substrings shown in the below diagrams, with length 7
```


```python
def longest_substring_no_repeating_chars(string):
    i = 0
    j = 0
    n = len(string)
    
    chars_count = {}
    longest_substring = -1
    
    while j < n:
        if string[j] not in chars_count:
            chars_count[string[j]] = 1
        else:
            while string[j] in chars_count:
                del chars_count[string[i]]
                i += 1
            chars_count[string[j]] = 1
        
        if j - i + 1 > longest_substring:
            longest_substring = j - i + 1
        
        j += 1
    
    return longest_substring

longest_substring_no_repeating_chars('abedefgghhhig')
```




    4



## Pick Toys

John is at a toy store help him pick maximum number of toys. He can only select in a continuous manner and he can select only two types of toys.

Input: abaccab

Output: 4


```python
def pick_toys(string):
    n = len(string)
    i = 0
    j = 0
    
    chars_count = {}
    max_toys = -1
    unique_chars = 0
    
    while j < n:
        if string[j] not in chars_count:
            unique_chars += 1
            chars_count[string[j]] = 1
        else:
            chars_count[string[j]] += 1
            
        if unique_chars > 2:
            while unique_chars > 2:
                if chars_count[string[i]] == 1:
                    del chars_count[string[i]]
                    unique_chars -= 1
                else:
                    chars_count[string[i]] -= 1
                i += 1
                
        if unique_chars == 2:
            if max_toys < j - i + 1:
                max_toys = j - i + 1
        
        j += 1
    
    return max_toys

pick_toys('abaccab')
```




    4



## Fruits into Baskets

In a row of trees, the i-th tree produces fruit with type tree[i].

You start at any tree of your choice, then repeatedly perform the following steps:

Add one piece of fruit from this tree to your baskets. If you cannot, stop. Move to the next tree to the right of the current tree. If there is no tree to the right, stop. Note that you do not have any choice after the initial choice of starting tree: you must perform step 1, then step 2, then back to step 1, then step 2, and so on until you stop.

You have two baskets, and each basket can carry any quantity of fruit, but you want each basket to only carry one type of fruit each.

What is the total amount of fruit you can collect with this procedure?

```
Example 1:

Input: [1,2,1] Output: 3 Explanation: We can collect [1,2,1]. Example 2:

Input: [0,1,2,2] Output: 3 Explanation: We can collect [1,2,2]. If we started at the first tree, we would only collect [0, 1]. Example 3:

Input: [1,2,3,2,2] Output: 4 Explanation: We can collect [2,3,2,2]. If we started at the first tree, we would only collect [1, 2]. Example 4:

Input: [3,3,3,1,2,1,1,2,3,3,4] Output: 5 Explanation: We can collect [1,2,1,1,2]. If we started at the first tree or the eighth tree, we would only collect 4 fruits.

```
Note:

1 <= tree.length <= 40000 0 <= tree[i] < tree.length


```python
def fruit_into_baskets(arr):
    n = len(arr)
    
    if n == 0:
        return 0
    
    start = 0
    end = 0
    
    max_fruits = 0
    unique_tree = dict()
    unique_tree_len = 0
    
    while end < n:
        if arr[end] not in unique_tree:
            unique_tree[arr[end]] = 1
            unique_tree_len += 1
        else:
            unique_tree[arr[end]] += 1
            
        while unique_tree_len > 2:
            if unique_tree[arr[start]] == 1:
                del unique_tree[arr[start]]
                unique_tree_len -= 1
            else:
                unique_tree[arr[start]] -= 1
            start += 1
            
        if 0 < unique_tree_len <= 2:
            max_fruits = max(max_fruits, end - start + 1)
            
        end += 1
    
    return max_fruits

fruit_into_baskets(['A', 'B', 'C', 'B', 'B', 'C'])
```




    5



## [Longest Substring with Same Letters after Replacement](https://www.geeksforgeeks.org/maximum-length-substring-having-all-same-characters-after-k-changes/)

We have a string of length n, which consist only UPPER and LOWER CASE characters and we have a number k (always less than n and greater than 0). We can make at most k changes in our string such that we can get a sub-string of maximum length which have all same characters.

```
n = length of string
k = changes you can make
Input : n = 5 k = 2
        str = ABABA
Output : maximum length = 5
which will be (AAAAA)

Input : n = 6 k = 4
       str = HHHHHH
Output : maximum length=6
which will be(HHHHHH) 
```


```python
def longest_substring_with_replacement(string, k):
    n = len(string)
    
    start = 0
    end = 0
    
    longest_substring = 0
    max_chars = 0
    unique_chars_dict = dict()
    
    while end < n:
        if string[end] not in unique_chars_dict:
            unique_chars_dict[string[end]] = 1
        else:
            unique_chars_dict[string[end]] += 1
        
        max_chars = max(max_chars, unique_chars_dict[string[end]])
        
        if (end - start + 1) - max_chars > k:
            if unique_chars_dict[string[start]] == 1:
                del unique_chars_dict[string[start]]
            else:
                unique_chars_dict[string[start]] -= 1
            
            start += 1
        
        longest_substring = max(longest_substring, end - start + 1)
        
        end += 1
    
    return longest_substring

longest_substring_with_replacement("abbcb", 1)
```




    4



## [Longest Subarray with Ones after Replacement](https://www.geeksforgeeks.org/longest-subsegment-1s-formed-changing-k-0s/)

Given a binary array a[] and a number k, we need to find length of the longest subsegment of ‘1’s possible by changing at most k ‘0’s. 

```
Input : a[] = {1, 0, 0, 1, 1, 0, 1}, 
          k = 1.
Output : 4
Explanation : Here, we should only change 1
zero(0). Maximum possible length we can get
is by changing the 3rd zero in the array, 
we get a[] = {1, 0, 0, 1, 1, 1, 1}
```


```python
def longest_subarray_after_replacement(arr, k):
    n = len(arr)
    
    start = 0
    end = 0
    
    longest_len = 0
    dig_count = [0, 0]
    count_one = 0
    
    while end < n:
        dig_count[arr[end]] += 1
        
        if arr[end] == 1:
            count_one += 1
        
        if (end - start + 1) - count_one > k:
            if arr[start] == 1:
                count_one -= 1
            start += 1
            
        longest_len = max(longest_len, end-start+1)
        
        end += 1
    
    return longest_len   

longest_subarray_after_replacement([0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1], 3)
```




    9


