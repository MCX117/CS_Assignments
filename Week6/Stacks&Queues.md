# Assignment 7 - Stacks & Queues
## Tasks
### Illustrations:
1. PUSH(S,4), PUSH(S,1), PUSH(S,3), POP(S), PUSH(S,8), and POP(S) on an initially empty stack stored in array $S$[1...6]. 

![ScreenShot](https://github.com/MCX117/CS_Assignments/blob/ccc2f539bcc32d9be36453d723433a0c0ecea604/Week6/A7Task1.drawio.png)

2. ENQUEUE(Q,4), ENQUEUE(Q,1), ENQUEUE(Q,3), DEQUEUE(Q), ENQUEUE(Q,8), and DEQUEUE(Q) on an initially empty queue stored in array $Q$[1...6].

   &emsp; **NOTE:** S.head and S.tail should be <mark>**Q.head** and **Q.tail**</mark>. Please excuse the typo.

![ScreenShot](https://github.com/MCX117/CS_Assignments/blob/ccc2f539bcc32d9be36453d723433a0c0ecea604/Week6/A7Task2.drawio.png)

### Written Portion / Code:
3. Rewrite ENQUEUE and DEQUEUE to detect underflow and overflow of a queue.
```
Pseudocode:

Enqueue(Q, x)
  Q[Q.tail] = x
  if (Q.tail == Q.head)                  // Added this if statement to detect overflow
    cout << "Queue Overflow" << endl;
    return;                              // Escape the function
  elseif (Q.tail == Q.length)
      Q.tail = 1
  else
    Q.tail = Q.tail + 1


Dequeu(Q)
  x = Q[Q.head]
  if (Q.head == 0)                      // Added this if statement to detect underflow
    cout << "Queue Underflow" << endl;
    return;
  elseif (Q.head == Q.length)
      Q.head = 1
  else
    Q.head = Q.head + 1
  return x
```

4. A stack allows insertion and deletion of elements at only end, and a queue allows insertion at one end and deletion at the other end, a **deque** (double-ended queue) allows insertion and deletion at both ends. Write four **O(1)**-time procedures to insert elements into and delete elements from both ends of a deque implemented by an array.
```
Pseudocode:

frontInsert(D, x)
  D.head = (D.head - 1) mod D.length;    // I need the head to shift to the left first
  if (D.head == D.tail)
    cout << "Deque Overflow" << endl;
    return
  D[D.head] = x;                            // Then I add the number "x" in...

endInsert(D, x)
  Q[Q.tail] = x
  if (Q.tail == Q.head)                  // Copy/paste from enqueu
    cout << "Queue Overflow" << endl;
    return;
  elseif (Q.tail == Q.length)
      Q.tail = 1
  else
    Q.tail = Q.tail + 1

frontDelete(D)
  x = D[D.head]                            // Basically endqueue...
  if (D.head == 0)
    cout << "Deque Underflow" << endl;
    return;
  elseif (Q.head == Q.length)
      Q.head = 1
  else
      Q.head = Q.head + 1
  return x

endDelete(D)
  x = D[D.tail];
  if (D.head == 0)                         // head at 0 is always empty
    cout << "Deque Underflow" << endl;
    return;
  D.tail = (D.tail -1) mod D.length;
  return x;
```
