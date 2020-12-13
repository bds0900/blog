+++
title="Max subarray - leetcode .53"
date=2020-09-17

[taxonomies]
categories = ["Post"]
tags = ["post","leetcode"]
+++

## Problem
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Follow up**: If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

**Example 1:**
> **Input:** nums = [-2,1,-3,4,-1,2,1,-5,4]  
> **Output:** 6  
> **Explanation:** [4,-1,2,1] has the largest sum = 6.

**Example 2:**
> **Input:** nums = [1]  
> **Output:** 1

**Example 3:**
> **Input:** nums = [0]  
> **Output:** 0

**Example 4:**
> **Input:** nums = [-1]  
> **Output:** -1

**Example 5:**
> **Input:** nums = [-2147483647]  
> **Output:** -2147483647

**Constraints:**
- 1 <= nums.length <= 2 * 104
- -231 <= nums[i] <= 231 - 1

## My solution
```
impl Solution {
    pub fn max_sub_array(nums: Vec<i32>) -> i32 {
        if nums.len()==0{
            return 0
        }
        let mut max:i32=nums[0];
        for i in 0..nums.len(){
            let mut sum:i32=0;
            for j in i..nums.len(){
                sum=sum+nums[j];
                if sum>max{
                    max=sum;
                }
            }
        }
        max
        
    }
}
```
It is brute force way to find the max subarray. Time complexity is **O(n^2)**  
<br/>
<br/>


## Other person's solution

```
impl Solution {
    pub fn max_sub_array(nums: Vec<i32>) -> i32 {
        let mut sum: i32 = 0;
        let mut maxSum: i32 = std::i32::MIN;
        for (i, j) in nums.iter().enumerate(){
            sum = sum + nums[i];
            if sum > maxSum {
                maxSum = sum
            }
            if sum < 0 {    // highlight-line
                sum  = 0    // highlight-line
            }               // highlight-line
        }
        
        return maxSum;
    }
}
```
For example, we have an array of size 10.  

> |index|0|1|2|3|4|5|6|7|8|9|
> |---|---|---|---|---|---|---|---|---|---|---|
> value|-3|-4|-4|-5|3|4|7|-1|0|1|

Max subarray=`nums[4]` + `num[5]` + `num[6]` = 3 + 4 + 7 = 14


### Get sum of subarray
> **sum(nums[0]..nums[6])**  
> `nums[0]`+`nums[1]`+...+`nums[5]`+`nums[6]`  
>=-3+(-4)+(-4)+(-5)+3+4+7  
>=-2  


> **sum(nums[0]..nums[3])**   
> `nums[0]`+`nums[1]`+`nums[2]`+`nums[3]`  
>=-3+(-4)+(-4)+(-5)  
>=-16  


> **sum(nums[4]..nums[6])**  
> `nums[4]`+`nums[5]`+`nums[6]`  
>=3+4+7  
>=14

> sum(nums[**4**]..nums[**6**])=sum(nums[**0**]..nums[**6**])-sum(nums[**0**]..nums[**3**])

> sum(nums[**i**]..nums[**j**])=sum(nums[**0**]..nums[**j**])-sum(nums[**0**]..nums[**i-1**])


### What does this block of code mean...? 
```
if sum < 0 {
    sum  = 0
}
```

> *if*  
> **x < 0**  
> *then*  
> **x + y < y**  


> *if*  
> **sum(nums[0]..nums[3]) < 0**  
> *then*  
> **sum(nums[0]..nums[3]) + sum(nums[4]..nums[6])<sum(nums[4]..nums[6])** 

##### If `x` and `y` are added and the value is smaller than `y`, then removing `x` and using only the value of `y` will yield a larger value.

**sum(nums[i]..nums[j])=0+sum(nums[i]..nums[j])**