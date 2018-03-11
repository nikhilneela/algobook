# Problem

#### Given a binary array, find the largest sub array within this array which has equal number of ones and zeros.

# Solution

A bruteforce approach to this problem is to find all substring and check whether equal number of ones and zeros exist in substrings and record the maximum found so far and finally return the maximum. This solution requires O\(n2\) time complexity. A slight improvement can be made to this brute force approach by \*NOT\* computing the number of zeros and ones again and again by storing the number of 1s and 0s encountered thus far. This is a dynamic programming approach. This requires to use a table to store the number of 1s and 0s at every substring. But this is still O\(n2\) as we need to find for every substring. However, there is a linear time solution.It is very obvious that if we take a count and increment it by 1 when a 1 is seen and decrement it when a 0 is seen, then whenever the count equals to a value that it was initialized with, then it means that there are equal number of 1s and 0s in that interval.



