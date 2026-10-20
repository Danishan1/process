Absolutely. Here's a **revision sheet for all 10**, using the same recursion/BFS patterns we've been building up.

 The goal isn't just to memorize code — notice that most of these problems are variations of **3–4 core patterns**.

 # 7\. Binary Trees — Revision

 ## 104 — Maximum Depth of Binary Tree

 ### Idea

 For every node:

```
height = 1 + max(left height, right height)
```

```js
var maxDepth = function(root) {
    if (!root) return 0;

    const left = maxDepth(root.left);
    const right = maxDepth(root.right);

    return 1 + Math.max(left, right);
};
```

 ### Pattern

```
null → 0
node → 1 + max(left, right)
```

 **Time:** O(n)\
 **Space:** O(h), where `h` = tree height.

---

 # 100 — Same Tree

 ### Idea

 Two trees are the same if:

 1. Both are `null`
2. Their values are equal
3. Their left subtrees are the same
4. Their right subtrees are the same

```js
var isSameTree = function(p, q) {
    if (!p && !q) return true;

    if (!p || !q) return false;

    if (p.val !== q.val) return false;

    return (
        isSameTree(p.left, q.left) &&
        isSameTree(p.right, q.right)
    );
};
```

 ### Pattern

 This is basically:

```
compare current
    ↓
compare left
    ↓
compare right
```

 The important base cases:

```js
if (!p && !q) return true;
if (!p || !q) return false;
```

 **Time:** O(n)\
 **Space:** O(h)

---

 # 226 — Invert Binary Tree

 ### Idea

 Swap the left and right children at every node.

```
Before:

       1
      / \
     2   3

After:

       1
      / \
     3   2
```

```js
var invertTree = function(root) {
    if (!root) return null;

    const temp = root.left;
    root.left = root.right;
    root.right = temp;

    invertTree(root.left);
    invertTree(root.right);

    return root;
};
```

 You can also recursively swap:

```js
var invertTree = function(root) {
    if (!root) return null;

    const left = invertTree(root.left);
    const right = invertTree(root.right);

    root.left = right;
    root.right = left;

    return root;
};
```

 ### Pattern

```
swap
 ↓
recurse left
 ↓
recurse right
```

 **Time:** O(n)\
 **Space:** O(h)

---

 # 102 — Binary Tree Level Order Traversal

 This is your **BFS / Queue** pattern.

 ### Idea

 Process the tree one level at a time.

```js
var levelOrder = function(root) {
    if (!root) return [];

    const q = [root];
    const ans = [];

    while (q.length > 0) {
        const levelSize = q.length;
        const level = [];

        for (let i = 0; i < levelSize; i++) {
            const node = q.shift();

            level.push(node.val);

            if (node.left) {
                q.push(node.left);
            }

            if (node.right) {
                q.push(node.right);
            }
        }

        ans.push(level);
    }

    return ans;
};
```

 ### The important trick

```js
const levelSize = q.length;
```

 This tells you:

 > How many nodes belong to the current level?

 Then:

```js
for (let i = 0; i < levelSize; i++)
```

 processes exactly that level.

 ### Queue pattern

```
push → add
shift → remove front
```

 For production/large input, an index-based queue is better than repeated `shift()`:

```js
const q = [root];
let front = 0;

while (front < q.length) {
    const node = q[front++];
}
```

 **Time:** O(n)\
 **Space:** O(n)

---

 # 543 — Diameter of Binary Tree

 This is the first important **"return one thing, calculate another thing"** pattern.

 ### Idea

 At every node:

```js
       node
      /    \
    left  right
```

 Potential diameter through this node:

```js
left height + right height
```

 But the recursive function returns **height**.

```js
var diameterOfBinaryTree = function(root) {
    let diameter = 0;

    const height = (node) => {
        if (!node) return 0;

        const left = height(node.left);
        const right = height(node.right);

        diameter = Math.max(diameter, left + right);

        return 1 + Math.max(left, right);
    };

    height(root);

    return diameter;
};
```

 ### Remember

 What we **calculate**:

```js
left + right
```

 What we **return**:

```js
1 + Math.max(left, right)
```

 Why?

 Because the parent can continue through only **one branch**.

 **Time:** O(n)\
 **Space:** O(h)

---

 # 110 — Balanced Binary Tree

 ### Definition

 A tree is balanced if at **every node**:

```js
abs(leftHeight - rightHeight) <= 1
```

 ### Solution

 We can use the same height pattern as diameter.

```js
var isBalanced = function(root) {
    const height = (node) => {
        if (!node) return 0;

        const left = height(node.left);

        if (left === -1) return -1;

        const right = height(node.right);

        if (right === -1) return -1;

        if (Math.abs(left - right) > 1) {
            return -1;
        }

        return 1 + Math.max(left, right);
    };

    return height(root) !== -1;
};
```

 ### Why `-1`?

 We're using `-1` as:

```js
-1 = "this subtree is unbalanced"
```

 Normally:

```js
return height;
```

 But if we discover imbalance:

```js
return -1;
```

 Then the parent immediately knows:

```js
if (left === -1) return -1;
```

 This avoids repeatedly calculating heights.

 ### Core pattern

```
calculate left height
calculate right height
check difference
return height
```

 **Time:** O(n)\
 **Space:** O(h)

---

 # 236 — Lowest Common Ancestor

 ### Idea

 Search both sides.

```js
var lowestCommonAncestor = function(root, p, q) {
    if (!root || root === p || root === q) {
        return root;
    }

    const left = lowestCommonAncestor(root.left, p, q);
    const right = lowestCommonAncestor(root.right, p, q);

    if (left && right) {
        return root;
    }

    return left || right;
};
```

 ### Understand the 3 cases

 #### Found nothing

```
null
```

 #### Found one target

```
p
```

 or

```
q
```

 #### Found targets on both sides

```
        root
       /    \
      p      q
```

 Therefore:

```
return root;
```

 ### The pattern

 This is another very reusable tree pattern:

```js
const left = dfs(node.left);
const right = dfs(node.right);

if (left && right) {
    return node;
}

return left || right;
```

 **Time:** O(n)\
 **Space:** O(h)

---

 # 199 — Binary Tree Right Side View

 There are two good solutions.

 ## BFS

 Since you already know level-order traversal, this is probably the easiest to remember.

```js
var rightSideView = function(root) {
    if (!root) return [];

    const q = [root];
    const ans = [];

    while (q.length > 0) {
        const levelSize = q.length;

        for (let i = 0; i < levelSize; i++) {
            const node = q.shift();

            if (i === levelSize - 1) {
                ans.push(node.val);
            }

            if (node.left) {
                q.push(node.left);
            }

            if (node.right) {
                q.push(node.right);
            }
        }
    }

    return ans;
};
```

 ### Trick

 Process a whole level.

 Then:

```
i === levelSize - 1
```

 means:

 > This is the last node of this level.

 Therefore it's the node visible from the right.

---

 ## DFS alternative

```js
var rightSideView = function(root) {
    const ans = [];

    const dfs = (node, depth) => {
        if (!node) return;

        if (depth === ans.length) {
            ans.push(node.val);
        }

        dfs(node.right, depth + 1);
        dfs(node.left, depth + 1);
    };

    dfs(root, 0);

    return ans;
};
```

 ### Trick

 Visit:

```
RIGHT → LEFT
```

 and take the **first node at each depth**.

 **Time:** O(n)\
 **Space:** O(h) for DFS, O(n) worst case for BFS.

---

 # 105 — Construct Binary Tree from Preorder + Inorder

 This one is all about understanding the traversal orders.

 ### Preorder

```
ROOT → LEFT → RIGHT
```

 Therefore:

```
preorder[0]
```

 is always the root.

 ### Inorder

```
LEFT → ROOT → RIGHT
```

 Find the root inside `inorder`.

 Everything before it:

```
LEFT SUBTREE
```

 Everything after it:

```
RIGHT SUBTREE
```

 ### Easy-to-understand solution

```js
var buildTree = function(preorder, inorder) {
    if (preorder.length === 0) {
        return null;
    }

    const rootVal = preorder[0];
    const root = new TreeNode(rootVal);

    const rootIndex = inorder.indexOf(rootVal);

    const leftInorder = inorder.slice(0, rootIndex);
    const rightInorder = inorder.slice(rootIndex + 1);

    const leftPreorder = preorder.slice(1, rootIndex + 1);
    const rightPreorder = preorder.slice(rootIndex + 1);

    root.left = buildTree(leftPreorder, leftInorder);
    root.right = buildTree(rightPreorder, rightInorder);

    return root;
};
```

 ### The mental model

```
preorder[0]
     ↓
   ROOT
     ↓
find root in inorder
     ↓
┌─────────────┐
LEFT         RIGHT
```

 This version is easy to understand but uses `slice()` and `indexOf()`, so it can be O(n²).

---

 # 124 — Binary Tree Maximum Path Sum

 This is probably the most important/hardest one in this list.

 Again, the recursive function returns **one thing**, while we calculate the answer separately.

```js
var maxPathSum = function(root) {
    let ans = -Infinity;

    const dfs = (node) => {
        if (!node) return 0;

        const left = Math.max(0, dfs(node.left));
        const right = Math.max(0, dfs(node.right));

        // Best path passing through this node
        const currentPath = left + node.val + right;

        ans = Math.max(ans, currentPath);

        // Give parent only one branch
        return node.val + Math.max(left, right);
    };

    dfs(root);

    return ans;
};
```

 ### Why `Math.max(0, ...)`?

 A negative subtree makes the path worse.

 So:

```
const left = Math.max(0, dfs(node.left));
```

 means:

 > Use the left subtree only if it helps.

 ### The two formulas

 **Calculate answer:**

```js
left + node.val + right
```

 We can use both sides because the path can look like:

```js
left → node → right
```

 **Return to parent:**

```js
node.val + Math.max(left, right)
```

 We can only return **one side** because a path cannot branch.

---

 # The Patterns You Should Actually Memorize

 Don't try to memorize 10 separate solutions.

 There are really only a few patterns.

 ## Pattern 1 — Basic tree recursion

 Used by:

 - 104 Maximum Depth
- 100 Same Tree
- 226 Invert Tree

```js
function dfs(node) {
    if (!node) {
        return ...;
    }

    const left = dfs(node.left);
    const right = dfs(node.right);

    return ...;
}
```

---

 ## Pattern 2 — Height + global answer

 Used by:

 - 543 Diameter
- 124 Maximum Path Sum
- 110 Balanced Tree

 The general structure:

```js
let ans = ...;

const dfs = (node) => {
    if (!node) return ...;

    const left = dfs(node.left);
    const right = dfs(node.right);

    // Calculate something using left + right
    ans = Math.max(ans, ...);

    // Return something to parent
    return ...;
};

dfs(root);

return ans;
```

 This pattern is **extremely important**.

---

 ## Pattern 3 — BFS by levels

 Used by:

 - 102 Level Order
- 199 Right Side View

```js
const q = [root];

while (q.length > 0) {
    const levelSize = q.length;

    for (let i = 0; i < levelSize; i++) {
        const node = q.shift();

        if (node.left) q.push(node.left);
        if (node.right) q.push(node.right);
    }
}
```

 Remember:

```
queue
push → add to back
remove front → shift / front index
```

---

 ## Pattern 4 — Search both subtrees

 Used by:

 - 236 LCA

```
const left = dfs(node.left);
const right = dfs(node.right);

if (left && right) {
    return node;
}

return left || right;
```

---

 ## Pattern 5 — Traversal reconstruction

 Used by:

 - 105 Preorder + Inorder
- 106 Inorder + Postorder

 Remember:

```
PREORDER:
ROOT → LEFT → RIGHT
```

 and:

```
INORDER:
LEFT → ROOT → RIGHT
```

 Therefore:

```
preorder first element
        ↓
       ROOT
        ↓
find ROOT in inorder
     ↙       ↘
   LEFT     RIGHT
```

---

 # One-Page Cheat Sheet

 | LC | Problem | Core pattern |
| --- | --- | --- |
| **104** | Max Depth | `1 + max(left, right)` |
| **100** | Same Tree | Compare node + recurse both |
| **226** | Invert Tree | Swap left/right + recurse |
| **102** | Level Order | BFS + `levelSize` |
| **543** | Diameter | Height + global `left + right` |
| **110** | Balanced | Height + detect difference |
| **236** | LCA | Search left/right |
| **199** | Right View | BFS + last node |
| **105** | Build Tree | Preorder root + split inorder |
| **124** | Max Path Sum | `left + node + right`, return one side |

If you're revising these for interviews, the **highest-value thing to practice is recognizing which pattern the problem belongs to**, rather than memorizing each implementation independently.