#The unique solution to a number equation
Now suppose we have such a number equation:
                                        AB^C = DEF*GHIJ
where all the LETTERS are in `range(10)`, which means the number list `[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]`. What we need to do is just to fill all the letters into the formula above to make the eqautin hold, of course all the letter should be unique.

There is a natural consideration to solve this problem by using the **full permutations**. If we can give out all the possible permutations of the number list `[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]`, then the question comes to verify the "solutions" one by one. 

Python has a built-in module called **`itertools`** with a function **`permutations`**, which can help us to generate all the permutations. But the result shows this enumerated method is quite slow. It takes around 1 minute on average.

```Python
import itertools
import numpy as np
import time

START_TIME = time.clock()

num_set = list(range(10))
perms = list(itertools.permutations(num_set))

to_AB = np.array([10, 1])
to_DEF = np.array([100, 10, 1])
to_GHIJ = np.array([1000, 100, 10, 1])

results = []

for perm in perms:
    if perm[0] <= 2:
        continue
    
    C = perm[0]
    AB = np.array(perm[1:3]).dot(to_AB)
    DEF = np.array(perm[3:6]).dot(to_DEF)
    GHIJ = np.array(perm[6:]).dot(to_GHIJ)
    
    if AB**C == DEF*GHIJ:
        results.append(perm)
    else:
        continue

RESULTS = results.copy()

for result in results:
    if any(np.array(result)[[1, 3, 6]] == 0):
        RESULTS.remove(result)
        
TIME_DURATION = time.clock() - START_TIME 

print('   %i SOLUTIONS TO THE EQUATION   '.center(65, '=') % len(RESULTS), '\n')    
for result in RESULTS:
    C = result[0]
    AB = np.array(result[1:3]).dot(to_AB)
    DEF = np.array(result[3:6]).dot(to_DEF)
    GHIJ = np.array(result[6:]).dot(to_GHIJ)
    print("   %i**%i == %i*%i   ".center(65, ' ') % (AB, C, DEF, GHIJ))

print('\n', 'TIME DURATION: %.2f seconds'.rjust(65, ' ') % TIME_DURATION)


================   1 SOLUTIONS TO THE EQUATION   =============== 

                         84**3 == 576*1029                         

                                       TIME DURATION: 67.21 seconds
```
                                       
Then we have to try some more ways to optimize the stupid algorithm, notice that DEF.GHIJ must be more than 100000, then C will be choosen in **`range(3, 10)`**. So we can firstly choose C in `range(3, 10)`, and then A, B will be choosen in the candidate list, which is `AB_list = list(range(10)); AB_list.remove(C)`
