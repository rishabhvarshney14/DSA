# Heaps

I use heapq library for solving these problems. heapq only provides implementation of min_heap hence to use max_heap convert the element to negative (-ele) and again return the negative value. By using this you can perform
max_heap.

Note: If smallest ask go for max_heap else min_heap

- [Kth Smallest element](#Kth-Smallest-element)
- [Kth largest element](#Kth-largest-element)
- [Sort a K sorted array (or a nearly sorted array)](#Sort-a-K-sorted-array-(or-a-nearly-sorted-array))
- [K closet numbers](#K-closet-numbers)
- [Top K frequent numbers](#Top-K-frequent-numbers)
- [Frequency Sort](#Frequency-Sort)
- [K-closet point to origin](#K-closet-point-to-origin)
- [Connect ropes to minimise the cost](#Connect-ropes-to-minimise-the-cost)
- [Sum of Elements between k1 and k2 smallest numbers](#Sum-of-Elements-between-k1-and-k2-smallest-numbers)


```python
import heapq
```

## [Kth Smallest element](https://www.geeksforgeeks.org/kth-smallestlargest-element-unsorted-array/)

Given an array and a number k where k is smaller than the size of the array, we need to find the k’th smallest element in the given array. It is given that all array elements are distinct.

```
Input: arr[] = {7, 10, 4, 3, 20, 15} 
k = 3 
Output: 7

Input: arr[] = {7, 10, 4, 3, 20, 15} 
k = 4 
Output: 10 
```


```python
def k_smallest_ele(arr, k):
    # Use max heap here
    max_heap = []
    
    for ele in arr:
        heapq.heappush(max_heap, -ele)
        if len(max_heap) > k:
            heapq.heappop(max_heap)
    
    return -heapq.heappop(max_heap)

k_smallest_ele([11, 5, 113, 8, 102, 6, 4, 1, 0, 75, 14, 7], 3)
```




    4



## [Kth largest element](https://www.geeksforgeeks.org/kth-smallestlargest-element-unsorted-array/)


```python
def k_largest_ele(arr, k):
    min_heap = []
    
    for ele in arr:
        heapq.heappush(min_heap, ele)
        if len(min_heap) > k:
            heapq.heappop(min_heap)
    
    return heapq.heappop(min_heap)

k_largest_ele([11, 5, 113, 8, 102, 6, 4, 1, 0, 75, 14, 7], 2)
```




    102



## [Sort a K sorted array (or a nearly sorted array)](https://www.geeksforgeeks.org/nearly-sorted-algorithm/)

Given an array of n elements, where each element is at most k away from its target position, devise an algorithm that sorts in O(n log k) time. For example, let us consider k is 2, an element at index 7 in the sorted array, can be at indexes 5, 6, 7, 8, 9 in the given array.

```
Input : arr[] = {6, 5, 3, 2, 8, 10, 9}
            k = 3 
Output : arr[] = {2, 3, 5, 6, 8, 9, 10}

Input : arr[] = {10, 9, 8, 7, 4, 70, 60, 50}
         k = 4
Output : arr[] = {4, 7, 8, 9, 10, 50, 60, 70}
```


```python
def k_sorted_arr(arr, k):
    idx = 0
    min_heap = []
    
    for ele in arr:
        heapq.heappush(min_heap, ele)
        if len(min_heap) > k:
            arr[idx] = heapq.heappop(min_heap)
            idx += 1
    
    for _ in range(k):
        arr[idx] = heapq.heappop(min_heap)
        idx += 1
    
    return arr

k_sorted_arr([6, 5, 3, 2, 8, 10, 9], 3)
```




    [2, 3, 5, 6, 8, 9, 10]



## [K closet numbers](https://www.geeksforgeeks.org/find-k-closest-elements-given-value/)

Given a sorted array arr[] and a value X, find the k closest elements to X in arr[].

Check if ele is same as x.

```
Input: K = 4, X = 35
       arr[] = {12, 16, 22, 30, 35, 39, 42, 
               45, 48, 50, 53, 55, 56}
Output: 30 39 42 45
```


```python
def k_closet_num(arr, k, x):
    max_heap = []
    
    for ele in arr:
        heapq.heappush(max_heap, (-abs(ele - x), ele))
        if len(max_heap) > k:
            heapq.heappop(max_heap)
    
    return [item[1] for item in max_heap]

k_closet_num([1, 2, 3, 4, 5, 6, 7, 8, 9], 3, 5)
```




    [4, 6, 5]



## [Top K frequent numbers](https://www.geeksforgeeks.org/find-k-numbers-occurrences-given-array/)

Given an array of n numbers and a positive integer k. The problem is to find k numbers with most occurrences, i.e., the top k numbers having the maximum frequency. If two numbers have the same frequency then the larger number should be given preference. The numbers should be displayed in decreasing order of their frequencies. It is assumed that the array consists of k numbers with most occurrences.

```
Input: 
arr[] = {3, 1, 4, 4, 5, 2, 6, 1}, 
k = 2
Output: 4 1
Explanation:
Frequency of 4 = 2
Frequency of 1 = 2
These two have the maximum frequency and
4 is larger than 1.
```


```python
def top_k_frequent_num(arr, k):
    min_heap = []
    ele_count = dict()
    
    for ele in arr:
        if ele in ele_count:
            ele_count[ele] += 1
        else:
            ele_count[ele] = 1
    
    for key, value in ele_count.items():
        heapq.heappush(min_heap, (value, key))
        if len(min_heap) > k:
            heapq.heappop(min_heap)
        
    return [item[1] for item in min_heap]

top_k_frequent_num([1, 2, 3, 4, 5, 6, 1, 2, 1, 2, 1, 1, 1, 5, 6, 3, 8, 2, 5, 2, 3, 8, 9, 1], 4)
```




    [3, 5, 2, 1]



## [Frequency Sort](https://www.geeksforgeeks.org/sort-elements-by-frequency/)

Print the elements of an array in the decreasing frequency if 2 numbers have same frequency then print the one which came first. 

```
Input:  arr[] = {2, 5, 2, 8, 5, 6, 8, 8}
Output: arr[] = {8, 8, 8, 2, 2, 5, 5, 6}

Input: arr[] = {2, 5, 2, 6, -1, 9999999, 5, 8, 8, 8}
Output: arr[] = {8, 8, 8, 2, 2, 5, 5, 6, -1, 9999999}
```


```python
def frequency_sort(arr):
    max_heap = []
    ele_count = dict()
    
    for ele in arr:
        if ele in ele_count:
            ele_count[ele] += 1
        else:
            ele_count[ele] = 1
    
    for key, value in ele_count.items():
        heapq.heappush(max_heap, (-value, key))
    
    arr = []
    for _ in range(len(max_heap)):
        ele = heapq.heappop(max_heap)
        arr += [ele[1]] * (-ele[0])
        
    return arr

frequency_sort([1, 2, 3, 4, 5, 6, 1, 2, 1, 2, 1, 1, 1, 5, 6, 3, 8, 2, 5, 2, 3, 8, 9, 1])
```




    [1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 3, 3, 3, 5, 5, 5, 6, 6, 8, 8, 4, 9]



## [K-closet point to origin](https://www.geeksforgeeks.org/find-k-closest-points-to-the-origin/)

Given a list of points on the 2-D plane and an integer K. The task is to find K closest points to the origin and print them.
Note: The distance between two points on a plane is the Euclidean distance.

```
Input : point = [[3, 3], [5, -1], [-2, 4]], K = 2
Output : [[3, 3], [-2, 4]]
Square of Distance of origin from this point is 
(3, 3) = 18
(5, -1) = 26
(-2, 4) = 20
So rhe closest two points are [3, 3], [-2, 4].
```


```python
def k_closet_point_to_origin(arr, k):
    max_heap = []
    
    for point in arr:
        heapq.heappush(max_heap, (-distance(point), point))
        if len(max_heap) > k:
            heapq.heappop(max_heap)
    
    return [item[1] for item in max_heap]

# Calculate distance from origin (0, 0)
def distance(point):
    x = point[0] ** 2
    y = point[1] ** 2
    # Simply returning x + y can work too
    return (x + y) ** (1/2)

points = [(1, 3), (-2, 2), (5, 8), (0, 1)]
k = 2
k_closet_point_to_origin(points, k)
```




    [(-2, 2), (0, 1)]



## [Connect ropes to minimise the cost](https://www.geeksforgeeks.org/connect-n-ropes-minimum-cost/)

There are given n ropes of different lengths, we need to connect these ropes into one rope. The cost to connect two ropes is equal to the sum of their lengths. We need to connect the ropes with minimum cost.
For example, if we are given 4 ropes of lengths 4, 3, 2, and 6. We can connect the ropes in the following ways. 

1. First, connect ropes of lengths 2 and 3. Now we have three ropes of lengths 4, 6, and 5. 
2. Now connect ropes of lengths 4 and 5. Now we have two ropes of lengths 6 and 9. 
3. Finally connect the two ropes and all ropes have connected.

Total cost for connecting all ropes is 5 + 9 + 15 = 29. This is the optimized cost for connecting ropes. Other ways of connecting ropes would always have same or more cost. For example, if we connect 4 and 6 first (we get three strings of 3, 2 and 10), then connect 10 and 3 (we get two strings of 13 and 2). Finally we connect 13 and 2. Total cost in this way is 10 + 13 + 15 = 38.


```python
def connect_ropes(arr):
    min_heap = []
    
    for ele in arr:
        heapq.heappush(min_heap, ele)
    
    min_cost = 0
    while len(min_heap) > 1: # min_heap should have atleast 2 elements 
        cost_min_1 = heapq.heappop(min_heap)
        cost_min_2 = heapq.heappop(min_heap)
        cost_join = cost_min_1 + cost_min_2
        min_cost += cost_join
        heapq.heappush(min_heap, cost_join)
    
    return min_cost

connect_ropes([1, 2, 3, 4, 5])
```




    33



## [Sum of Elements between k1 and k2 smallest numbers](https://www.geeksforgeeks.org/sum-elements-k1th-k2th-smallest-elements/)

Given an array of integers and two numbers k1 and k2. Find the sum of all elements between given two k1’th and k2’th smallest elements of the array. It may be assumed that (1 <= k1 < k2 <= n) and all elements of array are distinct.

```
Input : arr[] = {20, 8, 22, 4, 12, 10, 14},  k1 = 3,  k2 = 6  
Output : 26          
         3rd smallest element is 10. 6th smallest element 
         is 20. Sum of all element between k1 & k2 is
         12 + 14 = 26
```


```python
def sum_of_ele_btw_k1_k2(arr, k1, k2):
    k1_smallest_ele = k_smallest_ele(arr, k1)
    k2_smallest_ele = k_smallest_ele(arr, k2)
    
    sum_ele = 0
    for ele in arr:
        if k1_smallest_ele <= ele <= k2_smallest_ele:
            sum_ele += ele
    
    return sum_ele

sum_of_ele_btw_k1_k2([1, 2, 3, 4, 5, 6, 7, 8, 9], 3, 6)
```




    18




```python
# Median of Stream of Running Integers
# https://www.geeksforgeeks.org/median-of-stream-of-running-integers-using-stl/
```
