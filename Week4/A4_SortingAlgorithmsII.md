# Assignment 4 - Sorting Algorithms II
# Tasks
**1.** To prove that, under the **average-case** scenario, insertion sort has a time complexity of O(N<sup>2</sup>), let's look at how a sorting algorithm works.
   ```C++ 
    int insertionSort(int arr[], int size) {
        for (int i=1; i<size; i++) {          // # OUTER LOOP: # of passes is N-1
            int key = arr[i];                 // Now, for tracking purposes and a deeper dive, I could talk about how these are accounted, but I'll elect to ignore them to simplify
            int j = i-1;                      // Thus, these operations (i.e. j=i-1; and below like j--;) will be omitted.
            while (j >= 0 && arr[j] > key){   // INNER LOOP: # of comparisons (not accounting for j>=0). Dependent on "i".
               arr[j+1] = arr[j];             // Shift elements to the right to make space for the key. Not accounted for.
            }
            arr[j+1] = key;                   // Key insertion. Not accounted for.
    }
    ```
* If we have **"i"** starting at **1** for the outer loop that iterates until it reaches **N**, that's **N-1**.
* In the inner loop, we have **"j"** equal to **i-1**. But if the **key** is equal to **arr[i]**, and i increments to the right (+1) which is then compared to the elements to its left, then the iterations per pass is **i**.
For every index of N starting at 1, we will iterate i times in the worst case (the same number as the current index). Then, repeat until i in the outer loop reaches N-1.
For every loop, that's **Loop = (N-1)+(N-2)+(N-3)...**. There's a pattern here. We are adding in ascending order, which equates to the **triangle formula: N(N-1)/2**. In the **WORST CASE SCCENARIO** there would be just as many swaps as there are comparisons. That would be **O(2 * (N(N-1))/2) = O(N(N-1))** (if you imagine 2 triangles). But what if it was on **average**? Well, let's assume that it's compares and swaps half the time. Then it would be **O(2 * (N(N-1))/4) = O((N(N-1))/2)**  Here's a visual:
   
<img width="814" height="951" alt="InsertionSort drawio" src="https://github.com/user-attachments/assets/c8bea9dc-dc15-442f-91d5-d4e350deaf0b" />

**2.** So if I 




Proof that, under the average-case scenario, the insertion sort has a time complexity of O ( N 2 ) . Draw a clear figure and show all the operations clearly. 2 pts

At the start of the insertion sort, the index of the inspected value is set to 1. Change the index of the inspected value and verify that the total number of operations equals 20. Consider the worst-case scenario. Use N=5, where N is the number of elements. 4 pts

The following function returns whether or not a capital “X” is present within a string.
