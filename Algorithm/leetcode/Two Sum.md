Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have *exactly* one solution, and you may not use the *same* element twice.

Example:
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

1. $O(n^2)$ Solution

```js
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> ret;
        int ei, ej;
        for (int i = 0; i < nums.size(); ++i) {
            ei = nums[i];
            for (int j = i + 1; j < nums.size(); ++j) {
                ej = nums[j];
                if (ei + ej == target) {
                    ret.push_back(i);
                    ret.push_back(j);
                    break;
                }
            }
            if (ei + ej == target) break;
        }
        return ret;
    }
};
```

2. $O(nlogn)$ Solution

C++의 자료 구조 중 하나인 Pair는 first, second로 이루어진 Struct와 같은 구조라고 생각해본다.

```c++
/*
struct pair {   // try to understand pair is similar to struct
    int first;
    int second;
};
*/

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        // Sort all numbers
        // i = 0, j = n - 1
        // if (nums[i] + nums[j] == target) return (i, j)
        // if (nums[i] + nums[j] > target) j--;
        // if if (nums[i] + nums[j] < target) i++;
        // [3,2,1] ==> [1,2,3] remember original indexes
        // pair<int, int> p;
        // p.first -> p.second -> int
        // Pair<int, int> nums[i], i
        vector<pair<int, int>> nums2;
        vector<int> ret;
        for (int i = 0; i < nums.size(); ++i) {
            pair<int, int> temp(nums[i], i);    // pair(value, index)
            nums2.push_back(temp);
        }
        
        // sort nums2;
        sort(nums2.begin(), nums2.end());
        
        // i = 0, j = n - 1;
        int i = 0;
        int j = nums2.size() - 1;
        while (i < j) {
            int sum = nums2[i].first + nums2[j].first;
            if (sum == target) {
                ret.push_back(nums2[i].second);
                ret.push_back(nums2[j].second);
                return ret;
            }
            else if (sum > target) --j;
            else                   ++i;
        }
    }
};
```

