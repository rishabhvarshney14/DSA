# Hashing

- [Two Sum Problem](#Two-Sum-Problem)
- [Longest Consecutive Sequence](#Longest-Consecutive-Sequence)
- [Longest Substring with no repeating characters (Also in Sliding Window)](#Longest-Substring-with-no-repeating-characters-(Also-in-Sliding-Window))

## Two Sum Problem


```python

def two_sum_problem(arr, t):
    table = {}
    
    for i, ele in enumerate(arr):
        if t-ele in table:
            return table[t-ele], i
        table[ele] = i
        
two_sum_problem([2, 7, 11, 15], 9)
```




    (0, 1)



## Longest Consecutive Sequence


```python
def longestConsecutive(nums):
    D = set(nums)

    largest_seq = 0
    for ele in nums:
        if not ele - 1 in D:
            current_num = ele
            current_seq = 0

            while current_num in D:
                current_seq += 1
                current_num += 1

            largest_seq = max(largest_seq, current_seq)

    return largest_seq

longestConsecutive([100,4,200,1,3,2])
```




    4



## Longest Substring with no repeating characters (Also in Sliding Window)


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


