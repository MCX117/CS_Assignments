# Assignment 1 - Array Data Structures
1. To create an array of 100 elements, you can do something as simple as declaring an array of 100 int.
   ```c++
   int arr[100];
2. The size of each element in an array can be shown using:
   ```c++
   sizeof(arr[0]) // this is just for 1 element
   sizeof(arr) // the full thing
3. For an array containing 100 elements, provide the number of steps the following operations would take:
    * Reading - **1 STEP**
    * Searching for a value not contained within the array - **Will be the worst case scenario, which is the size of the array (i.e. 100)**
     * Insertion at the beginning of the array - **100 + 1 = 101 STEPS**
     * Insertion at the end of the array - **1 STEP**
     * Deletion at the beginning of the array - **100-1 = 99 STEPS**
     * Deletion at the end of the array - **1 STEP**\
4. To find every instance of the word "apple" in an array, the steps it would take would be denoted as **O(N)**. Basically, it's **N** steps.
5. To find the memory address of an array in C++ (to make life simpler):
   ```c++
   arr // this points (pointer) to the first element, which is the address to the start of the array
   &arr[1] // "&" will give you the address of (something) the element number you choose (i.e. it doesn't have to be 1).

## Video
https://github.com/user-attachments/assets/bc8c5ded-0a31-49be-8915-b5060646f9a7

