# Assignment 2 - Linear & Binary Search
1. It would take **O(N)** steps to perform a linear search for the ordered array, [2, 4, 6, 8, 10, 12, 13]. While it only takes 4 steps here, Big-O-wise, it's O(N).

2. It would take **O(log<sub>2</sub>N)** for a binary search using the previous (above) ordered array. While it only takes 2 steps here, Big-O-wise, it's O(log<sub>2</sub>N).

3. The max number of steps it would take to perform a binary search on an array of size 100,000 would be **17 steps**. Why? Because O(log<sub>2</sub>100,000) is 16.61. We round to the nearest decimal value.

4. ```c++
    #include <iostream>
    using namespace std;
    
    int linearSearch(int arr[], int size, int target, int &steps);
    int binarySearch(int arr[], int size, int target, int &steps);
    
    int main() {
        const int size = 100000;
        int arr[size];  // using an array instead of vector, because the question calls for an array...
    
        for (int i=0; i<size; i++)
            arr[i] = i;
    
        int target;
        cout<<"Enter the search value: ";
        cin>>target;
    
        int linearSteps = 0;
        int binarySteps = 0;
    
        linearSearch(arr, size, target, linearSteps);
        binarySearch(arr, size, target, binarySteps);
    
        cout<<"\nSteps for linear search: " << linearSteps << endl;
        cout<<"Steps for binary search: " << binarySteps << endl;
    
        return 0;
    }

    int linearSearch(int arr[], int size, int target, int &steps) {
        steps = 0;
        for (int i=0; i<size; i++) {
            if (arr[i] == target)
                return i;
            steps++;
        }
        return -1;
    }
    
    int binarySearch(int arr[], int size, int target, int &steps) {
        steps = 0;
        int left = 0;
        int right = size-1;
        while (left <= right) {
            int mid = (left + right) / 2;  // could go safer with left + (left - right) / 2 ?
            if (arr[mid] < target)
                left = mid + 1;
            if (arr[mid] > target)
                right = mid - 1;
            else
                return mid;
            steps++;
        }
        return -1;
    }

So what's going on here?
  * **Linear Search** : If you use the code above, you will get the same number of steps as there is N (or target, being your input). So for a linear search, it must go through each index of N elements to find your target/input. In the worst case, which is what Big O really cares about, it would be **O(N)**. Thus, we say that a linear search is of **O(N) complexity**.
  * **Binary Search** : This search is faster the bigger the N. This search lowers the steps it takes to get to it's target value by cutting the search in half each time. Let's say you have a value of 10 for SIZE. Now, based on the lowest index being 0, and the highest being 9, (0+9)/2 = middle. So you have 4.5, or 4 (floored). Now, based on whether your target number is greater (>) or less than (<) the middle number, then the highest and lowest number will change to the middle number plus (+) or minus (-) 1, which will set a new middle number. So basically, it will find a new middle ground to find your target value. That makes the complexity of the search less that O(N), with a search complexity of **O(log<sub>2</sub>N)** -- and that's at its worst. It could be **O(1)**, too, in the best-case scenario.

5. * Pseudo Code:
     ```c++
     RandSearchAlgo(vector V, key) // I got a key and vector. I also need to use randomized indices without repitition...
     /* REMEMBER: 100,000 = n */
       Need:
            V -> vector w/ n elements. Says distinct elements...
            key -> Whatever it is I'm looking for

       Random index. No reusable index.   // Pretty sure I could get away with a boolean as a flag. Like a vector<bool> checked(n, false) or something...
         - 1) I could use a boolean flag. But what if it repeats an index randomly?  // Search for options. Found musical chairs: Fisher-yates method...
         - 2) Fisher-yates method        // Using this instead
       steps = 0                         // step counter
       n = V.size                        // I need to end the loop somehow 

       Main:
         * Populate V (array) with distinct elements
         * Call result of RandSearchAlgo
         * Show:
           * Index # via RandSearchAlgo result
           * Number in V[index]
           * Number of steps taken
      
         So, for RandSearch Algo:
           for (int i=n-1; i>=0; i--):       // Fisher-yates method
             Randomly generate b/w 0 to i    // distribution
             new integer j -> generate random number via dist(0, n-1)
             SWAP:
               * new integer "temp" -> V[i]
               * V[i] -> V[j]
               * V[j] -> temp (AKA V[i])    // Sequence: Back index (V[i]) becomes random # (V[j]) and V[j] becomes V[i]. Basically, if generated, it goes to the back and is out.
             CHECK:
               * if V[i] == key             // if V[i], which is now the random #, is equal to the key...
                 return i;
             STEP COUNTER:
               * steps++
           Return (-1, steps)
    
    * Actual Code:
      ```c++
      #include <vector>
      #include <random>
      #include <iostream>
      using namespace std;
      
      int randSearchAlgo(vector<int>& Varray, int key, int& steps);
      
      int main() {
      
          const int SIZE = 100000;
          vector<int> Varray(SIZE);      // I know, bad naming sense. Couldn't help but name it that...
      
          for (int i = 0; i < SIZE; i++) // b/c "distinct"
          {
              Varray[i] = i;
          }
      
          int key;
          cout << "Enter your search value/key: ";
          cin >> key;
      
          int steps = 0;
          int result = randSearchAlgo(Varray, key, steps);   
      
          if (result != -1)
              cout << "Index at: " << result << endl;
          else
              cout << "Target not found." << endl;
          cout << "Number in Index: " << Varray[result] << endl;
          cout << "Comparisons: " << steps << endl;
          
          return 0;
      }
      
      
      int randSearchAlgo(vector<int>& Varray, int key, int& steps) {
      
          int n = Varray.size();
          random_device rd;
          default_random_engine gen(rd());
          steps = 0;
      
          for (int i=n-1; i>=0; i--) {
              uniform_int_distribution<int> dist(0, i);
              int j = dist(gen);
      
              int temp = Varray[i];
              Varray[i] = Varray[j];
              Varray[j] = temp;
              steps++;
              if (Varray[i] == key) {
                  return i;
              }
          }
          return -1;  // no dice
      }

    * Comparison & Analysis:
      <br />
      &emsp; The random search method is not as bad as I thought. Everything is randomized. But complexity-wise, it never goes past the number of elements it carries (i.e. size), Thus making it **O(N)** in general and **O(1)** at best. With the Fisher-yates method, if I visited a number in an array, it gets thrown in the back of the list, never to be seen again. Then the size goes down, the visited numbers are past the new size limit (and thus, inaccessible) and we move on.
      &emsp; Now, mind you, I originally thought of using a Boolean scheme to get this working, which turned out pretty bad. In that scenario, I was simply rolling the dice and checking if a random number (Array[i]) had been visited. This led to indexes being repeated. This could mean that, if I you had gotten really lucky (on a 1 in 100,000 chance) that you got the number of your choice on the first try, then it was **O(1)**. But, that's a big stretch. Hence, that method is not O(1), not O(N), but **O(NlogN)** for its repeats that go past the size(N).

      &emsp; When compared to the **linear search** and the **binary search**, the random search had the <u>same complexity as the linear</u> and more than the binary. Here are the key differences:
      * Linear Search:
         - **O(N)** generally, **O(1)** at best
         - Straightforward
      * Randomized Search:
         - **O(N)** generally, **O(1)** at best. Same as Linear Search
         - Required some setup before hand (i.e. randomization, etc.)
         - Could possibly lead to repeated numbers, which could cause a complexity of **O(NlogN)**
      * Binary Search:
         - **O(log<sub>2</sub>N)** at most,**O(1)** at best
         - Best for bigger data sets/sizes
         - Requires sorted data (another overhead like reandomized searching). Could add more complexity (i.e. **O(NlogN)**

      &emsp; And with all that, I conclude that it would be best to stick with a linear search than a randomized one. It's the same thing, without the flare. Now, I know there was reading material on **hash tables**, but after thinking about it, why use that if we're talking about randomization? It would be **O(1)**, unless there was a collision, as it states in the modules. In fact, it doesn't seem like there would be any randomization at all. (**NOTE:** Please let me know if I'm wrong about that)

# Video
[![Assignment 2 Video](https://img.youtube.com/vi/Ehig_6wULos/maxresdefault.jpg)](https://youtu.be/EhiG_6wULos)

**Note:** This is the first Youtube video I've ever done. I also might have some image quality issues because I'm using a wrapper to access Zoom in Linux.
