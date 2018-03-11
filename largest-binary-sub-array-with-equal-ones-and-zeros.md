# Problem

#### Given a binary array, find the largest sub array within this array which has equal number of ones and zeros.

# Solution

A bruteforce approach to this problem is to find all substring and check whether equal number of ones and zeros exist in substrings and record the maximum found so far and finally return the maximum. This solution requires O\(n2\) time complexity. A slight improvement can be made to this brute force approach by \*NOT\* computing the number of zeros and ones again and again by storing the number of 1s and 0s encountered thus far. This is a dynamic programming approach. This requires to use a table to store the number of 1s and 0s at every substring. But this is still O\(n2\) as we need to find for every substring. 

However, there is a linear time solution. It is very obvious that if we take a count and increment it by 1 when a 1 is seen and decrement it when a 0 is seen, then whenever the count equals to a value that it was initialized with, then it means that there are equal number of 1s and 0s in that interval.

Let's take an example to understand this further.

| **Index** | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Value** | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 1 |
| **Count** | -1 | -2 | -1 | -2 | -3 | -4 | -3 | -2 |

In the above example, count is initialized with zero and decremented by 1 when a zero is seen and incremented by 1 when a 1 is seen. At index 2, observe that count again became -1 \(It was -1 at index 0\). What  does this imply?. As said earlier, this means that there are equal number of zeros and ones in  this interval. Here the interval is \[1, 2\]. The same thing happens at index 3, where the count again becomes -2 \(It was -2 at index 1\). This means that there are equal number of zeros and ones in the interval \[2, 3\]. Similarly, at \[5, 6\] \(for count -3\). At index 7, count becomes -2, \(It was -2 also at index 1\) which means that there are equal number of 1s and 0s from index 1 to 7. The interval is \[2, 7\] whose length is **6**. 

##### How do we formalize an algorithm for this solution

Idea : Record the first occurrences \(indexes\) of the various values of count in a table and whenever the same value of count appears again, find the interval length by subtracting the index found from the table with the current index. Keep track of the largest interval found so far.

Let's run the algorithm on the example above. The initial state of the table will be like this. \[If this \(0, -1\) is confusing just sit back and think about why this is needed\]

| Key \(Count value\) | Value \(Index in array\) |
| :--- | :--- |
| 0 | -1 |

**Iteration 0**

Count becomes -1. Check whether a -1 key exists in the table. As it does not exist insert it into the table.

| Key \(Count value\) | Value \(index in array\) |
| :--- | :--- |
| 0 | -1 |
| -1 | 0 |

**Iteration 1**

Count becomes -2. Check whether a -2 key exists in the table. A it does not exist insert it into the table

| Key \(Count Value\) | Value \(index in array\) |
| :--- | :--- |
| 0 | -1 |
| -1 | 0 |
| -2 | 1 |

**Iteration 2**

Count becomes -1. Check whether a -1 exists in the table, As it exists, find the interval length by subtracting the index found from table from the current index. which 2 - 0 = 2. Record this as the current maximum length seen with equal 1s and 0s

**Iteration 3**

Count becomes -2. As it exists, find the interval length as 3 - 1 = 2. 

**Iteration 4**

Count becomes -3. Since it is not present in the table add it to the table.

| Key \(Count Value\) | Value \(index in array\) |
| :--- | :--- |
| 0 | -1 |
| -1 | 0 |
| -2 | 1 |
| -3 | 4 |

**Iteration 5**

Count becomes -4. Add it to the table

| Key \(Count Value\) | Value \(index in array\) |
| :--- | :--- |
| 0 | -1 |
| -1 | 0 |
| -2 | 1 |
| -3 | 4 |
| -4 | 5 |

**Iteration 6**

Count becomes -3. As it is present find the interval length as 6 - 4 = 2.

**Iteration 7**

Count becomes -2. As it is present find the interval length as 7 - 1 = 6. Update max as 6.



```cpp
class Solution {
public:
    int longestSubStringWithEqualOnesAndZeros(string& input) {
        unordered_map<int, int> countMap;
        int count = 0;
        int max = 0;
        countMap[0] = -1;
        
        for (int i = 0; i < input.size(); ++i) {
            count += input[i] == '0' ? -1 : 1;
            if (countMap.find(count) != countMap.end()) {
                max = std::max(max, i - countMap[count]);
            } else {
                countMap[count] = i;
            }
        }
        return max;
    }
};

```



