# Sort a 2D array

<b>
Question: Given a 2D array where all the rows are sorted, write a program to sort the array. Can you do this in-place?   
    
Example:    
Input:  
[[ 5, 12, 17, 21, 23],  
 [ 1,  2,  4,  6,  8],  
 [12, 14, 18, 19, 27],  
 [ 3,  7,  9, 15, 25]]  

Output:  
[[ 1,  2,  3,  4,  5],  
 [ 6,  7,  8,  9, 12],  
 [12, 14, 15, 17, 18],  
 [19, 21, 23, 25, 27]]  
    
</b>


Thought Process:
* Seems to be similar to merge K sorted Lists. However, in this case, they are arrays. We could do something like that, but we will have to maintain indices so we know what element to process next.
* A less optimal solution (from O(nlogk) to O(nlogn)), but nonetheless interesting alternative would be a generalization of quicksort for the 2D case.

```python
def sort2DArray(A):
    m, n = len(A), len(A[0])
    def helper(l, r):
        if l == r: return
        pivot = A[l // n][l % n]
        i, j = l + 1, r
        while i <= j:
            if A[i // n][i % n] < pivot:
                i += 1
            else:
                A[i // n][i % n], A[j // n][j % n] = A[j // n][j % n], A[i // n][i % n]
                j -= 1
        A[l // n][l % n], A[(i - 1) // n][(i - 1) % n] = A[(i - 1) // n][(i - 1) % n], A[l // n][l % n]
        helper(l, i - 1)
        helper(i, r)
    helper(0, m * n - 1)
```
