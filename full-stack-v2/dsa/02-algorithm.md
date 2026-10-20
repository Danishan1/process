Perfect. I'd actually organize **Competitive Programming Algorithms** into layers, just like experienced CP programmers mentally categorize them. Instead of learning algorithms one by one, you learn the *family* they belong to.

# 🧠 Competitive Programming Algorithms — Revision Mind Map

```text
ALGORITHMS
│
├── 1. BRUTE FORCE
│   │
│   ├── Think
│   │   ├── Try everything
│   │   ├── Small constraints
│   │   └── Baseline solution
│   │
│   ├── Common Patterns
│   │   ├── Nested loops
│   │   ├── All pairs
│   │   ├── All triplets
│   │   ├── Complete search
│   │   └── Simulation
│   │
│   └── Remember
│       ├── Optimize later
│       └── Often passes for N ≤ 500 or 1000
│
│
├── 2. GREEDY
│   │
│   ├── Think
│   │   ├── Best local choice
│   │   ├── Hope for global optimum
│   │   └── Needs proof
│   │
│   ├── Common Patterns
│   │   ├── Sorting
│   │   ├── Earliest finish
│   │   ├── Largest first
│   │   ├── Smallest first
│   │   └── Interval scheduling
│   │
│   └── Watch
│       ├── Greedy isn't always correct
│       └── Counterexamples
│
│
├── 3. BINARY SEARCH
│   │
│   ├── Think
│   │   ├── Answer space
│   │   ├── Sorted property
│   │   └── Monotonic function
│   │
│   ├── Variants
│   │   ├── First True
│   │   ├── Last True
│   │   ├── Lower Bound
│   │   ├── Upper Bound
│   │   └── Binary Search on Answer
│   │
│   └── Watch
│       ├── Infinite loop
│       ├── Mid overflow
│       └── Boundaries
│
│
├── 4. TWO POINTERS
│   │
│   ├── Think
│   │   ├── Left pointer
│   │   ├── Right pointer
│   │   └── Move intelligently
│   │
│   ├── Common Problems
│   │   ├── Sorted arrays
│   │   ├── Pair sum
│   │   ├── Merge process
│   │   └── Window expansion
│   │
│   └── Watch
│       └── Pointer movement logic
│
│
├── 5. SLIDING WINDOW
│   │
│   ├── Think
│   │   ├── Maintain window
│   │   ├── Expand
│   │   └── Shrink
│   │
│   ├── Used For
│   │   ├── Longest substring
│   │   ├── Fixed window
│   │   ├── Variable window
│   │   └── Frequency maintenance
│   │
│   └── Watch
│       └── Window invariant
│
│
├── 6. PREFIX / SUFFIX TECHNIQUES
│   │
│   ├── Prefix Sum
│   ├── Prefix XOR
│   ├── Difference Array
│   ├── Prefix Frequency
│   └── Prefix Minimum/Maximum
│
│
├── 7. HASHING
│   │
│   ├── Think
│   │   ├── Store seen values
│   │   ├── Fast lookup
│   │   └── Frequency counting
│   │
│   ├── Applications
│   │   ├── Duplicate detection
│   │   ├── Prefix sums
│   │   ├── String hashing
│   │   └── Memoization
│
│
├── 8. SORTING
│   │
│   ├── Purpose
│   │   ├── Ordering
│   │   ├── Simplify problem
│   │   └── Enable binary search
│   │
│   ├── Common Sorts
│   │   ├── Merge
│   │   ├── Quick
│   │   ├── Heap
│   │   ├── Counting
│   │   └── Radix
│   │
│   └── Watch
│       ├── Stability
│       └── Comparator correctness
│
│
├── 9. RECURSION
│   │
│   ├── Think
│   │   ├── Divide problem
│   │   ├── Base case
│   │   └── Recursive case
│   │
│   ├── Uses
│   │   ├── Trees
│   │   ├── Backtracking
│   │   ├── DFS
│   │   └── Divide & Conquer
│   │
│   └── Watch
│       ├── Stack overflow
│       └── Missing base case
│
│
├── 10. BACKTRACKING
│   │
│   ├── Think
│   │   ├── Choose
│   │   ├── Explore
│   │   ├── Undo
│   │   └── Repeat
│   │
│   ├── Applications
│   │   ├── N Queens
│   │   ├── Sudoku
│   │   ├── Permutations
│   │   └── Combinations
│
│
├── 11. DIVIDE & CONQUER
│   │
│   ├── Divide
│   ├── Solve
│   ├── Merge
│   └── Examples
│       ├── Merge Sort
│       ├── Quick Sort
│       └── Binary Search
│
│
├── 12. DYNAMIC PROGRAMMING
│   │
│   ├── Core Idea
│   │   ├── Overlapping subproblems
│   │   ├── Optimal substructure
│   │   └── State transition
│   │
│   ├── Styles
│   │   ├── Memoization
│   │   ├── Tabulation
│   │   ├── Space Optimization
│   │   ├── Bitmask DP
│   │   ├── Tree DP
│   │   ├── Digit DP
│   │   └── Interval DP
│
│
├── 13. GRAPH ALGORITHMS
│   │
│   ├── Traversal
│   │   ├── DFS
│   │   └── BFS
│   │
│   ├── Shortest Path
│   │   ├── Dijkstra
│   │   ├── Bellman Ford
│   │   ├── Floyd Warshall
│   │   └── 0-1 BFS
│   │
│   ├── MST
│   │   ├── Kruskal
│   │   └── Prim
│   │
│   ├── DAG
│   │   ├── Topological Sort
│   │   └── DP on DAG
│   │
│   ├── Connectivity
│   │   ├── SCC
│   │   ├── Bridges
│   │   ├── Articulation Points
│   │   └── DSU
│   │
│   └── Tree Algorithms
│       ├── LCA
│       ├── Euler Tour
│       ├── Binary Lifting
│       ├── Heavy-Light Decomposition
│       └── Centroid Decomposition
│
│
├── 14. RANGE QUERY ALGORITHMS
│   │
│   ├── Prefix Sum
│   ├── Sparse Table
│   ├── Fenwick Tree
│   ├── Segment Tree
│   ├── Lazy Propagation
│   ├── Mo's Algorithm
│   └── Persistent Structures
│
│
├── 15. STRING ALGORITHMS
│   │
│   ├── KMP
│   ├── Z Algorithm
│   ├── Rabin-Karp
│   ├── Trie
│   ├── Suffix Array
│   ├── Suffix Automaton
│   ├── Manacher
│   └── Aho-Corasick
│
│
├── 16. MATHEMATICAL ALGORITHMS
│   │
│   ├── Number Theory
│   │   ├── GCD
│   │   ├── LCM
│   │   ├── Modular Arithmetic
│   │   ├── Fast Power
│   │   ├── Modular Inverse
│   │   ├── Sieve
│   │   ├── Prime Factorization
│   │   └── Euler Totient
│   │
│   ├── Combinatorics
│   ├── Matrix Exponentiation
│   ├── Probability
│   └── Game Theory
│
│
├── 17. BITMASK TECHNIQUES
│   │
│   ├── Subsets
│   ├── Masks
│   ├── State Compression
│   ├── SOS DP
│   └── Meet in the Middle
│
│
└── 18. ADVANCED TOPICS
    │
    ├── Network Flow
    ├── Bipartite Matching
    ├── FFT / NTT
    ├── Convex Hull Trick
    ├── Li Chao Tree
    ├── Geometry
    ├── Randomization
    └── Interactive Algorithms
```

---

# 🎯 CP Problem-Solving Decision Tree

```text
Given a Problem
│
├── Small Constraints?
│      └── Brute Force
│
├── Need Best Local Choice?
│      └── Greedy
│
├── Monotonic Answer?
│      └── Binary Search
│
├── Contiguous Subarray?
│      ├── Sliding Window
│      └── Prefix Sum
│
├── Pair / Two Ends?
│      └── Two Pointers
│
├── Repeated States?
│      └── Dynamic Programming
│
├── Nodes + Edges?
│      └── Graph Algorithms
│
├── Tree?
│      └── Tree Algorithms
│
├── Range Queries?
│      └── Segment Tree / Fenwick / Sparse Table
│
├── Strings?
│      └── String Algorithms
│
├── Mathematical Pattern?
│      └── Number Theory
│
├── Huge Search Space?
│      └── Backtracking / Bitmask / Meet-in-the-Middle
│
└── Nothing Fits?
       ├── Observe Patterns
       ├── Simplify
       ├── Sort
       ├── Think Greedy
       ├── Think DP
       └── Think Graph
```

## 📚 A practical learning order

If your goal is to become strong in contests (Codeforces, AtCoder, ICPC, LeetCode Hard), I'd recommend this progression:

1. Complexity Analysis
2. Arrays & Strings
3. Sorting & Searching
4. Two Pointers
5. Sliding Window
6. Prefix Sum / Difference Arrays
7. Hashing
8. Binary Search on Answer
9. Recursion & Backtracking
10. Greedy
11. Dynamic Programming
12. Graph Algorithms (BFS, DFS, Topological Sort, Shortest Paths)
13. Trees (LCA, Binary Lifting, Euler Tour)
14. Range Query Structures (Fenwick, Segment Tree, Sparse Table)
15. String Algorithms (KMP, Z, Trie, Suffix Structures)
16. Number Theory & Combinatorics
17. Bitmask Techniques
18. Advanced Algorithms (Flow, Matching, FFT, Geometry, CHT)

This sequence builds from the most frequently used techniques to the specialized algorithms that appear in higher-rated competitive programming problems.
