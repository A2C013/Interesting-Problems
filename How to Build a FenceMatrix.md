#How to Build a FenceMatrix

A fence matrix is a _square matrix_ consisted of 0 and 1 with an _**odd**_ order. And the central element of a fence matrix must be **1**. 

The 1, 3 and 5 oder fence matrixes is presented below:
* 1 order 

    1|
    ---|
* 3 order

    0|0|0
    ---|---|---
    0|1|0
    0|0|0

* 5 order

    1|1|1|1|1
    ---|---|---|---|---
    1|0|0|0|1
    1|0|1|0|1
    1|0|0|0|1
    1|1|1|1|1

So comes the question: _**How to build a fence matrix with a given order (odd number)?**_

Here is the code which can create or build such a matrix on the platform of **_MATLAB_**:
```Matlab
function X = FenceMatrix()
    n = input('Please enter the matrix order(must be odd):');
    
    while mod(n, 2) == 0
        n = input('INVALID INPUT, ENTER THE ORDER AGAIN:');
    end
        
    X = zeros(n);
    if n == 1
        X = ones(1);
    elseif mod((n+1)/2,2) == 0
        for i = 2:2:(n+1)/2
            X([i,n+1-i], i:n+1-i) = 1;
            X(i:n+1-i, [i,n+1-i]) = 1;
        end
    else
        for i = 1:2:(n+1)/2
            X([i,n+1-i], i:n+1-i) = 1;
            X(i:n+1-i, [i,n+1-i]) = 1;
        end
    end
```
```MATLAB
>> FenceMatrix()
Please enter the matrix order(must be odd)12
INVALID INPUT, ENTER THE ORDER AGAIN:7

ans =

     0     0     0     0     0     0     0
     0     1     1     1     1     1     0
     0     1     0     0     0     1     0
     0     1     0     1     0     1     0
     0     1     0     0     0     1     0
     0     1     1     1     1     1     0
     0     0     0     0     0     0     0
```
The solution or function above also can be transplanted to the platform of _**Python**_.

This is the Python version of `FenceMatrix()`:
```Python
import numpy as np

def FenceMatrix():
    n = input('Enter the matrix order(must be odd):')
    while n%2 == 0:
        n = input('INVALID INPUT, ENTER AGAIN:')
    
    X = np.zeros((n, n))
    if n == 1:
        return np.ones(1)
    elif (n+1)/2%2 == 0:
    	for i in range(1, (n+1)/2+1, 2):
    		X[[i,-i-1], i:-i] = 1
    		X[i:-i, [i,-i-1]] = 1
    		
    else:
    	for i in range(0, (n+1)/2+1, 2):
    		X[[i,-i-1], i:n-i] = 1
    		X[i:n-i, [i,-i-1]] = 1
    return X
```
```Python
In [xx]: FenceMatrix()
Enter he matrix order(must be odd):9
Out[xx]:
array([[ 1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.],
       [ 1.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  1.],
       [ 1.,  0.,  1.,  1.,  1.,  1.,  1.,  0.,  1.],
       [ 1.,  0.,  1.,  0.,  0.,  0.,  1.,  0.,  1.],
       [ 1.,  0.,  1.,  0.,  1.,  0.,  1.,  0.,  1.],
       [ 1.,  0.,  1.,  0.,  0.,  0.,  1.,  0.,  1.],
       [ 1.,  0.,  1.,  1.,  1.,  1.,  1.,  0.,  1.],
       [ 1.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  1.],
       [ 1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.]])

```
By now, we have figured out a direct but kind of "stupid way" to solve the problem. Considering the fence matrix's structural features, a _**recursive function**_ may be a good choice.

We then rewrite the `FenceMatrix()` function as below:

