---
title: 133. Clone Graph 
author: Bhargav Lad 
date: 2020-07-27 22:14
category: [Programming-Problems , leetcode]
tags: [graph, pointer]
---

## Description 
[133. Clone Graph](https://leetcode.com/problems/clone-graph/)

### Problem:
Given a reference of a node in a connected undirected graph.

Return a deep copy (clone) of the graph.

Each node in the graph contains a val (int) and a list (List[Node]) of its neighbors.

## Idea

As we need to clone the whole graph, we will have to traverse the complete graph. We can either use BSF or DFS to do so. I choose DFS as its easy to implement and modify. The next key thing to focous on is the graph is undirected and is represented with Node structure. Hence, we need to remember the node we created so that if any other node refrences the previously created node, we should returen the same refrence. This is a key point of the problem.

1. traverse the Node and not null
    * if this node is in dictionary then return previously stored node
    * else create a new node and traverse neighbours

## Solution

```cpp
class Solution {
private: unordered_map<Node*,Node*> dic;
public:
    Node* dfs(Node* s)
    {
        if(s!=NULL)
        {
            
            //check if this is already created nofr
            if(dic.find(s)!=dic.end())
            {
                return dic[s];
            }
            else
            {
                Node* ns = new Node(s->val,{});
                dic[s]=ns;
                for(Node* w: s->neighbors)
                { 
                    ns->neighbors.push_back(dfs(w));
                }
                return ns;
            }
        }
        else
            return NULL;
        
    }
    Node* cloneGraph(Node* node) {
        return dfs(node);
    }
};
```



