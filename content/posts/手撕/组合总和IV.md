---
date: '2026-05-29T18:03:25+08:00'
draft: false
title: '组合总和IV'
tags: ["背包问题"]
categories: ["Hot100"]
---
1. 组合总和是一个完全背包 -> 容量正序
2. 组合关乎排序顺序， 容量 -> 物品

```c++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        size_t n = nums.size();

        vector<unsigned long long> dp(target+1, 0);
        dp[0] = 1;

        for(int i = 1; i <= target; i++){
            for(int num : nums){
                if(i >= num)
                    dp[i] += dp[i-num];
            }
        }

        return dp[target];
    }
};
```