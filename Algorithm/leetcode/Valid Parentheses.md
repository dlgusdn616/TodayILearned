Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

Example 2:
```
Input: "()[]{}"
Output: true
```

Example 4:
```
Input: "([)]"
Output: false
```

$O(n^2) Solution (recursive)$

```js
class Solution {
public:
    bool isValid(string& s) {
        if (s.size() == 0) return true;
        if (s.find("()") != string::npos) {
            int index = s.find("()");
            s.replace(index, 2, "");
            return isValid(s);
        }
        else if (s.find("{}") != string::npos) {
            int index = s.find("{}");
            s.replace(index, 2, "");
            return isValid(s);
        }
        else if (s.find("[]") != string::npos) {
            int index = s.find("[]");
            s.replace(index, 2, "");
            return isValid(s);
        }
        else {
            return false;
        }
    }
};
```

$O(n) Solution$

```c++
class Solution {
public:
    bool isValid(string& s) {
        if (s.size() == 0) return true;
        if (s.size() == 1) return false; // ")" || "("
        
        // can assume s.size() >= 2
        int n = s.size();
        
        stack<char> st;
        // if first elem in s is ')' || '}' || ']' return false
        if (s[0] == ')' || s[0] == '}' || s[0] == ']') {
            return false;
        }
        else {
            st.push(s[0]);
        }
        
        for (int i = 1; i < n; i++) {
            if (s[i] == ')' || s[i] == '}' || s[i] == ']') {
                // pop elements
                if (st.empty()) {
                    return false;
                }
                else {
                    if (st.top() == '(' && s[i] == ')' || st.top() == '{' && s[i] == '}' || st.top() == '[' && s[i] == ']')
                        st.pop();
                    else {
                        return false;
                    }
                }
            }
            else {
                st.push(s[i]);
            }
        }
        
        if (st.empty()) return true;
        else return false;
    }
};
```





