---
title: leetcode_day1
date: 2018-02-01 00:28:26
tags: [中位数,回文串,整型反转]
categories: [leetcode整理总结]
---
## leetcode problem 4~7
### problem 4--Median of Two Sorted Arrays
***QUESTION***  
There are two sorted arrays nums1 and nums2 of size m and n respectively.  
Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).  
**example1**
```
nums1 = [1, 3]
nums2 = [2]
The median is 2.0
```
**example2**

```
nums1 = [1, 2]
nums2 = [3, 4]
The median is (2 + 3)/2 = 2.5
```

***问题分析***   
这道题的难点在于运行程序的时间复杂度是O(log(m+n))。一开始看到题目时候的第一想法就是归并排序后取中间一个或者两个数的平均值，这就是中位数。但是归并排序的时间复杂度是O(nlgn)，显然不符合题目的要求，因此始终time error  
***java代码***  
**归并排序法**

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int l,mid;
        double result;
        l=nums1.length+nums2.length;
        mid=l/2;
        int[] arr=new int[l];
        if(nums1==null&&nums2!=null){
            System.arraycopy(nums2,0,arr,0,nums2.length);
        }
        else if(nums1!=null&&nums2==null){
            System.arraycopy(nums1,0,arr,0,nums1.length);
        }
        else{
            for(int x=0,y=0,i=0;i<l;i++){
                if(nums1[x]<nums2[y]){
                    arr[i]=nums1[x];
                    x=x+1;
                }else{
                    arr[i]=nums2[y];
                    y=y+1;
                }
                if(x>=nums1.length){
                    System.arraycopy(nums2,y,arr,i+1,nums2.length-y);
                    break;
                }
                if(y>=nums2.length){
                    System.arraycopy(nums1,x,arr,i+1,nums1.length-x);
                    break;
                }
            }
        }
        if(l%2==0){
            result=(arr[mid-1]+arr[mid])/2;
        }
        else{
            result=arr[mid];
        }
        return result;
    }
}
```

换个角度思考中位数的定义，其实可以将这个问题转换成搜索两个有序序列的第k个元素，当m+n为奇数时，k=(m+n)/2+1；为偶数时，k=(m+n)/2和(m+n)/2+1。而对于有序序列，对于一序列的前p个元素和二序列的前q个元素，令其满足p+q=k-1，此时总序列中必满足k-1个元素小于等于第k个元素，如此第p+1或者第q+1个元素就是我们搜索的第k个元素。  
因此我们可以利用分治法将问题规模缩小，令p=k/2-1,则q=k-p-1。如果一序列第p个元素小于二序列第q个元素，一序列的前p个元素肯定都小于第k个元素，因此可以将一序列的前p个元素全部抛弃，形成一个较短的新序列，再找重新生成的两个序列的第k-p个元素，依次递归。同理，若一序列第p个元素大于二序列第q个元素，则反之。  
递归终止条件:  
- 较短序列所有元素被抛弃，则返回较长序列的第k个元素
- 一序列第p个元素等于二序列第q个元素,此时总序列第p+q=k-1个元素的后一个元素就是目标元素。    
这里面还有一个细节，当抛弃其中一段较小序列时，此时另一序列并不需要整个序列进行递归，如else if(A[i+posA-1]<B[j+posB-1]) return findMid(A,B,i+posA,i2,j,j+posB-1,k-posA);中的j->j+posB-1。这是因为已经搜索出posA个元素，即使后面的元素都在二序列中选择，也只会搜索到j+posB-1,这是由于posA+posB=k。

**分治法/order statistics/topK**

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if((nums1.length+nums2.length)%2==1)
            return findMid(nums1,nums2,0,nums1.length-1,0,nums2.length-1,(nums1.length+nums2.length)/2+1);
        else
            return (findMid(nums1,nums2,0,nums1.length-1,0,nums2.length-1,(nums1.length+nums2.length)/2)    
               +findMid(nums1,nums2,0,nums1.length-1,0,nums2.length-1,(nums1.length+nums2.length)/2+1))/2.0;
    }
    private int findMid(int A[],int[] B,int i,int i2,int j,int j2,int k){
        int m=i2-i+1;
        int n=j2-j+1;
        if(m>n)
            return findMid(B,A,j,j2,i,i2,k);
        if(m==0)
            return B[j+k-1];
        if(k==1)
            return Math.min(A[i],B[j]);
        int posA=Math.min(k/2,m);
        int posB=k-posA;
        if(A[i+posA-1]==B[j+posB-1])
            return A[i+posA-1];
        else if(A[i+posA-1]<B[j+posB-1])
            return findMid(A,B,i+posA,i2,j,j+posB-1,k-posA);
        else
            return findMid(A,B,i,i+posA-1,j+posB,j2,k-posB);
    }
}
```

---
### problem 5--Longest Palindromic Substring
***QUESTION***  
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.  
**example1**

```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**example2**

```
Input: "cbbd"
Output: "bb"
```

***问题分析***   
这道题是输出最长回文子字符串  
***java代码***
- 两侧比较法  
这个方法是通过对字符串内外循环遍历进行回文字符串判断，判断方法是字符串两侧比较。由于这方法属于暴力解决问题，因此leetcode显示time limit exceeded。后续再尝试能否解决这个问题(关键应该在于substring)。

```java
class Solution {
    public String longestPalindrome(String s) {
        String longestPalindrome=null;
        int length=s.length();
        int Palen=0;
        if(length==1){
            longestPalindrome=s;
        }
        else{
            for(int i=0;i<length;i++){
            for(int j=i+1;j<length;j++){
                int len=j-i;
                String curr=s.substring(i,j+1);
                if(isPalindrome(curr)){
                    if(len>Palen){
                        Palen=len;
                        longestPalindrome=curr;
                    }
                }
            }
        }
        }
        return (longestPalindrome==null)?String.valueOf(s.charAt(0)):longestPalindrome;
    }
    public static boolean isPalindrome(String s){
        for(int i=0;i<s.length();i++){
            if(s.charAt(i)!=s.charAt(s.length()-1-i))
                return false;
        }
        return true;
    }
}
```

- 动态规划法  
定义状态方程和转移方程：
    - P[i,j]=0表示子字符串不是回文字符串，P[i,j]=1表示子字符串是回文串
    - P[i][i]=1
    - if(s[i]==s[j]) P[i][j]=P[i+1][j-1]
    - if(s[i]!=s[j]) P[i][j]=0  ]

根据状态方程和转移方程分情况：P[i][i],P[i][i+1],P[i][j]，然后再编写程序。此处有一点在leetcode上需要注意，由于一开始再每种情况里面使用substring求得回文串，这样就会导致time问题。如果使用索引的方法，获取最长回文串的起始索引点，以及回文串长度，这样就可以只使用一次substring获得目标字符串。

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s == null || s.length() == 1){
           return s;
        }
        int len = s.length();
         boolean[][] flags = new boolean[1000][1000];
        int start = 0;
         int maxlen = 0;
         for(int i=0; i<len; i++){
             flags[i][i] = true;
             if( i<len-1 && s.charAt(i) == s.charAt(i+1)){
                 flags[i][i+1] = true;
                 start = i;
                 maxlen = 2;
             }
         }
         for(int m = 3; m <= len; m++){
            for(int i = 0; i <= len-m; i++ ){
                 int j = i+m-1;
                 if(flags[i+1][j-1] && s.charAt(i)==s.charAt(j)){
                     flags[i][j] = true;
                     start = i;
                     maxlen = m;
                 }
             }
         }
         if(maxlen >=2 ){
             return s.substring(start, start+maxlen);
         }
        return String.valueOf(s.charAt(0));
    }
}
```

- 中心扩展法  
对整个字符串进行遍历，以每个字符为中心判断最长的回文字符串，最后获得目标回文串。  
需要注意的一点就是回文字符串有“abcba”与“abba”两种，因此需要判断两次回文串长度，即isPalindrome(s,i,i)与isPalindrome(s,i,i+1)。

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s.isEmpty()){
            return null;
        }
        if(s.length()==1){
            return s;
        }
        String longest=s.substring(0,1);
        for(int i=0;i<s.length();i++){
            String tmp=isPalindrome(s,i,i);
            if(tmp.length()>longest.length()){
                longest=tmp;
            }

            tmp=isPalindrome(s,i,i+1);
            if(tmp.length()>longest.length()){
                longest=tmp;
            }
        }
        return longest;
    }
    public static String isPalindrome(String s,int begin,int end){
        while(begin>=0&&end<=s.length()-1&&s.charAt(begin)==s.charAt(end)){
            begin--;
            end++;
        }
        String subS=s.substring(begin+1,end);
        return subS;
    }
}
```

---
### problem 6--ZigZag Conversion
***QUESTION***  
The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)  

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: "PAHNAPLSIIGYIR"
Write the code that will take a string and make this conversion given a number of rows:  

```
string convert(string text, int nRows);
```

convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".   
***问题分析***   
这是一道规律题。首先题目要求是将字符串竖的Z型摆放，然后读每一行的字符，最后拼凑成输出结果。  
规律如下：所有行重复周期是2*nRows-2;中间行为2*nRows-2-2*i。  

***java代码***

```java
class Solution {
    public String convert(String s, int numRows) {
        int len = s.length();  
        if (len == 0 || numRows < 2) return s;  

        String ret = "";  
        int lag = 2*numRows - 2;
        for (int i = 0; i < numRows; i++) {  
            for (int j = i; j < len; j += lag) {  
                ret += s.charAt(j);  

                if (i > 0 && i < numRows-1) {  
                    int t = j + lag - 2*i;  
                    if (t < len) {  
                        ret += s.charAt(t);  
                    }  
                }  
            }  
        }  
        return ret;
    }
}
```

---
### problem 7--Reverse Integer
***QUESTION***  
Given a 32-bit signed integer, reverse digits of an integer.
**example1**

```
Input: 123
Output:  321
```

**example2**

```
Input: -123
Output: -321
```

**example3**

```
Input: 120
Output: 21
```

**note**
Assume we are dealing with an environment which could only hold integers within the 32-bit signed integer range. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.  
***问题分析***   
这道题求解的是整型反转。考点在于注意整型的溢出，先用long定义变量，再对结果进行整型范围的判断，溢出返回0，否则返回整型结果。  
***java代码***

```java
class Solution {
    public int reverse(int x) {
    int sign = 1;
    if(x < 0){
        sign = -1;
        x *= -1;
    }
    long result = 0;
    while(x != 0){
        result = result * 10 + x % 10;
        if(sign * result > Integer.MAX_VALUE ||
                sign * result < Integer.MIN_VALUE)
            return 0;
        x = x / 10;
    }
    return (int)result * sign;
    }
}
```
