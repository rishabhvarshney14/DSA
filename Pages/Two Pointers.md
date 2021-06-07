# Two Pointers

- [3 Sum](#3-Sum)
- [Trapping Rain Water](#Trapping-Rain-Water)
- [Remove Duplicates from sorted Array](#Remove-Duplicates-from-sorted-Array)
- [Maximum Ones After Modification](#Maximum-Ones-After-Modification)
- [Four Sum](#Four-Sum)
- [Reverse Pairs](#Reverse-Pairs)

## [3 Sum](https://www.geeksforgeeks.org/find-triplets-array-whose-sum-equal-zero/)

Given an array of distinct elements. The task is to find triplets in the array whose sum is zero.

```
Input : arr[] = {0, -1, 2, -3, 1}
Output : (0 -1 1), (2 -3 1)

Explanation : The triplets with zero sum are
0 + -1 + 1 = 0 and 2 + -3 + 1 = 0  
```


```python
def three_sum(arr):
    n = len(arr)
    count = 0
    
    arr.sort()
    
    for i in range(n-2):
        a = -arr[i]
        
        low = i+1
        high = n-1
        while low < high:
            if arr[low] + arr[high] == a:
                count += 1
                low += 1
                high -= 1
                
                # Remove Repeatation
                while low < high and arr[low] == arr[low+1]:
                    low += 1
                while low < high and arr[high] == arr[high-1]:
                    high -= 1

            elif arr[low] + arr[high] > a:
                high -= 1
            else:
                low += 1
        
    return count

three_sum([-1, 0, 1, 2, -1, -4])
```




    3



## [Trapping Rain Water](https://www.geeksforgeeks.org/trapping-rain-water/)

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

```
Input: arr[]   = {3, 0, 2, 0, 4}
Output: 7
```


```python
def trap_rain_water(arr):
    n = len(arr)
    
    low = 0
    high = n-1
    
    left_max = 0
    right_max = 0
    
    result = 0
    
    while low < high:
        if arr[low] <= arr[high]:
            if arr[low] >= left_max:
                left_max = arr[low]
            else:
                result += left_max - arr[low]
            low += 1
        else:
            if arr[high] >= right_max:
                right_max = arr[high]
            else:
                result += right_max - arr[high]
            high -= 1
    
    return result

trap_rain_water([0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1])
```




    6



## [Remove Duplicates from sorted Array](https://www.geeksforgeeks.org/remove-duplicates-sorted-array/)

Given a sorted array, the task is to remove the duplicate elements from the array.

```
Input  : arr[] = {1, 2, 2, 3, 4, 4, 4, 5, 5}
Output : arr[] = {1, 2, 3, 4, 5}
```


```python
def remove_duplicates(nums):
    n = len(nums)

    if n <= 1:
        return n

    low = 0
    high = 0

    curr = 0

    while low < n and high < n:
        if nums[low] == nums[high]:
            high += 1
        else:
            curr += 1
            nums[curr] = nums[high]
            low = high
            high += 1

    return curr + 1

remove_duplicates([0,0,1,1,1,2,2,3,3,4])
```




    5



## [Maximum Ones After Modification](https://www.geeksforgeeks.org/longest-subsegment-1s-formed-changing-k-0s/)

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
def max_one_after_modf(A, B):
    n = len(A)

    ptr = 0
    count = 0

    ans = 0

    for i in range(n):
        if A[i] == 0:
            count += 1
        while count > B:
            if A[ptr] == 0:
                count -= 1
            ptr += 1
        ans = max(ans, i-ptr+1)

    return ans

max_one_after_modf([0, 1, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 1, 1], 2)
```




    8



## [Four Sum](https://www.geeksforgeeks.org/find-four-elements-that-sum-to-a-given-value-set-2/)

Given an array of integers, find anyone combination of four elements in the array whose sum is equal to a given value X

```
Input: array = {10, 2, 3, 4, 5, 9, 7, 8} 
       X = 23 
Output: 3 5 7 8
Sum of output is equal to 23, 
i.e. 3 + 5 + 7 + 8 = 23.
```


```python
def four_sum(nums, target):
    n = len(nums)

    nums.sort()

    result = []

    for a_idx in range(n-3):
        for b_idx in range(a_idx+1, n-2):
            cd_sum = target - nums[a_idx] - nums[b_idx]

            low = b_idx + 1
            high = n-1

            while low < high:
                if nums[low] + nums[high] == cd_sum:
                    elements = (nums[a_idx], nums[b_idx], nums[low], nums[high])
                    if elements not in result:
                        result.append(elements)
                    low += 1
                    high -= 1
                elif nums[low] + nums[high] > cd_sum:
                    high -= 1
                else:
                    low += 1

    return result

four_sum([1,0,-1,0,-2,2], 0)
```




    [(-2, -1, 1, 2), (-2, 0, 0, 2), (-1, 0, 0, 1)]



## Reverse Pairs


```python

```
