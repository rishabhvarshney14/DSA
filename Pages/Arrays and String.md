# Arrays and Strings

- [Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?](#1.-Implement-an-algorithm-to-determine-if-a-string-has-all-unique-characters.-What-if-you-cannot-use-additional-data-structures?)
- [Write a function rotate(ar[], d, n) that rotates arr[] of size n by d elements.](#2.-Write-a-function-rotate(ar[],-d,-n)-that-rotates-arr[]-of-size-n-by-d-elements.)
- [Search an element in a sorted and rotated array](#3.-Search-an-element-in-a-sorted-and-rotated-array)
- [Given a sorted and rotated array, find if there is a pair with a given sum](#4.-Given-a-sorted-and-rotated-array,-find-if-there-is-a-pair-with-a-given-sum)
- [Find maximum value of Sum( i*arr[i]) with only rotations on given array allowed](#5.-Find-maximum-value-of-Sum(-i*arr[i])-with-only-rotations-on-given-array-allowed)
- [Maximum sum of i*arr[i] among all rotations of a given array](#6.-Maximum-sum-of-i*arr[i]-among-all-rotations-of-a-given-array)
- [Find the Rotation Count in Rotated Sorted array](#7.-Find-the-Rotation-Count-in-Rotated-Sorted-array)
- [Quickly find multiple left rotations of an array](#8.-Quickly-find-multiple-left-rotations-of-an-array)
- [Find the minimum in rotated and sorted array](#9.-Find-the-minimum-in-rotated-and-sorted-array)
- [Rearrange array in alternating positive & negative items with O(1) extra space](#10.-Rearrange-array-in-alternating-positive-&-negative-items-with-O(1)-extra-space)
- [Move all zeroes to end of array](#11.-Move-all-zeroes-to-end-of-array)

## 1. Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?


```python
# Brute Force
def find_unique_chars_bf(string):
    for char_i in string:
        for char_j in string:
            if char_i == char_j:
                return False
    return True

# Same as Brute Force
def find_unique_chars_wo(string):
    def find_char(string, char):
        if char in string:
            return True
        return False
    
    for i, char in enumerate(string):
        if find_char(string[i+1:], char):
            return False
    return True

# Using Hash Tables
def find_unique_chars(string):
    char_dict = dict()
    for char in string:
        if char in char_dict:
            return False
        char_dict[char] = 1
    return True
        
# By Sorting 
def find_unique_chars(string):
    new_string = sorted(string)
    prev = new_string[0]
    for char in new_string[1:]:
        if char == prev:
            return False
        prev = char
    return True

# Using Bit Manipulation
```

## 2. Write a function rotate(ar[], d, n) that rotates arr[] of size n by d elements.


```python
# Using Slices ----- Complexity = O(d)
def rotate(arr, d):
    return arr[d:] + arr[:d]

# Single item rotation
def rotate(arr, d):
    for rotation_count in range(d):
        temp = arr[0]
        for index in range(len(arr) - 1):
            arr[index] = arr[index+1]
        arr[-1] = temp
    return arr

# Using GCD TODO
def GCD(value_1, value_2):
    for value in range(min(value_1, value_2), 1, -1):
        if value_1 % value == 0 and value_2 % value == 0:
            return value
    return 1

def rotate(arr, d):
    n = len(arr)
    gcd = GCD(n, d)
    for g in range(gcd):
        temp = arr[g]
        for index in range(g, n - gcd, gcd):
            arr[index] = arr[index + gcd]
        arr[g - gcd] = temp
    return arr

# Using Reversal Algorithm
def reverse(arr, start, end):
    while start < end:
        temp = arr[start]
        arr[start] = arr[end]
        arr[end] = temp
        start += 1
        end -= 1
    return arr

def rotate(arr, d):
    if d == 0:
        return arr
    arr = reverse(arr, 0, d-1)
    arr = reverse(arr, d, len(arr) - 1)
    return reverse(arr, 0, len(arr) - 1)

# Rotate Reverse
def rotate(arr, d):
    for rotation_count in range(d):
        temp = arr[-1]
        for index in range(len(arr) - 1, 0, -1):
            arr[index] = arr[index - 1]
        arr[0] = temp
    return arr
        
```

## 3. Search an element in a sorted and rotated array


```python
# Find the first element which breaks the sorting iterating over the array ---- O(n)
def find_the_sorting_breaker(arr):
    small_ele = arr[0]
    for i, ele in enumerate(arr):
        if ele < small_ele:
            return i
    return 0

def binary_search(arr, ele):
    n = len(arr)
    max_index = n - 1
    min_index = 0
    while min_index <= max_index:
        mid_index = (max_index + min_index) // 2
        if arr[mid_index] == ele:
            return mid_index
        elif arr[mid_index] > ele:
            max_index = mid_index - 1
        else:
            min_index = mid_index + 1
    return False
    

def search(arr, ele):
    slicing_num = find_the_sorting_breaker(arr)
    if slicing_num == 0:
        return binary_search(arr, ele)
    ele_index = binary_search(arr[slicing_num:], ele)
    if ele_index:
        return ele_index + slicing_num
    ele_index = binary_search(arr[:slicing_num], ele)
    return ele_index if ele_index else False

# TODO
# Using binary search to fnd the element which break the array
def find_the_sorting_breaker(arr):
    max_index = len(arr) - 1
    min_index = 0
    while max_index >= min_index:
        mid_index = (max_index + min_index) // 2
        if arr[mid_index] > arr[mid_index + 1]:
            return mid_index + 1
        elif arr[mid_index] < arr[mid_index - 1]:
            return mid_index - 1
        elif arr[mid_index] > arr[min_index]:
            min_index = mid_index + 1
        else:
            max_index = mid_index - 1
    return -1

def binary_search(arr, ele):
    n = len(arr)
    max_index = n - 1
    min_index = 0
    if arr[-1] > arr[0]:
        return 0
    while min_index <= max_index:
        mid_index = (max_index + min_index) // 2
        if arr[mid_index] == ele:
            return mid_index
        elif arr[mid_index] > ele:
            max_index = mid_index - 1
        else:
            min_index = mid_index + 1
    return -1
    

def search(arr, ele):
    slicing_num = find_the_sorting_breaker(arr)
    if slicing_num == -1:
        return False
    if slicing_num == 0:
        return binary_search(arr, ele)
    ele_index = binary_search(arr[slicing_num:], ele)
    if ele_index:
        return ele_index + slicing_num
    ele_index = binary_search(arr[:slicing_num], ele)
    return ele_index if ele_index else False

# Using just binary search
def search(arr, ele, min_index, max_index):
    if min_index > max_index:
        return -1
    mid_index = (min_index + max_index) // 2
    if arr[mid_index] == ele:
        return mid_index
    if arr[min_index] <= arr[mid_index]:
        if ele >= arr[min_index] and ele <= arr[mid_index]:
            return search(arr, ele, min_index, mid_index - 1)
        return search(arr, ele, mid_index + 1, max_index)
    
    if ele >= arr[mid_index] and ele <= arr[max_index]:
        return search(arr, ele, mid_index + 1, max_index)
    return search(arr, ele, min_index, max_index)
```

## 4. Given a sorted and rotated array, find if there is a pair with a given sum


```python
# Iterating over array and check
def find_sum(arr, s):
    for ele1 in arr:
        for ele2 in arr:
            if ele1 + ele2 == s:
                return True
    return False

# finding the pivot poing (Might not work if array is not rotated i.e. min_ele is at index 0)
def find_sum(arr, s):
    piv_index = 0
    n = len(arr)
    for index in range(n-1):
        if arr[index] > arr[index + 1]:
            piv_index = index
            
    min_ele_index = (piv_index + 1) % n
    max_ele_index = piv_index
    while min_ele_index != max_ele_index:
        if arr[min_ele_index] + arr[max_ele_index] == s:
            return True
        elif arr[min_ele_index] + arr[max_ele_index] > s:
            max_ele_index = (n + max_ele_index - 1) % n
        else:
            min_ele_index = (min_ele_index + 1) % n
    
    return False

# Find all pairs
def find_sum(arr, s):
    piv_index = 0
    n = len(arr)
    for index in range(n-1):
        if arr[index] > arr[index + 1]:
            piv_index = index
            break
    
    min_ele_index = (piv_index + 1) % n
    max_ele_index = piv_index
    
    count = 0
    while min_ele_index != max_ele_index:
        if arr[max_ele_index] + arr[min_ele_index] == s:
            count += 1
        elif arr[max_ele_index] + arr[min_ele_index] > s:
            max_ele_index = (n + max_ele_index - 1) % n
        else:
            min_ele_index = (min_ele_index + 1) % n
    
    return count
```


```python
def pairInSortedRotated( arr, n, x ): 
      
    # Find the pivot element 
    for i in range(0, n - 1): 
        if (arr[i] > arr[i + 1]): 
            break; 
              
    # l is now index of smallest element         
    l = (i + 1) % n 
    # r is now index of largest element 
    r = i      
  
    # Keep moving either l  
    # or r till they meet 
    while (l != r): 
          
        # If we find a pair with  
        # sum x, we return True 
        if (arr[l] + arr[r] == x): 
            return True; 
              
        # If current pair sum is less, 
        # move to the higher sum 
        if (arr[l] + arr[r] < x): 
            l = (l + 1) % n; 
        else: 
              
            # Move to the lower sum side 
            r = (n + r - 1) % n; 
      
    return False; 
```

## 5. Find maximum value of Sum( i*arr[i]) with only rotations on given array allowed


```python
# Rotating and calculating for every step
def rotate_by_one(arr):
    return arr[1:] + [arr[0]]

def find_sum(arr):
    s = 0
    for i, ele in enumerate(arr):
        s += i * ele
    return s

def find_max_sum(arr):
    arr_len = len(arr)
    
    max_sum = 0
    for _ in range(arr_len - 1):
        arr = rotate_by_one(arr)
        s = find_sum(arr)
        if s > max_sum:
            max_sum = s
    
    return max_sum

# Efficient Method
def find_sum(arr):
    s = 0
    for i, ele in enumerate(arr):
        s += i * ele
    return s

def find_max_sum(arr):
    n = len(arr)
    arr_sum = sum(arr)
    cur_sum = find_sum(arr)
    max_sum = cur_sum
    
    for i in range(1, len(arr)):
        cur_sum = cur_sum + arr_sum - n * arr[n-i]
        if cur_sum > max_sum:
            max_sum = cur_sum
    return max_sum
```

## 6. Maximum sum of i*arr[i] among all rotations of a given array


```python
def find_max(arr):
    max_ele = 0
    for ele in arr:
        if max_ele < ele:
            max_ele = ele
    
    return max_ele * (len(arr) - 1)
```

## 7. Find the Rotation Count in Rotated Sorted array


```python
# Using Linear Search
def find_rotation(arr):
    n = len(arr)
    index = 0
    for i in range(n):
        if arr[i] > arr[i + 1]:
            index = i
            break
    
    return n - index - 1

# Using Binary Search
def find_pivot(arr, min_index, max_index):
    if min_index > max_index:
        return -1
    if min_index == max_index:
        return min_index
    
    mid_index = (min_index + max_index) // 2
    if arr[mid_index] > arr[mid_index + 1]:
        return mid_index
    elif arr[mid_index] < arr[mid_index - 1]:
        return mid_index - 1
    elif arr[mid_index] < arr[max_index]:
        find_pivot(arr, mid_index + 1, max_index)
    else:
        find_pivot(arr, min_index, mid_index - 1)
        
def find_rotation(arr):
    index = find_pivot(arr, 0, len(arr) - 1)
    return len(arr) - index - 1
```

## 8. Quickly find multiple left rotations of an array 


```python
# Using Slicing O(K)
def multiple_rotate(arr, k):
    rotate_count = k % len(arr)
    return arr[rotate_count:] + arr[:rotate_count]

# Using extra space
def multiple_rotate(arr, k):
    temp_arr = arr * 2
    rotate_count = k % len(arr)
    return temp_arr[rotate_count:rotate_count + len(arr)]
```

## 9. Find the minimum in rotated and sorted array


```python
# Linear Search
def find_min(arr):
    for i in range(len(arr)):
        if arr[i] > arr[i + 1]:
            return i +1
        
# Binary Search
def find_min(arr, l, r):
    if l > r:
        return -1
    if l == r:
        return l
    
    m = (l + r) // 2
    
    if arr[m] > arr[m+1]:
        return m + 1
    elif arr[m] < arr[m-1]:
        return m
    elif arr[m] < arr[r]:
        return find_min(arr, m + 1, r)
    else:
        return find_min(arr, l, m - 1)
```

## 10. Rearrange array in alternating positive & negative items with O(1) extra space


```python
def right_rotate(arr, start_index, end_index):
    temp = arr[end_index]
    for index in range(end_index, start_index - 1, -1):
        arr[index] = arr[index-1]
    arr[start_index] = temp
    return arr

def rearrange(arr):
    out_index = -1
    for index in range(len(arr)):
        if out_index >= 0:
            if (arr[index] >= 0 and arr[out_index] < 0) or (arr[index] < 0 and arr[out_index] >= 0):
                arr = right_rotate(arr, out_index, index)
                if index - out_index >= 2:
                    out_index = 2 + out_index
                else:
                    out_index = -1
        else:
            if (arr[index] >= 0 and index % 2 == 0) or (arr[index] < 0 and index % 2 != 0):
                out_index = index
    return arr
```

## 11. Move all zeroes to end of array


```python
# Brute Force
def move_zeros_to_end(arr):
    result = []
    zero_count = 0
    for ele in arr:
        if ele != 0:
            result.append(ele)
        else:
            zero_count += 1
    
    return result + [0] * zero_count

# O(1) space
def move_zeros_to_end(arr):
    zero_count = 0
    for index in range(len(arr)):
        if arr[index] != 0:
            arr[zero_count] = arr[index]
            zero_count += 1
    return arr[:zero_count] + [0] * (len(arr) - zero_count)
```


```python
move_zeros_to_end([1, 2, 3, 4, 5, 0, 0,0 ,0 , 1, 2, 3])
```




    [1, 2, 3, 4, 5, 1, 2, 3, 0, 0, 0, 0]




```python
# Find Duplicate (If array is sorted Binary search is prefered else use finding Cycle in Linked list algorithm)
def findDuplicate(nums):
    slow = nums[0]
    fast = nums[0]

    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]

        if slow == fast:
            break

    fast = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]

    return fast
```


```python
# Sort an array of 0's, 1's and 2's
def sort_012(arr):
    n = len(arr)
    
    low = 0
    mid = 0
    high = n-1
    
    while mid <= high:
        if arr[mid] == 0:
            arr[low], arr[mid] = arr[mid], arr[low]
            low += 1
            mid += 1
        elif arr[mid] == 1:
            mid += 1
        elif arr[mid] == 2:
            arr[mid], arr[high] = arr[high], arr[mid]
            high -= 1
    
    return arr
```


```python
sort_012([0, 1, 2, 0, 1, 2, 0, 1, 0, 2, 0, 1, 0, 2, 0])
```




    [0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 2, 2, 2, 2]


