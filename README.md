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

## Design Linked List

### Related Questions

**Key Points**


#### 🔹Question 1: Leetcode_707

```

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
    let left = 0;
    let right = 0;
    let sum = 0;
    let ans = Infinity;

    while (right < nums.length ) {
        sum = sum + nums[right];

        while (sum >= target) {
            ans = Math.min (ans, right - left + 1);
            sum = sum - nums[left]
            left ++;
        }
        right ++;
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
      left = mid + 1;
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



