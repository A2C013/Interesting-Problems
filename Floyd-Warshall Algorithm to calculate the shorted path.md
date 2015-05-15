#### [Floyd-Warshall Algorithm](http://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm) to calculate the shorted path between any two vertexes in a [directed graph](http://en.wikipedia.org/wiki/Directed_graph).

We all know that [Dijkstra's algorithm](http://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) can be used to calculate all the shorted pathes between the vertex source and all the other vertexes. And the Floyd-Warshall Algorithm can solve the shortest path problem between any two vertexes in a directed graph.

Let's see a simple case:

```Python
## Floyd algorithm to calculate the shortest path between two vertexes.
## Directed Graph
import numpy as np

M = np.inf
## initializing the weighted adjacent matrix **origD**
origD = np.array([[-1, 1, 5, 12],
                  [M, -1, 3, M],
                  [M, M, -1, 7],
                  [5, M, 10, -1]])

pathMap = {key: val for key, val in enumerate('ABCD')}
origPath = np.zeros_like(origD, dtype='<U11')

#############initializing the original path##############
r, c = origPath.shape

for i in range(r):
    for j in range(c):
        if origD[i, j] == -1:
            origPath[i, j] = 'X'
        elif origD[i, j] == np.inf:
            origPath[i, j] = '|'
        else:
            origPath[i, j] = '->'.join([pathMap[i], pathMap[j]])

print('The original weighted adjacent matrix and pathes:')
print(origD)
print(origPath)
print('-'*25)
############# calculate the optimal pathes ##############

ex_D = origD.copy()
ex_Path = origPath.copy()
new_D = ex_D.copy()
new_Path = ex_Path.copy()

for i in range(r):
    index = list(range(r))
    index.remove(i)

    for j in index:
        for k in index:
            if ex_D[j, k] > ex_D[j, i] + ex_D[i, k]:
                new_D[j, k] = ex_D[j, i] + ex_D[i, k]
                new_Path[j, k] = '->'.join([ex_Path[j, i], ex_Path[i, k][3:]])

    new_D, ex_D = new_D.copy(), new_D
    new_Path, ex_Path= new_Path.copy(), new_Path

print('The shortest length and pathes:')
print(new_D)
print(new_Path)
```

The results:
```Python
The original weighted adjacent matrix and pathes:
[[ -1.   1.   5.  12.]
 [ inf  -1.   3.  inf]
 [ inf  inf  -1.   7.]
 [  5.  inf  10.  -1.]]
[['X' 'A->B' 'A->C' 'A->D']
 ['|' 'X' 'B->C' '|']
 ['|' '|' 'X' 'C->D']
 ['D->A' '|' 'D->C' 'X']]
-------------------------
The shortest length and pathes:
[[ -1.   1.   4.  11.]
 [ 15.  -1.   3.  10.]
 [ 12.  13.  -1.   7.]
 [  5.   6.   9.  -1.]]
[['X' 'A->B' 'A->B->C' 'A->B->C->D']
 ['B->C->D->A' 'X' 'B->C' 'B->C->D']
 ['C->D->A' 'C->D->A->B' 'X' 'C->D']
 ['D->A' 'D->A->B' 'D->A->B->C' 'X']]
```
