# 2022-04-18
## shallow copy & deep copy
- list
    - list.copy()
    ```python
    a = [1, 2, 3, 4]
    b = a.copy()
    b[0]=10

    # unchanged
    print(a, b) # [1, 2, 3, 4] [10, 2, 3, 4]
    ```

- nested list
    - list.copy()
    ```python
    a = [[1, 2], [3, 4]]
    b = a.copy()
    b[0] = [10, 11]

    # unchanged
    print(a, b) # [[1, 2], [3, 4]] [[10, 11], [3, 4]]
    ```

    - list.copy()
    ```python
    a = [[1, 2], [3, 4]]
    b = a.copy()
    b[0][0] = 10

    # changed!
    print(a, b) # [[10, 2], [3, 4]] [[10, 2], [3, 4]]
    ```

    - copy.deepcopy()
    ```python
    import copy
    a = [[1, 2], [3, 4]]
    b = copy.deepcopy(a)
    b[0] = [10, 11]

    # unchanged
    print(a, b) # [[1, 2], [3, 4]] [[10, 11], [3, 4]]
    ```

    - copy.deepcopy()
    ```python
    import copy
    a = [[1, 2], [3, 4]]
    b = copy.deepcopy(a)
    b[0][0] = 10

    # unchanged
    print(a, b) # [[1, 2], [3, 4]] [[10, 2], [3, 4]]
    ```

to copy a list recursively, use ```copy.deepcopy(...)``` method. (But you need to think about overhead.)

# 2022-04-19
## break nested loop
```python
for yi in range(9):
    for xi in range(9):
        if board[yi][xi] == '.':
            break
    else:  # only execute when it's no break in the inner loop
        continue
    break
```

# 2022-04-20
## enumerate
iterable의 index와 값을 둘다 얻고 싶을 때 나는 다음처럼 사용했다.
```python
min_idx=-1
min_value=math.inf

for i in range(len(my_list)):
    if my_list[i] < min_value:
        min_idx=i
        min_value=my_list[i]
```
code가 짧을 땐 상관없지만, nested loop라도 사용하게 되면 점점 직관적으로 이해하기 힘들어진다. 이럴 때 enumerate를 사용하면 간편하다.
```python
min_idx=-1
min_value=math.inf

for i, val in enumerate(my_list):
    if val < min_value:
        min_idx=i
        min_value=val
```

***

## reduce
```python
from functools import reduce
sets = [set1, set2, set3]
intersection = reduce(set.intersection, sets)
```

## DFS
조건
- DFS의 종료 조건을 계산하는 cost가 크다.
- DFS가 최대 몇 번 일어나는지 안다. (tree의 높이를 안다.)

current depth parameter를 사용해 종료 조건을 판별할 수 있다.  
Ex) LeetCode [37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

```python
dfs(depth, q)
    if depth == len(q):
        return True

    for e in q:
        ...
        if dfs(depth+1, q):
            return True
```

# 2022-04-21
## iterates two iterables at once
```python
for i,j in zip(list1,list2):
    ...
```

## set remove vs. discard
my_set.remove(key) raises an error when there is no such key.  
my_set.discard(key) doesn't raise an error.

# 2022-04-25
## dictionary
dictionary는 insertion order를 보장한다. (from Python3.7)

*Performing list(d) on a dictionary returns a list of all the keys used in the dictionary, in **insertion order** (if you want it sorted, just use sorted(d) instead).* 
[>>see python document](https://docs.python.org/3.8/tutorial/datastructures.html?highlight=dictionary#dictionaries)

[>>also see stackoverflow](https://stackoverflow.com/questions/39980323/are-dictionaries-ordered-in-python-3-6)
