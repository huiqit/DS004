[Link](https://www.lintcode.com/problem/two-sum-iii-data-structure-design/description)
```cpp
class TwoSum {
public:
    void add(int number) {
        s.insert(number);
    }
    bool find(int value) {
        for (auto a : s) {
            int cnt = a == value - a ? 1 : 0;
            if (s.count(value - a) > cnt) {
                return true;
            }
        }
        return false;
    }
private:
    unordered_multiset<int> s;
};
```
