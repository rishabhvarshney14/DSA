# Dynamic Programming

- [0/1 Knapsack](#0/1-Knapsack)
 - Methods to solve 0/1 Knapsack
   - [Recursive Method](#Recursive-Method)
   - [Memomization Method](#Memomization-Method)
   - [Top-Down Approach](#Top-Down-Approach)
 - Problems based on 0/1 Knapsack
   - [Subset Sum Problem](#Subset-Sum-Problem)
   - [Equal Sum Partition](#Equal-Sum-Partition)
   - [Subset with sum divisible by m](#Subset-with-sum-divisible-by-m)
   - [Count of Subsets of a given Sum](#Count-of-Subsets-of-a-given-Sum)
   - [Minimum Subset Sum Difference](#Minimum-Subset-Sum-Difference)
   - [Count the number of subset with a given difference](#Count-the-number-of-subset-with-a-given-difference)
   - [Target Sum](#Target-Sum-(same-as-num_subset_given_diff))
- [Unbounded Knapsack](#Unbounded-Knapsack)
 - Problems based on Unbounded Knapsack
   - [Rod Cutting Problem](#Rod-Cutting-Problem)
   - [Maximize the number of segments of length p, q and r](#Maximize-the-number-of-segments-of-length-p,-q-and-r)
   - [Coin change problem : Maximum number of ways](#Coin-change-problem-:-Maximum-number-of-ways)
   - [Coin change problem : Minimum number of coins](#Coin-change-problem-:-Minimum-number-of-coins)
- [Longest common subsequence](#Longest-common-subsequence)
 - Methods to solve LCS Problems
   - [Using Recursion](#Using-Recursion)
   - [Memomization](#Memomization)
   - [Top Down Approach](#Top-Down-Approach)
 - Problesm based on LCS
   - [Longest Common Substring](#Longest-Common-Substring)
   - [Print Longest Common subsequence](#Print-Longest-Common-subsequence)
  

## [0/1 Knapsack](https://www.geeksforgeeks.org/0-1-knapsack-problem-dp-10/)

Given weights and values of n items, put these items in a knapsack of capacity W to get the maximum total value in the knapsack. In other words, given two integer arrays val[0..n-1] and wt[0..n-1] which represent values and weights associated with n items respectively. Also given an integer W which represents knapsack capacity, find out the maximum value subset of val[] such that sum of the weights of this subset is smaller than or equal to W. You cannot break an item, either pick the complete item or don’t pick it (0-1 property).

### Inputs


```python
wt = [1, 2, 3, 4, 5, 6]
val = [6, 1, 9, 4, 2, 8]
w = 7
```

### Recursive Method


```python
def recursive_01_knapsack(wt, val, w, n):
    if n == 0 or w == 0:
        return 0
    
    if wt[n-1] <= w:
        taken = val[n-1] + recursive_01_knapsack(wt, val, w-wt[n-1], n-1)
        not_taken = recursive_01_knapsack(wt, val, w, n-1)
        return max(taken, not_taken)
    else:
        return recursive_01_knapsack(wt, val, w, n-1)


recursive_01_knapsack(wt, val, w, len(wt))
```




    75



### Memomization Method


```python
t = [[-1 for _ in range(w+1)] for _ in range(len(wt)+1)]
def memomization_01_knapsack(wt, val, w, n):
    if n == 0 or w == 0:
        return 0
    
    if t[n][w] != -1:
        return t[n][w]
    elif wt[n-1] <= w:
        taken = val[n-1] + memomization_01_knapsack(wt, val, w-wt[n-1], n-1)
        not_taken = memomization_01_knapsack(wt, val, w, n-1)
        t[n][w] = max(taken, not_taken)
    else:
        t[n][w] = memomization_01_knapsack(wt, val, w, n-1)
    
    return t[n][w]
```


```python
memomization_01_knapsack(wt, val, w, len(wt))
```




    75



### Top-Down Approach


```python
def top_down_01_knapsack(wt, val, w, n):
    t = [[-1 for _ in range(w+1)] for _ in range(n+1)]
    for i in range(n+1):
        for j in range(w+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            elif wt[i-1] <= j:
                t[i][j] = max(val[i-1] + t[i-1][j - wt[i-1]], t[i-1][j])
            else:
                t[i][j] = t[i-1][j]
    
    return t[n][w]

top_down_01_knapsack(wt, val, w, len(wt))
```




    75



### [Subset Sum Problem](https://www.geeksforgeeks.org/subset-sum-problem-dp-25/)

Given a set of non-negative integers, and a value sum, determine if there is a subset of the given set with sum equal to given sum.

#### Input / Output

Input:arr = [3, 34, 4, 12, 5, 2], s = 9

Output: True

There is a subset (4, 5) with sum 9.


```python
def subset_sum_problem(arr, s):
    n = len(arr)
    t = [[False for _ in range(s+1)] for _ in range(n+1)]
    
    for i in range(n+1):
        for j in range(s+1):
            if i == 0:
                t[i][j] = False
                t[0][0] = False
            if j == 0:
                t[i][j] = True
            elif arr[i-1] <= j:
                t[i][j] = t[i-1][j-arr[i-1]] or t[i-1][j]
            else:
                t[i][j] = t[i-1][j]
    return t[i][j]

subset_sum_problem([3, 34, 4, 12, 5, 2], 9)
```




    True



### [Equal Sum Partition](https://www.geeksforgeeks.org/partition-problem-dp-18/)

Partition problem is to determine whether a given set can be partitioned into two subsets such that the sum of elements in both subsets is the same. 

#### Input/Output

arr[] = {1, 5, 11, 5}

Output: true 

The array can be partitioned as {1, 5, 5} and {11}


```python
def equal_sum_partition(arr):
    sum_arr = sum(arr)
    if sum_arr % 2 != 0:
        return False
    else:
        s = sum_arr // 2
        n = len(arr)
        t = [[False for _ in range(s+1)] for _ in range(n+1)]
        for i in range(n+1):
            for j in range(s+1):
                if i == 0:
                    t[i][j] = False
                    t[0][0] = True
                elif j == 0:
                    t[i][j] = True
                    
                elif arr[i-1] <= j:
                    t[i][j] = t[i-1][j-arr[i-1]] or t[i-1][j]
                else:
                    t[i][j] = t[i-1][j]
    
        return t[n][s]
    
equal_sum_partition([1, 5, 11, 5])
```




    True



### [Subset with sum divisible by m](https://www.geeksforgeeks.org/subset-sum-divisible-m/)

Given a set of non-negative distinct integers, and a value m, determine if there is a subset of the given set with sum divisible by m.

#### Input/Output

Input : arr = [3, 1, 7, 5], m = 6

Output : True

Input : arr = [1, 6], m = 5

Output : NO


```python
def subset_with_sum_div_m(arr, m):
    n = len(arr)

    dp = [False for _ in range(m)]

    for i in range(n):
        temp = [False for _ in range(m)]
        for j in range(m):
            if dp[j] == True and dp[(j+arr[i]) % m] == False:
                temp[(j+arr[i]) % m] = True

        for j in range(m):
            if temp[j] == True:
                dp[j] = True

        dp[arr[i]%m] = True

    return dp[0]
    
subset_with_sum_div_m([3, 1, 7, 5], 6)
```




    True



### [Count of Subsets of a given Sum](https://www.geeksforgeeks.org/count-of-subsets-with-sum-equal-to-x/)

Number of subsets in a given array whose sum is equal to given value of sum.

#### Input/Output

Input : arr[] : [2, 3, 5, 6, 8, 10], s = 10

Output : 3

Explanation : Three subsets [2, 3, 5], [2, 8], [10] exists whose sum is equal to s.


```python
def count_of_subsets_of_given_sum(arr, s):
    n = len(arr)
    t = [[-1 for _ in range(s+1)] for _ in range(n+1)]
    
    for i in range(n+1):
        for j in range(s+1):
            if i == 0:
                t[i][j] = 0
                t[0][0] = 1
            elif j == 0:
                t[i][j] = 1
            elif arr[i-1] <= j:
                t[i][j] = t[i-1][j-arr[i-1]] + t[i-1][j]
            else:
                t[i][j] = t[i-1][j]
    
    return t[n][s]

count_of_subsets_of_given_sum([2, 3, 5, 6, 8, 10], 10)
```




    3



### [Minimum Subset Sum Difference](https://www.geeksforgeeks.org/partition-a-set-into-two-subsets-such-that-the-difference-of-subset-sums-is-minimum/)

Given a set of integers, the task is to divide it into two sets S1 and S2 such that the absolute difference between their sums is minimum. 

If there is a set S with n elements, then if we assume Subset1 has m elements, Subset2 must have n-m elements and the value of abs(sum(Subset1) – sum(Subset2)) should be minimum.

#### Input/Output

Input:  arr[] = {1, 6, 11, 5} 

Output: 1

Explanation:

Subset1 = {1, 5, 6}, sum of Subset1 = 12 

Subset2 = {11}, sum of Subset2 = 11    


```python
def subset_sum(arr, s):
    n = len(arr)
    t = [[False for _ in range(s+1)] for _ in range(n+1)]
    
    for i in range(n+1):
        for j in range(s+1):
            if i == 0:
                t[i][j] = False
                t[0][0] = True
            elif j == 0:
                t[i][j] = True
            elif arr[i-1] <= j:
                t[i][j] = t[i-1][j-arr[i-1]] or t[i-1][j]
            else:
                t[i][j] = t[i-1][j]
    
    return [i for i in range(s // 2 + 1) if t[-1][i]]

def minimum_subset_sum_difference(arr):
    s = sum(arr)
    v = subset_sum(arr, s)
    mn = float('inf')
    
    for i in v:
        mn = min(mn, s - 2*i)
    return mn

minimum_subset_sum_difference([1, 5, 6, 11])
```




    1



### Count the number of subset with a given difference

Given a set of integers, the task is to divide it into two sets S1 and S2 such that the absolute difference between their sums should be equal to given value of s. 

If there is a set S with n elements, then if we assume Subset1 has m elements, Subset2 must have n-m elements and the value of abs(sum(Subset1) – sum(Subset2)) should be equal to given value of s.

#### Input/Output

Input:  arr[] = [1, 1, 2, 3], s = 1

Output: 3


```python
def count_subset_sum(arr, s):
    n = len(arr)
    t = [[0 for _ in range(s + 1)] for _ in range(n + 1)]
    
    for i in range(n+1):
        for j in range(s+1):
            if i == 0:
                t[i][j] = 0
                t[0][0] = 1
            elif j == 0:
                t[i][j] = 1
            elif arr[i-1] <= j:
                t[i][j] = t[i-1][j-arr[i-1]] + t[i-1][j]
            else:
                t[i][j] = t[i-1][j]
    
    return t[n][s]

def num_subset_given_diff(arr, diff):
    s_arr = sum(arr)
    sum_subset1 = (s_arr + diff) // 2
    
    count = count_subset_sum(arr, sum_subset1)
    
    return count

num_subset_given_diff([1, 1, 2, 3], 1)
```




    3



### Target Sum (same as num_subset_given_diff)

You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols + and -. For each integer, you should choose one from + and - as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

#### Input/Output

Input: nums is [1, 1, 1, 1, -1], S is 3. 

Output: 5

Explanation: 

-1+1+1+1+1 = 3

+1-1+1+1+1 = 3

+1+1-1+1+1 = 3

+1+1+1-1+1 = 3

+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.


```python
def target_sum(arr, s):
    return num_subset_given_diff(arr, s)

target_sum([1, 1, 2, 3], 1)
```




    3



## [Unbounded Knapsack](https://www.geeksforgeeks.org/unbounded-knapsack-repetition-items-allowed/)

Given a knapsack weight W and a set of n items with certain value vali and weight wti, we need to calculate the maximum amount that could make up this quantity exactly. This is different from classical Knapsack problem, here we are allowed to use unlimited number of instances of an item.

### Inputs


```python
wt = [1, 2, 3, 4, 5, 6]
val = [6, 1, 9, 4, 2, 8]
w = 7
```


```python
def unbounded_knapsack(wt,  val, w):
    n = len(wt)
    t = [[0 for _ in range(w+1)] for _ in range(n+1)]
    
    for i in range(n+1):
        for j in range(w+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            if wt[i-1] <= j:
                t[i][j] = max(val[i-1] + t[i][j-wt[i-1]], t[i-1][j])
            else:
                t[i][j] = t[i-1][j]
                
    return t[n][w]

unbounded_knapsack(wt, val, w)
```




    42



### [Rod Cutting Problem](https://www.geeksforgeeks.org/cutting-a-rod-dp-13/)

Given a rod of length n inches and an array of prices that contains prices of all pieces of size smaller than n. Determine the maximum value obtainable by cutting up the rod and selling the pieces. For example, if length of the rod is 8 and the values of different pieces are given as following, then the maximum obtainable value is 22 (by cutting in two pieces of lengths 2 and 6).

#### Input/Output

Input:

length[] = [1, 2, 3, 4, 5, 6, 7, 8]

price [] = [1, 5, 8, 9, 10, 17, 17, 20]
 
Rod length: 4

Output:

Cut the rod into two pieces of length 2 each to gain revenue of 5 + 5 = 10


```python
def rod_cutting_problem(length, price, max_len):
    n = len(length)
    t = [[0 for _ in range(max_len+1)] for _ in range(n + 1)]
    
    for i in range(n+1):
        for j in range(max_len+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            elif length[i-1] <= j:
                t[i][j] = max(price[i-1] + t[i][j - length[i-1]], t[i-1][j])
            else:
                t[i][j] = t[i-1][j]
                
    return t[n][max_len]

rod_cutting_problem([1, 2, 3, 4, 5, 6, 7, 8], [1, 5, 8, 9, 10, 17, 17, 20], 8)
```




    22



### [Maximize the number of segments of length p, q and r](https://www.geeksforgeeks.org/maximize-the-number-of-segments-of-length-p-q-and-r/)

Given a rod of length L, the task is to cut the rod in such a way that the total number of segments of length p, q and r is maximized. The segments can only be of length p, q, and r.

#### Input / Output

input: l = 11, p = 2, q = 3, r = 5

Output: 5 

Segments of 2, 2, 2, 2 and 3

Input: l = 7, p = 2, q = 5, r = 5 

Output: 2 

Segments of 2 and 5


```python
def maximizeTheCuts(l,p,q,r):
    dp = [-1 for _ in range(l+1)]
    dp[0] = 0

    for i in range(l+1):
        if dp[i] == -1:
            continue

        if i+p <=l:
            dp[i+p] = max(dp[i+p], dp[i]+1)
        if i+q <=l:
            dp[i+q] = max(dp[i+q], dp[i]+1)
        if i+r <=l:
            dp[i+r] = max(dp[i+r], dp[i]+1)

    return dp[l] if dp[l] != -1 else 0

maximizeTheCuts(7, 2, 5, 5)
```

### [Coin change problem : Maximum number of ways](https://www.geeksforgeeks.org/coin-change-dp-7/)

Given a value N, if we want to make change for N cents, and we have infinite supply of each of S = { S1, S2, .. , Sm} valued coins, how many ways can we make the change? The order of coins doesn’t matter.

#### Input/Output

Input: amount = 5, coins = [1, 2, 5]

Output: 4

Explanation: there are four ways to make up the amount:

5=5

5=2+2+1

5=2+1+1+1

5=1+1+1+1+1


```python
def coin_change_1(coins, s):
    n = len(coins)
    t = [[0 for _ in range(s + 1)] for _ in range(n+1)]
    
    for i in range(n+1):
        for j in range(s+1):
            if j == 0:
                t[i][j] = 1
                t[0][0] = 0
            elif coins[i-1] <= j:
                t[i][j] = t[i][j-coins[i-1]] + t[i-1][j]
            else:
                t[i][j] = t[i-1][j]
    
    return t[n][s]

coin_change_1([1, 3, 5], 10)
```




    7



### [Coin change problem : Minimum number of coins](https://www.geeksforgeeks.org/find-minimum-number-of-coins-that-make-a-change/)

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

#### Input/Output

Input: coins = [1,2,5], amount = 11

Output: 3

Explanation: 11 = 5 + 5 + 1


```python
import math

def coin_change_2(coins, s):
    n = len(coins)
    t = [[-1 for _ in range(s+1)] for _ in range(n+1)]
    
    for i in range(n+1):
        for j in range(s+1):
            if i == 0:
                t[i][j] = math.inf
            elif j == 0:
                t[i][j] = 0
            elif i == 1:
                if j%coins[i-1]==0:
                    t[i][j] = j // coins[i-1]
                else:
                    t[i][j] = math.inf
            elif coins[i-1] <= j:
                t[i][j] = min(1+t[i][j-coins[i-1]], t[i-1][j])
            else:
                t[i][j] = t[i-1][j]
    
    return t[n][s]

coin_change_2([1, 2, 3], 5)
```




    2



## Longest common subsequence

 Given two sequences, find the length of longest subsequence present in both of them. A subsequence is a sequence that appears in the same relative order, but not necessarily contiguous. For example, “abc”, “abg”, “bdf”, “aeg”, ‘”acefg”, .. etc are subsequences of “abcdefg”.
 
 ### Inputs


```python
X,Y = 'abcdgh', 'abcdfghr'
lenX, lenY = len(X), len(Y)
```

### Using Recursion


```python
def longest_common_subsequence_rec(X, Y, lenX, lenY):
    if lenX == 0 or lenY == 0:
        return 0
    elif X[lenX-1] == Y[lenY-1]:
        return 1 + longest_common_subsequence_rec(X, Y, lenX-1, lenY-1)
    else:
        return max(longest_common_subsequence_rec(X, Y, lenX-1, lenY), 
                   longest_common_subsequence_rec(X, Y, lenX, lenY-1))
    
longest_common_subsequence_rec(X, Y, lenX, lenY)
```




    6



### Memomization


```python
t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]
def longest_common_subsequence_memo(X, Y, lenX, lenY):
    if lenX == 0 or lenY == 0:
        t[lenX][lenY] = 0
    if t[lenX][lenY] != -1:
        pass
    elif X[lenX-1] == Y[lenY-1]:
        t[lenX][lenY] = 1 + longest_common_subsequence_memo(X, Y, lenX-1, lenY-1)
    else:
        t[lenX][lenY] = max(longest_common_subsequence_memo(X, Y, lenX-1, lenY),
                            longest_common_subsequence_memo(X, Y, lenX, lenY-1))
    
    return t[lenX][lenY]

longest_common_subsequence_memo(X, Y, lenX, lenY)
```




    6



### Top Down Approach


```python
def longest_common_subsequence_top_down(X, Y, lenX, lenY):
    t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]
    
    for i in range(lenX+1):
        for j in range(lenY+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            elif X[i-1] == Y[j-1]:
                t[i][j] = 1 + t[i-1][j-1]
            else:
                t[i][j] = max(t[i-1][j], t[i][j-1])
    
    return t[lenX][lenY]

longest_common_subsequence_top_down(X, Y, lenX, lenY)
```




    6



### Longest Common Substring

Given two strings ‘X’ and ‘Y’, find the length of the longest common substring.

#### Input/Output

Input : X = “GeeksforGeeks”, y = “GeeksQuiz” 

Output : 5 

Explanation:

The longest common substring is “Geeks” and is of length 5.


```python
def longest_common_substring(X, Y, lenX, lenY):
    t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]
    longest_common = 0
    
    for i in range(lenX+1):
        for j in range(lenY+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            elif X[i-1] == Y[j-1]:
                t[i][j] = 1 + t[i-1][j-1]
                if t[i][j] > longest_common:
                    longest_common = t[i][j]
            else:
                t[i][j] = 0
    
    return longest_common

longest_common_substring(X, Y, lenX, lenY)
```




    4



### Print Longest Common subsequence

Given two sequences, print the longest subsequence present in both of them.

#### Input/Output

LCS for input Sequences “ABCDGH” and “AEDFHR” is “ADH” of length 3.


```python
def print_longest_common_subsequence(X, Y, lenX, lenY):
    t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]
    
    for i in range(lenX+1):
        for j in range(lenY+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            if X[i-1] == Y[j-1]:
                t[i][j] = 1 + t[i-1][j-1]
            else:
                t[i][j] = max(t[i][j-1], t[i-1][j])
    
    i = lenX
    j = lenY 
    longest_common = ''
    while i != 0 or j != 0:
        if X[i-1] == Y[j-1]:
            longest_common = X[i-1] + longest_common
            i -= 1
            j -= 1
        else:
            if t[i][j-1] > t[i-1][j]:
                j -= 1
            else:
                i -= 1
                
    return longest_common

print_longest_common_subsequence(X, Y, lenX, lenY)
```




    'abcdgh'



### Print Longest common substring

Given two sequences, print the longest substring present in both of them.


```python
def print_longest_common_substring(X, Y, lenX, lenY):
    t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]
    longest_common = ''
    
    for i in range(lenX+1):
        for j in range(lenY+1):
            if  i == 0 or j == 0:
                t[i][j] = ''
            elif X[i-1] == Y[j-1]:
                t[i][j] = t[i-1][j-1] + X[i-1]
                
                if len(t[i][j]) > len(longest_common):
                    longest_common = t[i][j]
            else:
                t[i][j] = ''
        
    return longest_common

print_longest_common_substring(X, Y, lenX, lenY)
```




    'abcd'



### Shortest Common SuperSubsequence

Given two strings str1 and str2, the task is to find the length of the shortest string that has both str1 and str2 as subsequences.

#### Input/Output

Input:   str1 = "geek",  str2 = "eke"

Output: 5

Explanation: 

String "geeke" has both string "geek" 
and "eke" as subsequences.


```python
def shortest_common_super_subsequence(X, Y, lenX, lenY):
    t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]
    
    longest_common = -1
    
    for i in range(lenX+1):
        for j in range(lenY+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            elif X[i-1] == Y[i-1]:
                t[i][j] = 1 + t[i-1][j-1]
                if longest_common < t[i][j]:
                    longest_common = t[i][j]
            else:
                t[i][j] = 0
    
    return lenX + lenY - longest_common

shortest_common_super_subsequence(X, Y, lenX, lenY)
```




    10



### Minimum number of insertion and deletion to convert a string X to Y

Given two strings ‘str1’ and ‘str2’ of size m and n respectively. The task is to remove/delete and insert the minimum number of characters from/in str1 to transform it into str2. It could be possible that the same character needs to be removed/deleted from one point of str1 and inserted to some another point.

#### Input/Output

Input : 

str1 = "heap", str2 = "pea" 

Output : 

Minimum Deletion = 2 and

Minimum Insertion = 1

Explanation:

p and h deleted from heap
Then, p is inserted at the beginning
One thing to note, though p was required yet
it was removed/deleted first from its position and
then it is inserted to some other position.
Thus, p contributes one to the deletion_count
and one to the insertion_count.



```python
def min_insrt_del(X, Y, lenX, lenY):
    t = [[-1 for _ in range(lenY+1)] for _ in range (lenX+1)]

    num_insrt = 0
    num_del = 0
    
    longest_common = 0
    
    for i in range(lenX+1):
        for j in range(lenY+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            elif X[i-1] == Y[j-1]:
                t[i][j] = 1 + t[i-1][j-1]
                if t[i][j] > longest_common:
                    longest_common = t[i][j]
            else:
                t[i][j] = max(t[i][j-1], t[i-1][j])
                
    num_del = lenX - longest_common
    num_insrt = lenY - longest_common
    
    return num_del, num_insrt

min_insrt_del(X, Y, lenX, lenY)
```




    (0, 2)



### Longest Palindromic Subsequence

Given a sequence, find the length of the longest palindromic subsequence in it.

#### Input/Output

Input: string = "cbbd"

Output: 2

Explaination:

One possible longest palindromic subsequence is "bb".


```python
def longest_palindromic_subsequence(string):
    n = len(string)
    rev_string = string[::-1]
    t = [[-1 for _ in range(n+1)] for _ in range(n+1)]
    
    longest_palindromic = 0
    
    for i in range(n+1):
        for j in range(n+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            elif string[i-1] == rev_string[j-1]:
                t[i][j] = 1 + t[i-1][j-1]
                if t[i][j] > longest_palindromic:
                    longest_palindromic = t[i][j]
            else:
                t[i][j] = max(t[i-1][j], t[i][j-1])                
    
    return longest_palindromic

longest_palindromic_subsequence('agbcba')
```




    5



### Minimum number of deletion to make a string palindrome

Given a string of size ‘n’. The task is to remove or delete the minimum number of characters from the string so that the resultant string is a palindrome. 

Note: The order of characters should be maintained.

#### Input/Output

Input : aebcbda

Output : 2

Remove characters 'e' and 'd'
Resultant string will be 'abcba'
which is a palindromic string


```python
def min_del_to_palindrome(string):
    n = len(string)
    rev_string = string[::-1]
    
    t = [[-1 for _ in range(n+1)] for _ in range(n+1)]
    
    longest_common = 0
    
    for i in range(n+1):
        for j in range(n+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            elif string[i-1] == rev_string[j-1]:
                t[i][j] = 1 + t[i-1][j-1]
                if t[i][j] > longest_common:
                    longest_common = t[i][j]
                    
            else:
                t[i][j] = max(t[i-1][j], t[i][j-1])
    
    return n - longest_common

min_del_to_palindrome('agbcba')
```




    1



### Print Shortest Common SuperSequence

Given two strings X and Y, print the shortest string that has both X and Y as subsequences. If multiple shortest supersequence exists, print any one of them.

#### Input/Output


Input: X = "AGGTAB",  Y = "GXTXAYB"

Output: "AGXGTXAYB" OR "AGGXTXAYB" OR Any string that represents shortest
supersequence of X and Y


```python
def print_shortest_super_sequence(X, Y, lenX, lenY):
    t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]
    
    result = ''
    
    for i in range(lenX+1):
        for j in range(lenY+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            elif X[i-1] == Y[j-1]:
                t[i][j] = 1 + t[i-1][j-1]
            else:
                t[i][j] = max(t[i-1][j], t[i][j-1])
    
    i = lenX
    j = lenY
    
    while i != 0 or j != 0:
        if X[i-1] == Y[j-1]:
            result = X[i-1] + result
            i -= 1
            j -= 1
        elif t[i][j-1] > t[i-1][j]:
            result = Y[j-1] + result
            j -= 1
        else:
            result = X[i-1] + result
            i -= 1

    return result

print_shortest_super_sequence('acbcf', 'abcdaf', 5, 6)
```




    'acbcdaf'



### Longest Repeating SubSequence

Given a string, find length of the longest repeating subseequence such that the two subsequence don’t have same string character at same position, i.e., any i’th character in the two subsequences shouldn’t have the same index in the original string.

#### Input/Output

Input: str = "aab"


Output: 1

The two subssequence are 'a'(first) and 'a'(second). 
Note that 'b' cannot be considered as part of subsequence 
as it would be at same index in both.


```python
def longest_repeating_subsequence(string):
    n = len(string)
    
    t = [[-1 for _ in range(n+1)] for _ in range(n+1)]
    
    result = ''
    
    for i in range(n+1):
        for j in range(n+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            elif string[i-1] == string[j-1] and i != j:
                t[i][j] = 1 + t[i-1][j-1]
                if string[i-1] not in result:
                    result = result + string[i-1]
            else:
                t[i][j] = max(t[i-1][j], t[i][j-1])
    
    return result, t[n][n]

longest_repeating_subsequence('AABEBCDD')
```




    ('ABD', 3)



### Sequence Pattern Matching


```python
def sequence_pattern_matching(a, b):
    len_a = len(a)
    len_b = len(b)

    t = [[-1 for _ in range(len_b+1)] for _ in range(len_a+1)]
    
    for i in range(len_a+1):
        for j in range(len_b+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            elif a[i-1] == b[j-1]:
                t[i][j] = 1 + t[i-1][j-1]
            else:
                t[i][j] = max(t[i-1][j], t[i][j-1])
                
    return t[len_a][len_b] == len_a or t[len_a][len_b] == len_b

sequence_pattern_matching('AXY', 'ADXCPY')
```




    True



### Minimum number of insertion to make a string palindrome
#### (number of insertion == number of deletion)

Given a string str, the task is to find the minimum number of characters to be inserted to convert it to palindrome.

Example:

ab: Number of insertions required is 1 i.e. bab

aa: Number of insertions required is 0 i.e. aa


```python
def min_insrt_palindrome(string):
    n = len(string)
    rev_string = string[::-1]
    
    t = [[-1 for _ in range(n+1)] for _ in range(n+1)]
    
    for i in range(n+1):
        for j in range(n+1):
            if i == 0 or j == 0:
                t[i][j] = 0
            elif string[i-1] == rev_string[j-1]:
                t[i][j] = 1 + t[i-1][j-1]
            else:
                t[i][j] = max(t[i-1][j], t[i][j-1])
                
    return n - t[n][n]

min_insrt_palindrome('aebcbda')
```




    2



## Matrix Chain Multiplication

Given a sequence of matrices, find the most efficient way to multiply these matrices together. The problem is not actually to perform the multiplications, but merely to decide in which order to perform the multiplications.

We have many options to multiply a chain of matrices because matrix multiplication is associative. In other words, no matter how we parenthesize the product, the result will be the same. For example, if we had four matrices A, B, C, and D, we would have: 

(ABC)D = (AB)(CD) = A(BCD) = ....

However, the order in which we parenthesize the product affects the number of simple arithmetic operations needed to compute the product, or the efficiency. For example, suppose A is a 10 × 30 matrix, B is a 30 × 5 matrix, and C is a 5 × 60 matrix. Then,  

(AB)C = (10×30×5) + (10×5×60) = 1500 + 3000 = 4500 operations
A(BC) = (30×5×60) + (10×30×60) = 9000 + 18000 = 27000 operations.

### Inputs


```python
arr = [40, 20, 30, 10, 30]
n = len(arr)
```


```python
import math

def matrix_chain_multiplication(arr, i ,j):
    if i >= j:
        return 0
    
    ans = math.inf

    for k in range(i, j):
        before = matrix_chain_multiplication(arr, i, k)
        after = matrix_chain_multiplication(arr, k+1, j)
        temp = before + after + arr[i-1] * arr[k] * arr[j]
        if temp < ans:
            ans = temp
    
    return ans        

matrix_chain_multiplication(arr, 1, n-1)
```




    26000



### Matrix Chain Multiplication Memomization


```python
t = [[-1 for _ in range(n)] for _ in range(n)]
def matrix_chain_multiplication_memo(arr, i, j):
    if i >= j:
        return 0
    if t[i][j] != -1:
        return t[i][j]
    
    temp = math.inf
    for k in range(i, j):
        before = matrix_chain_multiplication_memo(arr, i, k)
        after = matrix_chain_multiplication_memo(arr, k+1, j)
        temp = min(temp, before+after+arr[i-1]*arr[k]*arr[j])
    
    t[i][j] = temp
    return temp

matrix_chain_multiplication_memo(arr, 1, n-1)
```




    26000



## Palindrome Partitioning 

Given a string, a partitioning of the string is a palindrome partitioning if every substring of the partition is a palindrome. For example, “aba|b|bbabb|a|b|aba” is a palindrome partitioning of “ababbbabbababa”. Determine the fewest cuts needed for a palindrome partitioning of a given string. For example, minimum of 3 cuts are needed for “ababbbabbababa”. The three cuts are “a|babbbab|b|ababa”. If a string is a palindrome, then minimum 0 cuts are needed. If a string of length n containing all different characters, then minimum n-1 cuts are needed.

### Inputs


```python
import math
string = "abbab"
n = len(string)
i = 0
j = n - 1
```

### Palindrome Partitioning Recursive


```python
def is_palindrome(string):
    if len(string) == 1:
        return True
    elif string == string[::-1]:
        return True
    return False

def palindrome_partition(string, i, j):
    if i >= j:
        return 0
    if is_palindrome(string[i:j+1]):
        return 0
    
    max_ = math.inf
    for k in range(i, j):
        before = palindrome_partition(string, i, k)
        after = palindrome_partition(string, k+1, j)
        temp = before + after + 1
        if temp < max_:
            max_ = temp
        
    return max_

palindrome_partition(string, i, j)
```




    1



### Palindrome Partitioning Memomization


```python
t = [[-1 for _ in range(n)] for _ in range(n)]
def palindrome_partition_memo(string, i, j):
    if i >= j:
        return 0
    if is_palindrome(string[i:j+1]):
        return 0
    if t[i][j] != -1:
        return t[i][j]
    
    temp = math.inf
    for k in range(i, j):
        before = palindrome_partition_memo(string, i, k)
        after  = palindrome_partition_memo(string, k+1, j)
        temp = min(temp, before+after+1)
    
    t[i][j] = temp
    return t[i][j]

palindrome_partition_memo(string, i, j)
```




    1



### Palindrome Partition Optimization

Optimizations: We can store our answer of isPalindrome(s, i, j) in a dp state (i, j). Note that every recursive call for the left and right parts of the substring stop when the base case (i.e the partitioned string is a palindrome) is reached. The major optimization in this question is that we partition the string only when the left part i.e. s[i...k] is a palindrome. Doing this we're effectively reducing a lot of unnecessary checks for every k.


```python
t = [[-1 for _ in range(n+1)] for _ in range(n+1)]
def palindrome_partition_opt(string, i, j):
    if i >= j:
        return 0
    if is_palindrome(string[i:j+1]):
        return 0
    
    temp = math.inf
    for k in range(i, j):
        # Optimization 
        if not self.is_palindrome(string[i:k+1]):
            continue
        if t[i][k] != -1:
            left = t[i][k]
        else:
            left = palindrome_partition_opt(string, i, k)
        if t[k+1][j] != -1:
            right = t[k+1][j]
        else:
            right = palindrome_partition_opt(string, k+1, j)
            
        temp = min(temp, left+right+1)
        
    t[i][j] = temp
    
    return t[i][j]

palindrome_partition_opt(string, i, j)
```




    1



### Evaluate expression to True (Boolean Parenthisization)

Given a boolean expression with following symbols. 

Symbols

 - 'T' ---> true 
 -  'F' ---> false 

And following operators filled between symbols 

Operators
 -    &   ---> boolean AND
 -   |   ---> boolean OR
 -   ^   ---> boolean XOR 

Count the number of ways we can parenthesize the expression so that the value of expression evaluates to true. 

#### Input


```python
string = 'T|F&T^F'
n = len(string)
i = 0
j = n-1
```

### Recursive Method


```python
def eval_expr_true(string, i, j, isTrue=True):
    if i > j:
        return False
    if i == j:
        if isTrue == True:
            return string[i] == 'T'
        else:
            return string[i] == 'F'
        
    ans = 0
    
    for k in range(i+1, j):
        if string[k] in '|&^':
            lt = eval_expr_true(string, i, k-1, True)
            lf = eval_expr_true(string, i, k-1, False)
            rt = eval_expr_true(string, k+1, j, True)
            rf = eval_expr_true(string, k+1, j, False)
            
            if string[k] == '|':
                temp = (lt and rt) + (lf and rt) + (lt and rf) if isTrue else (lf and rf)
            elif string[k] == '&':
                temp = (lt and rt) if isTrue else (lf and rf) + (lf and rt) + (lt and rf)
            elif string[k] == '^':
                temp = (lt and rf) + (lf and rt) if isTrue else (lf and rf) + (lt and rt)
            
            ans += temp
    return ans

eval_expr_true(string, i, j)
```




    4



### Memomization Method


```python
t = [[[-1 for _ in range(n)] for _ in range(n)] for _ in range(2)]
def eval_expr_true_memo(string, i, j, isTrue=True):
    if i > j:
        return 0
    if i == j:
        return string[i] == 'T' if isTrue else string[i] == 'F'
    
    T = 1 if isTrue else 0
    if t[T][i][j] != -1:
        return t[T][i][j]

    ans = 0
    
    for k in range(i+1, j):
        if string[k] in '|&^':
            lt = eval_expr_true_memo(string, i, k-1, True)
            lf = eval_expr_true_memo(string, i, k-1, False)
            rt = eval_expr_true_memo(string, k+1, j, True)
            rf = eval_expr_true_memo(string, k+1, j, False)
            
            if string[k] == '|':
                temp = (lt and rt) + (lf and rt) + (lt and rf) if isTrue else (lf and rf)
            elif string[k] == '&':
                temp = (lt and rt) if isTrue else (lf and rf) + (lf and rt) + (lt and rf)
            elif string[k] == '^':
                temp = (lt and rf) + (lf and rt) if isTrue else (lf and rf) + (lt and rt)
            
            ans += temp
            
    t[T][i][j] = ans
    return ans

eval_expr_true_memo(string, i, j)
```




    4



## Scrambled String

Given two strings S1 and S2 of equal length, the task is to determine if S2 is a scrambled form of S1.

### Inputs


```python
A, B = 'rgtae', 'great'
n = len(A)
```

### Recursion


```python
def scrambled_string(A, B):
    len_A = len(A)
    len_B = len(B)
    
    if len_A != len_B:
        return False
    if len_A < 1 or len_B < 1:
        return False
    if A == B:
        return True
    if sorted(A) != sorted(B):
        return False
    
    for i in range(1, len_B):
        if scrambled_string(A[:i], B[:i]) and scrambled_string(A[i:], B[i:]):
            return True
        if scrambled_string(A[:i], B[-i:]) and scrambled_string(A[-i:], B[:i]):
            return True
    
    return False

scrambled_string(A, B)
```




    True



### Memomization


```python
t = [[-1 for _ in range(n)] for _ in range(n)]
def scrambled_string_memo(A, B):
    len_A = len(A)
    len_B = len(B)
    
    if len_A != len_B:
        return False
    if len_A < 1 or len_B < 1:
        return False
    if A == B:
        return True
    if t[len_A-1][len_A-1] != -1:
        return t[len_A-1][len_A-1]
    if sorted(A) != sorted(B):
        return False
    
    for i in range(1, len_B):
        if scrambled_string_memo(A[:i], B[:i]) and scrambled_string_memo(A[i:], B[i:]):
            t[i][i] = True
            return True
        if scrambled_string_memo(A[:i], B[-i:]) and scrambled_string_memo(A[-i:], B[:i]):
            t[i][i] = True
            return True
    
    t[i][i] = False
    return False

scrambled_string_memo(A, B)
```




    True



## Egg Droping Problem

Given a certain amount of floors of a building (say f number of floors) and also given certain amount of eggs (say e number of eggs) …

What is the least amount of egg drops one has to perform to find out the threshold floor? (Threshold floor is one from which the egg starts breaking and also egg breaks for all the floors above. Also, if egg dropped from any floor below the threshold floor, it won’t 
break.)

Constraints:

- An egg that survives a fall can be used again.

- A broken egg must be discarded.

- The effect of a fall is the same for all eggs.

- If an egg breaks when dropped, then it would break if dropped from a higher floor.

- If an egg survives a fall then it would survive a shorter fall.

### Inputs


```python
e = 3
f = 5
```

### Recursion


```python
import math

def egg_droping(e, f):
    if e == 0 or f == 0:
        return 0
    
    if e == 1 or f == 1:
        return f
    
    mn = math.inf
    
    for k in range(1, f+1):
        temp = 1 + max(egg_droping(e-1, k-1), egg_droping(e, f-k))
        mn = min(mn, temp)
        
    return mn

egg_droping(e, f)
```




    3



### Memomization


```python
t = [[-1 for _ in range(e)] for _ in range(f)]
def egg_droping_memo(e, f):
    if e == 0 or f == 0:
        return 0
    if e == 1 or f == 1:
        return f
    
    if t[f-1][e-1] != -1:
        return t[f-1][e-1]
    
    mn = math.inf
    
    for k in range(1, f+1):
        temp = 1 + max(egg_droping_memo(e-1, k-1), egg_droping_memo(e, f-k))
        mn = min(temp, mn)
        
    t[f-1][e-1] = mn
    return mn

egg_droping_memo(e, f)
```




    3



# GeeksForGeeks DP

### [Gold Mine Problem](https://www.geeksforgeeks.org/gold-mine-problem/)


```python
def gold_mine_problem(mat):
    n = len(mat)
    m = len(mat[0])
    
    cache = [[-1 for _ in range(m)] for _ in range(n)]
    
    def helper(i, j):
        if cache[i][j] != -1:
            return cache[i][j]
        
        if j == m-1:
            cache[i][j] = mat[i][j]
        else:
            right_up = 0
            if i != 0:
                right_up = helper(i-1, j+1)
            
            right = helper(i, j+1)
            
            right_down = 0
            if i != n-1:
                right_down = helper(i+1, j+1)
                
            cache[i][j] = mat[i][j] + max(right_up, right, right_down)
        
        return cache[i][j]
    
    result = 0
    for i in range(n):
        result = max(result, helper(i, 0))
        
    return result

gold_mine_problem([[1, 3, 3], [2, 1, 4], [0, 6, 4]])
```




    12


