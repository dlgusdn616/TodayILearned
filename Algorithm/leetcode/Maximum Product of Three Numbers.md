Given an integer array, find three numbers whose product is maximum and output the maximum product.

Example1:

```
Input: [1,2,3]
Output: 6
```

Example2:

```
Input: [1,2,3,4]
Output: 24
```

Note:

1. The length of the given array will be in range [3,104] and all elements are in the range [-1000, 1000].
2. Multiplication of any three numbers in the input won't exceed the range of 32-bit signed integer.

$O(nlog(n)) Solution$

```js
class Solution {
public:
    int maximumProduct(std::vector<int>& nums) {
        // step1: sort the nums vector
        sort(nums.begin(), nums.end()); // ascending order O(nlongn).
        
        // step2: calculate p1 and p2
        int n = nums.size();
        int p1 = nums[n - 1] * nums[n - 2] * nums[n - 3];
        int p2 = nums[0] * nums[1] * nums[n - 1];
        
        // step3: return max of p1 and p2
        return max(p1, p2);
    }
};
```

$O(n) Solution$

```c++
class Solution {
public:
    int maximumProduct(std::vector<int>& nums) {
        int max1, max2, max3, min1, min2;
        max1 = max2 = max3 = -1001;
        min1 = min2 = 1001;
        
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > max3) {
                max1 = max2;
                max2 = max3;
                max3 = nums[i];
            } else if (nums[i] > max2) {
                max1 = max2;
                max2 = nums[i];
            } else if (nums[i] > max1) {
                max1 = nums[i];
            }
            
            if (nums[i] < min1) {
                min2 = min1;
                min1 = nums[i];
            } else if (nums[i] < min2) {
                min2 = nums[i];
            }
        }
        int ret1 = max1 * max2 * max3;
        int ret2 = min1 * min2 * max3;
        
        if (ret1 > ret2)
            return ret1;
        else
            return ret2;
    }
};
```

