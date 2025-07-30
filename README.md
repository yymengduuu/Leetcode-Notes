# Array

## Two Pointers

### Key Points:
- Use two pointers: `fast` scans the array, `slow` marks the position to overwrite.
- Loop condition: `fast < nums.length`
- When `nums[fast] !== target`, assign `nums[slow] = nums[fast]` and move `slow++`
- When `nums[fast] === target`, skip (do not move `slow`)
- Final value of `slow` is the new length of the array

### Related Questions

#### 🔹Question 1: Leetcode_27

Describtion: Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

```
var removeElement = function(nums, val) {
    let slow = 0;
    for (let fast = 0; fast < nums.length; fast++) {
        if (nums[fast] !== val) {
            nums[slow++] = nums[fast];
        }
    }
    return slow;
};
```

---

#### 🔹Question 2: Leetcode_283

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

#### 🔹Question 3: Leetcode_844

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

#### 🔹Question 4: Leetcode_977

Describtion: Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

```
var sortedSquares = function(nums) {
    let arr = new Array (nums.length);
    let left = 0, right = arr.length - 1, pos = arr.length - 1; 
    while (left <= right) {
        const leftarr = nums[left] * nums[left];
        const rightarr = nums[right] * nums[right];
        if (leftarr > rightarr) {
            arr[pos--] = leftarr;
            left++;
        } else {
            arr[pos--] = rightarr;
            right--;
        }
    }
    return arr;
};
```

---

## Binary Search

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

Key Points:
- Loop condition: left <= right
- On each iteration, we shrink the range by updating either left or right
- Final result: if found, return index; if not, return -1


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

---

💡Key Points:
- Loop condition: left < right
- Right is exclusive, so we don’t use nums.length - 1 like Method 1
- Commonly used for finding insert positions or first occurrence

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

