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


## [43. Multiply Strings](https://leetcode.com/problems/multiply-strings/)
1. regular multiplication, accumulate the position based on the position of num1 and num2
2. use vector to represent the result, never use int/long directly



```c++
class Solution {
public:
    string multiply(string num1, string num2) {
        int i, j;
        int m = num1.size(), n = num2.size();
        // max (m + n) digits
        vector<int> product(m + n, 0);
        string result;

        // reverse for ease of calc
        reverse(num1.begin(), num1.end());
        reverse(num2.begin(), num2.end());

        // digit i * digit j contributes to digit i + j
        for (i = 0; i < m; i++) {
            for (j = 0; j < n; j++) {
                product[i + j] += (num1[i] - '0') * (num2[j] - '0');
                product[i + j + 1] += product[i + j] / 10;
                product[i + j] %= 10;
            }
        }

        // remove leading 0; keep last 0 if all 0
        for (i = m + n - 1; i > 0 && 0 == product[i]; i--)
            ;
        
        for (; i >= 0; i--)
            result += to_string(product[i]);

        return result;
    }
};
```


## [67.Add Binary](https://leetcode.com/problems/add-binary/)
1. add from right to left, update carry accordingly

```c++
class Solution {
public:
    string addBinary(string a, string b) {
        string ans="";
        int i=a.size()-1;
        int j=b.size()-1;
        int carry=0;
        
        while(i>=0||j>=0){
            
            if(i>=0) carry+=a[i]-'0';
            if(j>=0) carry+=b[j]-'0';
            ans=(char)(carry%2+'0')+ans;
            carry/=2;
            i--;
            j--;
        }    
        if(carry>0) ans='1'+ans;
        return ans;
    }
};
```
