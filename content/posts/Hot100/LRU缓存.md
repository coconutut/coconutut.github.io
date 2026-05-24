---
date: '2026-05-20T19:07:15+08:00'
draft: false
title: 'LRU缓存'
tags: ["LRU", "C++"]
categories: ["Hot100"]
---

1. 重点记忆private变量有
  * node* head
  * node* tail
  * unordered_map<int, node*> cache (key -> node*)
  * int capacity
2. 扩展：[LRU（时间维度）、LFU（频率维度）](https://www.cnblogs.com/erichi101/p/18861024)  

```c++
#include <bits/stdc++.h>
using namespace std;

//链表节点
struct node{
    int key;
    int value;
    node* next;
    node* prev;
    node() : key(0), value(0), next(nullptr), prev(nullptr) {}
    node(int key, int val) : key(key), value(val), next(nullptr), prev(nullptr) {}
};

class LRUCache{
public:
    //构造函数 双向链表初始化
    LRUCache(int cap) : capacity(cap) {
        head = new node();
        tail = new node();
        head->next = tail;
        tail->prev = head;
    }
    
    //插入键值对
    void put(int key, int val){
        //存在相同键时
        if(cache.find(key) != cache.end()){
            cache[key]->value = val;
            //先去除再头插
            remove(cache[key]);
            moveToHead(cache[key]);
        }
        //不存在时
        else{
            //容量等于上限时进行删除末尾
            if(cache.size() == capacity){
                node* toRemove = tail->prev;
                remove(toRemove);
                cache.erase(toRemove->key);
                delete toRemove;
            }
            //进行创建新节点
            node* nodeInstance = new node(key, val);
            moveToHead(nodeInstance);
            cache[key] = nodeInstance;
        }
    }
    
    //获取键值对，通过哈希表key->node*
    int get(int key){
        if(cache.find(key) != cache.end()){
            remove(cache[key]);
            moveToHead(cache[key]);
            return cache[key]->value;
        }
        return -1;
    }

private:
    int capacity;
    node* head;
    node* tail;
    unordered_map<int, node*> cache; //哈希映射，保证O1

private:

    //去除链表中的一个节点（不删除）
    void remove(node* nodeInstance){
        node* prev = nodeInstance->prev;
        node* next = nodeInstance->next;
        prev->next = next;
        next->prev = prev;
    }
    
    //将节点头插
    void moveToHead(node* nodeInstance){
        //头插法
        nodeInstance->next = head->next;
        head->next->prev = nodeInstance;
        nodeInstance->prev = head;
        head->next = nodeInstance;
    }
};

//test
int main(){
    LRUCache instance(3);
    instance.put(1, 1);
    instance.put(2, 2);
    // cout << instance.get(1) << endl;
    // instance.put(3, 3);
    instance.put(4, 4);
    cout << instance.get(1) << endl;
}
```