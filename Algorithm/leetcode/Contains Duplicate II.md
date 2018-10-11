Given an array of integers and an integer *k*, find out whether there are two distinct indices *i* and *j* in the array such that `nums[i]` = `nums[j]` and the absolute difference between *i* and *j* is at most *k*.

Example1:
```
Input: nums = [1,2,3,1], k = 3
Output: true
```

Example2:
```
Input: nums = [1,0,1,1], k = 1
Output: true
```

$O(n^2)$ Solution

```js
class Solution {
public:
  bool containsNearbyDuplicate(vector<int>& nums, int k) {
    int n = nums.size();
    for(int i = 0; i < n; i++) {
      int j = i - 1;
      while(j >= 0) {
        if (abs(j - i) > k) break;
        if (nums[i] == nums[j]) return true;
        --j;
      }
    }
    return false;
}
```

time limit

$O(n)$ Solution using Hash Map - 70.84%

```js
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        int n = nums.size();
        unordered_map<int, int> h;  // key-value = number-index
        for(int i = 0; i < n; i++) {
            if (h.find(nums[i]) != h.end()) {
                if (i - h[nums[i]] <= k) return true;
                else h[nums[i]] = i;
            }
            else {
                h[nums[i]] = i;
            }
        }
        return false;
    }
};
```



Another Solution - 97%

```js
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        int len = nums.size();
        int dest;
        unordered_map<int, int> hash_map;
        
        for (int i = 0; i < len; i++) {
            if (hash_map.find(nums[i]) == hash_map.end()) {
                hash_map.insert(make_pair(nums[i], i));
            }
            else {
                for (int j = i - 1; j >= 0; j--) {
                    if (hash_map[nums[i]] == hash_map[nums[j]]) {
                        dest = i - j;
                        if (dest <= k) return true;
                    }
                }
                hash_map.insert(make_pair(nums[i], i));
            }
        }
        return false;
    }
};
```



