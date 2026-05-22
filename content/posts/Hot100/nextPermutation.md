---
date: '2026-05-22T18:30:53+08:00'
draft: false
title: 'NextPermutation'
tags: ["下一个排序", "字典序"]
categories: ["Hot100"]
---

1. 做题思路：
    * 从后往前遍历，找到下降点
    * 从尾部重新遍历找到大于该值的最小的值，进行swap
    * 将swap之后的数字按升序排序
    * 如果未进行swap操作，反转数组
```c++
    class Solution{
        void nextPermutation(vector<int>& nums){
            size_t size = nums.size();
            //记录交换的下标
            int index = -1;
            for(int i = size-1; i > 0; i--){
                //寻找下降点
                if(nums[i] > nums[i-1]){
                    //找到大于下降点最小的值
                    for(int j = size-1; j > i-1; j--){
                        if(nums[j] > nums[i-1]){
                            //交换加退出
                            swap(nums[j], nums[i-1]);
                            index = i-1;
                            break;
                        }
                    }
                    break;
                }
            }

            //有swap操作
            if(index != -1){
                sort(nums.begin() + flag + 1, nums.end()) //i-1后续的进行升序排序
            }else{
                //反转，从最大字典序->最小字典序
                reverse(nums.begin(), nums.end());
            }
        }
    };
```
