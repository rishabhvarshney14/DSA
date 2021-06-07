# Recursion

- [Print 1 to N](#Print-1-to-N)
- [Print N to 1](#Print-N-to-1)

## Print 1 to N


```python
def printToN(n):
    if n == 1:
        print(1, end=" ")
    else:
        printToN(n-1)
        print(n, end=" ")
    
printToN(10)
```

    1 2 3 4 5 6 7 8 9 10 

## Print N to 1


```python
def printNto1(n):
    if n == 1:
        print(1, end=" ")
    else:
        print(n, end=" ")
        printNto1(n-1)
        
printNto1(10)
```

    10 9 8 7 6 5 4 3 2 1 


```python

```
