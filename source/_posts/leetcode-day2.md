---
title: leetcode-day2
date: 2018-02-14 00:15:56
tags: [堆]
categories: [leetcode整理总结]
---
## leetcode Stack(堆) problems--EASY LEVEL

### problem 682--Baseball Game

***QUESTION***

You're now a baseball game point recorder.  
Given a list of strings, each string can be one of the 4 following types:  
1. Integer (one round's score): Directly represents the number of points you get in this round.
2. "+" (one round's score): Represents that the points you get in this round are the sum of the last two valid round's points.
3. "D" (one round's score): Represents that the points you get in this round are the doubled data of the last valid round's points.
4. "C" (an operation, which isn't a round's score): Represents the last valid round's points you get were invalid and should be removed.  
Each round's operation is permanent and could have an impact on the round before and the round after.  
You need to return the sum of the points you could get in all the rounds.

**example1**  

```
Input: ["5","2","C","D","+"]
Output: 30
Explanation:
Round 1: You could get 5 points. The sum is: 5.
Round 2: You could get 2 points. The sum is: 7.
Operation 1: The round 2's data was invalid. The sum is: 5.  
Round 3: You could get 10 points (the round 2's data has been removed). The sum is: 15.
Round 4: You could get 5 + 10 = 15 points. The sum is: 30.
```

**example2**

```
Input: ["5","-2","4","C","D","9","+","+"]
Output: 27
Explanation:
Round 1: You could get 5 points. The sum is: 5.
Round 2: You could get -2 points. The sum is: 3.
Round 3: You could get 4 points. The sum is: 7.
Operation 1: The round 3's data is invalid. The sum is: 3.  
Round 4: You could get -4 points (the round 3's data has been removed). The sum is: -1.
Round 5: You could get 9 points. The sum is: 8.
Round 6: You could get -4 + 9 = 5 points. The sum is 13.
Round 7: You could get 9 + 5 = 14 points. The sum is 27.
```

**note**

- The size of the input list will be between 1 and 1000.
- Every integer represented in the list will be between -30000 and 30000

***问题分析***

根据题目要求实现对字符串数组进行入栈，一旦遇到字符‘C’，则取消堆顶数字；一旦遇到字符‘D’，则对堆顶数字加倍入栈；一旦遇到字符‘+’，则对堆顶以及堆顶的下一个数字求和，再进行入栈。最后，再对栈中所有数字求和，即为棒球比赛的最后得分。

***java代码***

```java
class Solution {
    public int calPoints(String[] ops) {
        Stack<Integer> stack=new Stack();
        for(String op : ops){
            if(op.equals("+")){
                int top=stack.pop();
                int result1=top+stack.peek();
                stack.push(top);
                stack.push(result1);
            }
            else if(op.equals("C")){
                stack.pop();
            }else if(op.equals("D")){
                stack.push(2*stack.peek());
            }else{
                stack.push(Integer.valueOf(op));
            }
        }
        int res=0;
        for(int score : stack){
            res+=score;
        }
        return res;
    }
}
```

### problem 20--Valid Parentheses

***QUESTION***

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.  
The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

***问题分析***

这是一道典型的栈问题。题目要求有效的字符串为字符‘(’与‘)’对应，字符‘{’与‘}’对应，字符‘[’与‘]’对应。例如‘{([])}’。对字符数组遍历入栈，一旦遇到‘)’、‘}’、‘]’则需要判断栈中是否有对应的字符，无则返回false，有则出栈，继续遍历，直至最后若栈中无元素，则为有效字符串，否则为false。

***java代码***

```java
class Solution {
    public boolean isValid(String s) {
        if(s.length()==0||s.length()==1){
            return false;
        }
        char[] res=s.toCharArray();
        Stack<Character> stack=new Stack();
        for(char op : res){
            if(op==')'){
                if(stack.size()==0||stack.pop()!='('){
                    return false;
                }
            }else if(op=='}'){
                //char res2=stack.pop();
                if(stack.size()==0||stack.pop()!='{'){
                    return false;
                }
            }else if(op==']'){
                if(stack.size()==0||stack.pop()!='['){
                    return false;
                }
            }else{
                stack.push(op);
            }
        }
        if(stack.size()==0){
           return true;
        }else{
            return false;   
        }
    }
}
```

### problem 496--Next Greater Element I

***QUESTION***

You are given two arrays (without duplicates) nums1 and nums2 where nums1’s elements are subset of nums2. Find all the next greater numbers for nums1's elements in the corresponding places of nums2.  
The Next Greater Number of a number x in nums1 is the first greater number to its right in nums2. If it does not exist, output -1 for this number.

**example1**

```
Input: nums1 = [4,1,2], nums2 = [1,3,4,2].
Output: [-1,3,-1]
Explanation:
    For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
    For number 1 in the first array, the next greater number for it in the second array is 3.
    For number 2 in the first array, there is no next greater number for it in the second array, so output -1.
```

**example2**

```
Input: nums1 = [2,4], nums2 = [1,2,3,4].
Output: [3,-1]
Explanation:
    For number 2 in the first array, the next greater number for it in the second array is 3.
    For number 4 in the first array, there is no next greater number for it in the second array, so output -1.
```

**note**
- All elements in nums1 and nums2 are unique.
- The length of both nums1 and nums2 would not exceed 1000.   

***问题分析***

这是一道局部比较的题目，从集合中获取目标元素的下一个比它大的第一个元素。遍历集合并入栈，入栈期间判断栈顶元素是否小于遍历的元素，一旦符合则为其的下一个更大元素，即为问题的解，将其与解存入hashmap中，直至遍历完成，最后通过map.getOrDefault()筛选出要求的元素所对应的解。

***java代码***

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Map<Integer,Integer> map=new HashMap<>();
        Stack<Integer> stack=new Stack();
        for(int num : nums2){
            while(!stack.isEmpty()&&stack.peek()<num){
                map.put(stack.pop(),num);
            }
            stack.push(num);
        }
        for(int i=0;i<nums1.length;i++){
            nums1[i]=map.getOrDefault(nums1[i],-1);
        }
        return nums1;
    }
}
```

### problem 232--Implement Queue using Stacks

***QUESTION***

Implement the following operations of a queue using stacks.  
- push(x) -- Push element x to the back of queue.
- pop() -- Removes the element from in front of queue.
- peek() -- Get the front element.
- empty() -- Return whether the queue is empty.

**Notes:**

- You must use only standard operations of a stack -- which means only push to top, peek/pop from top, size, and is empty operations are valid.
- Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
- You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

***问题分析***

本题实现通过使用栈来实现队列的功能。构建两个栈，其中一个栈正常入栈出栈，另一个栈逆序存放栈中元素。在队列添加元素时需要将两个栈进行元素更新，一个出栈，另一个则入栈，实现两个栈的元素顺序相反。

***java代码***

```java
class MyQueue {

    Stack<Integer> stack=new Stack();
    Stack<Integer> stack2=new Stack();
    /** Initialize your data structure here. */
    public MyQueue() {

    }

    /** Push element x to the back of queue. */
    public void push(int x) {
        while(!stack2.isEmpty()){
            stack.push(stack2.pop());
        }
        stack.push(x);
        while(!stack.isEmpty()){
            stack2.push(stack.pop());
        }
    }

    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        return stack2.pop();
    }

    /** Get the front element. */
    public int peek() {
        return stack2.peek();
    }

    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stack2.isEmpty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

### problem 225--Implement Stack using Queues

***QUESTION***  
Implement the following operations of a stack using queues.
- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- empty() -- Return whether the stack is empty.

**Notes:**

- You must use only standard operations of a queue -- which means only push to back, peek/pop from front, size, and is empty operations are valid.
- Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.
- You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).

***问题分析***

本题实现通过使用队列来实现栈的功能。构建两个队列，使用其中一个队列为主队列，在出栈时将主队列除最后一个元素外的所有元素压到副队列中，再对主队列作移除操作，即出栈，同时将副队列指向为主队列。

***java代码***

```java
class MyStack {

    Queue<Integer> q1=new LinkedList();
    Queue<Integer> q2=new LinkedList();
    /** Initialize your data structure here. */
    public MyStack() {

    }

    /** Push element x onto stack. */
    public void push(int x) {
        q1.offer(x);
    }

    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        while(q1.size()>1){
            q2.offer(q1.poll());
        }
        int result=q1.poll();
        Queue tmp=q1;
        q1=q2;
        q2=tmp;
        return result;
    }

    /** Get the top element. */
    public int top() {
        while(q1.size()>1){
            q2.offer(q1.poll());
        }
        int result=q1.peek();
        q2.offer(q1.poll());
        Queue tmp=q1;
        q1=q2;
        q2=tmp;
        return result;
    }

    /** Returns whether the stack is empty. */
    public boolean empty() {
        return q1.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

### problem 155--Min Stack

***QUESTION***

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.

**example**

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

***问题分析***

通过两个栈来对栈进行操作，获取pop,push,top以及getMin等函数实现

***java代码***

```java
class MinStack {

    private Stack<Integer> stack=new Stack();
    private Stack<Integer> minstack=new Stack();

    /** initialize your data structure here. */
    public MinStack() {
    }

    public void push(int x) {
       if(minstack.isEmpty()||x<=minstack.peek()){
           minstack.push(x);
       }
        stack.push(x);
    }

    public void pop() {
        if(stack.peek().equals(minstack.peek())){
            minstack.pop();
        }
        stack.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return minstack.peek();
    }
}
/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```
