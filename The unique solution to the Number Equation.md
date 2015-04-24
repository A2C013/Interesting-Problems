#The unique solution to a number equation
Now suppose we have such a number equation:
                                        AB^C = DEF*GHIJ
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


