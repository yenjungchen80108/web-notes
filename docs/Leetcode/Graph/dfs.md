# Depth-First Search (DFS)

## 概念

Depth-First Search (DFS) is a graph traversal algorithm that explores as deeply as possible along each branch before backtracking. In JavaScript, it can be implemented using either recursion or an iterative approach with a stack.

### Core Concepts:

- Start Node:
  Choose a starting node in the graph.

- Explore Deeply:
  From the current node, explore one of its unvisited neighbors. Continue this process, moving deeper into the graph along that path.

- Backtrack:
  When a node with no unvisited neighbors is reached (a "leaf" in the current path), or if all neighbors have been visited, backtrack to the previous node and continue exploring other unvisited branches from there.

- Visited Tracking:
  Maintain a mechanism (e.g., a Set or an array) to keep track of visited nodes to prevent cycles and redundant processing in graphs.

### Implementation in JavaScript:

1. Recursive Approach:

```js
function dfsRecursive(graph, startNode, visited = new Set()) {
  // Mark the current node as visited
  visited.add(startNode);
  console.log(startNode); // Process the node (e.g., print it)

  // Recursively visit unvisited neighbors
  for (const neighbor of graph[startNode]) {
    if (!visited.has(neighbor)) {
      dfsRecursive(graph, neighbor, visited);
    }
  }
}
```

2. Iterative Approach (using a Stack):

```js
function dfsIterative(graph, startNode) {
  const stack = [startNode];
  const visited = new Set();
  visited.add(startNode);

  while (stack.length > 0) {
    const currentNode = stack.pop();
    console.log(currentNode); // Process the node

    // Add unvisited neighbors to the stack
    for (const neighbor of graph[currentNode]) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        stack.push(neighbor);
      }
    }
  }
}
```

### Key Points:

- Graph Representation:
  Graphs are typically represented using an adjacency list (e.g., an object where keys are nodes and values are arrays of their neighbors) or an adjacency matrix.

- Visited Set:
  The visited set is crucial to prevent infinite loops in graphs with cycles and to ensure each node is processed only once.

- Applications:
  DFS is used in various graph algorithms, including finding connected components, cycle detection, topological sorting, and pathfinding.
