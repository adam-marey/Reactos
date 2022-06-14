# Merge Two Linked Lists

## Learning Objective
* Apply pointer logic to multiple linked lists


## Interviewer Prompt

Write a function that takes in the heads of two Singly Linked Lists that are in sorted order, respectively. The function should merge the lists in place (i.e., it shouldn't create a brand new list) and return the head of the merged list; the merged list should be in sorted order.

Each `LinkedList` node has an integer `value` as well as a `next` node pointing to the next node in the list or to `null` if it's the tail of the list.

## Examples

```
headOne = 2 -> 6 -> 7 -> 8 // the head node with value 2
headTwo = 1 -> 3 -> 4 -> 5 -> 9 -> 10 // the head node with value 1
mergeLinkedLists(headOne, headTwo) = 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 // the new head node with value 1
```

## Interviewer Strategy Guide

First, make sure the interviewee understands the structure of a `LinkedList` and its constraints. Because the lists are already sorted, traditional sorting algorithms won't be needed. 

The interviewee can implement this algorithm either iteratively or recursively following nearly identical logic, so you can leave that up to them.

Most interviewees will get the process, but it's very easy to "solve" the problem by creating a malformed `LinkedList`. Make sure 3 nodes are being manipulated at every step to ensure the final LinkedList doesn't have any holes or misdirected pointers.

## Hints

* Linked lists are just objects!
* You can iterate through the Linked Lists from head to tail and merge them along the way by inserting nodes from the second Linked List into the first Linked List.
* You'll need to manipulate three nodes at once at every step.
* Once they've decided on an approach, either iteration or recursion, you can elaborate on what those pointers might be if they get stuck. 
```
p1 = current node in the first linked list
p2 = current node in the second linked list
prev or temp (depending on how you want to tackle this) = previous node from first linked list, or
a temporary variable to hold the reference to a node so you don't lose it.
```

## Solutions

**Solution 1: Iterative** 

Time Complexity: O(n+m)&nbsp; &nbsp; &nbsp; &nbsp; Space complexity: 0(1)<br/>n = length of list 1<br/>m = length of list 2 

```js
function mergeTwoLists(headone, headtwo) {
  // If either head is null, determine which one isn't return that head.
  if (headone === null || headtwo === null)
    return headone === null ? headtwo : headone

  // Determine which head holds the smallest value and set p1 
  // to start at the smaller head.
  // This will be the linked list we build from.
  let p1 = headone.value <= headtwo.value ? headone : headtwo
  let p2 = headone === p1 ? headtwo : headone
  let head = p1

  while (p1.next !== null && p2 !== null) {
    // if p1.next is less than p2 we can just move ahead one spot with p1
    if (p1.next.value <= p2.value) {
      p1 = p1.next
    }
    // if p2 is less than p1.next we'll need to slot p2
    // in between p1 and p1.next without losing the reference to p1.next
    else {
      let temp = p1.next
      p1.next = p2
      p2 = temp
    }
  }

  p1.next = p2
  return head
}
```

**Solution 2: Recursive** 
<br/>
Time Complexity: O(n+m)&nbsp; &nbsp; &nbsp; &nbsp; Space complexity: 0(n+m)<br/>n = length of list 1<br/>m = length of list 2

```js
function mergeLinkedLists(headOne, headTwo) {
  recursiveMerge(headOne, headTwo, null);
  // Determine which head holds the list after 
  // recursiveMerge returns above.
  return headOne.value < headTwo.value ? headOne : headTwo;
}
​
function recursiveMerge(p1, p2, p1Prev) {
//if p1 is null we've run out of links to check.
//set p1Prev.next to the rest of p2 and return
  if (p1 === null) {
    p1Prev.next = p2;
    return;
  }
  //same as above, but just return the rest of the list
  if (p2 === null) return;

​  if (p1.value < p2.value) {
    recursiveMerge(p1.next, p2, p1);
  } else {
    if (p1Prev !== null) p1Prev.next = p2;
    const newP2 = p2.next;
    p2.next = p1;
    recursiveMerge(p1, newP2, p2);
  }
}
```

**Solution 3: Recursive** 
<br/>
Time Complexity: O(n+m)&nbsp; &nbsp; &nbsp; &nbsp; Space complexity: 0(n+m)<br/>n = length of list 1<br/>m = length of list 2

```js
function mergeLists(headone, headtwo) {
  // If either head is null, determine which one isn't and
  // return that head.
  if (headone === null || headtwo === null) {
    return headone === null ? headtwo : headone
  }

  // Determine which head holds the smallest value and set 
  // p1 to start at the smaller head.
  // This will be the linked list we build from.
  let p1 = headone.val <= headtwo.val ? headone : headtwo
  let p2 = p1 === headone ? headtwo : headone
  const head = p1

  head.next = mergeLists(p1.next, p2)

  return head
}
```

## Resources

* [Linked List Review](https://youtube.com/playlist?list=PLx0iOsdUOUmmR3kE0iA2eIYNS_beMg8ti)
* [Linked Lists for Technical Interviews - Full Course](https://youtu.be/Hj_rA0dhr2I)
* [Excalidraw Diagram](https://excalidraw.com/#json=fBpe1eWK-WKnX3oLDxB8s,6kEsIFKOwdSMZmKlBM9xhQ)
* [Merge LL Slides](https://docs.google.com/presentation/d/105Hsu_VrqS8Iax1WuwtwlsAlq_pW2eIk/edit?usp=sharing&ouid=115886749169523888275&rtpof=true&sd=true)
* [AlgoExpert Link](https://www.algoexpert.io/questions/Merge%20Linked%20Lists)
* [Merge N Linked List Gist](https://gist.github.com/seanhagstrom/b1762010974f40c2b945d78d73647fad)

for testing
```js
class LinkedList {
  constructor(val) {
    this.value = val;
    this.next = null;
  }
}

function createList (nums) {
  let head = new LinkedList(nums[0])
  let origHead = head
  for (let i = 1; i < nums.length; i++) {
    head.next = new LinkedList(nums[i])
    head = head.next
  }
  return origHead
}

let headone = createList([2, 6, 7, 8])
let headtwo = createList([1, 3, 4, 5, 9, 10])
```