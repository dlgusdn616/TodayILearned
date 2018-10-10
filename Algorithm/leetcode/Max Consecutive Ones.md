Given a binary array, find the maximum number of consecutive 1s in this array.

Example 1:

```
Input: [1,1,0,1,1,1]
Output: 3
Explaination: The first two digits or the last three digits are consecutive 1s. The maximum number of consecutive 1s is 3.
```
* The input array will only contain `0` and `1`.
* The length of input array is a positive integer and will not exceed 10,000.

$O(n^2 Solution)$

```js
class Solution {
public:
    int findMaxConsecutiveOnes(std::vector<int>& nums);
};

int Solution::findMaxConsecutiveOnes(std::vector<int> &nums) {
    int max_l = 0;
    int n = nums.size();
    for (int i = 0; i < n; i++) {
        if (nums[i] == 1) {
            int cur_l = 1;
            int r = i + 1;
            while (r < n && nums[r] == 1) {
                ++cur_l;
                ++r;
            }
            max_l = std::max(max_l, cur_l);
        }
    }
    return max_l;
}
```

$O(n) Solution$

```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int max_l = 0;
        int n = nums.size();
        int i = 0;
        while (i < n) {
            if (nums[i] == 1) {
                int cur_l = 1;
                int r = i + 1;
                while (r < n && nums[r] == 1) {
                    ++r;
                    ++cur_l;
                }
                // very important line
                i = r;
                max_l = max(max_l, cur_l);
            } else {
                ++i;
            }
        }
        return max_l;
    }
};
```