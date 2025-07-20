# Breadth-First Search (BFS)

## 概念

Breadth-First Search (BFS) is a graph traversal algorithm that explores a graph level by level. It begins at a specified starting node and visits all its immediate neighbors before moving on to their unvisited neighbors, and so on. This systematic exploration ensures that all nodes at a given depth are visited before proceeding to the next depth level.

### Core Concepts:

- Queue Data Structure:
  BFS relies heavily on a queue (First-In, First-Out) to manage the order of nodes to visit. Nodes are added to the back of the queue and processed from the front.

- Visited Set/Array:
  To prevent infinite loops in graphs with cycles and to avoid processing the same node multiple times, a mechanism (e.g., a Set or a boolean array) is used to keep track of already visited nodes.
- Level-by-Level Exploration:
  The fundamental principle of BFS is to explore nodes in increasing order of their distance (number of edges) from the starting node. This characteristic makes BFS suitable for finding the shortest path in unweighted graphs.

### Implementation in JavaScript:

```js
function bfs(graph, startNode) {
  const queue = [startNode]; // Initialize queue with the starting node
  const visited = new Set(); // Keep track of visited nodes
  visited.add(startNode); // Mark the starting node as visited
  const result = []; // To store the order of visited nodes

  while (queue.length > 0) {
    const currentNode = queue.shift(); // Dequeue the first node
    result.push(currentNode); // Add to the result list

    // Explore neighbors
    if (graph[currentNode]) {
      // Check if the current node has neighbors
      for (const neighbor of graph[currentNode]) {
        if (!visited.has(neighbor)) {
          visited.add(neighbor); // Mark neighbor as visited
          queue.push(neighbor); // Enqueue unvisited neighbor
        }
      }
    }
  }
  return result;
}

// Example Usage:
const graph = {
  A: ["B", "C"],
  B: ["D", "E"],
  C: ["F"],
  D: [],
  E: ["F"],
  F: [],
};

console.log(bfs(graph, "A")); // Output: ['A', 'B', 'C', 'D', 'E', 'F']
```

### Explanation of the Code:

- Initialization:
  A queue is created and initialized with the startNode. A visited Set is used to store nodes that have already been processed to avoid revisiting them.

- Looping through the Queue:
  The while loop continues as long as there are nodes in the queue to process.

- Dequeue and Process:
  In each iteration, queue.shift() removes the first node from the queue, which becomes the currentNode. This node is then added to the result array.

- Explore Neighbors:
  The code iterates through the neighbors of the currentNode (assuming an adjacency list representation of the graph).

- Enqueue Unvisited Neighbors:
  If a neighbor has not been visited yet, it is marked as visited and added to the queue for future processing.
  Return Result:
  The function returns the result array, which contains the nodes in the order they were visited by the BFS algorithm.
