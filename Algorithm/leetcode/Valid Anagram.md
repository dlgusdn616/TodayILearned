Given two strings s and t , write a function to determine if t is an anagram of s.

```
Example 1:
Input: s = "anagram", "nagaram"
Output: true
```

```
Example 2:
Input: s = "rat", t = "car"
Output: false
```

Note:
You may assume the string contains only lowercase alphabets.

Follow up:
What if the inputs contain unicode characters? How would you adapt your solution to such case?

$O(nlog(n))$ Solution

```js
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size()) return false;
        sort(s.begin(), s.end());
        sort(t.begin(), t.end());
        return s == t;
    }
};
```

$O(n)$ Solution using Hash Map

```js
class Solution {
public:
    bool isAnagram(string s, string t) {
         unordered_map<char, int> H;
        for (int i = 0; i < s.size(); i++) {
            H[s[i]]++;
        }
        for (int i = 0; i < t.size(); i++) {
            H[t[i]]--;
        }
        for (auto elem : H) {
            if (elem.second != 0) return false;
        }
        return true;
    }
};
```

Another Solution

```js
class Solution {
public:
    bool isAnagram(string s, string t) {
        // ASCII
        vector<int> H(128, 0);
        for (int i = 0; i < s.size(); i++) {
          H[s[i]]++;
        }
        for (int i = 0; i < t.size(); i++) {
          H[t[i]]--;
        }
        for (int i = 0; i < 128; i++) {
          if (H[i] != 0) return false;
        }
        return true;
    }
};
```

