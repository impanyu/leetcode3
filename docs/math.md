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

## [963. Minmum Area Rectangle II](https://leetcode.com/problems/minimum-area-rectangle-ii/)
1. use a long int to represent the point
2. iterate through all 3 points, try to construct the fourth point, check if it's a rectangle.


```c++
class Solution {
public:
    double minAreaFreeRect(vector<vector<int>>& points) {
        int N = points.size();

        long A[N];
        unordered_set<long> point_set;
        for (int i = 0; i < N; ++i) {
            A[i] =points[i][0]*40001+points[i][1];
            point_set.insert(A[i]);
        }

        double ans = 1e20;
        for (int i = 0; i < N; ++i) {
            long p1 = A[i];
            long p1_x = p1/40001;
            long p1_y = p1%40001;
            for (int j = 0; j < N; ++j) 
                if (j != i) {
                    long p2 = A[j];
                    long p2_x = p2/40001;
                    long p2_y = p2%40001;
                    for (int k = j+1; k < N; ++k) 
                        if (k != i) {
                            long p3 = A[k];
                            long p3_x = p3/40001;
                            long p3_y = p3%40001;
                            long p4_x= p2_x+p3_x-p1_x;
                            long p4_y=p2_y+p3_y-p1_y;
                            long p4=p4_x*40001+p4_y;

                            if (point_set.count(p4)) {
                                long dot = ((p2_x - p1_x) * (p3_x - p1_x) +
                                           (p2_y - p1_y) * (p3_y - p1_y));
                                if (dot == 0) {
                                    double area = distance(p1_x,p1_y,p2_x,p2_y)*distance(p1_x,p1_y,p3_x,p3_y);
                                    if (area < ans)
                                        ans = area;
                                }
                            }
                    }
            }
        }

        return ans < 1e20 ? ans : 0;
    }
    
    double distance(double p1_x,double p1_y, double p2_x, double p2_y){
        return sqrt((p1_x-p2_x)*(p1_x-p2_x)+(p1_y-p2_y)*(p1_y-p2_y));
    }
};
```
