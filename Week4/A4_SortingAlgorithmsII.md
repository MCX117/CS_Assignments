# Assignment 4 - Sorting Algorithms II
# Tasks
**1.** To prove that, under the **average-case** scenario, insertion sort has a time complexity of O(N<sup>2</sup>), let's look at how a sorting algorithm works.
```C++ 
    int insertionSort(int arr[], int size) {
        for (int i=1; i<size; i++) {          // # OUTER LOOP: # of passes is N-1
            int key = arr[i];                 // Now, for tracking purposes and a deeper dive, I could talk about how these are accounted, but I'll elect to ignore them to simplify
            int j = i-1;                      // Thus, these operations (i.e. j=i-1; and below like j--;) will be omitted.
            while (j >= 0 && arr[j] > key){   // INNER LOOP: # of comparisons (not accounting for j>=0). Dependent on "i". (N(N-1))/2 in the worst case scenario.
               arr[j+1] = arr[j];             // Shift elements to the right to make space for the key. (N(N-1)) / 2
            }
            arr[j+1] = key;                   // Key insertion. Not accounted for. Is N-1 in the worst case
    } // Thus, 2 * (N(N-1))/2 = N(N-1) <- worst case. N(N-1) / 2 for shifts, N(N-1) / 2 for comparisons.
```
* If we have **"i"** starting at **1** for the outer loop that iterates until it reaches **N**, that's **N-1**.
* In the inner loop, we have **"j"** equal to **i-1**. But if the **key** is equal to **arr[i]**, and i increments to the right (+1) which is then compared to the elements to its left, then the iterations per pass is **i**.
For every index of N starting at 1, we will iterate i times in the worst case (the same number as the current index). Then, repeat until i in the outer loop reaches N-1.
For every loop, that's **Loop = (N-1)+(N-2)+(N-3)...**. There's a pattern here. We are adding in ascending order, which equates to the **triangle formula: N(N-1)/2**. In the **WORST CASE SCCENARIO** there would be just as many swaps as there are comparisons. That would be **O(2 * (N(N-1))/2) = O(N(N-1))** (if you imagine 2 triangles).

&emsp; But what if it was on **average**? Well, let's assume that it's compares and swaps half the time. Then it would be **O(2 * (N(N-1))/4) = O((N(N-1))/2)**. Again, imagine 2 triangles (swap and compare). Here's a visual:
   
<img width="814" height="951" alt="InsertionSort drawio" src="https://github.com/user-attachments/assets/c8bea9dc-dc15-442f-91d5-d4e350deaf0b" />

**2.** In an insertion sort where N=5, the number of steps in the worse case scenario are as follows:
```c++
#include <iostream>
using namespace std;

int insertionSort(int arr2[], int size, int& steps);

int main() {               // I've removed bubblesort function...

    const int size = 5;    // N = 5
    int arr2[size];

    for (int i=0; i<size; i++) {
        arr2[i] = size - i; // worst case for both bubble and insertion sort
    }                       // Will fill up array in descending order

    int steps = 0;
    int insertionSteps = insertionSort(arr2, size, steps);
  
    cout << "Steps for insertion sort: " << insertionSteps << endl;

    return 0;
}

int insertionSort(int arr2[], int size, int& steps) {
    steps = 0;
    for (int i=1; i<size; i++) {
        int key = arr2[i];
        int j = i-1;
        while (j >= 0){ 
            if (arr2[j] > key) {         // 1 step per comparison
                steps++;
                arr2[j+1] = arr2[j];     // Shift elements to the right. 1 step per shift
                j--;
                steps++;
            }
        }
        arr2[j+1] = key;                 // Insertion. Not counted, but if it was, it's N-1.
    }
    return steps;
}
```
<img width="2880" height="1759" alt="Screenshot From 2026-02-27 23-29-03" src="https://github.com/user-attachments/assets/5c8be592-94c9-4742-8585-3deb61a6d026" />

&emsp; **a)** At **i = 1**, the total step count is at **20**. See above. <br />
&emsp; **b)** At **i=2**, the <mark>total step count is **18**</mark>. At **i=3**, the <mark>total is **14**</mark>.
&emsp; **c)** For **b**, at **i=2** and **i=3**, the sorting algorithm <mark>does not sort the entire array</mark>. <br>
&emsp; &emsp; We had an array of size 5 in descending order: A=[5,4,3,2,1]. if we start at i=2, A[i] = 3. We start from the middle of the array, and no longer account for A[0] and A[1]. What would happen is the **"key"** being compared to all the other elements to its left starts wherever A[i] is. If it skips indexes, then those number never get compared or swapped. So <mark>at i=2, A gets rearranged into A=[1,2,3,5,4]. At i=3, the array gets sorted as A=[1,2,5,4,3].</mark>

**3)** 


The following function returns whether or not a capital “X” is present within a string.
function containsX(string) {
	foundX = false;
	for(let i = 0; i < string.length; i++) { 
		if (string[i] === "X") {
			foundX = true; 
		}
	}
	return foundX; 
}

(a) What is this function’s time complexity regarding Big O Notation?

(b) Then, modify the code to improve the algorithm’s efficiency for best- and average-case scenarios.
