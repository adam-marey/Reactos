## Solving Graphs

## Learning Objective
Understand adjacency list implementation of graphs

## Interviewer Prompt

Write a function that determines if a path exists between two vertices of a directed graph.

The graph will be represented as an object, each of whose keys represents a vertex of the graph and whose value represents all vertices that can be reached from the aforementioned key.

In the example below, there is a connection from vertex a to vertex b and a connection from vertex b to vertices c and d but not a connection from vertex b to vertex a.

```javascript
{
  a: ['b'],
  b: ['c', 'd'],
  c: ['d']
}
```


---

## Example Output

Note: interviewee does not have to construct their own graphs.

.center[![graph](https://i.imgur.com/uqyXmfh.png)]

```javascript
const graph = {
  a: ['b'],
  b: ['c', 'd'],
  c: ['d'],
  d: []
}
doesPathExist(graph, 'a', 'b') // true
doesPathExist(graph, 'b', 'a') // false
doesPathExist(graph, 'a', 'd') // true
doesPathExist(graph, 'a', 'a') // false
```

---

## Example Output continued

.center[![graph](https://i.imgur.com/ehvb9qx.png)]

```javascript
const graph = {
  a: ['a', 'c'],
  c: ['r', 's'],
  r: ['a'],
  s: []
}
doesPathExist(graph, 'a', 'a') // true
doesPathExist(graph, 'c', 'c') // true
doesPathExist(graph, 'r', 's') // true
doesPathExist(graph, 's', 'a') // false
```
---

class: center middle
## Interviewer Guide

---

### RE

This is a good place to have your interviewee draw out the graph and think through how they want to walk through the nodes.

#### Guide:

* Interviewers: make it clear that the nodes in this graph are represented by strings like 'a' and 'b', not by instances of a Node class.

* If your interviewee continues without asking questions, stop them and ask, "Do you have any questions about what the graph looks like?" Your prompt mentions that the graph is directed, but they may not have caught on to this fact.

* Your interviewee may also not realize that we are allowing cyclic connections in these graphs. Again, this is easy to visualize/reason about if they have drawn out an example graph or two.

---

### AC

This problem is essentially a DFS/BFS problem. Either algorithm is sufficient. The only catch is that graphs can be cyclic. In other words, it's possible for a loop to exist in the graph.

#### Guide:

* You can let your interviewee come up with a solution that does not account for cycles, then let them refine it afterwards.

* See if they can come up with their own way of keeping track of visited nodes. If they are stuck, you can suggest that they use a separate object, either 'globally' or as another argument to their function.

---

### TO

Have your interviewee test their implementation on acyclic and cyclic graphs.

#### Guide:
* If your interviewee finishes, ask them:
  * What are the time and space complexities for their approach?

  * Can you think of alternative data structures for keeping track of visited nodes? (Sets and Maps are always good to know)

---

## Solution

Depth First Search Solution
```javascript
const doesPathExist = (graph, start, target, visited = {}) => {
  if (!graph[start]) return false
  visited[start] = true;
  return graph[start].some((vertex) => {
    if (vertex === target) return true;
    if (!visited[vertex]) {
      return doesPathExist(graph, vertex, target, visited);
    } else {
      return false;
    }
  });
}
```
[MDN .some()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some)
---

Breadth First Search Solution
```javascript
const doesPathExist = (graph, start, target, visited = {}) => {
  if (!graph[start]) return false
  let queue = [start]
  let pointer = 0;
  while (pointer < queue.length) { //As long as you haven't reached
    let node = queue[pointer]      //the end of the queue, keep traversing graph
    visited[node] = true;
    let neighbors = graph[node]
    for (let i = 0; i < neighbors.length; i++) { //Iterate through all neighbors
      let vertex = neighbors[i]                  //for current node
      if (vertex === target) return true //return true if it's our target
      if (!visited[vertex]) {  //Only add to queue if you haven't
        queue.push(vertex)     //seen the node before
      }
    }
    pointer++ //Move on to next item in queue. Could alternatively use shift.
  }
  return false
}
```
## Big O

* Both DFS and BFS take O(V + E) time where V is the number of vertices or nodes and E is the number of edges. We MUST attempt to visit every node, which will take us through potentially many edges.

  * For acyclic graphs, we might visit every node and hit the leaves
  * For cyclic graphs, the cycle might not occur until a 'leaf'
  * For dense graphs, edges will dominate
  * For sparse graphs, vertices will dominate

* Keeping track of a 'visited' object means we need O(v) additional space

---
## Summary

* All trees are graphs. Techniques you learn to solve graphs can be applied for various kinds of trees, and often vice versa.

* You will often run into problems where you need to track 'visited' nodes
  * Sometimes this means keeping a table of visited nodes

  * If nodes are objects, this could mean adding a 'visited' property

---

## Discussion

The data structure seen ***earlier*** used to represent the graph is called an **adjacency list**. An alternative data structure exists for representing graphs called adjacency matrices. The cyclic graph above could have been modeled as ***follows*** using an **adjacency matrix**:

        a  c  s  r
      a 1  1  0  0
      c 0  0  1  1
      s 0  0  0  0
      r 1  0  0  0

In javascript, this table would be represented using an array of arrays or object of objects. A 1 indicates that a given vertex has an edge pointing to another vertex, and a 0 indicates that it does not. Both of these show direction!

---

## Discussion

Adjacency matrix:

        a  c  s  r
      a 1  1  0  0
      c 0  0  1  1
      s 0  0  0  0
      r 1  0  0  0

This table reads as follows:<br>

`a -> a`<br>
`a -> c`<br>
`c -> s`<br>
`c -> r`<br>
`r -> a`

---

## Discussion

Consider the tradeoffs between using one of these data structures or the other. Which do we prefer for the following operations?

* Testing if a given edge exists
  * Adjacency matrix O(1)
* Finding the # of edges of a vertex
  * Adjacency list O(1)
* Insertion/deletion of edges
  * AM O(1), AL O(d) where d is degree of vertex
* Memory usage for sparse graphs
  * AL O(v + e), AM O(v^2)
* Memory usage for dense graphs
  * AM O(v^2)
* Graph traversal
  * AL O(v + e), AM O(n^2)
* Better overall
  * Adjacency List

Comparison from The Algorithm Design Manual, Skiena - second Edition - page 152
---

## Resources
_Feel free to PR any useful resources! :)_

* [Sample Slides](https://docs.google.com/presentation/d/1W9xj2U-8Xttqe6qHb5oa_bZtAEl5pNfcCMDpNEjQcpU/edit?usp=sharing)
* [YouTube Videos](https://www.youtube.com/watch?v=DBRW8nwZV-g&ab_channel=freeCodeCamp.org) (https://www.youtube.com/watch?v=cWNEl4HE2OE&ab_channel=Fireship)
* [Structy interview problem](https://structy.net/problems/has-path)
* [DFS Python Tutor](https://pythontutor.com/visualize.html#code=const%20doesPathExist%20%3D%20%28graph,%20start,%20target,%20visited%20%3D%20%7B%7D%29%20%3D%3E%20%7B%0A%20%20if%20%28!graph%5Bstart%5D%29%20return%20false%0A%20%20visited%5Bstart%5D%20%3D%20true%3B%0A%0A%20%20return%20graph%5Bstart%5D.some%28%28vertex%29%20%3D%3E%20%7B%0A%20%20%20%20if%20%28vertex%20%3D%3D%3D%20target%29%20return%20true%3B%0A%20%20%20%20if%20%28!visited%5Bvertex%5D%29%20%7B%0A%20%20%20%20%20%20return%20doesPathExist%28graph,%20vertex,%20target,%20visited%29%3B%0A%20%20%20%20%7D%20else%20%7B%0A%20%20%20%20%20%20return%20false%3B%0A%20%20%20%20%7D%0A%20%20%7D%29%3B%0A%7D%0A%0Aconst%20graph%20%3D%20%7B%0A%20%20a%3A%20%5B'a',%20'c'%5D,%0A%20%20c%3A%20%5B'r',%20's'%5D,%0A%20%20r%3A%20%5B'a'%5D,%0A%20%20s%3A%20%5B%5D%0A%7D%0A%0AdoesPathExist%28graph,%20'r',%20's'%29%20//%20true&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=js&rawInputLstJSON=%5B%5D&textReferences=false)
* [BFS Python Tutor](https://pythontutor.com/visualize.html#code=const%20doesPathExist%20%3D%20%28graph,%20start,%20target,%20visited%20%3D%20%7B%7D%29%20%3D%3E%20%7B%0A%20%20if%20%28!graph%5Bstart%5D%29%20return%20false%0A%0A%20%20let%20queue%20%3D%20%5Bstart%5D%0A%20%20let%20pointer%20%3D%200%3B%0A%20%20while%20%28pointer%20%3C%20queue.length%29%20%7B%20//As%20long%20as%20you%20haven't%20reached%0A%20%20%20%20let%20node%20%3D%20queue%5Bpointer%5D%20%20%20%20%20%20//the%20end%20of%20the%20queue,%20keep%20traversing%20graph%0A%20%20%20%20visited%5Bnode%5D%20%3D%20true%3B%0A%20%20%20%20let%20neighbors%20%3D%20graph%5Bnode%5D%0A%20%20%20%20for%20%28let%20i%20%3D%200%3B%20i%20%3C%20neighbors.length%3B%20i%2B%2B%29%20%7B%20//Iterate%20through%20all%20neighbors%0A%20%20%20%20%20%20let%20vertex%20%3D%20neighbors%5Bi%5D%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20//for%20current%20node%0A%20%20%20%20%20%20if%20%28vertex%20%3D%3D%3D%20target%29%20return%20true%20//return%20true%20if%20it's%20our%20target%0A%20%20%20%20%20%20if%20%28!visited%5Bvertex%5D%29%20%7B%20%20//Only%20add%20to%20queue%20if%20you%20haven't%0A%20%20%20%20%20%20%20%20queue.push%28vertex%29%20%20%20%20%20//seen%20the%20node%20before%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%20%20pointer%2B%2B%20//Move%20on%20to%20next%20item%20in%20queue.%20Could%20alternatively%20use%20shift.%0A%20%20%7D%0A%20%20return%20false%0A%7D%0A%0Aconst%20graph%20%3D%20%7B%0A%20%20a%3A%20%5B'a',%20'c'%5D,%0A%20%20c%3A%20%5B'r',%20's'%5D,%0A%20%20r%3A%20%5B'a'%5D,%0A%20%20s%3A%20%5B%5D%0A%7D%0A%0AdoesPathExist%28graph,%20'r',%20's'%29%20//%20true&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=js&rawInputLstJSON=%5B%5D&textReferences=false)