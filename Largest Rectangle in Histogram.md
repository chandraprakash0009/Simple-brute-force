## 84. Largest Rectangle in Histogram

Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

 

Example 1:


Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation: The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.
Example 2:


Input: heights = [2,4]
Output: 4
 

Constraints:

1 <= heights.length <= 10^5
0 <= heights[i] <= 10^4

### Approach 

Maintain a monotonic stack and two array - left and right to keep track of first index at which height is less than the current height in left and right direction respectively.

Now for each index, find the area of rectangle by using the ith height and width from left and right array.

### Java code - 
```
class Solution {
    public int largestRectangleArea(int[] heights) {
        Stack<Integer> st = new Stack<>();
        int n = heights.length;
        int left[]=new int[n];  // this will store the first index in left of current index at which height is less than current height.
        int right[]=new int[n];  // this will store the first index in right of current index at which height is smaller than current element
        Arrays.fill(left,-1);
        Arrays.fill(right,n);
        for(int i=0; i<n; i++){
            while(!st.isEmpty() && heights[st.peek()]>=heights[i]){
                right[st.pop()]=i;
            }
            if(!st.isEmpty()){
                left[i]=st.peek();
            }
            st.push(i);
        }
        int max=-1;
        for(int i=0; i<n; i++){
            max=Math.max(max, heights[i]*(right[i]-left[i]-1));
        }
        return max;
    }
}
