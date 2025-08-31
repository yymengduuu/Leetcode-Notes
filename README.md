代码随想录算法训练营第二十五天

#### 🔹Question : Leetcode_



**Key Points**

```

```


#### 🔹Question : Leetcode_



**Key Points**

```

```


#### 🔹Question : Leetcode_



**Key Points**

```

```


#### 🔹Question : Leetcode_



**Key Points**

```

```


#### 🔹Question : Leetcode_



**Key Points**

```

```


#### 🔹Question : Leetcode_



**Key Points**

```

```










# 回溯算法

```
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

## 组合（combination）

对于组合问题，

- 如果是一个集合来求组合的话，就需要startIndex
- 如果是多个集合取组合，各个集合之间相互不影响，那么就不用startIndex

### Related Questions

#### 🔹Question 组合: Leetcode_77

Given two integers n and k, return all possible combinations of k numbers chosen from the range [1, n].

You may return the answer in any order.

```
var combine = function(n, k) {
    let res = [], path = [];
    const backtracking = function(n, k, startIndex) {
        if(path.length == k) {
            res.push(path.slice());
            return;
        }
        for(let i = startIndex; i <= n; i++ ) {
            path.push(i);
            backtracking(n, k, i + 1);
            path.pop();
        }
    }
    backtracking(n, k, 1);
    return res;
};
```

#### 🔹Question 组合总和III: Leetcode_216

Find all valid combinations of k numbers that sum up to n such that the following conditions are true:

Only numbers 1 through 9 are used.
Each number is used at most once.
Return a list of all possible valid combinations. The list must not contain the same combination twice, and the combinations may be returned in any order.

```
var combinationSum3 = function(k, n) {
    let res = [], path = [], sum = 0;
    const backtracking = function(k, n, startIndex) {
        if(path.length == k && sum == n){
            res.push(path.slice());
            return;
        }
        for(let i = startIndex; i <= 9; i++) {
            sum += i;
            path.push(i);
            backtracking(k, n, i + 1);
            sum -= i;
            path.pop();
        }
    }
    backtracking(k, n, 1);
    return res;
};
```

#### 🔹Question 电话号码的字母组合: Leetcode_17

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

```
var letterCombinations = function(digits) {
    if(!digits || digits.length === 0) return [];
    const phoneMap = [
        '',
        '',
        'abc',
        'def',
        'ghi',
        'jkl',
        'mno',
        'pqrs',
        'tuv',
        'wxyz',
    ]
    // 哈希问题，但是这道题用数组比哈希表简单，因为数组的index可以直接用来替代电话键1-9

    let res = [];
    const backtracking = function(d, startIndex, path) {
        if(path.length === d.length) {
            res.push(path);
            return;
        }
        
        let letter = phoneMap[d[startIndex]];
        if(!letter) return;

        for (let i of letter) {
            backtracking(d, startIndex + 1, path + i);
        }
    }
    backtracking(digits, 0, '');
    return res;
};
```

#### 🔹Question 组合总和: Leetcode_39

Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

**Key Points**

类似于前面的77， 唯一不同在于这次可以反复利用数组中的数字，所以回溯时不需i + 1；

```
var combinationSum = function(candidates, target) {
    let res = [], path = [], sum = 0;
    const backtracking = function(nums, target, startIndex) {
        if(sum == target) {
            res.push(path.slice());
            return;
        }
        if(sum > target) return;
        for(let i = startIndex; i < nums.length; i++) {
            sum += nums[i];
            path.push(nums[i]);
            backtracking(nums, target, i);
            sum -= nums[i];
            path.pop();
        }
    }
    backtracking(candidates, target, 0);
    return res;
};
```

#### 🔹Question 组合总和II: Leetcode_40

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target.

Each number in candidates may only be used once in the combination.

Note: The solution set must not contain duplicate combinations.

**Key Points**

类似于前面的39， 唯一不同在于同一树层上的“使用过”的数组需要去重，所以需要对数组排序并给出控制条件；

```
var combinationSum2 = function(candidates, target) {
    let res = [], path = [], sum = 0;
    let arr = candidates.sort((a,b) => a - b);
    const backtracking = function(arr, target, startIndex) {
        if(sum == target) {
            res.push(path.slice());
            return;
        }
        if(sum > target) return;
        for(let i = startIndex; i < arr.length; i++) {
            if (i > startIndex && arr[i] === arr[i-1]) continue;
            sum += arr[i];
            path.push(arr[i]);
            backtracking(arr, target, i + 1);
            sum -= arr[i];
            path.pop();
        }
    }
    backtracking(candidates, target, 0);
    return res;
};
```

---

## 分割（partitioning）

### Related Questions

#### 🔹Question 分割回文串: Leetcode_131

Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

**Key Points**

总体回溯思路一致，难点在于：

- 回溯中返回的标准是判断回文，因此需要一个单独的函数检查是否回文；
- 回溯的终止条件是startIndex >= s.length，因为切割最多就是切出s长度，不可能比这个还多；

```
var partition = function(s) {
    let res = [], path = [];

    const backtracking = function(s, startIndex) {
        if(startIndex >= s.length){
            res.push(path.slice());
            return;
        }
        for(let i = startIndex; i < s.length; i++) {
            if(isPalindrome(s, startIndex, i)){
                let strings = s.slice(startIndex, i + 1);
                // slice 的右端不含当前位，想要包含 i，应写 i + 1
                path.push(strings);
            } else {
                continue;
            }
            backtracking(s, i + 1);
            path.pop();
        }
    }

    const isPalindrome = function(s, left, right) {
        while(left < right) {
            if(s[left] !== s[right]) return false;
            left++;
            right--;
        }
        return true;
    }

    backtracking(s, 0);
    return res;
};
```

#### 🔹Question 分割回文串: Leetcode_93

A valid IP address consists of exactly four integers separated by single dots. Each integer is between 0 and 255 (inclusive) and cannot have leading zeros.

For example, "0.1.2.201" and "192.168.1.1" are valid IP addresses, but "0.011.255.245", "192.168.1.312" and "192.168@1.1" are invalid IP addresses.
Given a string s containing only digits, return all possible valid IP addresses that can be formed by inserting dots into s. You are not allowed to reorder or remove any digits in s. You may return the valid IP addresses in any order.

**Key Points**

```
var restoreIpAddresses = function(s) {
    let res = [], path = [];
    const backtracking = function(s, startIndex) {
        // condition 1: 分割为四段
        if(path.length === 4){
            // condition 2：必须刚好用完所有字符
            if(startIndex === s.length) {
            res.push(path.join('.'));
            return;
            }
        }
        for(let i = startIndex; i < s.length; i++ ) {
            if (!isValid(s, startIndex, i)) break;
            let string = s.slice(startIndex, i + 1);
            path.push(string);
            backtracking(s, i + 1);
            path.pop();
        }
    }

    // Condition 2: 
    const isValid = function(s, start, end){
        // 起点大于终点，说明区间无效
        if(start > end) return false;

        // 段位以0为开头的数字不合法
        if(s[start] == '0' && start !== end) return false;

        // 段位如果大于255,有非正整数字符不合法
        let num = Number(s.slice(start, end + 1));
        if(isNaN(num) || num > 255) return false;

        return true;
    }

    backtracking(s, 0);
    return res;
};
```

---

## 子集（subsets）

### Related Questions

#### 🔹Question 子集: Leetcode_78

Given an integer array nums of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

**Key Points**

无序，取过的元素不会重复取，写回溯算法的时候，for就要从startIndex开始，而不是从0开始！

```
var subsets = function(nums) {
    let res = [], path = [];
    const backtracking = function(s, startIndex){
        res.push([...path]);
        for(let i = startIndex; i < s.length; i++) {
            path.push(s[i]);
            backtracking(s, i + 1);
            path.pop();
        }
        
    }
    backtracking(nums, 0);
    return res;
};
```


#### 🔹Question 子集II: Leetcode_90

Given an integer array nums that may contain duplicates, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

**Key Points**

- 这道题目和78.子集 区别就是集合里有重复元素了，而且求取的子集要去重
- 去重想nums[i - 1] === s[i] 的情况


```
var subsetsWithDup = function(nums) {
    let res = [], path = [];
    let s = nums.sort((a,b) => a - b);
    const backtracking = function(s, startIndex) {
        res.push([...path]);
        for(let i = startIndex; i < s.length; i++) {
        if(s[i - 1] === s[i] && i > startIndex) continue;
        path.push(s[i]);
        backtracking(s, i + 1);
        path.pop();      
        }
    }
    backtracking(s, 0);
    return res;
};
```
---

## 排列（permutations）

- 每层都是从0开始搜索而不是startIndex
- 需要used数组记录path里都放了哪些元素了

### Related Questions

#### 🔹Question 全排列: Leetcode_46

Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

**Key Points**

```
var permute = function(nums) {
    const res = [], path = [];
    const backtracking = function(nums, l, used) {
        if(path.length === l) {
            res.push(path.slice());
            return;
        }
        for(let i = 0; i < l; i++) {
            if(used[i]) continue;
            // 这个元素在这一条递归分支里已经被选过了，之后不能再选
            used[i] = true;
            path.push(nums[i]);
            backtracking(nums, l, used);
            path.pop();
            // 回溯到上一层后，再把 used[i]恢复，以便后续其他分支还可以使用这个数字
            used[i] = false;
        }
    }
    backtracking(nums, nums.length, []);
    return res;
};
```


#### 🔹Question 全排列 II: Leetcode_47

Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

**Key Points**

和46.全排列 (opens new window)的区别在与给定一个可包含重复数字的序列，要返回所有不重复的全排列。

```
var permuteUnique = function(nums) {
    let res = [], path = [];
    let s = nums.sort((a,b) => a - b);
    const backtracking = function(s, l, used) {
        if(path.length === l) {
            res.push(path.slice());
            return;
        }
        for(let i = 0; i < l; i++) {
            
            // 同一层，不允许从一组相等数字里重复“开头”；但如果前一个相同值在更高层已经被选用（used[i-1]===true），那当前这个相同值可以在下一层继续用。
            if(i > 0 && s[i] === s [i - 1] && !used[i - 1]) continue;
            // 这个元素在这一条递归分支里已经被选过了，之后不能再选
            if(used[i]) continue;
            used[i] = true;
            path.push(s[i]);
            backtracking(s, l, used);
            path.pop();
            // 回溯到上一层后，再把 used[i]恢复，以便后续其他分支还可以使用这个数字
            used[i] = false;
        }
    }
    backtracking(s, s.length, []);
    return res;
};
```

---

## 子序列 (subsequence)

### Related Questions

#### 🔹Question 递增子序列: Leetcode_491

Given an integer array nums, return all the different possible non-decreasing subsequences of the given array with at least two elements. You may return the answer in any order.

**Key Points**

需要和90子集进行重点区分，区别在于

- 子集不讲顺序只关心元素是否被选到
- 子序列元素的顺序必须和原序列元素顺序相同，例如原数组：nums = [4, 6, 7, 7, 3]，子序列不能出现 [3, 4, 6, 7, 7, ]

```
var findSubsequences = function(nums) {
    let res = [], path = [];

    const backtracking = function(nums, startIndex) {
        // 子序列至少由两个数字组成
        if(path.length > 1) {
            res.push(path.slice());
        }

        const used = new Set();
        // 这一层用过的值

        for(let i = startIndex; i < nums.length; i++){
            // 保证递增，不用nums[i] === nums[i - 1] 因为只在数组已经排序时才成立
            if(path.length > 0 && nums[i] < path[path.length-1]) continue;
            // 本层已经用过这个值，跳过
            if (used.has(nums[i])) continue;

            used.add(nums[i]);
            path.push(nums[i]);
            backtracking(nums, i + 1);
            path.pop();
        }
    }
    backtracking(nums, 0);
    return res;
};
```
---

# Binary Tree

## 二叉树

### 二叉树的属性

#### 🔹Question 完全二叉树的节点个数: Leetcode_222

Given the root of a complete binary tree, return the number of the nodes in the tree.
According to Wikipedia, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.
Design an algorithm that runs in less than O(n) time complexity.

#### Method1: DFS递归

```
var countNodes = function(root) {
    const getNodeSum = function(node) {
        if(!node) return 0;       
        let leftSum = getNodeSum(node.left);
        let rightSum = getNodeSum(node.right);
        return leftSum + rightSum + 1;
    }
    return getNodeSum(root);
};
```

#### Method2: BFS层序

```
var countNodes = function(root) {
    if(!root) return 0;
    let queue = [root];
    let sum = 0;
    while(queue.length) {
        let size = queue.length;
        for (let i = 0; i < size; i++) {
            let node = queue.pop();
            sum += 1;
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
    }  
    return sum; 
};
```

#### 🔹Question 路径总和: Leetcode_112

Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.
A leaf is a node with no children.

#### Method1: DFS递归

```
var hasPathSum = function(root, targetSum) {
    
    const travelsal = function(node,count) {
        if(count === 0 && !node.left && !node.right) return true; 
        // 在叶子节点判断是否满足条件，满足才进入递归
        if(!node.left && !node.right) return false; 
        // 到叶子但没凑够条件，终止

        if(node.left && travelsal(node.left, count - node.left.val)) return true;
        if(node.right && travelsal(node.right, count - node.right.val)) return true;
        return false;
        // 递归的兜底出口，保证所有路径都走完没找到，就明确告诉上层没有符合条件的路径
    }
    if(!root) return false;
    return travelsal(root, targetSum - root.val);
};
```

#### Method2: BFS迭代

```
var hasPathSum = function(root, targetSum) {
    if(!root) return false;
    let queue = [root];
    let valueQ = [0];
    while(queue.length) {
        let node = queue.shift();
        let val = valueQ.shift();
        val += node.val;
        if (!node.left && !node.right && targetSum === val) return true;
        if (node.left) {
            queue.push(node.left);
            valueQ.push(val);
        }
        if (node.right) {
            queue.push(node.right);
            valueQ.push(val);
        }
    }
    return false;
};
```

#### 🔹Question 路径总和2: Leetcode_113

Given the root of a binary tree and an integer targetSum, return all root-to-leaf paths where the sum of the node values in the path equals targetSum. Each path should be returned as a list of the node values, not node references.
A root-to-leaf path is a path starting from the root and ending at any leaf node. A leaf is a node with no children.

#### Method1: DFS递归

```
var pathSum = function(root, targetSum) {
    const res = [];
    let path = [];
    const traversal = function(node, cnt) {
        path.push(node.val);
        if(cnt === 0 && !node.left && !node.right){
            res.push([...path]);
            path.pop();
            return;
        }
        if(!node.left && !node.right) {
            path.pop();
            return;
        }
        if(node.left) {
            traversal(node.left, cnt - node.left.val);
        }
        if(node.right) {
            traversal(node.right, cnt - node.right.val);
        }
        path.pop();
        return;
    }
    if(!root) return [];
    traversal(root, targetSum - root.val);
    return res;
};
```

#### Method2: DFS迭代

```
var pathSum = function(root, targetSum) {
    let res = [];
    if(!root) return res;
    let queue = [root];
    let valueQ = [root.val];
    let pathQ = [[root.val]];

    while(queue.length) {
        let node = queue.shift();
        let value = valueQ.shift();
        let path = pathQ.shift();

        if (!node.left && !node.right && targetSum === value) {
            res.push(path);
        }
        if (node.left) {
            queue.push(node.left);
            valueQ.push(value + node.left.val);
            pathQ.push([...path, node.left.val]);
        }
        if (node.right) {
            queue.push(node.right);
            valueQ.push(value + node.right.val);
            pathQ.push([...path, node.right.val]);
        }
    }
    return res;
};
```

#### 🔹Question 对称二叉树: Leetcode_101

Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

**Key Points**

核心是位置和值的对称；

#### Method1: DFS递归

```
var isSymmetric = function(root) {
    if (root === null) return true; 
    const compare = function(left,right){
        if (!left && !right) return true;
        if (!left || !right) return false;
        if (left.val !== right.val) return false;  
        let inSide = compare(left.right,right.left);
        let outSide = compare(left.left, right.right);  
        return inSide && outSide;     
    }
    return compare(root.left, root.right);
};
```

#### Method2: BFS层序

```
var isSymmetric = function(root) {
    if (root === null) return true;
    let queue = [root.left, root.right];
    while (queue.length) {
        
        let left = queue.shift();
        let right = queue.shift();
        if (!left && !right) continue;
        if (!left || !right) return false;
        if (left.val !== right. val) return false;           
        queue.push(left.left, right.right);
        queue.push(left.right, right.left);
    }
    return true;
};
```

#### 🔹Question 最大深度: Leetcode_104

Given the root of a binary tree, return its maximum depth.
A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

#### Method1: DFS递归

```
var maxDepth = function(root) {
    const getDepth = function(node) {
        if (node === null) return 0;
        let leftDepth = getDepth(node.left);
        let rightDepth = getDepth(node.right);
        let depth = Math.max(leftDepth, rightDepth) + 1;
        return depth;
    }
    return getDepth(root);
};
```

#### Method2: BFS层序

```
var maxDepth = function(root) {
    let depth = 0;
    if (root === null) return depth;
    let queue = [root];

    while (queue.length) {
        let size = queue.length;
        while (size--) {
            let node = queue.shift();
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
        depth++;
    }
    return depth;
};
```

#### 🔹Question 最小深度: Leetcode_111

Given a binary tree, find its minimum depth.
The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.
Note: A leaf is a node with no children.

#### Method1: DFS递归

```
var minDepth = function(root) {
    const getDepth = function(node) {
        if (node === null) return 0;
        let leftDepth = getDepth(node.left);
        let rightDepth = getDepth(node.right);
        if (!node.left && node.right) return 1 + rightDepth;
        if (!node.right && node.left) return 1 + leftDepth;
        return 1 + Math.min(leftDepth, rightDepth);
    }
    return getDepth(root);
};
```

#### Method2: BFS层序

```
var minDepth = function(root) {
    let depth = 0;
    if (root === null) return depth;
    let queue = [root];

    while (queue.length) {
        let size = queue.length;
        depth ++;
        while (size--) {
            let node = queue.shift();
            if (node.left === null && node.right === null) {
                return depth;
            }
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
    }
    return depth;
};
```

#### 🔹Question 平衡二叉树: Leetcode_110

Given a binary tree, determine if it is height-balanced(A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.).

**Key Points**

二叉树节点的深度：指从根节点到该节点的最长简单路径边的条数，用前序遍历;二叉树节点的高度：指从该节点到叶子节点的最长简单路径边的条数，用后序遍历。

```
var isBalanced = function(root) {
    const getDepth = function(node){
        if (!node) return 0;
        let leftDepth = getDepth(node.left);
        let rightDepth = getDepth(node.right);
        if (leftDepth === -1) return -1;
        if (rightDepth === -1) return -1;
        if (Math.abs(leftDepth - rightDepth) > 1) {
            return -1;
        } else {
            return 1 + Math.max(leftDepth, rightDepth);
        }
    }
    return getDepth(root) !== -1;
};
```

#### 🔹Question 二叉树的所有路径: Leetcode_257

Given the root of a binary tree, return all root-to-leaf paths in any order.
A leaf is a node with no children.

```
var binaryTreePaths = function(root) {
    let res = [];
    let path = [];
    if (!root) return res;

    const dfs = function(node){
        path.push(node.val);
        if (!node.left && !node.right) {
            res.push(path.join('->'));
        }
        if (node.left) dfs(node.left);
        if (node.right) dfs(node.right);
        path.pop();
    } 
    dfs(root);
    return res;
};
```

#### 🔹Question 左叶子之和: Leetcode_404

Given the root of a binary tree, return the sum of all left leaves.
A leaf is a node with no children. A left leaf is a leaf that is the left child of another node.

```
var sumOfLeftLeaves = function(root) {

    const getLeftLeaf = function(node) {
        if (!node) return 0;
        if (!node.left && !node.right) return 0;

        let sum = 0;
        let leftValue = getLeftLeaf(node.left);
        let rightValue = getLeftLeaf(node.right);
        let midValue = 0;

        if (node.left && !node.left.left && !node.left.right) {
            midValue = node.left.val;
        }
        
        sum = midValue + leftValue + rightValue;
        return sum;
    }
    return getLeftLeaf(root);
};
```
---

## 二叉树的修改与构造

#### 🔹Question 翻转二叉树: Leetcode_226

Given the root of a binary tree, invert the tree, and return its root.

**Key Points**

核心思想就是需要用temp暂时记录root.left或root.right的值，然后进行交换；

#### Method1: DFS递归

```
var invertTree = function(root) {
    if(root === null) return root;
    const tem = root.right;
    root.right = invertTree(root.left);
    root.left = invertTree(tem);
    return root;
};
```

#### Method2: BFS层序

```
var invertTree = function(root) {
    if(root === null) return root;
    let queue = [root];
    while (queue.length) {
        let size = queue.length;
        for (let i = 0; i < size; i++ ) {
            let node = queue.shift();
            let tem = node.left;
            node.left = node.right;
            node.right = tem;
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
    }
    return root;
};
```

#### 🔹Question 构建最大二叉树: Leetcode_654

You are given an integer array nums with no duplicates. A maximum binary tree can be built recursively from nums using the following algorithm:

Create a root node whose value is the maximum value in nums.
Recursively build the left subtree on the subarray prefix to the left of the maximum value.
Recursively build the right subtree on the subarray suffix to the right of the maximum value.
Return the maximum binary tree built from nums.

```
var constructMaximumBinaryTree = function(nums) {
    const buildTree = function(left, right, arr) {
        if(left > right) return null;
        let maxValue = -1;
        let maxIndex = -1;
        for(let i = left; i <= right; i++) {
            if(arr[i] > maxValue) {
                maxValue = arr[i];
                maxIndex = i;
            }
        }

        let root = new TreeNode(maxValue);
        root.left = buildTree(left, maxIndex - 1, arr);
        root.right = buildTree(maxIndex + 1, right, arr);
        return root;
    }
    root = buildTree(0, nums.length - 1, nums);
    return root;
};
```


#### 🔹Question 合并二叉树: Leetcode_617

You are given two binary trees root1 and root2.
Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree.
Return the merged tree.

**Key Points**

遍历是同步进行的，因此只需要分四个情况考虑：

- 没有root1；
- 没有root2；
- 同时没有root1，root2；
- 同时有root1，root2；

```
var mergeTrees = function(root1, root2) {
    const DFS = function(node1, node2) {
        if(!node1) return node2;
        if(!node2) return node1;
        node1.val += node2.val;
        node1.left = DFS(node1.left, node2.left);
        node1.right = DFS(node1.right, node2.right);
        return node1;
    }
    return DFS(root1, root2);
};
```

#### 🔹Question 从中序与后序遍历序列构造二叉树: Leetcode_106

Given two integer arrays inorder and postorder where inorder is the inorder traversal of a binary tree and postorder is the postorder traversal of the same tree, construct and return the binary tree.

**Key Points**

- 第一步：如果数组大小为零的话，说明是空节点了。

- 第二步：如果不为空，那么取后序数组最后一个元素作为节点元素(后序为左右中，最后一个元素一定是root)。

- 第三步：找到后序数组最后一个元素在中序数组的位置，作为切割点

- 第四步：切割中序数组，切成中序左数组和中序右数组 （顺序别搞反了，一定是先切中序数组）

- 第五步：切割后序数组，切成后序左数组和后序右数组

- 第六步：递归处理左区间和右区间


```
var buildTree = function(inorder, postorder) {
    if(!inorder.length) return null;
    const rootVal = postorder.pop();
    const rootIndex = inorder.indexOf(rootVal);
    const root = new TreeNode(rootVal);
    root.left = buildTree(inorder.slice(0, rootIndex), postorder.slice(0, rootIndex));
    root.right = buildTree(inorder.slice(rootIndex + 1), postorder.slice(rootIndex));
    return root;
};
```

#### 🔹Question 从前序与中序遍历序列构造二叉树: Leetcode_105

Given two integer arrays preorder and inorder where preorder is the preorder traversal of a binary tree and inorder is the inorder traversal of the same tree, construct and return the binary tree.

```
var buildTree = function(preorder, inorder) {
    if(!preorder.length) return null;
    const rootVal = preorder.shift();
    const rootIndex = inorder.indexOf(rootVal);
    const root = new TreeNode(rootVal);
    root.left = buildTree(
        preorder.slice(0, rootIndex), 
        inorder.slice(0, rootIndex)       
    );
    root.right = buildTree(
        preorder.slice(rootIndex),   
        inorder.slice(rootIndex + 1)      
    );

    return root;
};
```

### 二叉树公共祖先问题

### Related Questions

#### 🔹Question 二叉树的最近公共祖先: Leetcode_236

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”


```
var lowestCommonAncestor = function(root, p, q) {
    const DFS = function(node, p, q) {
        if(!node || node === p || node === q) return node;
        let left = DFS(node.left, p, q);
        let right = DFS(node.right, p, q);
        if(left && right) return node;
        if(!left) return right;
        if(!right) return left;
    }
    return DFS(root, p, q);
};
```

#### 🔹Question 二叉搜索树的最近公共祖先: Leetcode_235

Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

**Key Points**

类似于236，二叉树的公共祖先。不同点在于可以利用二叉搜索树特性，左树<root，右树>root；

#### Method1: DFS递归

```
var lowestCommonAncestor = function(root, p, q) {
    const DFS = function(node, p, q) {
        if(!node) return;
        if(node.val > p.val && node.val > q.val) {
            return node.left = DFS(node.left, p, q);
        }
        if(node.val < p.val && node.val < q.val) {
            return node.right = DFS(node.right, p, q);
        }
        return node;
    }
    return DFS(root, p, q);
};
```

#### Method2: DFS迭代

```
var lowestCommonAncestor = function(root, p, q) {
    while(root) {
        if(!root) return;
        if(root.val > p.val && root.val > q.val) {
            root = root.left;
        } else if(root.val < p.val && root.val < q.val) {
            root = root.right;
        } else{
            return root;
        }
    }
    return null;
};
```

---

## 二叉搜索树

### 二叉搜索树的属性

#### 🔹Question 二叉搜索树中的搜索: Leetcode_700

You are given the root of a binary search tree (BST) and an integer val.
Find the node in the BST that the node's value equals val and return the subtree rooted with that node. If such a node does not exist, return null.

**Key Points**

二叉搜索树是一个有序树：

- 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
- 它的左、右子树也分别为二叉搜索树

因此历遍时可以利用这个特性，大于目标值左树历遍，小于时历遍右树；

```
var searchBST = function(root, val) {
    const DFS = function(node,val) {
        if(!node || node.val == val) return node;
        if(node.val > val) return DFS(node.left, val);
        if(node.val < val) return DFS(node.right, val);
        return node;
    }
    return DFS(root,val);
};
```

#### 🔹Question 验证二叉搜索树: Leetcode_98

Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:

The left subtree of a node contains only nodes with keys strictly less than the node's key.
The right subtree of a node contains only nodes with keys strictly greater than the node's key.
Both the left and right subtrees must also be binary search trees.

**Key Points**

- 中序是最好的解决方案，因为：左<中<右；
- DFS中序历遍后压栈到数组中，检查数组是否始终保证前一个值小于后一个值；

```
var isValidBST = function(root) {
    let arr = [];
    const DFS = function(node){
        if(node) {
            DFS(node.left);
            arr.push(node.val);
            DFS(node.right);
        }
    }
    DFS(root);
    for(let i = 0; i < arr.length; i++) {
        if(arr[i] <= arr[i - 1]) return false;
    }
    return true;
};
```

#### 🔹Question 二叉搜索树的最小绝对差: Leetcode_530

Given the root of a Binary Search Tree (BST), return the minimum absolute difference between the values of any two different nodes in the tree.

**Key Points**

中序递归，转换为有序数组

```
var getMinimumDifference = function(root) {
    let arr = [];
    const DFS = function(node) {
        if(!node) return;
        if(node) {
            DFS(node.left);
            arr.push(node.val);
            DFS(node.right);
        }
    }
    DFS(root);

    let minDiff = Infinity;
    for(let i = 1; i < arr.length; i++) {
        minDiff = Math.min(minDiff, arr[i] - arr[i - 1]);
    }
    return minDiff;
    }
    
```

#### 🔹Question 二叉搜索树中的众数: Leetcode_501

Given the root of a binary search tree (BST) with duplicates, return all the mode(s) (i.e., the most frequently occurred element) in it.

If the tree has more than one mode, return them in any order.

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than or equal to the node's key.
The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
Both the left and right subtrees must also be binary search trees.

```
var findMode = function(root) {
    let res = [];
    let count = 0, maxCount = 1;
    let pre = null;
    const DFS = function(node) {
        if(!node) return;
        DFS(node.left); //左

        if(pre && pre.val === node.val) {
            count++;
        } else {
            count = 1;
        }
        pre = node;

        if(count === maxCount){
            res.push(node.val);
        } else if(count > maxCount) {
            maxCount = count;
            res = [node.val];
        }
        
        DFS(node.right); //右
    }
    DFS(root);
    return res;
};
```

#### 🔹Question 把二叉搜索树转换为累加树: Leetcode_538

Given the root of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus the sum of all keys greater than the original key in BST.

As a reminder, a binary search tree is a tree that satisfies these constraints:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

**Key Points**

反中序遍历得到降序数组，实现累加比当前大的数


```
var convertBST = function(root) {
    let pre = 0;
    const DFS = function(node) {
        if(!node) return null;
        DFS(node.right);
        node.val += pre;
        pre = node.val;
        DFS(node.left);
        return node;
    }

    return DFS(root);
};
```

---


### 二叉搜索树的修改与构造

#### 🔹Question 将有序数组转换为二叉搜索树: Leetcode_108

Given an integer array nums where the elements are sorted in ascending order, convert it to a height-balanced binary search tree.

**Key Points**

分割点就是数组中间位置的节点,分割点左边为左树，右边为右树；


```
var sortedArrayToBST = function(nums) {
    const DFS = function(nums, left, right) {
        if(left > right) return null;
        let mid = Math.floor((left + right) / 2);
        const root = new TreeNode(nums[mid]);
        root.left = DFS(nums, left, mid - 1);
        root.right = DFS(nums, mid + 1, right);
        return root;
    }
    return DFS(nums, 0, nums.length - 1);
};
```

#### 🔹Question 二叉搜索树中的插入操作: Leetcode_701

You are given the root node of a binary search tree (BST) and a value to insert into the tree. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.

Notice that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.


```
var insertIntoBST = function(root, val) {
    const DFS = function(node, val) {
        if (node === null) {
            // 在 null 的地方创建新节点
            let node = new TreeNode(val);
            return node;
            }
        if(node.val > val) {
            node.left = DFS(node.left, val);
        }
        if(node.val < val) {
            node.right = DFS(node.right, val);
        }
        return node;
    }
    return DFS(root, val);
};
```


#### 🔹Question 删除二叉搜索树中的节点: Leetcode_450

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

Search for a node to remove.
If the node is found, delete the node.

**Key Points**

有以下五种情况：

- 第一种情况：没找到删除的节点，遍历到空节点直接返回了
找到删除的节点
- 第二种情况：左右孩子都为空（叶子节点），直接删除节点， 返回NULL为根节点
- 第三种情况：删除节点的左孩子为空，右孩子不为空，删除节点，右孩子补位，返回右孩子为根节点
- 第四种情况：删除节点的右孩子为空，左孩子不为空，删除节点，左孩子补位，返回左孩子为根节点
- 第五种情况：左右孩子节点都不为空，则将删除节点的左子树头结点（左孩子）放到删除节点的右子树的最左面节点的左孩子上，返回删除节点右孩子为新的根节点。

#### Method1: DFS递归

```
var deleteNode = function(root, key) {
    const DFS = function(node, key) {
        // 情况1
        if(!node) return null;
        // 情况2
        if(node.val > key) {
            node.left = DFS(node.left, key);
            return node;
        } else if(node.val < key) {
            node.right = DFS(node.right, key);
            return node;
        } else {
            // 情况3
            if(!node.left && !node.right) {
                return null;
                // 情况4
                } else if(!node.left) {
                    return node.right;
                } else if(!node.right){
                    return node.left;
                // 情况5
                } else {
                    let rightNode = node.right;
                    while(rightNode.left){
                        rightNode = rightNode.left;
                    }
                    node.val = rightNode.val;
                    node.right = DFS(node.right, rightNode.val)
                    return node; 
                }
        }
    }
    return DFS(root, key);
};
```


#### 🔹Question 修剪二叉搜索树: Leetcode_669

Given the root of a binary search tree and the lowest and highest boundaries as low and high, trim the tree so that all its elements lies in [low, high]. Trimming the tree should not change the relative structure of the elements that will remain in the tree (i.e., any node's descendant should remain a descendant). It can be proven that there is a unique answer.

Return the root of the trimmed binary search tree. Note that the root may change depending on the given bounds.

**Key Points**

利用BST的有序性：左子树所有值 < root.val，右子树所有值 > root.val。

```
var trimBST = function(root, low, high) {
    const DFS = function(node, low, high) {
        if(!node) return null;
        if(node.val < low) {
            return DFS(node.right, low, high);
        }
        if(node.val > high) {
            return DFS(node.left, low, high);
        }
        node.left = DFS(node.left, low, high);
        node.right = DFS(node.right, low, high);
        return node;
    }
    return DFS(root, low, high);
};
```

---

## BFS（Breadth First Search，广度优先搜索）

### Related Questions

#### 🔹Question 1: Leetcode_102

Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

```
var levelOrder = function(root) {
    let res = [];
    if (root === null) return res;
    let queue = [root];
    
    while (queue.length) {
        let cur = [];
        let size = queue.length;
        for (let i = 0; i < size; i++) {
            let node = queue.shift();
            cur.push(node.val);
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
        res.push(cur);
    }
    return res;
};
```

#### 🔹Question 2: Leetcode_107 (102的反转）

Given the root of a binary tree, return the bottom-up level order traversal of its nodes' values. (i.e., from left to right, level by level from leaf to root).

```
var levelOrderBottom = function(root) {
    let res = [];
    if (root === null) return res;
    let queue = [root];

    while (queue.length) {
        let size = queue.length;
        let cur = [];
        for (let i = 0; i < size; i++) {
            let node = queue.shift();
            cur.push(node.val);
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
        res.push(cur);
    }
    return res.reverse();
};
```

#### 🔹Question 3: Leetcode_199

Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

**Key Points**

只需要每一层最后的那个数字，所以决定的关键因素是当i达到length-1；

```
var rightSideView = function(root) {
    let res = [];
    if (root === null) return res;
    let queue = [root];

    while (queue.length) {
        let size = queue.length;

        for (let i = 0; i < size; i++ ){
            let node = queue.shift();
            if (i === size - 1) {
                res.push(node.val);
            }
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
    }
    return res;
};
```

#### 🔹Question 4: Leetcode_637

Given the root of a binary tree, return the average value of the nodes on each level in the form of an array. Answers within 10-5 of the actual answer will be accepted.

**Key Points**

只需要累加每一层所有数字，每历遍一次后再用累加的和除以该层的长度；

```
var averageOfLevels = function(root) {
    let res = [];
    let queue = [root];

    while (queue.length) {
        let size = queue.length;
        let cur = [];
        let sum = 0;
        for (let i = 0; i < size; i++ ) {
            let node = queue.shift();
            cur.push(node.val);
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
            sum += node.val;
        }
        let ave = sum / size;
        res.push(ave);
    }
    return res;
};
```

#### 🔹Question 5: Leetcode_429

Given an n-ary tree, return the level order traversal of its nodes' values.
Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).

**Key Points**

每一层可能有两个以上，用node.children替代left/right；

```
var levelOrder = function(root) {
    let res = [];
    if (root === null) return res;
    let queue = [root];
    
    while (queue.length) {
        let cur = [];
        let size = queue.length;
        while (size--) {
            let node = queue.shift();
            cur.push(node.val);
            for (let i of node.children) {
                i && queue.push(i);
            }
        }
        res.push(cur);
    }
    return res;
};
```

#### 🔹Question 6: Leetcode_515

Given the root of a binary tree, return an array of the largest value in each row of the tree (0-indexed).

**Key Points**

每一次历遍都比较记录最大值，每一层历遍后push最大值；

```
var largestValues = function(root) {
    let res = [];
    if (root === null) return res;
    let queue = [root];
    while (queue.length) {
        let size = queue.length;
        let cur = -Infinity;
        while (size--) {
            let node = queue.shift();
            cur = Math.max(cur, node.val)
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
        res.push(cur);
    }
    return res;
};
```

#### 🔹Question 7: Leetcode_116

You are given a perfect binary tree where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

```
var connect = function(root) {
    if (root === null) return root;
    let queue = [root];

    while (queue.length) {
        let size = queue.length;
        for (let i = 0; i < size; i++ ) {
            let node = queue.shift();
            if (i < size - 1) {
                node.next = queue[0];
            }
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
    }
    return root;
};
```

#### 🔹Question 8: Leetcode_117(与116的完整二叉树没有区别)

Given a binary tree

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

```
var connect = function(root) {
    if (root === null) return root;
    let queue = [root];

    while (queue.length) {
        let size = queue.length;
        for (let i = 0; i < size; i++ ) {
            let node = queue.shift();
            if (i < size - 1) {
                node.next = queue[0];
            }
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
    }
    return root;
};
```

#### 🔹Question 9: Leetcode_104

Given the root of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

```
var maxDepth = function(root) {
    let depth = 0;
    if (root === null) return depth;
    let queue = [root];

    while (queue.length) {
        let size = queue.length;
        while (size--) {
            let node = queue.shift();
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
        depth++;
    }
    return depth;
};
```

#### 🔹Question 10: Leetcode_111

Given a binary tree, find its minimum depth.
The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.
Note: A leaf is a node with no children.

```
var minDepth = function(root) {
    let depth = 0;
    if (root === null) return depth;
    let queue = [root];

    while (queue.length) {
        let size = queue.length;
        depth ++;
        while (size--) {
            let node = queue.shift();
            if (node.left === null && node.right === null) {
                return depth;
            }
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
    }
    return depth;
};
```

---

## DFS（Depth First Search，深度优先搜索）

**Key Points**

- DFS本质是调用栈，函数调用（call） 本质就是一次「压栈」动作

- Pre-order: 根 → 左子树 → 右子树
  
1. 先访问根，所以常用于：复制树、输出树结构
2. 比如表达式树，前序得到 前缀表达式。

- In-order: 左子树 → 根 → 右子树
  
1. 在二叉搜索树 (BST) 里，中序遍历的结果是升序排列
2. 所以经常用来：检查 BST 正确性、输出有序序列。

- Postorder: 左子树 → 右子树 → 根

1. 根在最后，常用于：删除 / 释放树、计算子树结果后再处理父节点
2. 在表达式树里，后序得到 后缀表达式 (逆波兰表达式)。


### Related Questions

#### 🔹Question 1: Leetcode_144

Given the root of a binary tree, return the **preorder** traversal of its nodes' values.

#### Method1: DFS递归

```
var preorderTraversal = function(root) {
    let ans = [];
    const dfs = function(root){
    // dfs: Depth-First Search
        if(root === null) return;
        ans.push(root.val);
        dfs(root.left); //压栈
        dfs(root.right); // 压栈
    };
    dfs(root);
    return ans;
};
```

#### Method2: DFS迭代

```
var preorderTraversal = function(root, res = []) {
    if (root === null) return res;
    let stack = [root]; //压入了节点而不是val，类似于stack = [ { val: 1, left: {...}, right: {...} } ]
    let cur = null;

    while (stack.length) {
      cur = stack.pop();
      res.push(cur.val);
      cur.right && stack.push(cur.right);
      cur.left && stack.push(cur.left);
    }
    return res;
};
```

#### 🔹Question 2: Leetcode_145

Given the root of a binary tree, return the **postorder** traversal of its nodes' values.

#### Method1: DFS递归

```
var postorderTraversal = function(root) {
    let ans = [];
    const dfs = function(root){
        if (root === null) return;
        dfs(root.left);
        dfs(root.right);
        ans.push(root.val);
    }
    dfs(root);
    return ans;
};
```

#### Method2: DFS迭代

```
var postorderTraversal = function(root, res = []) {
    if (root === null) return res;
    const stack = [root];
    let cur = null;  
    while (stack.length) {
        cur = stack.pop();
        res.push(cur.val);
        cur.left && stack.push(cur.left);
        cur.right && stack.push(cur.right);
    }
    return res.reverse();
};
```


#### 🔹Question 3: Leetcode_94

Given the root of a binary tree, return the **inorder** traversal of its nodes' values.

#### Method1: DFS递归

```
var inorderTraversal = function(root) {
    let ans = [];
    const dfs = function(root){
        if (root === null) return;
        dfs(root.left);
        ans.push(root.val);
        dfs(root.right);
    }
    dfs(root);
    return ans;
};
```

#### Method2: DFS迭代

```
var inorderTraversal = function(root, res = []) {
    const stack = [];
    let cur = root;

    while (stack.length || cur) {
        if (cur) {
            stack.push(cur);
            cur = cur.left;
        } else {
            cur = stack.pop();
            res.push(cur.val);
            cur = cur.right;
        }
    }
    return res;
};
```

---

# Stack

**Key Points**

1. 队列（Queue）的特性

	•	FIFO：先进的先出，如同演唱会买票，最早排队的人先买到票
	•	队头 → 最早进队的元素
	•	队尾 → 最晚进队的元素

2. 栈（Stack）的特性

	•	LIFO：后进的先出，如同一摞盘子，最后放的盘子最先拿
	•	栈顶 → 最后进来的元素

3. 两个栈如何变成一个队列, 我们用：

	•	stackIn → 专门负责入队（push）
	•	stackOut → 专门负责出队（pop）和 peek

4. 区别对比表

| 特性        | 栈（Stack） | 队列（Queue） |
|-------------|------------|---------------|
| **数据规则** | LIFO（后进先出） | FIFO（先进先出） |
| **插入位置** | 栈顶（Top） | 队尾（Rear） |
| **删除位置** | 栈顶（Top） | 队头（Front） |
| **现实类比** | 一摞盘子：最后放的盘子最先拿 | 排队买票：最早排队的人先买到票 |
| **常用操作** | `push()` 压栈<br>`pop()` 出栈<br>`peek()` 查看栈顶 | `enqueue()` 入队<br>`dequeue()` 出队<br>`peek()` 查看队首 |

栈（Stack）

tail [1, 2, 3] top

| 操作        |  代码  |  变化  |  Top  |
|-------------|------------|---------------|---------------|
| In        |  push(4)  |  [1,2,3,4]  |  4  |
| Out        |  pop()  |  [1,2,3]  |  3  |

队列（Queue）

top [1, 2, 3] tail

| 操作        |  代码  |  变化  |  Top  |
|-------------|------------|---------------|---------------|
| In        |  push(4)  |  [1,2,3,4]  |  1  |
| Out        |  shift()  |  [2,3,4]  |  2  |


5. 常用操作解释

栈（Stack）

- `push(x)`：将元素压入栈顶
- `pop()`：移除栈顶元素并返回
- `peek()`：查看栈顶元素但不移除
- 特性：只能在栈顶进行插入和删除操作

队列（Queue）
- `enqueue(x)`：将元素加入队尾
- `dequeue()`：移除队首元素并返回
- `peek()`：查看队首元素但不移除
- 特性：只能在队首删除元素、队尾添加元素

6. 用栈的相关模式

- 括号匹配 / 有效性检查；
- 消消乐 / 相邻抵消类问题；
- 逆序 / 回溯：十进制转二进制、括号展开、路径回溯；
- 单调栈问题：对于每个元素，要找到后面第一个比它大的/小的；

---

## Implement Stack

### Related Questions

#### 🔹Question 1: Leetcode_20

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.

#### Method1: switch
```
var isValid = function(s) {
    let stack = [];
    for (let i = 0; i < s.length; i++) {
        let char = s[i];
        switch(char) {
            case '(':
            stack.push(')');
            break;
            case '[':
            stack.push(']');
            break;
            case '{':
            stack.push('}');
            break;
            default:
            if (stack.length === 0 || char !== stack.pop()) return false;
        }
    }
    return stack.length === 0;
};
```

#### Method2: Map

```
var isValid = function(s) {
    let stack = [];
    let map = {
        '(': ')',
        '[': ']',
        '{': '}'
    };
    for (let char of s) {
        if (char in map) {
            stack.push(char);
            continue;
        };
        if(map[stack.pop()] !== char) return false; 
    }
    return stack.length === 0;
};
```

#### 🔹Question 2: Leetcode_1047

You are given a string s consisting of lowercase English letters. A duplicate removal consists of choosing two adjacent and equal letters and removing them.
We repeatedly make duplicate removals on s until we no longer can.
Return the final string after all such duplicate removals have been made. It can be proven that the answer is unique.

```
var removeDuplicates = function(s) {
    let stack = [];
    for (let i = 0; i < s.length; i++) {
        if (stack.length > 0 && stack[stack.length - 1] === s[i]) {
            stack.pop();
        } else {
            stack.push(s[i]);
        }
    }
    return stack.join('');
};
```

#### 🔹Question 3: Leetcode_15

根据 逆波兰表示法，求表达式的值。
有效的运算符包括 + ,  - ,  * ,  / 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

```
var evalRPN = function(tokens) {
    let stack = [];
    let res = 0;
    for (let i = 0; i< tokens.length; i++) {
        let token = tokens[i];
        if (isNaN(Number(token))){
            const n2 = stack.pop();
            const n1 = stack.pop();
            switch(token) {
                case "+":
                stack.push(n1 + n2);
                break;
                case "-":
                stack.push(n1 - n2);
                break;
                case "*":
                stack.push(n1 *n2);
                break;
                case "/":
                stack.push(n1 / n2 | 0);
                break;
            }
        } else {
            stack.push(Number(token));
        }
    }
    return stack[0];
};
```

#### 🔹Question 4: Leetcode_239

给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

```
var maxSlidingWindow = function (nums, k) {
    class MonoQueue {
        queue;
        constructor() {
            this.queue = [];
        }
        enqueue(value) {
        // enqueue 保证队列单调递减，最大值一直在队首。
            let back = this.queue[this.queue.length - 1];
            while (back !== undefined && back < value) {
                this.queue.pop();
                back = this.queue[this.queue.length - 1];
            }
            this.queue.push(value);
        }
        dequeue(value) {
        // 	dequeue 把滑出窗口的值移走（只在它等于队首时才删）。
            let front = this.front();
            if (front === value) {
                this.queue.shift();
            }
        }
        front() {
            return this.queue[0];
        }
    }
    let helperQueue = new MonoQueue();
    let i = 0, j = 0;
    let resArr = [];
    while (j < k) {
        helperQueue.enqueue(nums[j++]);
    }
    resArr.push(helperQueue.front());
    while (j < nums.length) {
        helperQueue.enqueue(nums[j]);
        helperQueue.dequeue(nums[i]);
        resArr.push(helperQueue.front());
        // 每次 front() 取的就是当前窗口最大值。
        i++, j++;
    }
    return resArr;
};
```

#### 🔹Question 5: Leetcode_347

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

示例 1:

输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
示例 2:

输入: nums = [1], k = 1
输出: [1]

```
var topKFrequent = function (nums, k) {
  const map = new Map();
  const res = [];
  //使用 map 统计元素出现频率
  for (const num of nums) {
    map.set(num, (map.get(num) || 0) + 1);
  }
  //创建小顶堆
  const heap = new PriorityQueue({
    compare: (a, b) => a.value - b.value
  })
  for (const [key, value] of map) {
    heap.enqueue({ key, value });
    if (heap.size() > k) heap.dequeue();
  }
  //处理输出
  while (heap.size()) res.push(heap.dequeue().key);
  return res;
};
```

---

## Implement Quene & Stack

### Related Questions

#### 🔹Question 1: Leetcode_232

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (push, peek, pop, and empty).
Implement the MyQueue class:

void push(int x) Pushes element x to the back of the queue.
int pop() Removes the element from the front of the queue and returns it.
int peek() Returns the element at the front of the queue.
boolean empty() Returns true if the queue is empty, false otherwise.
Notes:

You must use only standard operations of a stack, which means only push to top, peek/pop from top, size, and is empty operations are valid.
Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

```

var MyQueue = function() {
    this.stackIn = [];
    this.stackOut = [];
};

/** 
 * @param {number} x
 * @return {void}
 */
MyQueue.prototype.push = function(x) {
    this.stackIn.push(x);
};

/**
 * @return {number}
 */
MyQueue.prototype.pop = function() {
    const size = this.stackOut.length;
    if (size) {
        return this.stackOut.pop();
    }
    while (this.stackIn.length) {
        this.stackOut.push(this.stackIn.pop());
    }
    return this.stackOut.pop();
};

/**
 * @return {number}
 */
MyQueue.prototype.peek = function() {
    if (!this.stackOut.length) {
        while (this.stackIn.length) {
            this.stackOut.push(this.stackIn.pop());
        }
    }
    return this.stackOut[this.stackOut.length - 1];
};

/**
 * @return {boolean}
 */
MyQueue.prototype.empty = function() {
    return !this.stackIn.length && !this.stackOut.length;
};

```

#### 🔹Question 2: Leetcode_225

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (push, top, pop, and empty).
Implement the MyStack class:

void push(int x) Pushes element x to the top of the stack.
int pop() Removes the element on the top of the stack and returns it.
int top() Returns the element on the top of the stack.
boolean empty() Returns true if the stack is empty, false otherwise.

```
var MyStack = function() {
    this.quene = [];
};

/** 
 * @param {number} x
 * @return {void}
 */
MyStack.prototype.push = function(x) {
    this.quene.push(x);
};

/**
 * @return {number}
 */
MyStack.prototype.pop = function() {
    let size = this.quene.length;
    while (size-- > 1){
        this.quene.push(this.quene.shift());
    }
    return this.quene.shift();
};

/**
 * @return {number}
 */
MyStack.prototype.top = function() {
    const x = this.quene.pop();
    this.quene.push(x);
    return x;
};

/**
 * @return {boolean}
 */
MyStack.prototype.empty = function() {
    return !this.quene.length;
};
```

---



# String

**常用 ASCII 对照表**

- 空格 (space) **->** 32，
- 运算符号! " # $ % & ' ( ) * + , - . / **->** 33-47，
- '0'–'9' **->** 48–57，
- 继续的标点和比较运算符 : ; < = > ? @ **->** 58–64
- 'A'–'Z' = 65–90，
- 括号 `[ \ ] ^ _ `` **->** 91-96，
- 'a'–'z' = 97–122。

## KMP

要在文本串：aabaabaafa 中查找是否出现过一个模式串：aabaaf。

1. 前缀：
a,
aa,
aab,
aaba,
aabaa

2. 后缀：
f,
af,
aaf,
baaf,
abaaf

3. 最长相等前后缀
a       0
a | a   1
aab     0
aa| ba  1
aa b aa 2
aabaaf  0

4. 前缀表（next/prefix）：010121 ->(整体减1）-10-1010
前追表中最大的数字2刚好是最长前缀的长度

5. 代码实现

- 初始化：

求next数组

```
next = [];
```

j前缀末尾位
```
j=0;
next.push[j];
```

i后缀末尾位

```
for（let i = 1; i < needle.length; i++)
```

- 前后缀不相同：

```
while (j > 0 && needle[i] !== needle[j]) {
  j = next[j-1];
}
```

- 前后缀相同

```
if (needle[i] === needle[j]) {
  j++;
  next.push(j);
}
```

### Related Questions

#### 🔹Question 1: Leetcode_28

Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

```
var strStr = function(haystack, needle) {
if (needle.length === 0)
        return 0;

    const getNext = (needle) => {
        let next = [];
        let j = 0;
        next.push(j);

        for (let i = 1; i < needle.length; ++i) {
            while (j > 0 && needle[i] !== needle[j])
                j = next[j - 1];
            if (needle[i] === needle[j])
                j++;
            next.push(j);
        }

        return next;
    }

    let next = getNext(needle);
    let j = 0;
    for (let i = 0; i < haystack.length; ++i) {
        while (j > 0 && haystack[i] !== needle[j])
            j = next[j - 1];
        if (haystack[i] === needle[j])
            j++;
        if (j === needle.length)
            return (i - needle.length + 1);
    }

    return -1;
};
```

---

## Reverse String

### Related Questions

#### 🔹Question 1: Leetcode_344

Write a function that reverses a string. The input string is given as an array of characters s.
You must do this by modifying the input array in-place with O(1) extra memory.

```
var reverseString = function(s) {
    let left = 0, right = s.length - 1;
    while (left < right) {
        [s[left], s[right]] = [s[right], s[left]];
        left++;
        right--;
    }
    return s;
};
```

#### 🔹Question 2: Leetcode_541

Given a string s and an integer k, reverse the first k characters for every 2k characters counting from the start of the string.
If there are fewer than k characters left, reverse all of them. If there are less than 2k but greater than or equal to k characters, then reverse the first k characters and leave the other as original.

```
var reverseStr = function(s, k) {
    let arr = s.split('');
    let length = arr.length;
    for (let i = 0; i < length; i += 2*k) {
        let left = i, right = Math.min(i + k - 1, length - 1);
        while (left < right) {
            [arr[left], arr[right]] = [arr[right], arr[left]];
            left++;
            right--;
        }
    }
    return arr.join('');
};
```

#### 🔹Question 3: Leetcode_151

Given an input string s, reverse the order of the words.
A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.
Return a string of the words in reverse order concatenated by a single space.
Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

```
var reverseWords = function(s) {
    let arr = s.split(' ');
    let array = [];
    
    for (let i = 0; i < arr.length; i++){
        if (arr[i]) {
            array.push(arr[i]);
        }
    }

    let left = 0, right = array.length - 1;
    while (left < right) {
        [array[left], array[right]] = [array[right], array[left]];
        left++;
        right--;
    }
    return array.join(' ');
};
```


#### 🔹Question 4: 卡码
字符串的右旋转操作是把字符串尾部的若干个字符转移到字符串的前面。给定一个字符串 s 和一个正整数 k，请编写一个函数，将字符串中的后面 k 个字符移到字符串的前面，实现字符串的右旋转操作。.不能申请额外空间，只能在本串上操作。
例如，对于输入字符串 "abcdefg" 和整数 2，函数应该将其转换为 "fgabcde"。
输入：输入共包含两行，第一行为一个正整数 k，代表右旋转的位数。第二行为字符串 s，代表需要旋转的字符串。
输出：输出共一行，为进行了右旋转操作后的字符串。

```
function reverseString(s, left, right){
    while (left < right) {
            [s[left], s[right]] = [s[right], s[left]];
            left++;
            right--;
        }
    return s;
}

function rotateRight(s,k){
    let str = s.split('');
    const length = str.length - 1;
    
    str = reverseString(str, 0, length);
    str = reverseString(str, 0, k - 1)
    str = reverseString(str, k, length)
    
    return str.join('');
}
```

---

# Hash Table

**Key Points**

- 当我们需要查询一个元素是否出现过，或者一个元素是否在集合里的时候，就要考虑哈希法;
- 哈希问题考虑三种方式：Array/ Set/ Map
- Array（固定长度）: new Array(length).fill(0);
- Set（数据结构类似于数组，但不允许重复值）：new Set();
- Map（键值对）：new Map();
- Array → Set：let set = new Set(arr);
- Set → Array： let arr = [...set] ｜｜let arr = Array.from(set);

## Hash question: map

**Key Points**

- 数据范围未知,需要统计次数,需要索引;
- 要判断什么是key什么是value；
- LeetCode 常见例子: 两数之和（经典版）、单词频率统计、LRU 缓存;

---

**Map 常用方法**

| 方法 | 作用 | 示例 |
|------|------|------|
| `set(key, value)` | 添加或更新键值 | `map.set("a", 1)` |
| `get(key)` | 获取键对应的值，没有返回 `undefined` | `map.get("a") // 1` |
| `has(key)` | 判断是否存在键 | `map.has("a") // true` |
| `delete(key)` | 删除某个键值对，返回 `true/false` | `map.delete("a")` |
| `clear()` | 清空所有键值对 | `map.clear()` |
| `size` | 返回键值对数量（属性，不是方法） | `map.size // 2` |

### Related Questions

#### 🔹Question 1: Leetcode_349

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.

#### Method1: Object

```
var twoSum = function(nums, target) {
    let hash = {};
    for (let i = 0; i < nums.length; i++) {
        if (hash[target - nums[i]] !== undefined) {
            return [i, hash[target - nums[i]]];
        }
        hash[nums[i]] = i;
    }
    return [];
};
```

#### Method2: Map()

```
var twoSum = function(nums, target) {
    let hash = new Map();
    for (let i = 0; i < nums.length; i++) {
        if (hash.has(target - nums[i])) {
            return [i, hash.get(target - nums[i])];
        }
        hash.set(nums[i], i);
    }
    return [];
};
```

#### 🔹Question 2: Leetcode_454

Given four integer arrays nums1, nums2, nums3, and nums4 all of length n, return the number of tuples (i, j, k, l) such that:
0 <= i, j, k, l < n
nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0

```
var fourSumCount = function(nums1, nums2, nums3, nums4) {
    let map = new Map();
    let count = 0;
    for (let i = 0; i < nums1.length; i++) {
        for (let j = 0; j < nums2.length; j++) {
            const sum = nums1[i] + nums2[j];
            map.set(sum, (map.get(sum) || 0) + 1);
        }
    }
    for (let i = 0; i < nums3.length; i++) {
        for (let j = 0; j < nums4.length; j++) {
            const sum = nums3[i] + nums4[j];
            count += (map.get(0 - sum) || 0);
        }
    }
    return count;
};
```

#### 🔹Question 3: Leetcode_383

Given two strings ransomNote and magazine, return true if ransomNote can be constructed by using the letters from magazine and false otherwise.
Each letter in magazine can only be used once in ransomNote.

#### Method1: Map()

```
var canConstruct = function(ransomNote, magazine) {
    let map = new Map();

    for (let i of magazine) {
        map.set(i, (map.get(i) || 0) + 1);
    };
    for (let j of ransomNote) {
        if (!map.has(j) || map.get(j) <= 0){
            return false;
        }
        map.set(j, (map.get(j) || 0) - 1);
    }
    return true;
};
```

#### Method2: Array()

```
var canConstruct = function(ransomNote, magazine) {
    let arr = new Array(26).fill(0);
    const base = 'a'.charCodeAt();

    for (let s of magazine) {
        arr[s.charCodeAt() - base]++;
    };
    for (let s of ransomNote) {
        if (!arr[s.charCodeAt() - base]){
            return false;
        }
        arr[s.charCodeAt() - base]--;
    }
    return true;
};
```

#### 🔹Question 4: Leetcode_15

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.
Notice that the solution set must not contain duplicate triplets.

```
var threeSum = function(nums) {
   let ans = [];
   nums.sort((a,b) => a - b);
   for (let i = 0; i< nums.length; i++) {
    let left = i + 1, right = nums.length - 1;
    if (nums[i] > 0) return ans;
    if (i > 0 && nums[i] === nums[i - 1]) continue;
    while (left < right) {
        let sum = nums[i] + nums[left] + nums[right];
        if (sum < 0) {
            left++;
        } else if (sum > 0) {
            right--;
        } else {
            ans.push([nums[i], nums[left], nums[right]]);
            while (left < right && nums[left] === nums[left + 1]) {
                left++;
            }
            while (left < right && nums[right] === nums[right - 1]) {
                right--;
            }
            left++;
            right--;
        }
    }
   }
   return ans;
};

// a去重：nums[i] == nums [i - 1] continue
// left去重：l<r && l == l+1 l++
// right去重：l<r && r == r-1 r--
```

#### 🔹Question 5: Leetcode_18

Given an array nums of n integers, return an array of all the unique quadruplets [nums[a], nums[b], nums[c], nums[d]] such that:
0 <= a, b, c, d < n
a, b, c, and d are distinct.
nums[a] + nums[b] + nums[c] + nums[d] == target
You may return the answer in any order.

```
var fourSum = function(nums, target) {
    let ans = [];
    if (nums.length < 4) return ans;
    nums.sort((a, b) => a - b);
    for (let i = 0; i < nums.length - 3; i++) {
        if (i > 0 && nums[i] === nums[i - 1]) continue;
        for (let j = i + 1; j < nums.length - 2; j++) {
            if (j > i + 1 && nums[j] === nums[j - 1]) continue;
            let left = j + 1, right = nums.length - 1;

            while (left < right){
                let sum = nums[i] + nums[j] + nums[left] + nums[right];
                if (sum < target) {
                    left++;
                    } else if (sum > target) {
                    right--;
                    } else {
                    ans.push([nums[i], nums[j], nums[left], nums[right]]);
                    while (left < right && nums[left] === nums[left + 1]) {
                        left++;
                    }
                    while (left < right && nums[right] === nums[right - 1]) {
                        right--;
                    }
                    left++;
                    right--;
                }
            }
        }
    }
    return ans;
};
```

---

## Hash question: Set

**Key Points**

- 只需 判断是否存在 / 去重，不需要统计次数;
- 适合频率统计、映射关系;
- 想清楚map中的key和value用来存什么的；
- LeetCode 常见例子: 两数之和（简化版）、判断有无重复元素、滑动窗口去重;

### Related Questions

#### 🔹Question 1: Leetcode_1

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.

#### Method1: Array()

```
var intersection = function(nums1, nums2) {
    let ans = new Array(1001).fill(0);
    let res = [];
    for(let i of nums1){
        ans[i]++;
    }
    for(let i of nums2){
        if(ans[i] !== 0){
            res.push(i);
            ans[i] = 0;
        }
    }
    return res;
};
```

#### Method2: Set()

```
var intersection = function(nums1, nums2) {
    let resSet = new Set();
    let ansSet = new Set();
    for (let num of nums1){
        resSet.add(num);
    }
    for (let num of nums2) {
        if(resSet.has(num)) {
            ansSet.add(num);
        }
    }
    return Array.from(ansSet);
};
```

#### 🔹Question 1: Leetcode_202

Write an algorithm to determine if a number n is happy.

A happy number is a number defined by the following process:

Starting with any positive integer, replace the number by the sum of the squares of its digits.
Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
Those numbers for which this process ends in 1 are happy.
Return true if n is a happy number, and false if not.

```
var isHappy = function(n) {
    let set = new Set();
    let sum;
    
    while(sum !== 1){
        let arr = (String(sum || n)).split('');
        sum = arr.reduce((total, num) => {
            return total + num * num;
        }, 0)
        if (set.has(sum)) return false;
        set.add(sum);
    }
    return true;
};
```

---

## Hash question: Array

**Key Points**

- 数据范围已知且小（比如 26 个小写字母、数字 0-9);
- 只能用整数索引;
- LeetCode 常见例子: 字母异位词、计数排序、统计字符频率;

### Related Questions


#### 🔹Question 1: Leetcode_242

Given two strings s and t, return true if t is an anagram of s, and false otherwise.

```
var isAnagram = function(s, t) {
    if(s.length !== t.length) return false;
    let base = 'a'.charCodeAt();
    let res = new Array(26).fill(0);
    for(let i of s){
        res[i.charCodeAt() - base]++;
    }
    for(let i of t){
        if(!res[i.charCodeAt() - base]) return false;
        res[i.charCodeAt() - base]--;
    }
    return true;
};
```


---

# Linked List

链表是一种通过指针串联在一起的线性结构，每一个节点由两部分组成，一个是数据域一个是指针域（存放指向下一个节点的指针），最后一个节点的指针域指向null（空指针的意思）。链表的入口节点称为链表的头结点也就是head。

```
let dummy = new ListNode(0);
```

ListNode 是链表节点类，通常定义为：

```
function ListNode(val, next = null) {
    this.val = val;
    this.next = next;
}
```

---

## Interaction of Arrays

### Related Questions

#### 🔹Question 1: Leetcode_349

---

## Linked Lists Cycle

**Key Points**

- 先判断链表是否有环：fast 走两个节点，slow走一个节点，有环的话一定会在环内相遇：
  	•	slow 每次走 1 步（慢跑的人）
	•	fast 每次走 2 步（快跑的人）
如果没有环他们一定不会相遇，快的人会先到达终点；
- 找到这个环的入口：从相遇点走 c 步就是环入口，而从链表头走 a 步也是环入口，因此让slow回到head，fast留在相遇点，同时历遍slow，fast，就能找到环入口。

### Related Questions

#### 🔹Question 1: Leetcode_142

Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to (0-indexed). It is -1 if there is no cycle. Note that pos is not passed as a parameter.

Do not modify the linked list.

```
var detectCycle = function(head) {
    if(!head || !head.next) return null;

    let fast = head.next.next, slow = head.next;
    while (fast && fast.next && fast !== slow) {
        fast = fast.next.next;
        slow = slow.next;
    }
    if(!fast || !fast.next) return null;

    slow = head;
    while(fast !== slow){
        fast = fast.next;
        slow = slow.next;
    }
    return slow;
};
```

---

## Intersection of Two Linked Lists

**Key Points**

- 求出两个链表的长度，设定A为最长的链表，并求出两个链表长度的差值;
- 让curA移动到，和curB 末尾对齐的位置:
A: 1 -> 2 -> 5 -> 6 -> 7 -> 8
            curA
B: 4 -> 6 -> 7 -> 8
  curB
- 同时向后移动curA和curB，如果curA !== curB，则继续向后历遍，如果相等则返回最长链表A:
A: 1 -> 2 -> 5 -> 6 -> 7 -> 8
                 curA
B: 4 -> 6 -> 7 -> 8
       curB

### Related Questions

#### 🔹Question 1: Leetcode_19

Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.
e.g.
Input:
A: 1 -> 2 -> 5 -> 6 -> 7 -> 8
B: 4 -> 6 -> 7 -> 8

Output:
A: 1 -> 2 -> 5
                -> 6 -> 7 -> 8
B: 4

```
var getIntersectionNode = function(headA, headB) {
    let lenA = 0, lenB = 0;
    let temA = headA, temB = headB;
    while (temA) {
        lenA++;
        temA = temA.next;
    };
    while (temB) {
        lenB++;
        temB = temB.next;
    };

    let curA = headA, curB = headB;
    if (lenA < lenB) {
        [curA, curB] = [curB, curA];
        [lenA, lenB] = [lenB, lenA]; // 解构赋值交换: [a, b] = [b, a]直接交换 a 和 b
    }
    let gap = lenA - lenB;
    while (gap-- > 0) {
        curA = curA.next;
    }
    while (curA && curA !== curB) {
        curA = curA.next;
        curB = curB.next;
    }
    return curA;
};
```

---



## Swap Nodes in Linked List

**Key Points**
![IMG_0517](https://github.com/user-attachments/assets/d3fd738a-ddce-4b2a-b911-36d94b9aec1e)

- 快慢指针定位正数情况下n的位置；
- 快指针先走 n 步；
- 快慢指针一起走，直到 fast.next === null（slow 在目标前一个位置).

### Related Questions

#### 🔹Question 1: Leetcode_19

Given the head of a linked list, remove the nth node from the end of the list and return its head.

```
var removeNthFromEnd = function(head, n) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let fast = dummy, slow = dummy;
    while (n--) {
        fast = fast.next;
    }
    while (fast.next !== null) {
        slow = slow.next;
        fast = fast.next;
    }
    slow.next = slow.next.next;
    return dummy.next;
};
```

---

## Swap Nodes in Linked List

**Key Points**
![IMG_0516](https://github.com/user-attachments/assets/93c44e76-c273-4737-9d52-6cb3b45e9cd4)

- Condition：temp && temp.next;
- Three key points to save positions: tem -> pre -> cur;
- Swapping: break → link tail → link head.

### Related Questions

#### 🔹Question 1: Leetcode_24

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

```
var swapPairs = function(head) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let tem = dummy;
    while(tem.next && tem.next.next){
        let cur = tem.next.next, pre = tem.next;
        pre.next = cur.next;
        cur.next = pre;
        tem.next = cur;
        tem = pre;
    }
    return dummy.next;
};
```

---


## Remove elements

**Key Points**
- Insert a dummy head in most cases, especially when you might need to delete the head node. Skip it only if you’re just iterating without modifying the list；
- To avoid null pointer errors, always check curr.next !== null in the while condition.
(Null pointer error: Cannot read properties of null (reading 'val'));
- If curr.next.val === target, then delete the node by assigning curr.next = curr.next.next. Otherwise, move forward by setting curr = curr.next.;
- Always return dummy.next instead of head, because the original head node might have been removed.

### Related Questions

#### 🔹Question 1: Leetcode_203

Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.

```
var removeElements = function(head, val) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let current = dummy;

    while (current.next !== null) {
        if (current.next.val === val) {
            current.next = current.next.next;
        } else {
            current = current.next;
        }
    }
    return dummy.next;
};
```

---

#### 🔹Question 2: Leetcode_83

Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.
Example 1:
Input: head = [1,1,2]
Output: [1,2]

##### ✅Method 1: Insert dummy head

```
var deleteDuplicates = function(head) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let curr = dummy;

    while (curr.next !== null && curr.next.next !== null) {
        if (curr.next.val === curr.next.next.val) {
            curr.next = curr.next.next;
        } else {
            curr = curr.next
        }
    }
    return dummy.next;
};
```

##### ✅Method 2: Without dummy head

```
var deleteDuplicates = function(head) {
    let curr = head;

    while (curr !== null && curr.next !== null) {
        if (curr.val === curr.next.val) {
            curr.next = curr.next.next;
        } else {
            curr = curr.next
        }
    }
    return head;
};
```

---

## 链表的增删查改

**Key Points**
- **合法性判断**（index 是否越界）
 ```
if (index < 0 || index > this._size) return; // 插入
if (index < 0 || index >= this._size) return -1; // 查找 / 删除
```
- **创建 dummy 节点**，用 dummy避免处理第一个节点的特殊情况
```
let dummy = new ListNode(0); //这里控制的是value，dummy = { val: 0, next: null }
dummy.next = this._head; // 这里才是位置
```
- **遍历至目标 index 前一个节点（cur）**
```
let cur = dummy;
for (let i = 0; i < index; i++) {
    cur = cur.next; // cur一直移动，直到 index 的前一个节点停下，此时cur在目标target前一个index
}
```
- **基于 cur 执行插入/删除/修改操作**
插入
```
let newNode = new ListNode(val); 
newNode.next = cur.next; 
cur.next = newNode;
```
删除
```
cur.next = cur.next.next;
```
修改
```
cur.next.val = val;
```
- **如果操作影响了链表头，更新 this._head = dummy.next**

链表
```
dummy --> [0] --> [1] --> [99] --> [2] --> [3] --> null
```

更新真实头部
```
this._head = dummy.next;
```

更新后链表
```
this._head --> [1] --> [99] --> [2] --> [3] --> null
```

- **根据操作更新链表长度 this._size**
```
this._size++;
```
或
```
this._size--;
```

### Related Questions

#### 🔹Question 1: Leetcode_707

Design your implementation of the linked list. You can choose to use a singly or doubly linked list.
A node in a singly linked list should have two attributes: val and next. val is the value of the current node, and next is a pointer/reference to the next node.
If you want to use the doubly linked list, you will need one more attribute prev to indicate the previous node in the linked list. Assume all nodes in the linked list are 0-indexed.

Implement the MyLinkedList class:

MyLinkedList() Initializes the MyLinkedList object.
int get(int index) Get the value of the indexth node in the linked list. If the index is invalid, return -1.
void addAtHead(int val) Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
void addAtTail(int val) Append a node of value val as the last element of the linked list.
void addAtIndex(int index, int val) Add a node of value val before the indexth node in the linked list. If index equals the length of the linked list, the node will be appended to the end of the linked list. If index is greater than the length, the node will not be inserted.
void deleteAtIndex(int index) Delete the indexth node in the linked list, if the index is valid.

```
var MyLinkedList = function() {
    this.size = 0;
    this.head = null;
    this.tail = null;
};

/** 
 * @param {number} index
 * @return {number}
 */
MyLinkedList.prototype.get = function(index) {
    if (index < 0 || index >= this.size) return -1;
    let cur = this.head;
    for (let i = 0; i < index; i++) {
        cur = cur.next;
    }
    return cur.val;
};

/** 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtHead = function(val) {
    let node = new ListNode(val);
    if (this.size ===0) {
        this.tail = node; 
        this.head = node;
        } else {
    node.next = this.head;
    this.head = node;
    }
    this.size++;
};

/** 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtTail = function(val) {
    let node = new ListNode(val);
    if (this.size ===0) {
        this.tail = node;
        this.head = node;
        } else {
    this.tail.next = node;
    this.tail = node;
    }
    this.size++;
};

/** 
 * @param {number} index 
 * @param {number} val
 * @return {void}
 */
MyLinkedList.prototype.addAtIndex = function(index, val) {
    if(index < 0 || index > this.size) return;
    if (index === 0) return this.addAtHead(val);
    if (index === this.size) return this.addAtTail(val);
    let dummy = new ListNode(0);
    dummy.next = this.head;
    let cur = dummy;
    let node = new ListNode(val);
    for(let i = 0; i < index; i++){
        cur = cur.next;
    }
    node.next = cur.next;
    cur.next= node;
    this.head = dummy.next;
    this.size++;
};

/** 
 * @param {number} index
 * @return {void}
 */
MyLinkedList.prototype.deleteAtIndex = function(index) {
    if (index < 0 || index >= this.size) return;
    let dummy = new ListNode(0);
    dummy.next = this.head;
    let cur = dummy;
    
    for (let i = 0; i < index; i++){
        cur = cur.next;
        }
    cur.next = cur.next.next;
    this.head = dummy.next;

    if (index === this.size - 1) {
        let temp = this.head;
        while (temp && temp.next !== null) {
            temp = temp.next;
        }
        this.tail = temp;
    }

    this.size--;
};

/** 
 * Your MyLinkedList object will be instantiated and called as such:
 * var obj = new MyLinkedList()
 * var param_1 = obj.get(index)
 * obj.addAtHead(val)
 * obj.addAtTail(val)
 * obj.addAtIndex(index,val)
 * obj.deleteAtIndex(index)
 */
```

---

## Reverse Linked List

### Related Questions

#### 🔹Question 1: Leetcode_206

Given the head of a singly linked list, reverse the list, and return the reversed list.

#### Method1: 双指针法（cur&pre)，第三个指针tem只是用来保存cur.next，后续会消失

```
var reverseList = function(head) {
    let cur = head;
    let pre = null;
    while(cur){
        let temp = cur.next;
        cur.next = pre;
        pre = cur; //必须是pre先定义，因为如果先定义cur，pre就找不到对应的cur了
        cur = temp;
    }
    return pre;
};
```

#### Method2: 递归法（双指针法的衍生）

```
var reverse = function(cur, pre) { //对应下面的var reverseList = function(head, null)
    if (cur === null) return pre;
    let tem = cur.next;
    cur.next = pre; //只需要完成双指针法的前两步操作，其余会通过reverse本身完成
    return reverse(tem, cur); // 因为cur = temp; pre = cur; 对应上面的function(cur, pre)
}

var reverseList = function(head) {
    return reverse(head, null);
};
```

---

# Array

## Spiral Matrix

**Key Points:**

- Use for: Matrix traversal or construction in spiral (clockwise) order;
- Use 4 boundaries or startX/startY + offset to control traversal scope;
- Outer loop: number of layers = Math.floor(n / 2) (i.e. loop);
- Inner steps (always 4 in each loop):
    1.	Left to Right → top row
	2.	Top to Bottom ↓ right column
	3.	Right to Left ← bottom row
	4.	Bottom to Top ↑ left column;
- After each outer loop: startX++, startY++, offset++ to move to inner layer;
- For odd n, fill center value after loop: if (n % 2 === 1) res[mid][mid] = count;


### Related Questions

#### 🔹Question 1: Leetcode_59

Given a positive integer n, generate an n x n matrix filled with elements from 1 to n2 in spiral order.

```
var generateMatrix = function(n) {
    let startX = startY = 0;
    let offset = 1;
    let count = 1;
    let loop = Math.floor(n/2);
    let mid = Math.floor(n/2);
    let ans = new Array(n).fill(0).map(() => new Array(n).fill(0) )

    while(loop--){
        let row = startX, col = startY;
        for(; col < n - offset; col++){
            ans[row][col] = count ++;
        }
        for(; row < n - offset; row++ ){
            ans[row][col] = count ++;
        }
        for(; col > startY; col--){
            ans[row][col] = count ++;
        }
        for(; row > startX; row--){
            ans[row][col] = count ++;
        }
        startX ++;
        startY ++;
        offset += 1;
    }
    if (n % 2 === 1){
        ans[mid][mid] = count ++;
    }
    return ans;
};
```

---

#### 🔹Question 2: Leetcode_54

Given an m x n matrix, return all elements of the matrix in spiral order.

```
var spiralOrder = function(matrix) {
    let startX = 0;
    let startY = 0;
    let endX = m - 1;
    let endY = n - 1;
    let m = matrix.length;
    let n = matrix[0].length;
    let ans = [];

    while (startX <= endX && startY <= endY){
        for (let col = startY; col <= endY; col ++) {
            ans.push(matrix[startX][col]);
        }
        for (let row = startX + 1; row <= endX; row ++) {
            ans.push(matrix[row][endY]);
        }
        if (startX < endX) {
            for (let col = endY - 1; col >= startY; col--) {
                ans.push(matrix[endX][col]);
            }
        }
        if (startY < endY) {
            for (let row = endX - 1; row > startX; row--) {
                ans.push(matrix[row][startY]);
            }
        }
        startX ++;
        startY ++;
        endX --;
        endY --;
    }
    
    return ans;
};
```

---

#### 🔹Question 3: Leetcode_885

You start at the cell (rStart, cStart) of an rows x cols grid facing east. The northwest corner is at the first row and column in the grid, and the southeast corner is at the last row and column.

You will walk in a clockwise spiral shape to visit every position in this grid. Whenever you move outside the grid's boundary, we continue our walk outside the grid (but may return to the grid boundary later.). Eventually, we reach all rows * cols spaces of the grid.

Return an array of coordinates representing the positions of the grid in the order you visited them.

```
var spiralMatrixIII = function(rows, cols, rStart, cStart) {
    let direction = [[0,1],[1,0],[0,-1],[-1,0]];
    let ans = [[rStart,cStart]];
    let x = rStart, y = cStart;
    let step = 1;
    let currentD = 0;

    while(ans.length < rows * cols) {
        for (let i = 0; i < 2; i ++ ){
            const [dx,dy] = direction[currentD % 4];
            for (let j = 0; j < step; j ++){
                x += dx;
                y += dy;
                if (x >= 0 && x < rows && y >=0 && y < cols){
                    ans.push([x,y]);
                }
            }
            currentD ++;
        }
        step ++; 
    }
    return ans;
};
```

---

## Sliding Window

**Key Points:**

- Use for: Subarray / finding maximum / minimal length;
- Set an infinity ans to record maximum / minimal length;
- External loop: right < nums.length
- Internal loop: sum >= target, left++ || sum < target, right ++


### Related Questions

#### 🔹Question 1: Leetcode_209

Given an array of positive integers nums and a positive integer target, return the minimal length of a subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

```
var minSubArrayLen = function(target, nums) {
    let left = 0, sum = 0;
    let ans = Infinity;
    for(let right = 0; right < nums.length; right++){
        sum += nums[right];
            while (sum >= target){
                ans = Math.min(ans, right - left + 1);
                sum -= nums[left]
                left++;
            } 
    }
    return ans === Infinity ? 0 : ans;
};
```

---

#### 🔹Question 2: Leetcode_904

You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array fruits where fruits[i] is the type of fruit the ith tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
Once you reach a tree with fruit that cannot fit in your baskets, you must stop.
Given the integer array fruits, return the maximum number of fruits you can pick.

```
var totalFruit = function(fruits) {
    let left = 0;
    let ans = 0;
    let map = new Map();

    for (let right = 0; right < fruits.length; right ++) {

        map.set(fruits[right], (map.get(fruits[right]) || 0) + 1);

        while (map.size > 2) {

            map.set(fruits[left], map.get(fruits[left]) - 1);

            if (map.get(fruits[left]) == 0) {
                map.delete(fruits[left]);
            }

            left ++;
        }

        ans = Math.max(ans, right - left + 1)

    }

    return ans;
   
};
```

---

#### 🔹Question 3: Leetcode_643

You are given an integer array nums consisting of n elements, and an integer k.

Find a contiguous subarray whose length is equal to k that has the maximum average value and return this value. Any answer with a calculation error less than 10-5 will be accepted.

```
var findMaxAverage = function(nums, k) {
    let left = 0;
    let sum = 0;
    let ans = -Infinity;

    for(let right = 0; right < nums.length; right ++) {
        sum += nums[right];
        while (right - left + 1 >= k) {
            ans = Math.max (ans, sum / k);
            sum -= nums[left]
            left ++;
        }
    }
    return ans;
};
```

---

#### 🔹Question 4: Leetcode_1052

There is a bookstore owner that has a store open for n minutes. You are given an integer array customers of length n where customers[i] is the number of the customers that enter the store at the start of the ith minute and all those customers leave after the end of that minute.

During certain minutes, the bookstore owner is grumpy. You are given a binary array grumpy where grumpy[i] is 1 if the bookstore owner is grumpy during the ith minute, and is 0 otherwise.

When the bookstore owner is grumpy, the customers entering during that minute are not satisfied. Otherwise, they are satisfied.

The bookstore owner knows a secret technique to remain not grumpy for minutes consecutive minutes, but this technique can only be used once.

Return the maximum number of customers that can be satisfied throughout the day.

Example 1:

Input: customers = [1,0,1,2,1,1,7,5], grumpy = [0,1,0,1,0,1,0,1], minutes = 3

Output: 16

Explanation:

The bookstore owner keeps themselves not grumpy for the last 3 minutes.

The maximum number of customers that can be satisfied = 1 + 1 + 1 + 1 + 7 + 5 = 16.

```
var maxSatisfied = function(customers, grumpy, minutes) {
    let base = 0;
    let sum = 0;
    let ans = 0;

    for(let right = 0; right < grumpy.length; right ++) {
        if (grumpy[right] === 0) {
            base += customers[right]
        }
    }
    for(let right = 0; right < grumpy.length; right ++) {
        if (grumpy[right] === 1) {
            sum += customers[right];
        } if (right >= minutes && grumpy[right - minutes] === 1) {
            sum -= customers[right- minutes];
        }
        ans = Math.max(ans, sum);
    }
    return base + ans;
};
```

---

## Binary Search

**Key Points:**
- Use when the array (or value space) is sorted or monotonic(increasing or decreasing);
- Maintain two pointers: left and right;
- Loop condition: left <= right (or left < right for some variants);
- Compute mid: mid = Math.floor((left + right) / 2);
- Compare nums[mid] (or condition on mid) with target;
- Useful for: Searching in sorted array /	Finding insert position /	Locating boundaries (first/last occurrence) /	Searching in answer space (e.g. min/max days, capacity, speed)

**Method 1: Closed Interval [left, right]**

```
function binarySearch(nums, target) {
  let left = 0, right = nums.length - 1;
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (nums[mid] > target) {
      right = mid - 1;
    } else if (nums[mid] < target) {
      left = mid;
    } else {
      return mid;
    }
  }
  return -1;
}
```

---

💡Key Points:
- Loop condition: left <= right
- On each iteration, we shrink the range by updating either left or right
- Final result: if found, return index; if not, return -1

---

**Method 2: Left-closed, Right-open [left, right)**

```
function binarySearch(nums, target) {
  let left = 0, right = nums.length;
  while (left < right) {
    const mid = Math.floor((left + right) / 2);
    if (nums[mid] > target) {
      right = mid;
    } else if (nums[mid] < target) {
      left = mid + 1;
    } else {
      return mid;
    }
  }
  return -1;
}
```

💡Key Points:
- Loop condition: left < right
- Right is exclusive, so we don’t use nums.length - 1 like Method 1
- Commonly used for finding insert positions or first occurrence

---

### Related Questions

#### 🔹Question 1: Leetcode_704

Describtion: Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1. You must write an algorithm with O(log n) runtime complexity.

##### ✅Method: Closed Interval [left, right]

```
var search = function(nums, target) {
    let mid, left = 0, right = nums.length - 1; 
    while (left <= right) {
        mid = Math.floor((left + right) / 2);
        if(nums[mid] > target){
            right = mid - 1;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            return mid;
        }
    }
    return -1; 
};
```

---

#### 🔹Question 2: Leetcode_35

Describtion: Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

##### ✅Method 1: Closed Interval [left, right]

```
var searchInsert = function(nums, target) {
    let mid, left = 0, right = nums.length - 1;
    while (left <= right) {
        mid = Math.floor ((left + right) / 2);
        if (nums[mid] === target) {
            return mid;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return left;  
};
```

---

##### ✅Method 2: Left-closed, Right-open [left, right)

```
var searchInsert = function(nums, target) {
    let mid, left = 0, right = nums.length;
    while (left < right) {
        mid = Math.floor ((left + right) / 2);
        if (nums[mid] >= target) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;  
};
```

---

#### 🔹Question 3: Leetcode_367

Describtion: Given a positive integer num, return true if num is a perfect square or false otherwise. A perfect square is an integer that is the square of an integer. In other words, it is the product of some integer with itself. You must not use any built-in library function, such as sqrt.

##### ✅Method 1: Closed Interval [left, right]

```
var isPerfectSquare = function(num) {
    if (num < 2) return true;
    let left = 1, right = num;
        while (left <= right) {
          let mid = Math.floor((left + right) / 2);
          const square = mid * mid;
          if (square === num) {
            return true;
          } else if (square < num) {
            left = mid + 1;
          } else {
            right = mid - 1;
          }
        }
        return false;
};
```

##### ✅Method 2: Left-closed, Right-open [left, right)

```
var isPerfectSquare = function(num) {
    if (num < 2) return true;
    let left = 1, right = num;
        while (left < right) {
          let mid = Math.floor((left + right) / 2);
          const square = mid * mid;
          if (square === num) {
            return true;
          } else if (square < num) {
            left = mid + 1;
          } else {
            right = mid;
          }
        }
        return false;
};
```

---

#### 🔹Question 4: Leetcode_34

Describtion: Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value. If target is not found in the array, return [-1, -1]. You must write an algorithm with O(log n) runtime complexity.

##### ✅Method 1: Closed Interval [left, right]

```
var searchRange = function(nums, target) { 
    let left = 0, right = nums.length - 1, result = [-1,-1];

    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (nums[mid] < target) {
          left = mid + 1;
        } else {
          right = mid - 1;
        }
    }
    if(left >= nums.length || nums[left] !== target) return result;
    result[0] = left;
    right = nums.length - 1;

    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (nums[mid] > target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    result[1] = right;
    return result;
};
```

---

##### ✅Method 2: Left-closed, Right-open [left, right)

```
var searchRange = function(nums, target) { 
    let left = 0, right = nums.length - 1, result = [-1,-1];

    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        if (nums[mid] < target) {
          left = mid + 1;
        } else {
          right = mid;
        }
    }
    if(left === nums.length || nums[left] !== target) return result;
    result[0] = left;
    right = nums.length;
    left = result[0];

    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        if (nums[mid] > target) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    result[1] = right - 1;
    return result;
};
```

---

#### 🔹Question 5: Leetcode_69

Describtion: Given a non-negative integer x, return the square root of x rounded down to the nearest integer. The returned integer should be non-negative as well. You must not use any built-in exponent function or operator.

##### ✅Method 1: Closed Interval [left, right]

```
var mySqrt = function(x) {
    if(x < 2) return x;  
    let left = 0, right = x;
    while (left <= right){
        const mid = Math.floor((left + right) / 2);
        const root = mid * mid;
        if (root > x) {
            right = mid - 1;
        } else{
            left = mid + 1;
        }
    }
    return left - 1;
};
```

##### ✅Method 2: Left-closed, Right-open [left, right)

```
var mySqrt = function(x) {
    if(x < 2) return x;
    let left = 0, right = x;
    while (left < right){
        const mid = Math.floor((left + right) / 2);
        const root = mid * mid;
        if (root > x) {
            right = mid;
        } else{
            left = mid + 1;
        }
    }
    return left - 1;
};
```
---

# Two Pointers

**Key Points:**
- Use two pointers: `fast` scans the array, `slow` marks the position to overwrite.
- Loop condition: `fast < nums.length`
- When `nums[fast] !== target`, assign `nums[slow] = nums[fast]` and move `slow++`
- When `nums[fast] === target`, skip (do not move `slow`)
- Final value of `slow` is the new length of the array

## Related Questions

### 🔹Question 1: Leetcode_27

Describtion: Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

```
var removeElement = function(nums, val) {
    let slow = 0;
    for (let fast = 0; fast < nums.length; fast++) {
        if (nums[fast] !== val) {
            nums[slow] = nums[fast];
            slow ++;
        }
    }
    return slow;
};
```

---

### 🔹Question 2: Leetcode_283

Describtion: Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

```
var moveZeroes = function(nums) {
    let slow = 0;
    for (let fast = 0; fast < nums.length; fast++) {
        if (nums[fast] !== 0) {
            nums[slow++] = nums[fast];
        }      
    }
    nums.fill(0,slow);
    return nums;
};
```

---

### 🔹Question 3: Leetcode_844

Describtion: Given two strings s and t, return true if they are equal when both are typed into empty text editors. '#' means a backspace character.

```
var backspaceCompare = function(s, t) {
    let i = s.length - 1, j = t.length - 1;
    let skipS = 0, skipT = 0;
    while (i >= 0 || j >= 0){
        while (i >= 0){
            if (s[i] === "#") {
                i--;
                skipS++;
            } else if (skipS > 0) {
                skipS--;
                i--;
            } else {
                break;
            }
        }
        while (j >= 0) {
            if (t[j] === "#") {
                j--;
                skipT++;
            } else if (skipT > 0) {
                skipT--;
                j--;
            } else {
                break;
            }
        }
        if (s[i] !== t[j]) return false;
        i--;
        j--;
    }
    return true;
};
```

---

### 🔹Question 4: Leetcode_977

Describtion: Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

#### Method: two pointers
```
var sortedSquares = function(nums) {
    let arr = new Array (nums.length);
    let left = 0, right = arr.length - 1, pos = arr.length - 1; 
    while (left <= right) {
        const leftarr = nums[left] * nums[left];
        const rightarr = nums[right] * nums[right];
        if (leftarr > rightarr) {
            arr[pos] = leftarr;
            pos--
            left++;
        } else {
            arr[pos] = rightarr;
            pos--
            right--;
        }
    }
    return arr;
};
```

#### Method: brute-force

```
var sortedSquares = function(nums) {
    let newNums = [];

    for (let i = 0; i < nums.length; i++) {
        newNums.push(nums[i]*nums[i]);
    }

    return newNums.sort((a,b) => a-b);
    
};
```
---



