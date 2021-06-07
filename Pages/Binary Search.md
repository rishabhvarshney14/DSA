# Binary Search

- [Algorithm for Binary Search](#Algorithm-for-Binary-Search)
- [Reverse Sorted Array](#Reverse-Sorted-Array)
- [Sorting Order not defined](#Sorting-Order-not-defined)
- [First or Last occurence of element in an array](#First-or-Last-occurence-of-element-in-an-array)
- [Count of element in an array](#Count-of-element-in-an-array)
- [Number of times a sorted array is rotated](#Number-of-times-a-sorted-array-is-rotated)
- [Binary Search Recursion](#Binary-Search-Recursion)
- [Find an element in a Rotated Sorted Array](#Find-an-element-in-a-Rotated-Sorted-Array)
- [Searching in a nearly sorted array](#Searching-in-a-nearly-sorted-array)
- [Find floor of an element in a sorted array](#Find-floor-of-an-element-in-a-sorted-array)
- [Find ceil of an element in a sorted array](#Find-ceil-of-an-element-in-a-sorted-array)
- [Next alphabetical Element](#Next-alphabetical-Element)
- [Find position of an element in an infinite sorted array](#Find-position-of-an-element-in-an-infinite-sorted-array)
- [Index of first one in a infinite binary sorted array (includ 0 and 1 only)](#Index-of-first-one-in-a-infinite-binary-sorted-array-(includ-0-and-1-only))
- [Minimum difference element in a sorted array](#Minimum-difference-element-in-a-sorted-array)
- [Peak element : Element greater than its both neighbors](#Peak-element-:-Element-greater-than-its-both-neighbors)
- [Find maximum element in a bitonic array](#Find-maximum-element-in-a-bitonic-array)
- [Search in bitonic array](#Search-in-bitonic-array)
- [Search in row-wise and col-wise sorted array -- O(N + M) (worst case)](#Search-in-row-wise-and-col-wise-sorted-array----O(N-+-M)-(worst-case))
- [Allocates minimum number of Pages](#Allocates-minimum-number-of-Pages)

## Algorithm for Binary Search


```python
def binary_search(arr, ele):
    start = 0
    end = len(arr) - 1
    
    while not start > end:
        mid = start + (end - start) // 2  # For stop overflowing int
        if arr[mid] == ele:
            return mid
        
        if ele > arr[mid]:
            start = mid + 1
        else:
            end = mid - 1
    print("Element not found!")
    
binary_search([1, 2, 3, 4, 5], 5)
```




    4



## Reverse Sorted Array


```python
def binary_search_reverse_sorted(arr, ele):
    start = 0
    end = len(arr) - 1
    
    while not start > end:
        mid = start + (end - start) // 2
        
        if arr[mid] == ele:
            return mid
        elif ele > arr[mid]:
            end = mid - 1
        else:
            start = mid + 1
    
    print("Element not found!")
    
binary_search_reverse_sorted([6, 5, 4, 3, 2, 1], 5)
```




    1



## Sorting Order not defined


```python
def binary_search_order_unk(arr, ele):
    start = 0
    end = len(arr) - 1
    
    while not start > end:
        mid = start + (end - start) // 2
        if arr[mid] == ele:
            return mid
        if arr[0] < arr[-1] and len(arr) > 1:
            if ele > arr[mid]:
                start = mid + 1
            else:
                end = mid - 1
        else:
            if ele > arr[mid]:
                end = mid - 1
            else:
                start = mid + 1
    return -1

print(binary_search_order_unk([1, 2, 3, 4, 5, 6, 7], 5))
print(binary_search_order_unk([7, 6, 5, 4, 3, 2, 1], 10))
```

    4
    -1


## First or Last occurence of element in an array


```python
def first_occurence(arr, ele):
    start = 0
    end = len(arr) - 1
    
    first_ocr = -1
    
    while not start > end:
        mid = start + (end - start) // 2
        if arr[mid] == ele:
            first_ocr = mid
            end = mid - 1
        elif ele > arr[mid]:
            start = mid + 1
        else:
            end = mid - 1
            
    return first_ocr

def last_occurence(arr, ele):
    start = 0
    end = len(arr) - 1
    
    last_ocr = -1
    
    while not start > end:
        mid = start + (end - start) // 2
        if arr[mid] == ele:
            last_ocr = mid
            start = mid + 1
        elif ele > arr[mid]:
            start = mid + 1
        else:
            end = mid - 1
    
    return last_ocr

print(first_occurence([1, 2, 3, 5, 5, 5, 5, 8, 9], 5))
print(last_occurence([1, 2, 3, 5, 5, 5, 5, 8, 9], 5))
```

    3
    6


## Count of element in an array


```python
def count_element(arr, ele):
    first_ocr = first_occurence(arr, ele)
    last_ocr = last_occurence(arr, ele)
    
    return last_ocr - first_ocr + 1

count_element([1, 2, 3, 5, 5, 5, 5, 5, 6, 7, 8, 9, 10], 5)
```

## Number of times a sorted array is rotated


```python
def array_rotation_count(arr):
    n = len(arr)
    start = 0
    end = n - 1
    
    while not start > end:
        mid = start + (end - start) // 2
        prev_idx = (mid + n - 1) % n
        next_idx = (mid + 1) % n
        
        if arr[mid] <= arr[next_idx] and arr[mid] <= arr[prev_idx]:
            return mid
        elif arr[mid] >= arr[start]:
            start = mid + 1
        else:
            end = mid - 1
            
    return 0 

array_rotation_count([4, 5, 1, 2, 3, 10])
```




    2



## Binary Search Recursion


```python
def binary_search_rec(arr, ele, start, end):
    if start > end:
        return -1
    mid = start + (end - start) // 2
    if arr[mid] == ele:
        return mid
    elif ele > arr[mid]:
        return binary_search_rec(arr, ele, mid + 1, end)
    else:
        return binary_search_rec(arr, ele, start, mid - 1)
    
binary_search_rec([1, 2, 3, 4, 5, 6], 5, 0, 5)
```




    4



## Find an element in a Rotated Sorted Array


```python
def find_ele_sorted_array_rot(arr, ele):
    n = len(arr)
    start = 0
    end = n - 1
    
    # Search for the minimum element
    while not start > end:
        mid = start + (end - start) // 2
        prev_ele = (mid + n - 1) % n
        next_ele = (mid + 1) % n
        if arr[mid] <= arr[prev_ele] and arr[mid] <= arr[next_ele]:
            break
        elif arr[mid] >= arr[start]:
            start = mid + 1
        else:
            end = mid - 1
        
    left_search = binary_search_rec(arr, ele, 0, mid - 1)
    right_search = binary_search_rec(arr, ele, mid + 1, n - 1)
    
    if arr[mid] == ele:
        return mid
    elif left_search == -1:
        return right_search
    return left_search

find_ele_sorted_array_rot([4, 5, 1, 2, 3, 10], 1)
```




    2



## Searching in a nearly sorted array
element can be found at either position i, i - 1 or i + 1


```python
def search_nearly_sort(arr, ele):
    n = len(arr)
    start = 0
    end = n - 1
    
    while not start > end:
        mid = start + (end - start) // 2
        if arr[mid] == ele:
            return mid
        elif mid + 1 < end + 1 and arr[mid + 1] == ele:
            return mid + 1
        elif mid - 1 > -1 and arr[mid - 1] == ele:
            return mid - 1
        elif ele > arr[mid]:
            start = mid + 2
        else:
            end = mid - 2
            
    return -1

search_nearly_sort([1, 2, 3, 5, 4, 6, 7, 8], 5)
```




    3



## Find floor of an element in a sorted array


```python
def floor_ele(arr, ele):
    start = 0
    end = len(arr) - 1
    result = -1
    
    while not start > end:
        mid = start + (end - start) // 2
        if arr[mid] == ele:
            return ele
        elif ele > arr[mid]:
            result = arr[mid]
            start = mid + 1
        else:
            end = mid - 1
            
    return result

floor_ele([1, 2, 3, 4, 6, 7, 8], 5)
```




    4



## Find ceil of an element in a sorted array


```python
def ceil_ele(arr, ele):
    start = 0
    end = len(arr) - 1
    result = -1
    
    while not start > end:
        mid = start + (end - start) // 2
        if arr[mid] == ele:
            return ele
        elif ele < arr[mid]:
            result = arr[mid]
            end = mid - 1
        else:
            start = mid + 1
            
    return result

ceil_ele([1, 2, 3, 4, 6, 7, 8], 5)
```




    6



## Next alphabetical Element


```python
def next_alpha_ele(arr, key):
    start = 0
    end = len(arr) - 1
    result = -1
    
    while not start > end:
        mid = start + (end - start) // 2
        if mid + 1 < end + 1 and arr[mid] == key:
            return arr[mid + 1]
        elif ord(key) < ord(arr[mid]):
            result = arr[mid]
            end = mid - 1
        else:
            start = mid + 1
            
    return result

next_alpha_ele('cdefghijkl', 'a')
```




    'c'



## Find position of an element in an infinite sorted array


```python
def find_ele_inf_sort_arr(arr, ele):
    start = 0
    end = 1
    
    while ele > arr[end]:
        start = end
        end = 2 * end
    
    return binary_search_rec(arr, ele, start, end)

find_ele_inf_sort_arr([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20], 5)
```




    4



## Index of first one in a infinite binary sorted array (includ 0 and 1 only)


```python
def index_of_first_one(arr):
    start = 0
    end = 1
    
    while 1 > arr[end]:
        start = end
        end = 2 * start
        
    first_ocr = -1
    
    while not start > end:
        mid = start + (end - start) // 2
        if arr[mid] == 1:
            first_ocr = mid
            end = mid - 1
        else:
            start = mid + 1
        
    return first_ocr

index_of_first_one([0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1])
```




    8



## Minimum difference element in a sorted array


```python
def min_diff_ele(arr, ele):
    start = 0
    end = len(arr) - 1
    
    floor = -1
    while not start > end:
        mid = start + (end - start) // 2
        if arr[mid] == ele:
            floor = ele
            break
        elif ele > arr[mid]:
            floor = arr[mid]
            start = mid + 1
        else:
            end = mid - 1
            
    ceil = -1
    start = 0
    end = len(arr) - 1
    while not start > end:
        mid = start + (end - start) // 2
        if arr[mid] == ele:
            ceil = ele
            break
        elif ele < arr[mid]:
            ceil = arr[mid]
            end = mid - 1
        else:
            start = mid + 1
            
    if ele - floor < ceil - ele:
        return floor
    return ceil

# Using ceil_ele and floor_ele functions
def min_diff_ele(arr, ele):
    ceil = ceil_ele(arr, ele)
    floor = floor_ele(arr, ele)
    
    if ele - floor < ceil - ele:
        return floor
    return ceil

min_diff_ele([1, 2, 3, 4, 8, 9], 5)
```




    4



## Peak element : Element greater than its both neighbors

Array is unsorted


```python
def peak_ele(arr):
    n = len(arr)
    start = 0
    end = n - 1
    
    while not start > end:
        mid = start + (end - start) // 2
        if mid > 0 and mid < n-1:
            if arr[mid - 1] <= arr[mid] >= arr[mid + 1]:
                return mid
            elif arr[mid - 1] > arr[mid]:
                end = mid - 1
            else:
                start = mid + 1
        else:
            if mid == 0:
                if arr[mid + 1] <= arr[mid]:
                    return mid
                return 1
            elif mid == n-1:
                if arr[mid - 1] <= arr[mid]:
                    return mid
                return mid - 1

peak_ele([6, 1, 8, 2, 0, 2, 4, 3, 5, 10, 9])
```




    9



## Find maximum element in a bitonic array


```python
def max_ele_bitonic_arr(arr):
    n = len(arr)
    start = 0
    end = n - 1
    
    while not start > end:
        mid = start + (end - start) // 2
        if mid != 0 and mid != n-1:
            if arr[mid - 1] < arr[mid] > arr[mid + 1]:
                return arr[mid]
            elif arr[mid] < arr[mid + 1]:
                start = mid + 1
            else:
                end = mid - 1
        else:
            if mid == 0:
                if arr[0] > arr[1]:
                    return arr[0]
                return arr[1]
            else:
                if arr[n-1] > arr[n - 2]:
                    return arr[n - 1]
                return arr[n - 2]
            
    return -1

max_ele_bitonic_arr([1, 2, 3, 4, 5, 6, 7, 6, 5, 4, 3, 2, 1])
```




    7



## Search in bitonic array


```python
def search_bitonic(arr, ele):
    n = len(arr)
    start = 0
    end = n - 1
    
    peak_ele = -1
    while not start > end:
        mid = start + (end - start) // 2
        if mid != 0 and mid != n-1:
            if arr[mid - 1] < arr[mid] > arr[mid + 1]:
                peak_ele = mid
                break
            elif arr[mid] < arr[mid + 1]:
                start = mid + 1
            else:
                end = mid - 1
        else:
            if mid == 0:
                if arr[0] > arr[1]:
                    peak_ele = 0
                else:
                    peak_ele = 1
            else:
                if arr[n-1] > arr[n-2]:
                    peak_ele = n - 1
                else:
                    peak_ele = n - 2
    
    if arr[peak_ele] == ele:
        return peak_ele
    elif peak_ele == 0 and arr[0] == ele:
        return 0
    elif peak_ele == n-1 and arr[n-1] == ele:
        return n-1
    else:
        left_search = binary_search_order_unk(arr[:peak_ele - 1], ele)
        right_search = binary_search_order_unk(arr[peak_ele + 1:], ele)
        if left_search != -1:
            return left_search
        return right_search + peak_ele + 1
    
search_bitonic([1, 2, 3, 4, 5, 20, 19, 18, 13, 12, 11], 11)
```




    10



## Search in row-wise and col-wise sorted array -- O(N + M) (worst case)


```python
def matrix_search(arr, ele):
    N = len(arr)
    M = len(arr[0])
    row_idx = 0
    col_idx = M - 1
    
    while 0 <= row_idx < N and 0 <= col_idx < M:
        if arr[row_idx][col_idx] == ele:
            return (row_idx, col_idx)
        elif arr[row_idx][col_idx] > ele:
            col_idx -= 1
        else:
            row_idx += 1
    
    return -1

matrix = [[10, 20, 30, 40],
          [15, 25, 35, 45],
          [27, 29, 27, 45],
          [32, 33, 39, 50]]
key = 29
matrix_search(matrix, key)
```




    (2, 1)



## Allocates minimum number of Pages


```python
def allocat_min_num_pages(arr, k):
    start = max(arr)
    end = sum(arr)
    n = len(arr)
    result = -1
    
    while not start > end:
        mid = start + (end - start) // 2
        if is_valid(arr, n, k, mid):
            result = mid
            end = mid - 1
        else:
            start = mid + 1
    
    return result
        
def is_valid(arr, n, k, mid):
    student = 1
    std_sum = 0
    
    for i in range(n):
        std_sum += arr[i]
        if std_sum > mid:
            student += 1
            std_sum = arr[i]
    if student == k:
        return True
    return False

allocat_min_num_pages([10, 20, 30, 40, 50, 60, 70, 80, 90, 100], 2)
```




    280


