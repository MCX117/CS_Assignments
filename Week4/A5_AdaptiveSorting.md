# Assignment 5 - Adaptive Sorting
## Task
A)
```C++
#include <iostream>
using namespace std;

void selectionSort(int a[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (a[j] < a[minIdx]) minIdx = j;
        }
        int tmp = a[i];
        a[i] = a[minIdx];
        a[minIdx] = tmp;
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

// Count inversions (analysis only; does NOT sort)
long long countInversions(const int a[], int n) {
    long long inv = 0;
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if (a[i] > a[j]) inv++;
        }
    }
    return inv;
}

int main() {
    const int N = 50;
    int a[N];

    cout << "Enter 50 integers:\n";
    for (int i = 0; i < N; i++) cin >> a[i];

    // Don't actually sort...
    long long inv = countInversions(a, N);
    long long maxInv = 1LL * N * (N - 1) / 2;   // N(N-1)/2
    double ratio = (maxInv == 0) ? 0.0 : (double)inv / (double)maxInv;

    string scenario;
    if (inv == 0) scenario = "Best Case";
    else if (inv == maxInv) scenario = "Worst Case";
    else if (ratio >= 0.90) scenario = "Worst Case (threshold)";
    else scenario = "Average Case";

    cout << "\nAnalysis:\n";
    cout << "Inversions: " << inv << " out of " << maxInv << "\n";
    cout << "Disorder ratio: " << ratio << "\n";
    cout << "Scenario: " << scenario << "\n";

    // threshy
    // Assumption: insertion is best when nearly sorted (<= 10% inversions)
    bool chooseInsertion = (ratio <= 0.10);

    int work[N];
    for (int i = 0; i < N; i++) work[i] = a[i];

    cout << "\nChosen algorithm: " << (chooseInsertion ? "Insertion Sort" : "Selection Sort") << "\n";

    if (chooseInsertion) insertionSort(work, N);
    else selectionSort(work, N);

    cout << "\nSorted output:\n";
    for (int i = 0; i < N; i++) cout << work[i] << (i + 1 == N ? '\n' : ' ');

    return 0;
}
```

B)
```C++
#include <iostream>
using namespace std;

bool isStrictlyDecreasing(const int a[], int n) {
    for (int i = 0; i < n - 1; i++) {
        if (!(a[i] > a[i + 1])) return false;
    }
    return true;
}

int main() {
    const int N = 50;
    int a[N];

    cout << "Enter 50 integers:\n";
    for (int i = 0; i < N; i++) cin >> a[i];

    if (isStrictlyDecreasing(a, N))
        cout << "Worst Case\n";
    else
        cout << "Average Case\n";

    return 0;
}
```

C)
