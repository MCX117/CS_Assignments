# Assignment 5 - Adaptive Sorting
## Task
A) **Adaptive Sorting Selection** <br />
&emsp; I kind of overdid this one. This is basically parts A and B combined.
```C++
#include <iostream>
using namespace std;

void selectionSort(int a[], int size);
void insertionSort(int a[], int size);
int countInversions(int a[], int size);

int main() {
    int N = 50;
    int a[N]; // simplifying array to "a"

    
    /* for (int i = 0; i < N; i++) { // Worst case for both
        a[i] = N - i;  
    } */

    /*for (int i = 0; i < N; i++) {   // really basic random generator. For testing...
        a[i] = rand() % 101;  // 0 to 100
    } */

    
    for (int i = 0; i < N; i++) { // Best case for both
        a[i] = i;  
    }

    cout << "Original array:\n";
    for (int i = 0; i < N; i++) {   // Print original array
        cout << a[i] << " ";
    }

    // Phase 1: Analyze but don't actually sort...
    int inv = countInversions(a, N);    // See countInversion function below...
    int maxInv = N * (N - 1) / 2;       // N(N-1)/2
    double ratio = (maxInv == 0) ? 0.0 : (double)inv / (double)maxInv; // To avoid division by zero when N=0 or N=1

    string scenario; // scenario check
    if (inv == 0) { 
        scenario = "Best Case";
        cout << "\n\nBest Case" << endl;
    }else if (inv == maxInv) {
        scenario = "Worst Case";
        cout << "\n\nWorst Case" << endl;
    }else if (ratio >= 0.95) {
        scenario = "Worst Case (threshold)";
    }else {
        scenario = "Average Case";
        cout << "\n\nAverage Case" << endl;
    }
    
    bool chooseInsertion = (ratio <= 0.94); // Flag! Assumption: insertion is best when nearly sorted (<= 10% inversions)

    cout << "\nChosen algorithm: " << (chooseInsertion ? "Insertion Sort" : "Selection Sort") << "\n";

    if (chooseInsertion) {
        insertionSort(a, N);
    } else {
        selectionSort(a, N);
    }

    cout << "\nSorted output:\n";
    for (int i = 0; i < N; i++) cout << a[i] << (i + 1 == N ? '\n' : ' ');

    cout << "\nInversion: " << inv << endl;

    return 0;
}

void selectionSort(int a[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int smallest = i;
        for (int j = i + 1; j < n; j++) {
            if (a[j] < a[smallest]) {
                smallest = j;
            }
        }
        int tmp = a[i];
        a[i] = a[smallest];
        a[smallest] = tmp;
    }
}

void insertionSort(int a[], int n) {
    for (int i = 1; i < n; i++) {
        int key = a[i];
        int j = i - 1;
        while (j >= 0 && a[j] > key) {
            a[j + 1] = a[j];  // shift
            j--;
        }
        a[j + 1] = key;
    }
}

// Count inversions (doesn't sort)
int countInversions(int a[], int n) {
    int inv = 0;
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if (a[i] > a[j]) { 
                inv++;
            }
        }
    }
    return inv;
}
```
<img width="2259" height="1473" alt="Screenshot From 2026-03-02 01-51-58" src="https://github.com/user-attachments/assets/d5bae35c-d9d8-4edc-a809-d0a94f5d8655" />


B) **Case Classification Without Sorting** <br />
&emsp; Practically the same as part A. Removed insertionSort and selectionSort functions.
```C++
#include <iostream>
using namespace std;

int countInversions(int a[], int size);

int main() {
    int N = 50;
    int a[N]; // simplifying array to "a"

    
    /* for (int i = 0; i < N; i++) { // Worst case for both
        a[i] = N - i;  
    } */

    /*for (int i = 0; i < N; i++) {   // really basic random generator. For testing...
        a[i] = rand() % 101;  // 0 to 100
    } */

    
    for (int i = 0; i < N; i++) { // Best case for both
        a[i] = i;  
    }

    cout << "Original array:\n";
    for (int i = 0; i < N; i++) {   // Print original array
        cout << a[i] << " ";
    }

    // Phase 1: Analyze but don't actually sort...
    int inv = countInversions(a, N);    // See countInversion function below...
    int maxInv = N * (N - 1) / 2;       // N(N-1)/2
    double ratio = (maxInv == 0) ? 0.0 : (double)inv / (double)maxInv; // To avoid division by zero when N=0 or N=1

    string scenario; // scenario check
    if (inv == 0) { 
        scenario = "Best Case";
        cout << "\n\nBest Case" << endl;
    }else if (inv == maxInv) {
        scenario = "Worst Case";
        cout << "\n\nWorst Case" << endl;
    }else if (ratio >= 0.95) {
        scenario = "Worst Case (threshold)";
    }else {
        scenario = "Average Case";
        cout << "\n\nAverage Case" << endl;
    }
    
    bool chooseInsertion = (ratio <= 0.94); // Flag! Assumption: insertion is best when nearly sorted (<= 10% inversions)

    cout << "\nChosen algorithm: " << (chooseInsertion ? "Insertion Sort" : "Selection Sort") << "\n";

    cout << "\nInversion: " << inv << endl;

    return 0;
}

// Count inversions (doesn't sort)
int countInversions(int a[], int n) {
    int inv = 0;
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if (a[i] > a[j]) { 
                inv++;
            }
        }
    }
    return inv;
}
```

C) **Documentation**
* My **threshold definition** for this assignment was their <mark>**ratio of inversions**</mark>. Let's say between an (x,y), inversions check to see if x > y since x, being the before y, should be smaller (as it assumes ascending order). Since I changed my x and ys every time by incrementing my inner and outer loops, I get an inversion count that I just divide from a maximum inversion of **N(N-1)/2**. For **best case**, well, inversion was at **0**. For **worst case**, I set the threshold at **95% of 1225** (1225 from 50(49)/2). I did that because 50 elements is still considered small, and insertion sort has the advantage for smaller to medium-sized arrays. In reality, insertion sort would still beat selection sort probably even at 98% max inversions for a size 50 array. For the **average case**, I have it set for anything **above 0 and up t0 95% of max inversion (1225)**.
* My algorithm selects one sorting algorithm over another based on the **ratio of inversions** as noted above.
* Input order affects the time complexity of both sorts because of how they are built to analyze and sort the data of an array. There's a big difference, though. Selection sort doesn't change much with input order as it always has consistent comparisons based on the size of the array (N(N-1)/2), whereas insertion sort is highly sensitive to input order.
* * **Selection sort** is built to iterate over all of its elements (via the inner loop) based on the current position or index of its outer loop. This is regardless if it's already sorted or not. Thus, <mark>it will always compare N(N-1)/2 times</mark>. The only thing that changes is how many swaps it does, which is 1 or 0 per pass (N-1). Now, <mark>N(N-1)/2 is N<sup>2</sup>-N/2. This translates to N<sup>2</sup> in Big O, which is the time complexity of selection sort as it denotes the overall curve or trajectory of the algorithm overall (which means it's parabolic)</mark>.
  * **Insertion sort** also iterates through its elements, but it uses the outer loop to iterate over its elements while the inner loop checks to see if there is an inversion. If there is, it will shift the elements to the right and keep doing so until the current element (the key) being checked against elements to its left, is in its proper spot. If it finds that there are no inversions for that pass/iteration, or if the key finds its spot early, then the algorithm moves on. In that sense, <mark>**at best** it could simply just iterate once</mark>, meaning O(N-1) steps, <mark>which is **O(N)** in time complexity. **At worst**, it's O(N(N-1)/2) steps with **O(N<sup>2</sup>)** time complexity. and **on average**, it's **O(N<sup>2</sup>)** time complexity.</mark>
