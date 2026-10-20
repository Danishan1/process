If you want to practice **binary tree basics with recursive implementations**, here are the most common questions and their JavaScript solutions.

### Tree Node Implementation

```javascript
class TreeNode {
    constructor(val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}

// Example tree
const root = new TreeNode(1);
root.left = new TreeNode(2);
root.right = new TreeNode(3);
root.left.left = new TreeNode(4);
root.left.right = new TreeNode(5);
```

Tree:

```
        1
       / \
      2   3
     / \
    4   5
```

## 1. Preorder Traversal (Root → Left → Right)

```javascript
function preorder(root) {
    if (root === null) return;

    console.log(root.val);
    preorder(root.left);
    preorder(root.right);
}

preorder(root);
// 1 2 4 5 3
```

## 2. Inorder Traversal (Left → Root → Right)

```javascript
function inorder(root) {
    if (root === null) return;

    inorder(root.left);
    console.log(root.val);
    inorder(root.right);
}

inorder(root);
// 4 2 5 1 3
```

## 3. Postorder Traversal (Left → Right → Root)

```javascript
function postorder(root) {
    if (root === null) return;

    postorder(root.left);
    postorder(root.right);
    console.log(root.val);
}

postorder(root);
// 4 5 2 3 1
```

## 4. Count Number of Nodes

```javascript
function countNodes(root) {
    if (root === null) return 0;

    return 1 + countNodes(root.left) + countNodes(root.right);
}

console.log(countNodes(root)); // 5
```

## 5. Find Height / Maximum Depth

```javascript
function maxDepth(root) {
    if (root === null) return 0;

    return 1 + Math.max(
        maxDepth(root.left),
        maxDepth(root.right)
    );
}

console.log(maxDepth(root)); // 3
```

## 6. Search a Value

```javascript
function search(root, target) {
    if (root === null) return false;

    if (root.val === target) {
        return true;
    }

    return search(root.left, target) ||
           search(root.right, target);
}

console.log(search(root, 5)); // true
console.log(search(root, 10)); // false
```

## 7. Sum of All Nodes

```javascript
function sumTree(root) {
    if (root === null) return 0;

    return root.val +
        sumTree(root.left) +
        sumTree(root.right);
}

console.log(sumTree(root)); // 15
```

## 8. Count Leaf Nodes

```javascript
function countLeaves(root) {
    if (root === null) return 0;

    if (root.left === null && root.right === null) {
        return 1;
    }

    return countLeaves(root.left) +
           countLeaves(root.right);
}

console.log(countLeaves(root)); // 3
```

## 9. Mirror / Invert Binary Tree

```javascript
function invertTree(root) {
    if (root === null) return null;

    let temp = root.left;
    root.left = root.right;
    root.right = temp;

    invertTree(root.left);
    invertTree(root.right);

    return root;
}
```

## 10. Check if Two Trees Are Same

```javascript
function isSameTree(root1, root2) {
    if (root1 === null && root2 === null) {
        return true;
    }

    if (root1 === null || root2 === null) {
        return false;
    }

    return (
        root1.val === root2.val &&
        isSameTree(root1.left, root2.left) &&
        isSameTree(root1.right, root2.right)
    );
}
```

### Core Recursive Pattern for Trees

Almost every tree recursion follows this structure:

```javascript
function solve(root) {
    // Base case
    if (root === null) {
        return something;
    }

    // Solve left subtree
    let left = solve(root.left);

    // Solve right subtree
    let right = solve(root.right);

    // Combine results
    return answer;
}
```

**BST (Binary Search Tree)** and **AVL Tree** with their common **competitive programming (CP) patterns** and recursive implementations.

# 1. Binary Search Tree (BST)

### BST Property

For every node:

```
        root
       /    \
   smaller  larger
```

* Left subtree values `< root`
* Right subtree values `> root`

Example:

```
          8
        /   \
       3     10
      / \      \
     1   6      14
        / \
       4   7
```

## BST Node

```javascript
class Node {
    constructor(val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}
```

# Pattern 1: Insert in BST

### Recursive

```javascript
function insert(root, val) {
    if (root === null) {
        return new Node(val);
    }

    if (val < root.val) {
        root.left = insert(root.left, val);
    } else {
        root.right = insert(root.right, val);
    }

    return root;
}
```

Usage:

```javascript
let root = null;

[8,3,10,1,6,14].forEach(x => {
    root = insert(root, x);
});
```

# Pattern 2: Search in BST

```javascript
function search(root, val) {
    if (root === null) return false;

    if (root.val === val) {
        return true;
    }

    if (val < root.val) {
        return search(root.left, val);
    }

    return search(root.right, val);
}
```

Time:

```
Average: O(log n)
Worst:   O(n)
```

# Pattern 3: Find Minimum

```javascript
function findMin(root) {
    if (root.left === null) {
        return root;
    }

    return findMin(root.left);
}
```

# Pattern 4: Delete Node in BST

This is a very common CP question.

Cases:

### Case 1:

Node has no child

```
10

delete 10

null
```

### Case 2:

One child

```
10
 \
 15

delete 10

15
```

### Case 3:

Two children

Replace with inorder successor.

```javascript
function deleteNode(root, key) {

    if (root === null) return null;


    if (key < root.val) {
        root.left = deleteNode(root.left, key);
    }

    else if (key > root.val) {
        root.right = deleteNode(root.right, key);
    }

    else {

        // no child
        if (root.left === null)
            return root.right;

        if (root.right === null)
            return root.left;


        // two children
        let successor = findMin(root.right);

        root.val = successor.val;

        root.right = deleteNode(
            root.right,
            successor.val
        );
    }

    return root;
}
```

# Pattern 5: Validate BST

Given a tree, check if it is BST.

Wrong approach:

```
only compare child nodes ❌
```

Correct:

Maintain range.

```javascript
function isBST(root, min=-Infinity, max=Infinity){

    if(root===null)
        return true;


    if(root.val <= min || root.val >= max)
        return false;


    return isBST(
        root.left,
        min,
        root.val
    )
    &&
    isBST(
        root.right,
        root.val,
        max
    );
}
```

# AVL TREE

AVL = Self balancing BST

After every insertion:

```
balance factor = height(left)-height(right)
```

Allowed:

```
-1,0,1
```

If imbalance happens → rotation.

# AVL Node

```javascript
class AVLNode {

    constructor(val){
        this.val = val;
        this.left = null;
        this.right = null;
        this.height = 1;
    }
}
```

# Height Function

```javascript
function height(node){
    if(node===null)
        return 0;

    return node.height;
}
```

# Balance Factor

```javascript
function balance(node){

    if(node===null)
        return 0;

    return height(node.left)
          -
          height(node.right);
}
```

# Rotation Patterns

There are only 4 cases in AVL.

## LL Case

```
       30
      /
     20
    /
   10
```

Right rotation.

```javascript
function rightRotate(y){

    let x = y.left;
    let T = x.right;


    x.right = y;
    y.left = T;


    y.height =
    1 + Math.max(
        height(y.left),
        height(y.right)
    );


    x.height =
    1 + Math.max(
        height(x.left),
        height(x.right)
    );


    return x;
}
```

## RR Case

```
10
 \
 20
   \
    30
```

Left rotation.

```javascript
function leftRotate(x){

    let y=x.right;
    let T=y.left;


    y.left=x;
    x.right=T;


    x.height =
    1+Math.max(
        height(x.left),
        height(x.right)
    );


    y.height =
    1+Math.max(
        height(y.left),
        height(y.right)
    );


    return y;
}
```

# AVL Insert Pattern

This is the main CP template:

```javascript
function insertAVL(root,val){

    // normal BST insert
    if(root===null)
        return new AVLNode(val);


    if(val < root.val)
        root.left = insertAVL(root.left,val);

    else
        root.right = insertAVL(root.right,val);



    // update height
    root.height =
    1 + Math.max(
        height(root.left),
        height(root.right)
    );



    // check balance
    let bf = balance(root);



    // LL
    if(bf > 1 && val < root.left.val)
        return rightRotate(root);



    // RR
    if(bf < -1 && val > root.right.val)
        return leftRotate(root);



    // LR
    if(bf > 1 && val > root.left.val){
        root.left =
            leftRotate(root.left);

        return rightRotate(root);
    }



    // RL
    if(bf < -1 && val < root.right.val){

        root.right =
            rightRotate(root.right);

        return leftRotate(root);
    }


    return root;
}
```

# CP Pattern Summary

| Problem      | Pattern                   |
| ------------ | ------------------------- |
| Search BST   | Compare and go left/right |
| Insert BST   | Recursive placement       |
| Delete BST   | 3 cases                   |
| Validate BST | Range recursion           |
| Kth smallest | Inorder traversal         |
| LCA in BST   | Value comparison          |
| AVL insert   | BST insert + balance      |
| AVL balance  | Height difference         |
| AVL fix      | 4 rotations               |
