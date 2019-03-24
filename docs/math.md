## [1022. Smallest Integer Divisible by K](https://leetcode.com/contest/weekly-contest-129/problems/smallest-integer-divisible-by-k/)
```c++
class Solution {
//1.try each N in mod space
public:
    int smallestRepunitDivByK(int K) {
        int value = 0, length = 0;

        for (int i = 0; i < 1e5; i++) {
            value = (10 * value + 1) % K;
            length++;

            if (value == 0)
                return length;
        }

        return -1;
    }
};
```
