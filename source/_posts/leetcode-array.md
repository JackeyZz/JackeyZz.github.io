---
title: leetcode_array
date: 2018-04-17 00:40:39
tags: [array]
categories: [leetcode整理总结]
---
## 11.Container With Most Water
### 题目解析
给定一个数组$a_n$，相对应产生数组长度数量的点$(i,a_i)$，各个点$(i,a_i)$与点$(i,0)$组成$n$条线段，由任意两条线段加上$x$轴组成一个水桶，判断能够盛水最大容量桶的面积是多少？
### 解题思路
由数组左右两边出发，求解较短线段对应的面积，再判断左右两边线段的长短：
- 若$left>right$，此时如果判断$left$的右边的下一条线段$left_{next}$：
  - 若$left>left_{next}$，那么只有两种情况：
    - $left_{next}>right$，那么面积不变
    - $left_{next}<right$，那么面积减小，对于最大面积的求解没有意义
  - 若$left<left_{next}$，那么面积不变
  综上，判断$left_{next}$没有意义，因此选择判断$right_{next}$
- 若$left<right$，同时，选择判断$left_{next}$
- 直至$left==right$，返回最大面积

### leetcode代码
```java
class Solution {
    public int maxArea(int[] height) {
        int area=0;
        int l=0;
        int r=height.length-1;
        while(l<r&&l>=0&&r<=height.length-1){
            area=Math.max(area,Math.min(height[r],height[l])*(r-l));
            if(height[l]>height[r]){
                r--;
            }else{
                l++;
            }
        }
        return area;
    }
}
```
## 16.3Sum Closest
### 题目解析
给定一个数组$a_n$和一个目标值$target$，要求找到数组中的三个值之和最接近目标值，返回这三个值的和。
### 解题思路
先对数组进行排序，再对数组$a_{n-2}$遍历，遍历期间对于后面的数组进行首尾出发判断：
- 若$sum>target$，那么先判断接近值是多少，并更新最小接近值，同时执行$right--$(这是因为数组已经排序，要接近目标值，下一步只能判断$right_{next}$，才会使得$sum$减小，从而出现更接近目标值的和)
- 若$sum<target$，同理，先判断并更新最小接近值，同时执行$left++$

直至遍历完成，返回得到的最小接近值。
### leetcode代码
```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        if(nums.length<3){
            return 0;
        }
        Arrays.sort(nums);
        int closeDic=Integer.MAX_VALUE;
        int result=0;
        for(int i=0;i<nums.length-2;i++){
            if(i>0&&nums[i]==nums[i-1])
                continue;
            int left=i+1;
            int right=nums.length-1;
            int sum=0;
            while(left<right){
                sum=nums[i]+nums[left]+nums[right];
                if(sum>target){
                    if((sum-target)<closeDic){
                        closeDic=sum-target;
                        result=sum;
                    }
                    right--;
                }else if(sum<target){
                    if((target-sum)<closeDic){
                        closeDic=-sum+target;
                        result=sum;
                    }
                    left++;
                }else{
                    return sum;
                }
            }
        }
        return result;
    }
}
```
## 41.First Missing Positive
### 题目解析
给定一个未排序的数组$a_n$，找到这个数组第一个丢失的正整数。要求时间复杂度为$O(n)$，空间复杂度为常数。
### 解题思路
由于时间复杂度为$O(n)$，因此不可能使用嵌套循环，只能一层遍历。又由于空间复杂度为常数，因此只能在原数组中进行处理。思路是遍历整个数组，判断元素值$a[i]$是否等于$i+1$，若不等于，则与$a[a[i]-1]$交换，此时元素$a[a[i]-1]$必然满足元素值等于下标索引+1，然后继续判断直至满足判断条件才接下去遍历。遍历完成后，再重新遍历一次，判断第一个不符合判断条件的即找到了结果。
### leetcode代码
```java
class Solution {
public int firstMissingPositive(int[] A) {  
    if(A==null || A.length==0)  
    {  
        return 1;  
    }  
    for(int i=0;i<A.length;i++)  
    {  
        if(A[i]<=A.length && A[i]>0 && A[A[i]-1]!=A[i])  
        {  
            int temp = A[A[i]-1];  
            A[A[i]-1] = A[i];  
            A[i] = temp;  
            i--;  
        }  
    }  
    for(int i=0;i<A.length;i++)  
    {  
        if(A[i]!=i+1)  
            return i+1;  
    }  
    return A.length+1;  
}  
};
```
## 55.Jump Game
### 题目解析
给定一个正整数的数组$a[n]$，初始位置在第一个元素，每一个元素表示你可以跳跃的最大步长，例如$a[0]=2$，那么你可以跳跃到的位置有$a[1]$、$a[2]$。最终返回的结果是你是否可以跳到最后一个元素位置。
### 解题思路
动态规划的特点：  
- 每遍历一个元素都有可能影响最后的结果
- 都有局部解和最终解
- 都是通过解决局部解这个小问题而逐渐解决最终解的大问题

遍历$1->(nums.length-2)$，局部解就是$i+nums[i]$，遍历过程中保留当前能够跳跃的最远位置$max$，并进行两个判断
- 判断保留的$max$是否允许进行下一个元素的遍历，如果连下一个元素都无法达到，直接返回$false$
- 判断保留的$max$是否达到$nums.length$或者更大，则返回$true$

### leetcode代码
```java
class Solution {
    public boolean canJump(int[] nums) {
        if(nums.length==0){
            return false;
        }
        if(nums.length==1){
            return true;
        }
        int max=0;
        for(int i=0;i<nums.length-1;i++){
            max=Math.max(max,i+nums[i]);
            if(max<i+1){
                return false;
            }
            if(max>=(nums.length-1)){
                return true;
            }
        }
        return false;
    }
}
```
## 45.Jump Game II
### 题目解析
给定一个正整数的数组$a[n]$，初始位置在第一个元素，每一个元素表示你可以跳跃的最大步长，例如$a[0]=2$，那么你可以跳跃到的位置有$a[1]$、$a[2]$。最终返回到达最后一个元素位置的最小跳转次数
### 解题思路
在$Jump\quad Game$思路中，将全局最优转换成step(reach)最优和step-1(lastreach)最优。当走到超过step-1步最远的位置时，说明了step-1(lastreach)不能到达当前一步，则更新步数step+1。时间复杂度为$O(n)$，空间复杂度为$O(1)$
### leetcode代码
```java
class Solution {
    public int jump(int[] nums) {
        if(nums==null||nums.length==0){
            return 0;
        }
        int lastreach=0;
        int reach=0;
        int step=0;
        for(int i=0;i<=reach&&i<nums.length;i++){
            if(i>lastreach){
                step++;
                lastreach=reach;
            }
            reach=Math.max(reach,nums[i]+i);
        }
        if(reach<nums.length-1)
            return 0;
        return step;
    }
}
```
## 54.Spiral matrix
### 题目解析
给定一个$m*n$的矩阵，按照螺旋顺序返回包含所有元素的一维数组。
### 解题思路
**解法1**
按照上下左右遍历，根据遍历的上下左右偏移数目找到遍历的行列数，并将每个元素都赋给List集合，判断上下或者左右的偏移数目之和是否等于行数或者列数，跳出循环，返回结果。
**解法2**
遍历所有的元素，根据两个数组 $[0,1,0,-1]$与$[1,0,-1,0]$ 作为步长实现螺旋顺序取数赋值，最后返回结果。时间复杂度与空间复杂度都为 $O(m*n)$。
### leetcode代码
**解法1**
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result=new ArrayList<Integer>();
        if(matrix.length==0){
            return result;
        }
        int m=matrix.length;
        int n=matrix[0].length;
        int top=0;
        int bottom=0;
        int left=0;
        int right=0;
        while(true){
            //top
            for(int i=left;i<n-right;i++){
                result.add(matrix[top][i]);
            }
            top++;
            if((top+bottom)==m){
                break;
            }
            //right
            for(int i=top;i<m-bottom;i++){
                result.add(matrix[i][n-right-1]);
            }
            right++;
            if((left+right)==n){
                break;
            }
            //bottom
            for(int i=n-right-1;i>=left;i--){
                result.add(matrix[m-bottom-1][i]);
            }
            bottom++;
            if((top+bottom)==m){
                break;
            }
            for(int i=m-bottom-1;i>=top;i--){
                result.add(matrix[i][left]);
            }
            left++;
            if((left+right)==n){
                break;
            }
        }
        return result;
    }
}
```
**解法2**
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List ans = new ArrayList();
        if (matrix.length == 0) return ans;
        int R = matrix.length, C = matrix[0].length;
        boolean[][] seen = new boolean[R][C];
        int[] dr = {0, 1, 0, -1};
        int[] dc = {1, 0, -1, 0};
        int r = 0, c = 0, di = 0;
        for (int i = 0; i < R * C; i++) {
            ans.add(matrix[r][c]);
            seen[r][c] = true;
            int cr = r + dr[di];
            int cc = c + dc[di];
            if (0 <= cr && cr < R && 0 <= cc && cc < C && !seen[cr][cc]){
                r = cr;
                c = cc;
            } else {
                di = (di + 1) % 4;
                r += dr[di];
                c += dc[di];
            }
        }
        return ans;
    }
}
```
## 62.Unique Paths
### 题目解析
给定一个$m*n$的二维数组，以左上角为起点，只能right或者down移动，最终结束点在二维数组的右下角位置，返回所有路线的总和。
### 解题思路
**解法1**
在二维数组矩阵的内部某一元素，到达这一元素位置的路线总和必然是它左边元素位置的路线总和与它上边的元素位置路线总和相加得到，而矩阵边上的元素(第一行和第一列的元素)位置的路线只能是一种。遍历二维数组，将每个元素位置上对应的路线总和赋给对应位置的新矩阵中，最后得到一个对应原矩阵的路线新矩阵，返回新矩阵的右下角的元素值。
**解法2**
使用排列组合的方法。从起点走到终点，它只能right或者down，那么不管它怎么走，必然向右走了$n-1$步，向下走了$m-1$步，则一共走了$n-1+m-1$步，而不同的走法本质上是向右或者向下构成的$m-1+n-1$长度的序列不同，因此题目可以转换成在总步数中选出$n-1$代表向右走的走法个数或者选出$m-1$代表向下走的走法个数，即$C_{n-1+m-1}^{n-1}=C_{n-1+m-1}^{m-1}=\dfrac{(n-1+m-1)\cdot...\cdot(n)}{(m-1)\cdot...\cdot2\cdot1}$
### leetcode代码
**解法1**
```java
class Solution {
    public int uniquePaths(int m, int n) {
        if(m==0 || n==0){
            return 0;
        }
        if(m==1 || n==1){
            return 1;
        }
        int[][] result=new int[m][n];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i==0 || j==0){
                    result[i][j]=1;
                }else{
                    result[i][j]=result[i-1][j]+result[i][j-1];
                }
            }
        }
        return result[m-1][n-1];
    }
}
```
**解法2**
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int a = m > n ? m - 1 : n - 1;
        int b = m > n ? n - 1 : m - 1;
        long result = 1;
        for (int i = 0; i < b; i++) {
            result *= a + b - i;
        }
        for (int i = 1; i <= b; i++) {
            result /= i;
        }
        return (int) result;
    }
}
```
## 63.Unique Paths II
### 题目解析
在**Unique Paths**的基础上，增加障碍物导致一些路线无法通过。给定一个元素值为0和1的二维数组矩阵，其中1代表障碍物，无法通过，返回到达终点的所有可能路线总和。
### 解题思路
**解法1**
考虑两种情况：
- 矩阵边上存在障碍物。一旦矩阵边上某一位置存在障碍物，那么该位置后面的边上位置都无法通过。
- 矩阵内部存在障碍物。只能表示该位置无法通过。

而对于矩阵非边上的某一位置，根据动态规划，该位置的路线总和是其左边位置和上边位置的路线之和，只有毗邻的这两个位置都无法通过，这位置才会无法通过。
空间复杂度是$O(n*m)$
**解法2**
这个方法主要是降低了空间复杂度，为$O(n)$。对于空间可以看出我们每次只需要用到上一行当前列以及前一列当前行的信息，因此我们只需要用一个一维数组存上一行的信息即可，因为前一列当前行的信息在进行遍历的时候可以直接获取，然后在遍历下一行的时候使用一维数组信息并更新信息，所以空间复杂度为$O(n)$。(也可以选择行列较小那个，将其放置内层循环，则空间复杂度为$O(min(m,n))$)。
### leetcode代码
**解法1**
```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        if(obstacleGrid.length==0){
            return 0;
        }
        if(obstacleGrid[0][0]==1){
            return 0;
        }
        int m=obstacleGrid.length;
        int n=obstacleGrid[0].length;
        int[][] result=new int[m][n];
        int key=0;
        for(int i=0;i<n;i++){
            if(obstacleGrid[0][i]==1 || key==1){
                result[0][i]=0;
                key=1;
            }else{
                result[0][i]=1;
            }
        }
        key=0;
        for(int i=1;i<m;i++){
            if(obstacleGrid[i][0]==1 || key==1){
                result[i][0]=0;
                key=1;
            }else{
                result[i][0]=1;
            }
        }
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                if(obstacleGrid[i][j]==1){
                    result[i][j]=0;
                }else{
                        result[i][j]=result[i-1][j]+result[i][j-1];
                }
            }
        }
        return result[m-1][n-1];
    }
}
```
**解法2**
```java
class Solution {
   public int uniquePathsWithObstacles(int[][] obstacleGrid) {  
    if(obstacleGrid == null || obstacleGrid.length==0 || obstacleGrid[0].length==0)  
        return 0;  
    int[] res = new int[obstacleGrid[0].length];  
    res[0] = 1;  
    for(int i=0;i<obstacleGrid.length;i++)  
    {  
        for(int j=0;j<obstacleGrid[0].length;j++)  
        {  
            if(obstacleGrid[i][j]==1)  
            {  
                res[j]=0;  
            }  
            else  
            {  
                if(j>0)  
                    res[j] += res[j-1];  
            }  
        }  
    }  
    return res[obstacleGrid[0].length-1];  
}  
}
```
