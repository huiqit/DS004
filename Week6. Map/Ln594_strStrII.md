[Link](https://www.lintcode.com/problem/strstr-ii/description)
```java
public int strStr2(String source, String target) {

    if (source == null || target == null) {
      return -1;
    }
    if (source.length() < target.length()) {
      return -1;
    }
    if (target.length() == 0) {
      return 0;
    }
    int mod = 100000;
    int hashCode = 0;
    int n = source.length();
    int m = target.length();
    int power = 1;
    for (int i = 0; i < m; i++) {
      power = (power * 33) % mod;
    }

    for (int i = 0; i < m; i++) {
        hashCode = (hashCode * 33 + target.charAt(i) - 'a') % mod;
    }
    int hashTwo = 0;
    for (int i = 0; i < n; i++) {
        //if (i < m - 1) {
            hashTwo = (hashTwo * 33 + source.charAt(i) - 'a') % mod;
        if (i < m - 1) {
            continue;
        }
        
        if (i >= m) {
        //hashTwo = (hashTwo * 33 + source.charAt(i) - 'a') % mod;
        hashTwo = hashTwo - ((source.charAt(i - m) - 'a') * power) % mod;
        if (hashTwo < 0) {
            hashTwo += mod;
        }
        }
        if (hashCode == hashTwo) {
            if (source.substring(i - m + 1, i + 1).equals(target)) {
                return i - m + 1;
            }
        }
    }
    return -1;
}
```
