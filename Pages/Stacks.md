# Stacks

- [Nearest Greatest to Right (Next largest element)](#Nearest-Greatest-to-Right-(Next-largest-element))
- [Nearest Greatest to left](#Nearest-Greatest-to-left)
- [Nearest Smaller to Left](#Nearest-Smaller-to-Left)
- [Nearest Smaller to Right](#Nearest-Smaller-to-Right)
- [Stock Span Problem](#Stock-Span-Problem)
- [Maximum Area Histogram](#Maximum-Area-Histogram)
- [Maximum Area of Rectangle in Binary Matrix](#Maximum-Area-of-Rectangle-in-Binary-Matrix)
- [Rain Water Trapping](#Rain-Water-Trapping)
- [Minimum element in Stack (Implementation of MinStack)](#Minimum-element-in-Stack-(Implementation-of-MinStack))

## [Nearest Greatest to Right (Next largest element)](https://www.geeksforgeeks.org/next-greater-element/)

Given an array, print the Next Greater Element (NGE) for every element. The Next greater Element for an element x is the first greater element on the right side of x in the array. Elements for which no greater element exist, consider the next greater element as -1. 

Examples: 

1. For an array, the rightmost element always has the next greater element as -1.
2. For an array that is sorted in decreasing order, all elements have the next greater element as -1.
3. For the input array [4, 5, 2, 25], the next greater elements for each element are [5, 25, 25, -1].

#### Brute Force

In this approach we use two loops iterating over the array. Time Complexity for this solution is O(N2)
```python
for i in range(len(A)-1):
    for j in range(i+1, len(A)):
        if A[j] > A[i]:
            greaterEle = A[j]
            break
```

In this approach, in the second loop, the value of j is dependent on the value of i. If this happen with other proplem then the problem can be solved with stack approach.

#### Stack Approach


```python
def next_largest_element(arr):
    result = []
    stack = []
    
    for ele in arr[::-1]:
        if stack and ele >= stack[-1]:
            while stack and ele >= stack[-1]:
                stack.pop()
        
        result.append(stack[-1] if stack else -1)
        stack.append(ele)
        
    return result[::-1]

next_largest_element([1, 3, 2, 5])
```




    [3, 5, 5, -1]



## [Nearest Greatest to left](https://www.geeksforgeeks.org/closest-greater-or-same-value-on-left-side-for-every-element-in-array/)

Given an array of integers, find the closest (not considering the distance, but value) greater or the same value on the left of every element. If an element has no greater or same value on the left side, print -1.

#### Input / Output

input : arr[] = {10, 5, 11, 6, 20, 12} 

Output : -1, 10, -1, 10, -1, 20 


```python
def nearest_greatest_to_left(arr):
    result = []
    stack = []
    
    for ele in arr:
        if stack and ele >= stack[-1]:
            while stack and ele >= stack[-1]:
                stack.pop()
        
        result.append(stack[-1] if stack else -1)
        stack.append(ele)
        
    return result

nearest_greatest_to_left([5, 2, 3, 1])
```




    [-1, 5, 5, 3]



## [Nearest Smaller to Left](https://www.geeksforgeeks.org/find-the-nearest-smaller-numbers-on-left-side-in-an-array/)

Given an array of integers, find the nearest smaller number for every element such that the smaller element is on left side.

#### Input / Output

Input:  arr[] = {1, 6, 4, 10, 2, 5}

Output:         {_, 1, 1,  4, 1, 2}


```python
def nearest_smaller_to_left(arr):
    result = []
    stack = []
    
    for ele in arr:
        if stack and ele <= stack[-1]:
            while stack and ele <= stack[-1]:
                stack.pop()
        
        result.append(stack[-1] if stack else -1)
        stack.append(ele)
    
    return result
nearest_smaller_to_left([1, 2, 4, 7, 5, 1])
```




    [-1, 1, 2, 4, 4, -1]



## [Nearest Smaller to Right](https://www.geeksforgeeks.org/next-smaller-element/)

Given an array, print the Next Smaller Element (NSE) for every element. The Smaller smaller Element for an element x is the first smaller element on the right side of x in array. Elements for which no smaller element exist (on right side), consider next smaller element as -1.

Examples: 

1. For any array, rightmost element always has next smaller element as -1. 
2. For an array which is sorted in increasing order, all elements have next smaller element as -1. 
3. For the input array [4, 8, 5, 2, 25], the next smaller elements for each element are [2, 5, 2, -1, -1].


```python
def nearest_smaller_to_right(arr):
    result = []
    stack = []
    
    for ele in arr[::-1]:
        if stack and ele <= stack[-1]:
            while stack and ele <= stack[-1]:
                stack.pop()
        
        result.append(stack[-1] if stack else -1)
        stack.append(ele)
        
    return result[::-1]

nearest_smaller_to_right([4, 5, 2, 10, 8])
```




    [2, 2, -1, 8, -1]



## [Stock Span Problem](https://www.geeksforgeeks.org/the-stock-span-problem/)

The stock span problem is a financial problem where we have a series of n daily price quotes for a stock and we need to calculate span of stock’s price for all n days. 
The span Si of the stock’s price on a given day i is defined as the maximum number of consecutive days just before the given day, for which the price of the stock on the current day is less than or equal to its price on the given day. 

This problem is a variant of [Nearest Greatest to left](#Nearest-Greatest-to-left).

For example, if an array of 7 days prices is given as {100, 80, 60, 70, 60, 75, 85}, then the span values for corresponding 7 days are {1, 1, 1, 2, 1, 4, 6} 


```python
def stock_span_problem(arr):
    result = []
    stack = []
    
    for i, ele in enumerate(arr):
        if stack and ele > stack[-1][0]:
            while stack and ele > stack[-1][0]:
                stack.pop()
                
        result.append(i - stack[-1][1] if stack else i + 1)
        stack.append((ele, i))
    
    return result

stock_span_problem([100, 80, 60, 70, 60, 75, 85])
```




    [1, 1, 1, 2, 1, 4, 6]



## [Maximum Area Histogram](https://www.geeksforgeeks.org/largest-rectangle-under-histogram/)

Find the largest rectangular area possible in a given histogram where the largest rectangle can be made of a number of contiguous bars. For simplicity, assume that all bars have same width and the width is 1 unit. 

For example, consider the following histogram with 7 bars of heights {6, 2, 5, 4, 5, 1, 6}. The largest possible rectangle possible is 12 (see the below figure, the max area rectangle is highlighted in red)

<img src="https://media.geeksforgeeks.org/wp-content/cdn-uploads/histogram1.png">

#### Approach

1. Find the index of nearest smallest to left (say li is the index of smallest element from the left of element A[i])
2. Find the index of nearest smallest to right (say ri is the index of smallest element from the right of element A[i])
3. Calculate the width (say wi is the widht of element A[i] can be calculated as 'ri - li - 1')
4. Calculate the area (say ai is the area of element A[i] can be calculated as 'A[i] * wi')


```python
# Nearest Smallest to Left
def NSL(arr):
    result = []
    stack = []
    
    for i, ele in enumerate(arr):
        if stack and ele <= stack[-1][0]:
            while stack and ele <= stack[-1][0]:
                stack.pop()
        
        result.append(stack[-1][1] if stack else -1)
        stack.append((ele, i))
        
    return result

# Nearest Smallest to Right
def NSR(arr, n):
    result = []
    stack = []
    
    for i, ele in enumerate(arr[::-1]):
        if stack and ele <= stack[-1][0]:
            while stack and ele <= stack[-1][0]:
                stack.pop()
        
        result.append(stack[-1][1] if stack else -1)
        stack.append((ele, n-i-1))
        
    return result[::-1]

# Maximum Area Histogram
def MAH(arr):
    n = len(arr)
    left = NSL(arr)
    right = NSR(arr, n)
    
    width = [right[i]-left[i]-1 for i in range(n)]
    
    area = [arr[i]*width[i] for i in range(n)]
    
    return max(area)

MAH([6, 2, 5, 6, 4, 1, 6])
```




    12



## [Maximum Area of Rectangle in Binary Matrix](https://www.geeksforgeeks.org/maximum-size-rectangle-binary-sub-matrix-1s/)

Given a binary matrix, find the maximum size rectangle binary-sub-matrix with all 1’s.

Input:
```
0 1 1 0
1 1 1 1
1 1 1 1
1 1 0 0
```

Output :
```
1 1 1 1
1 1 1 1
```

Explanation : 

The largest rectangle with only 1's is from (1, 0) to (2, 3) which is
```
1 1 1 1
1 1 1 1 
```

This problem is similar to [Maximum Area Histogram](#Maximum-Area-Histogram).


```python
# Nearest Smallest to Left
def NSL(arr):
    result = []
    stack = []
    
    for i, ele in enumerate(arr):
        if stack and ele <= stack[-1][0]:
            while stack and ele <= stack[-1][0]:
                stack.pop()
        
        result.append(stack[-1][1] if stack else -1)
        stack.append((ele, i))
        
    return result

# Nearest Smallest to Right
def NSR(arr, n):
    result = []
    stack = []
    
    for i, ele in enumerate(arr[::-1]):
        if stack and ele <= stack[-1][0]:
            while stack and ele <= stack[-1][0]:
                stack.pop()
        
        result.append(stack[-1][1] if stack else n)
        stack.append((ele, n-i-1))
        
    return result[::-1]

# Maximum Area Histogram
def MAH(arr):
    n = len(arr)
    left = NSL(arr)
    right = NSR(arr, n)
    
    width = [right[i]-left[i]-1 for i in range(n)]
    area = [arr[i]*width[i] for i in range(n)]
    return max(area)

def max_area_of_rect_in_binary_matrix(mat):
    temp_mat = []
    
    for i, arr in enumerate(mat):
        if i == 0:
            temp_mat.append(mat[0])
        else:
            temp_mat.append([i+j if j != 0 else 0 for i, j in zip(temp_mat[i-1], mat[i])])
    
    result = [MAH(i) for i in temp_mat]
    
    return max(result)

mat = [[0, 1, 1, 0],
       [1, 1, 1, 1],
       [1, 1, 1, 1],
       [1, 1, 0, 0]]
max_area_of_rect_in_binary_matrix(mat)
```




    8



## [Rain Water Trapping](https://www.geeksforgeeks.org/trapping-rain-water/)

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

Stack is not used in this approach but stacks can be used to calculate the Maximum left and right of a particular element.

#### Input / Output

Input: [3, 0, 2, 0, 4]

Output: 7


```python
def rain_water_trapping(arr):
    max_l = []
    for i, ele in enumerate(arr):
        if i == 0:
            max_l.append(ele)
        else:
            max_l.append(max(max_l[i-1], ele))
    
    max_r = []
    for i, ele in enumerate(arr[::-1]):
        if i == 0:
            max_r.append(ele)
        else:
            max_r.append(max(max_r[i-1], ele))
    max_r = max_r[::-1]
    
    water_trapped = 0
    for i, ele in enumerate(arr):
        water_trapped += min(max_r[i], max_l[i]) - ele
    
    return water_trapped

rain_water_trapping([3, 0, 0, 2, 0, 4])
```




    10



## [Minimum element in Stack (Implementation of MinStack)](https://www.geeksforgeeks.org/design-a-stack-that-supports-getmin-in-o1-time-and-o1-extra-space/)


```python
class Stack:
    def __init__(self):
        self.stack = []
        self.min_stack = []
        
    def push(self, ele):
        if self.min_stack and self.min_stack[-1] >= ele:
            self.min_stack.append(ele)
        elif not self.min_stack:
            self.min_stack.append(ele)
        
        self.stack.append(ele)
    
    def pop(self):
        if self.min_stack[-1] == self.stack[-1]:
            self.min_stack.pop()
        return self.stack.pop()
    
    def min_ele(self):
        if self.min_stack:
            return self.min_stack[-1]
        return 0
```
