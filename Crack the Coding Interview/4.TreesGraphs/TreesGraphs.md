# Trees and Graphs

+ Tree Traversal

    + In order traversal

    + Pre order traversal

    + Post order traversal
  
    + Level order traversal

+ Binary Heaps(Min-Heaps and Max-Heaps)

+ Tries (Prefix Trees)

+ Graphs

+ Grapsh Search

+ Depth-first Search

+ Breadth-first Search

# Question 2: Minimal tree : Build BST from sorted array

Given a sorted(increasing order) array with unique integer elements, write an algorithm to create a binary search tree with minimal height

## Solution:

My thoughts:

```
arr1 = [1,2,3,4,5,6,7,8]
arr2 = [1,2,3,4,5,6,7,8,9]
just use array.length / 2
mid-point = 5,
mid-point = 5
```

TreeNode root = new TreeNode()
root.val = arr1.midPoint

root.left = sortedArrayToBST(arr1[0,array.length/2])

root.right = sortedArrayToBST(arr1[array.length/2+1, array.length -1])

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
 class Solution {
     public TreeNode sortedArrayToBST(int[] nums) {
         if (nums == null || nums.length == 0) return null;

         // Call helper function
         return buildBST(nums, 0, nums.length - 1);
     }
     /** Helper function for building BST
     * array, start index, end index
     */
     private TreeNode buildBST(int[] nums, int start, int end) {
         TreeNode root = new TreeNode();
         int mid = (left + right) / 2;

         root.val = nums[mid];
         root.left = buildBST(nums, start, mid - 1);
         root.right = buildBST(nums, mid + 1, end)
         
     }
 }
 ```


## Question 3: List of Depths: Level order Traversal

Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth.

### Solution 1 Recursion:

My thoughts:

This is basically breadth first search, for each depth, put all the nodes into an List.

In the result list, its index is precisely its depth.

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();

        levelOrderHelper(root, 0, result);
        return result
    }

    private levelOrderHelper(TreeNode root, int depth, List<List<Integer>> result) {
        if (root == null) return;

        if (depth == result.size()) {
            result.add(new ArrayList<Integer>());
        }

        result.get(depth).add(root.val);

        levelOrderHelper(root.left, depth + 1, result);
        levelOrderHelper(root.right, depth + 1, result);
    }
}
```

### Solution 2 : Use a queue

Algorithm:

1. Initialize a ArrayList result, a LinkedList queue.

2. queue.add(root)

3. While queue is not empty:

    + Initialze a list for this depth(level)
    + number of elements in this level = size of the queue
    + from i = 0 to number of elements in this level:

        + Take a treeNode node from the queue
        + Add node.val to level
        + add node.left, node.right to the end of the queue
    
    + add this level to result list

4. Return result

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();

        if (root == null) return result;

        Queue<TreeNode> queue = new LinkedList<>();

        queue.add(root);

        while (!queue.isEmpty()) {
            List<Integer> depth = new ArrayList<>();
            int cnt = queue.size();

            for (int i = 0; i < cnt; i++) {
                TreeNode node = queue.poll();      // E .poll(): Retrives and removes the head of the queue. 
                depth.add(node.val);
                
                if (node.left != null)  queue.add(node.left);
                if (node.right != null) queue.add(node.right);
            }
            result.add(depth);
        }
        return result;
    }
}
```

## Question 4: Check Balanced

Implement a function to check if a binary tree is balanced.

Balanced tree: a tree such that the heights of the two subtrees of any node never differ by more than one.

### Solution: Use definition

Use recursion, we recurse through the entire tree, and for each node, compute the heights of each tree.

```java
private int height(TreeNode n) {
    if (n == null) return -1;
    return Math.max(height(n.left), height(n.right)) + 1;
}

public boolean isBalanced(TreeNode root) {
    if (root == null) return true;

    int difference = height(root.left) - height(root.right);

    return Math.abs(abs) <=1 && isBalanced(root.left) && isBalanced(root.right);
}
```

### Solution 2: 

Improve upon the last solution


## Question 5: Validate BST

Implement a function to check if a binary tree is a binary search tree.

[leetcode solution](https://leetcode.com/problems/validate-binary-search-tree/solution/)

My thoughts:

Just do definition

This is wrong
```java
public boolean isValidBST(TreeNode root) {
    if (root == null) return true;
    return root.left.val < root.val && root.right.val > x && isValidBST(root.left) && isValidBST(root.right);
}
```


### Solution 1: Recursion

Take right side for example, a node not only has to be greater than its parent, it also has to be greater than everything to its right, less than everything to its left.

We need to keep a lower and upper bound for each node.

```java
public boolean isValidBST(TreeNode root) {
    int upperBound = Math.MAX_VALUE;
    int lowerBound = Math.MIN_VALUE;

    return isValidBST(root, upperBound, lowerBound);
}

private boolean isValidBST(TreeNode root, int lower, int upper) {
    if (node == null) return true;

    int val = node.val;

    if (lower != null && val <= lower) return false;
    if (upper != null && val >= upper) return false;

    // for right child, replace its lower-bound with node.val
    if (!isValidBST(node, val, upper)) return false;
    // for left child, replace its upper-bound with node.val
    is (!isValidBST(node, lower, val)) return false;

    return true;
}
```

### Solution 2: Iteration

Convert the above algorithm in to iteration.

```java
class Solution {
    List<TreeNode> stack = new LinkedList();
    List<Integer> uppers = new LinkedList();
    List<Integer> lowers = new LinkedList();

    public void update(TreeNode root, Integer lower, Integer upper) {
        stack.add(root);
        lowers.add(lower);
        uppers.add(upper);
    }

    public boolean isValidBST(TreeNode root) {
        Integer lower = null, upper = null, val;
        update(root, lower, null);

        while (!stack.isEmpty()) {
            root = stack.poll();
            lower = lowers.poll();
            upper = uppers.poll();

            if (root == null) continue;
            val = root.val;
            if (lower != null && val <= lower) return false;
            if (upper != null && val >= upper) return false;
            update(root.right, val, upper);
            update(root.left, lower, val);
        }
        return true;
    }
}
```

### Solution 3: In-order traversal

In-order traversal of a BST would give us a sorted array.

So every element is smaller than previous element for a BST in order traversal.

```java
class Solution {
    public boolean inValidBST(TreeNode root) {
        Stack<TreeNode> stack = new Stack();
        double inorder = - Double.MAX_VALUE;

        while (!stack.isEmpty() || root != null) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            if (root.val <= inorder) return false;
            inorder = root.val;
            root = root.right;
        } 
    }
}
```

## Question 6: Successor

Write an algorithm to find the "next" node (i.e. in-order successor) of a given node in a binary search tree. You many assume that each node has a link to its parent.

[Leetcode question](https://leetcode.com/problems/inorder-successor-in-bst/)



