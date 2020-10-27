---
title: Non-overlapping Intervals
author: Bhargav lad
date: 2020-08-16 12:04:12
categories: [Programming-Problems, Leetcode]
tags: [job-scheduling, greedy]
---

## Non-overlapping Intervals

[leetcode link](https://leetcode.com/explore/challenge/card/august-leetcoding-challenge/551/week-3-august-15th-august-21st/3425/)

<div class="question-detail">

<div class="question-description__3U1T">

<div>

Given a collection of intervals, find the minimum number of intervals
you need to remove to make the rest of the intervals non-overlapping.

**Example 1:**

    Input: [[1,2],[2,3],[3,4],[1,3]]
    Output: 1
    Explanation: [1,3] can be removed and the rest of intervals are non-overlapping.

**Example 2:**

    Input: [[1,2],[1,2],[1,2]]
    Output: 2
    Explanation: You need to remove two [1,2] to make the rest of intervals non-overlapping.

**Example 3:**

    Input: [[1,2],[2,3]]
    Output: 0
    Explanation: You don't need to remove any of the intervals since they're already non-overlapping.

**Note:**

1.  You may assume the interval's end point is always bigger than its
    start point.
2.  Intervals like \[1,2\] and \[2,3\] have borders "touching" but they
    don't overlap each other.

</div>

</div>

</div>

## Idea

Here our target is to minimize the number of jobs to be removed or conversly we need to select the maximum number of non overlaping scheduls. Hence we can conclude that this is an instance of job scheduling problem.

The heuristic behind the job scheduling problem is always pick the interval with the earliest end time.This is because, the interval with the earliest end time produces the maximal capacity to hold rest intervals.

## Psuedo code

1. sort the intervals based in end time.
2. keep track of latest end time.
3. select the task if its start is later than latest end time.

## Code

```cpp


int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int ans=0;

sort(intervals.begin(),intervals.end(),[](const vector<int> &a,
              const vector<int> &b)
{
    return ( a[1]< b[1] );
} );

int last_end = INT_MIN;
  for (auto x: intervals){
            int start = x[0];int end=x[1];

      if (start < last_end)
          ans++;
    else
        last_end=end;
  }


        return ans;
    }
};



```
