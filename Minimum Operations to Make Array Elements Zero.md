## 3495. Minimum Operations to Make Array Elements Zero

You are given a 2D array queries, where queries[i] is of the form [l, r]. Each queries[i] defines an array of integers nums consisting of elements ranging from l to r, both inclusive.

In one operation, you can:

Select two integers a and b from the array.  
Replace them with floor(a / 4) and floor(b /  4).  
Your task is to determine the minimum number of operations required to reduce all elements of the array to zero for each query. Return the sum of the results for all queries.

 

Example 1:

Input: queries = [[1,2],[2,4]]

Output: 3

Explanation:

For queries[0]:

The initial array is nums = [1, 2].
In the first operation, select nums[0] and nums[1]. The array becomes [0, 0].
The minimum number of operations required is 1.
For queries[1]:

The initial array is nums = [2, 3, 4].
In the first operation, select nums[0] and nums[2]. The array becomes [0, 3, 1].
In the second operation, select nums[1] and nums[2]. The array becomes [0, 0, 0].
The minimum number of operations required is 2.
The output is 1 + 2 = 3.

Example 2:

Input: queries = [[2,6]]

Output: 4

Explanation:

For queries[0]:

The initial array is nums = [2, 3, 4, 5, 6].
In the first operation, select nums[0] and nums[3]. The array becomes [0, 3, 4, 1, 6].
In the second operation, select nums[2] and nums[4]. The array becomes [0, 3, 1, 1, 1].
In the third operation, select nums[1] and nums[2]. The array becomes [0, 0, 0, 1, 1].
In the fourth operation, select nums[3] and nums[4]. The array becomes [0, 0, 0, 0, 0].
The minimum number of operations required is 4.
The output is 4.

 

Constraints:

1 <= queries.length <= 105
queries[i].length == 2
queries[i] == [l, r]
1 <= l < r <= 109


### Intuition
To reduce a number n to 0 by repeated division by say x we need floor(logₓn) + 1 operations. So we can group numbers based on the no. of operations they need.  
All numbers in [1, 3] (i.e., from 4⁰ to 4¹ - 1) require 1 division.  
All numbers in [4, 15] (i.e., from 4¹ to 4² - 1) require 2 divisions.   
All numbers in [16, 63] (i.e., from 4² to 4³ - 1) require 3 divisions.   
and so on   
My solution is based on this observation only...  

### Approach  
Now 4¹⁶ is greater than 1e9 so we need to atmost 16 intervals which is sufficient to cover all the numbers given in the range [l, r] of each query.  
Which is what is done in code, i.e., for each query:   
d represents the number of divisions required for numbers in a particular range.  
prev is set to 4 ^ (d - 1) (the start of the interval).  
cur is 4 ^ d (so the interval is [prev, cur − 1]).   
now we check overlap of numbers with each interval and add to operations the no. of divisions required i.e., (length of interval) * d.  
Then, we slide the interval (prev = cur) --> d = 1 : [1, 3] -> d = 2 : [4, 15] -> d = 3 : [16, 63]...  
Finally, since we can choose 2 numbers at a time we would need ceil(total operations / 2).   

### Complexity   

Time complexity: O(queries.length)

Space complexity: O(1)

### java code
```
class Solution {
    public long minOperations(int[][] nums) {
        long ans=0;
        for(int i=0; i<nums.length; i++){
            // finding the range
            long beg = nums[i][0], end=nums[i][1];
            long prev=1;
            long count=0;
            // for arr range i.e. 4^(n-1) to (4^n)-1  
            for(int pow=1; pow<17; pow++){
                long cur = prev*4;
                // find the common range if there is any
                long l= Math.max(beg,prev);
                long r = Math.min(end,cur-1);
                if(r>=l){
                    // count is updated in such a way that we pick one number at a time
                    count += (r-l+1)*pow;
                    
                }
                prev=cur;
            }
            // since we have picked one element at a time but we can pick two number at a time, so find the ceiling number by dividing it by 2
            ans+= (count+1)/2;
        }
        return ans;
    }
}
