# 🌳 Data Structures — Competitive Programming Revision Mind Map

```
DATA STRUCTURES (CP)
│
├── 1. ARRAY / VECTOR
│   │
│   ├── Core Idea
│   │   ├── Contiguous memory
│   │   ├── Fast indexing
│   │   └── Fixed/Resizable (array/vector)
│   │
│   ├── Remember
│   │   ├── Indexing: O(1)
│   │   ├── Cache friendly
│   │   ├── Insert/delete in middle → expensive
│   │   └── Most used DS in CP
│   │
│   ├── CP Usage
│   │   ├── Prefix sums
│   │   ├── Frequency arrays
│   │   ├── Dynamic programming storage
│   │   ├── Simulation
│   │   └── Graph adjacency storage
│   │
│   └── Things to Watch
│       ├── Index out of bound
│       ├── Memory limits
│       └── 0-based vs 1-based indexing
│
│
├── 2. STRING
│   │
│   ├── Core Idea
│   │   ├── Array of characters
│   │   └── Special handling of characters
│   │
│   ├── Remember
│   │   ├── Mutable vs immutable (language dependent)
│   │   ├── Character indexing
│   │   ├── ASCII values
│   │   └── String length handling
│   │
│   ├── CP Usage
│   │   ├── Frequency counting
│   │   ├── Hashing storage
│   │   ├── Pattern related storage
│   │   └── Parsing input
│   │
│   └── Watch
│       ├── Large strings
│       ├── Memory duplication
│       └── Character encoding issues
│
│
├── 3. LINKED LIST
│   │
│   ├── Types
│   │   ├── Singly linked list
│   │   ├── Doubly linked list
│   │   └── Circular linked list
│   │
│   ├── Remember
│   │   ├── Node-based storage
│   │   ├── No random access
│   │   ├── Pointer manipulation matters
│   │   └── Extra memory for links
│   │
│   ├── CP Usage
│   │   ├── Custom data structures
│   │   ├── LRU cache implementation
│   │   ├── Manual memory structures
│   │   └── When deletion/insertion is frequent
│   │
│   └── Watch
│       ├── Null pointers
│       ├── Losing references
│       └── Edge cases:
│           ├── Empty list
│           └── Single node
│
│
├── 4. STACK
│   │
│   ├── Property
│   │   └── LIFO
│   │
│   ├── Remember
│   │   ├── Top element access
│   │   ├── Push/pop
│   │   └── Usually implemented using vector
│   │
│   ├── CP Usage
│   │   ├── Monotonic stack storage
│   │   ├── Expression processing
│   │   ├── Undo operations
│   │   └── Recursion simulation
│   │
│   └── Watch
│       ├── Stack overflow (deep recursion)
│       └── Empty stack access
│
│
├── 5. QUEUE
│   │
│   ├── Property
│   │   └── FIFO
│   │
│   ├── Types
│   │   ├── Normal queue
│   │   ├── Circular queue
│   │   ├── Deque
│   │   └── Priority queue
│   │
│   ├── CP Usage
│   │   ├── BFS storage
│   │   ├── Sliding window
│   │   ├── Scheduling simulation
│   │   └── Stream processing
│   │
│   └── Watch
│       ├── Queue size
│       └── Front/back handling
│
│
├── 6. DEQUE
│   │
│   ├── Property
│   │   └── Insert/delete from both ends
│   │
│   ├── Remember
│   │   ├── More flexible than queue
│   │   └── Usually optimized implementation
│   │
│   ├── CP Usage
│   │   ├── Sliding window maximum/minimum
│   │   ├── Two-ended processing
│   │   └── Maintaining candidates
│   │
│   └── Watch
│       └── Maintaining correct front/back state
│
│
├── 7. HASH TABLE
│   │
│   ├── Forms
│   │   ├── Map
│   │   ├── Unordered map
│   │   └── Hash set
│   │
│   ├── Remember
│   │   ├── Key → Value mapping
│   │   ├── Average O(1) access
│   │   └── Depends on hash function
│   │
│   ├── CP Usage
│   │   ├── Frequency counting
│   │   ├── Duplicate detection
│   │   ├── Memoization storage
│   │   └── Coordinate compression support
│   │
│   └── Watch
│       ├── Collision
│       ├── Worst-case complexity
│       └── unordered_map hacking cases
│
│
├── 8. SET
│   │
│   ├── Types
│   │   ├── Ordered set
│   │   └── Unordered set
│   │
│   ├── Remember
│   │   ├── Stores unique values
│   │   ├── Searching
│   │   └── Automatic ordering (ordered)
│   │
│   ├── CP Usage
│   │   ├── Remove duplicates
│   │   ├── Maintain sorted data
│   │   └── Dynamic collection
│   │
│   └── Watch
│       ├── Duplicate insertion
│       └── Iterator invalidation
│
│
├── 9. TREE
│   │
│   ├── Important Types
│   │   ├── Binary Tree
│   │   ├── BST
│   │   ├── Heap
│   │   ├── Trie
│   │   └── Segment Tree
│   │
│   ├── Remember
│   │   ├── Node + edges
│   │   ├── Hierarchical structure
│   │   └── Recursive representation
│   │
│   ├── CP Storage
│   │   ├── Array representation
│   │   └── Adjacency list
│   │
│   └── Watch
│       ├── Height
│       ├── Balanced vs skewed
│       └── Memory allocation
│
│
├── 10. HEAP / PRIORITY QUEUE
│   │
│   ├── Property
│   │   ├── Complete binary tree
│   │   └── Parent-child ordering
│   │
│   ├── Types
│   │   ├── Min heap
│   │   └── Max heap
│   │
│   ├── Remember
│   │   ├── Top element easily accessible
│   │   └── Not fully sorted
│   │
│   ├── CP Usage
│   │   ├── Maintain minimum/maximum
│   │   ├── Top K problems
│   │   └── Dynamic ranking
│   │
│   └── Watch
│       └── Heap property ≠ sorted array
│
│
├── 11. TRIE
│   │
│   ├── Stores
│   │   └── Prefix-based data
│   │
│   ├── Remember
│   │   ├── Nodes represent characters
│   │   ├── Fast prefix search
│   │   └── Memory heavy
│   │
│   ├── CP Usage
│   │   ├── Dictionary problems
│   │   ├── Prefix matching
│   │   └── XOR trie
│   │
│   └── Watch
│       ├── Huge memory usage
│       └── Alphabet size
│
│
├── 12. DISJOINT SET UNION (DSU)
│   │
│   ├── Stores
│   │   └── Connected components
│   │
│   ├── Remember
│   │   ├── Parent array
│   │   ├── Size/rank tracking
│   │   └── Component count
│   │
│   ├── CP Usage
│   │   ├── Dynamic connectivity
│   │   ├── Group merging
│   │   └── Graph component problems
│   │
│   └── Watch
│       ├── Path compression
│       └── Correct initialization
│
│
├── 13. SEGMENT TREE
│   │
│   ├── Stores
│   │   └── Range information
│   │
│   ├── Remember
│   │   ├── Tree over array
│   │   ├── Node stores interval data
│   │   └── Flexible merge operation
│   │
│   ├── Variants
│   │   ├── Lazy propagation
│   │   ├── Iterative segment tree
│   │   └── Persistent segment tree
│   │
│   └── Watch
│       ├── Tree size allocation
│       ├── Merge function
│       └── Index mapping
│
│
└── 14. BIT / FENWICK TREE
    │
    ├── Stores
    │   └── Prefix information
    │
    ├── Remember
    │   ├── Compact segment tree alternative
    │   ├── Uses binary representation
    │   └── Array based
    │
    ├── CP Usage
    │   ├── Prefix sums
    │   ├── Frequency updates
    │   └── Inversion counting
    │
    └── Watch
        ├── 1-indexing
        └── Update/query direction
```

## 🧠 Final CP Data Structure Checklist

```
Before choosing a DS:

Need random access?
        ↓
     ARRAY

Need unique + fast lookup?
        ↓
     HASH SET / SET

Need ordering?
        ↓
     TREE / ORDERED SET

Need min/max repeatedly?
        ↓
     HEAP

Need prefix search?
        ↓
     TRIE

Need connectivity groups?
        ↓
     DSU

Need range query/update?
        ↓
     SEGMENT TREE / BIT

Need previous/next element tracking?
        ↓
     STACK / DEQUE

Need dynamic relationships?
        ↓
     TREE / GRAPH STRUCTURES
```

This is the **Data Structure revision map** you would typically keep before contests. It focuses on **what the structure stores, why CP uses it, and what mistakes to avoid** rather than definitions.
