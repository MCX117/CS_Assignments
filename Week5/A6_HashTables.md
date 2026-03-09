# Assignment 6 - Implementing a Hash Table with Separate Chaining
## Tasks:
  * Implement a hash table from scratch in C++
  * Understand collision handling using separate chaining
  * Compute and analyze load factor
  * Implement dynamic resizing (rehashing)
  * Evaluate performance experimentally
### Part 1 - Class Structure
I need to use the following internal structure:
```C++
#include <vector>
#include <list>
#include <string>
using namespace std;

class HashTable {
private:
    vector<list<pair<string, int>>> table;
    int currentSize;
    int capacity;
    int collisionCount;

    int hashFunction(const string& key) const;
    void rehash();

public:
    HashTable(int size = 11);

    void insert(const string& key, int value);
    bool remove(const string& key);
    int search(const string& key) const;
    double loadFactor() const;
    int size() const;
    bool isEmpty() const;
    void printTable() const;
};
```
I've made some comments regarding the following to help me formulate my thoughts.
```C++
#ifndef HASHTABLE_H
#define HASHTABLE_H

#include <vector>
#include <list>
#include <string>
using namespace std;

class HashTable {
private:                                        // Modifiable by member functions of the class only
    vector<list<pair<string, int>>> table;      // The actual hash table. vector = table, list = buckets, pair = key(string) & value(int)
    int currentSize;
    int capacity;                               // Capacity = # of buckets
    int collisionCount;                         // So I need to add this every function that gets an index/bucket with a key/value in it already

    int hashFunction(const string& key) const;  // So i get the key here and turn it into a hash#? What am I doing here? Index?
    void rehash();                              // If I'm rehashing, I guess that means I'm re-tabling? or does that mean rehash#? But that doesn't make sense. So it means the table then...

public:
    HashTable(int size = 11);                   // So this is my constructor. Why only size? size = #ofLists= #ofBuckets?

    void insert(const string& key, int value);  // Insert FUNCTION. For adding or updating the key/value pairs.
    bool remove(const string& key);             // Remove FUNCTION. Kick key/value pairs.
    int search(const string& key) const;        // Search FUNCTION.
    double loadFactor() const;                  // FUNCTION? What variables am I using for this? Currentsize & capacity, no?
    int size() const;                           // FUNCTION? Get size? 
    bool isEmpty() const;                       // FUNCTION? So get isEmpty()? True if table's empty...
    void printTable() const;                    // FUNCTION. Easy.
};

#endif
```
From there, I started building on things in the public part of the class.
```C++
#include <vector>
#include <list>
#include <string>
#include <iostream>
#include "HashTableChain.h"
using namespace std;

class HashTable {
private:
    vector<list<pair<string, int>>> table;
    int currentSize;
    int capacity;
    int collisionCount;

    int hashFunction(const string& key) const {
      // turn key into a hash then get the index
      // I had already inputted it into my code using the requirement in PART 2, but I didn't include it here... because I show it in part 2 below...
    }
    void rehash();                               // Do something about this...

public:                                          // Things aren't in the same order. I found it more understandble/trackable to sort it the way I did below.
    HashTable(int size = 11);

    double loadFactor() const{
      return static_cast<double>(currentSize) / capacity; }

    int size() const{ return currentSize; }

    bool isEmpty() const { return currentSize == 0; }

    int search(const string& key) const {
        int index = hashFunction(key) % capacity;   // Compute the index for the key using the hash function and modulo operator
        const auto& bucket = table[index];          // Get a reference to the bucket at that index. Just to say bucket is table[index]
        for (const auto& pair : bucket) {           // Iterate through the bucket to find the key. Remember: .first = key, .second = value. const for read-only access
            if (pair.first == key) {                // If key is found, return paired value
                return pair.second;
            }
        }
        return -1;                                  // Key wasn't found. Fail...
    };

    void insert(const string& key, int value){
        // I should probably do something about overflow...
        int index = hashFunction(key) % capacity;   // index is as big as buckets = capacity
        for (auto& pair : table[index]) {           // Iterate through the bucket at the computed index
            if (pair.first == key) {                // If the key already exists in the bucket, update its value. Remember: .first = key, .second = value. (*it).first
                pair.second = value;                // Update the value for the existing key. Basically, overwrite the old, in with the new.
                return;
            }
        }
        table[index].emplace_back(key, value);      // Add the new pair to the bucket using emplace_back, avoids all that unnecessary copying by making it right at that spot!
    }

    bool remove(const string& key) {                // Remember: Find the key first, then remove it. Can't just erase the whole bucket.
        int index = hashFunction(key) % capacity;
        auto& bucket = table[index];                // Get a reference to the bucket at that index
        for (auto it = bucket.begin(); it != bucket.end(); ++it) {  // Iterate through the bucket to find the key
            if (it->first == key) {                                 // If the key is found. Again, remember: .first = key, .second = value. (*it).first
                bucket.erase(it);                                   // Remove the key/value pair from chum bucket
                currentSize--;                                      // Decrease the current size of the hash table
                return true;                                        // Return true b/c successful removed
            }
        }
        return false; // Return false if the key was not found in the bucket

    }

    int collisions() const{ return collisionCount; }

    void printTable() const {
        for (size_t i = 0; i < table.size(); ++i) {    // Iterate through each bucket in the hash table
            cout << "Bucket " << i << ": ";            // Print the bucket index
            for (const auto& pair : table[i]) {
                cout << "[" << pair.first << " : " << pair.second << "] ";
            }
            cout << endl;                              // New line after each bucket for better readability
        }
    }
};
```

### Part 2 - Hash Function
I need to implement a polynomial rolling hash using:
```C++
int HashTable::hashFunction(const string& key) const {
    const int prime = 31;
    long long hash = 0;

    for (char c : key) {
        hash = hash * prime + c;
    }

    return hash % capacity;
}
```
So, doing just that, here's the same code in **part 1** but with the polynomial rolling hash code:
```C++
#include <vector>
#include <list>
#include <string>
#include <iostream>
#include "HashTableChain.h"
using namespace std;

class HashTable {
private:
    vector<list<pair<string, int>>> table;
    int currentSize;
    int capacity;
    int collisionCount;

    int hashFunction(const string& key) const {  // added the code in...
      const int prime = 31;
      long long hash = 0;
  
      for (char c : key) {
          hash = hash * prime + c;              // So the book/content says that it's: [(c1 * p^n-1) + ... (cn * p^0)]
      }
      return hash % capacity;
      }

    void rehash();           // This part comes up in PART 4                             

public:
    HashTable(int size = 11);
                                        
    double loadFactor() const{
      return static_cast<double>(currentSize) / capacity; }

    int size() const{ return currentSize; }

    bool isEmpty() const { return currentSize == 0; }

    int search(const string& key) const {
        int index = hashFunction(key) % capacity;   
        const auto& bucket = table[index];          
        for (const auto& pair : bucket) {           
                return pair.second;
            }
        }
        return -1;                                 
    };

    void insert(const string& key, int value){
        // I should probably do something about overflow...   
        int index = hashFunction(key) % capacity;   
        for (auto& pair : table[index]) {           
            if (pair.first == key) {                
                pair.second = value;                
                return;
            }
        }
        // If the key doesn't exist in the bucket...
        if (!table[index].empty()) {                
            collisionCount++;                       
        }
        table[index].emplace_back(key, value);      

    bool remove(const string& key) {                
        int index = hashFunction(key) % capacity;
        auto& bucket = table[index];               
        for (auto it = bucket.begin(); it != bucket.end(); ++it) {  
            if (it->first == key) {                                
                bucket.erase(it);                                   
                currentSize--;                                      
                return true;                                        
            }
        }
        return false; // Return false if the key was not found in the bucket

    }

    int collisions() const{ return collisionCount; }

    void printTable() const {
        for (size_t i = 0; i < table.size(); ++i) {   
            cout << "Bucket " << i << ": ";           
            for (const auto& pair : table[i]) {
                cout << "[" << pair.first << " : " << pair.second << "] ";
            }
            cout << endl;                              
        }
    }
};
```
### Part 3 - Insert & Collision Handling
I need to:
-   Use separate chaining (`vector<list<>>`)
-   If inserting into a non-empty bucket, increment `collisionCount`
-   If key already exists, update value instead of duplicating
**NOTE**: Please see comments below.
```C++
#include <vector>
#include <list>
#include <string>
#include <iostream>
#include "HashTableChain.h"
using namespace std;

class HashTable {
private:
    vector<list<pair<string, int>>> table;      // separate chaining started here...
    int currentSize;
    int capacity;
    int collisionCount;

    int hashFunction(const string& key) const {
      const int prime = 31;
      long long hash = 0;
  
      for (char c : key) {
          hash = hash * prime + c;              // So the book/content says that it's: [(c1 * p^n-1) + ... (cn * p^0)]
      }
      return hash;
      }

    void rehash();                              // This part comes up in PART 4                             

public:
    HashTable(int size = 11);
                                         
    double loadFactor() const{
      return static_cast<double>(currentSize) / capacity; }

    int size() const{ return currentSize; }

    bool isEmpty() const { return currentSize == 0; }

    int search(const string& key) const {
        int index = (hashFunction(key) % capacity + capacity) % capacity;    // Prevents a negative value, which would mess everything up... big time.
        const auto& bucket = table[index];          
        for (const auto& pair : bucket) {        // Seperate chaining here. It's going through every pair in a bucket.
                return pair.second;
            }
        }
        return -1;                                 
    };

    void insert(const string& key, int value){   // I'm supposed to focus on insertion more, right?
        // I should probably do something about overflow...   
        int index = (hashFunction(key) % capacity + capacity) % capacity;  
        for (auto& pair : table[index]) {        // Separate chaining...
            if (pair.first == key) {                
                pair.second = value;             // If the key already exists, it will update the value (pair.second). That's because: pair<1st, 2nd>   
                return;
            }
        }
        if (!table[index].empty()) {              // Added collision count...
            collisionCount++;                       
        }
        table[index].emplace_back(key, value);    // Again, avoids all that unnecessary copying by making it right at that spot!
        currentSize++;                            // You added a key. Now the size
    }

    bool remove(const string& key) {                
        int index = (hashFunction(key) % capacity + capacity) % capacity;
        auto& bucket = table[index];               
        for (auto it = bucket.begin(); it != bucket.end(); ++it) {  // And separate chaining, again...
            if (it->first == key) {                                
                bucket.erase(it);                                   
                currentSize--;                                      
                return true;                                        
            }
        }
        return false; // Return false if the key was not found in the bucket

    }

    int collisions() const{ return collisionCount; }

    void printTable() const {
        for (size_t i = 0; i < table.size(); ++i) {   
            cout << "Bucket " << i << ": ";           
            for (const auto& pair : table[i]) {                     // Basically separate chaining...
                cout << "[" << pair.first << " : " << pair.second << "] ";
            }
            cout << endl;                              
        }
    }
};
```
### Part 4 - Resizing(Rehashing)

When:

    loadFactor() > 0.75

You must:

1.  Double the table capacity
2.  Reinsert all existing elements
3.  Reset collision counter appropriately
So here's what I did (I was alluding to this in the comments to my code previously)
**NOTE**: I removed some of the code to make it easier to see what I changed. 
```C++
#include <vector>
#include <list>
#include <string>
#include <iostream>
#include "HashTableChain.h"
using namespace std;

class HashTable {
private:
  // I removed all the other code to show the only thing I changed here:

  void rehash() {                     // Make table bigger when it gets too full...
    int newCapacity = capacity * 2;   // Create a new table with double the capacity
    vector<list<pair<string, int>>> newTable(newCapacity); // Initialize new table with empty buckets

    currentSize = 0;                  // This took me forever to do. My BUG!!!!! I had an infinite loop because loadSize was blowing up!

    // Rehash all existing key-value pairs into the new table
    for (const auto& bucket : table) {    // Iterate through each bucket. & for non copying, const for read-only access
      for (const auto& pair : bucket) {   // Iterate through each key-value pair in the bucket. REMEMBER!: reference to avoid copying, const for read-only access
        const string& key = pair.first;
        int value = pair.second;                        // Compute the new index for the key in the new table
        long long h = hashFunction(key);
        int newIndex = (h % newCapacity + newCapacity) % newCapacity; // Insert the key/value pair into the new table. I added some safeguards becuase of a bug.
        newTable[newIndex].emplace_back(key, value);
        currentSize++;                // I only had this and never reset currentSize... BUG!!!
      }
    }
    table = move(newTable);             // Replace the old table with the new...
    capacity = newCapacity;
    // There a need to reset collision count and current size? capacity = size of table, current size = number of key/value pairs, collision count = 0 b/c we just rehashed everything into a new table. So no collisions yet.
    collisionCount = 0;
    };
                        
  }

public:                                          
    // I removed all the other code to show the only thing I changed here:

    HashTable(int size = 11)    // Constructor w/ 11 buckets
        : table(size), currentSize(0), capacity(size), collisionCount(0) {};    // I wanted to relate capacity to size, but then realized I should just initialize everything...

    void insert(const string& key, int value){
        if (loadFactor() > 0.75) {       // Here's the change... Added that loadFactor in since we could go over capacity inserting something.
            rehash();                    // Calls the rehash function above (in private)                  
        } 
        int index = (hashFunction(key) % capacity + capacity) % capacity;  
        for (auto& pair : table[index]) {
            if (pair.first == key) {                
                pair.second = value; 
                return;
            }
        }
        if (!table[index].empty()) {
            collisionCount++;                       
        }
        table[index].emplace_back(key, value);
};
```
### Part 5 - Testing Program
In `main()`:
1.  Insert at least 100 words
2.  Print:
    -   Table capacity
    -   Number of elements
    -   Load factor
    -   Total collisions
3.  Search for:
    -   Existing key
    -   Non-existing key
4.  Remove some keys and verify correctness

**NOTE**: I decided to forego using the **".h"** file and decided to put `my main(){}` inside a single **".cpp"** file with my class.
Wait a second! How am I supposed to get **table capacity** without a getter? Was I supposed to put that in? It's in private and there wasn't any instruction to make one. I'll make one anyway...
```C++
// added under public
int getCapacity() const { return capacity; }
```
I'm not really sure by what the first instruction means when it says at least 100 words. Does that mean code?
Anyway, here's my 'main(){}' function to test the program according to requirements 1-4:
```C++
int main() {

    HashTable ht;

    ht.insert("apple", 1);
    ht.insert("banana", 2);
    ht.insert("cherry", 3);
    ht.insert("date", 4);
    ht.insert("elderberry", 5);
    ht.insert("fig", 6);
    ht.insert("grape", 7);
    ht.insert("honeydew", 8);
    ht.insert("kiwi", 9);
    ht.insert("lemon", 10);

    cout << "Table capacity: " << ht.getCapacity() << endl;
    cout << "Number of elements: " << ht.size() << endl;
    cout << "Load factor: " << ht.loadFactor() << endl;
    cout << "Total collisions: " << ht.collisions() << endl;

    cout << "Search for existing key 'banana': " << ht.search("banana") << endl;
    cout << "Search for non-existing key 'orange': " << ht.search("orange") << endl;

    return 0;
}
```
Here's the result:
<img width="1411" height="444" alt="Screenshot From 2026-03-08 22-51-29" src="https://github.com/user-attachments/assets/6e5e6458-8997-4193-9df4-81d261e9eb75" />
From the looks of it, the capacity rehashed to 22 (should have been 11 as per size). Load factor triggers at greater than (>) 0.75. Has to be the 9th element "kiwi" because 9/11 = 0.82.

### Part 6 - Experimental Analysis
I need to do the following:
  - Test three input types:
  
    1.  Random strings  
    2.  Sequential keys (e.g., student1, student2, ...)  
    3.  Same prefix keys (e.g., data_0001, data_0002, ...)
  
  - Record:
    -   Total collisions
    -   Maximum bucket size
    -   Average bucket length
  Write a short explanation (1--2 paragraphs) describing what you observe.

Here's my results:
<img width="1879" height="1033" alt="Screenshot From 2026-03-08 23-35-19" src="https://github.com/user-attachments/assets/05e89f66-21a2-4a88-ac81-4dfbdfc42fc3" />

**Short Explanation:**

&emsp; I created a random generator using lower case and upper case alphabets. Then, using a for loop, I generated 20 random strings of up to 10 characters (length 10). I created a generator for sequential keys and same prefix keys as well using for loops. Upon running the program a couple of times (5 times in total), it seemed that barely any **collisions** were happening for the random strings and the sequential keys. However, for the same prefix keys, I always got a number greater than (>) 5 for collisions on 5 separate occasions. The random strings and sequential keys always seemed to range between 0-1 colissions. **Maximum bucket size** stayed at 44 (that's 3 rehashes) for random strings and 88 (4 rehashes) for both sequential keys and prefix keys on all runs. The **average bucket length** seemed consistent with both random strings and sequential keys at 0.45, but the same prefix keys always averaged at 0.68 -- meaning that for same prefix keys, there were more keys within the same bucket/index. 

&emsp; Overall, random strings and sequential keys generated about the same results on average. <mark>The only one that had higher numbers for all recorded categories</mark> (total collisions, maximum bucket size, and average bucket length) <mark>was the same prefix keys.</mark>
