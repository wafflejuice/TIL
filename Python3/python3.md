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
