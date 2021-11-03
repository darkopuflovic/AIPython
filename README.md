```python
def fibonacciMemo(n, memo = {}):
    if (n <= 1):
        memo[n] = n
        return n
    else:
        if ((n - 1) in memo.keys()):
            nm1 = memo[n - 1]
        else:
            nm1 = fibonacciMemo(n - 1, memo)
            memo[n - 1] = nm1
        if ((n - 2) in memo.keys()):
            nm2 = memo[n - 2]
        else:
            nm2 = fibonacciMemo(n - 2, memo)
            memo[n - 2] = nm2
    return nm1 + nm2
```