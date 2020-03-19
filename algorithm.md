# Algorithm

## Bits

- Clear the least significant bit: `x ^ (x - 1)`
- [Bit Twiddling Hacks](http://graphics.stanford.edu/~seander/bithacks.html)

## Graph

- Cycle detection
  - Linked list: hare and tortoise
  - Undirected graph: union find and find a union for the same roots
- Shortest path
  - Unweighted graph: BFS
  - Non-negative-weighted graph
    - Dijkstra (BFS-like)
    - A\*
      - A heuristics function must be "admissible" (never overestimate the cost) to provide the shortest path

## Computational geometry

- [Line segment intersection by sweep line algorithm](http://page.mi.fu-berlin.de/panos/cg13/l03.pdf)
