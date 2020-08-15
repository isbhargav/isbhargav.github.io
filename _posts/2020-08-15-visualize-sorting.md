---
title: Visualize Sorting Algorithm using React 
author: Bhargav lad
date: 2020-08-15 18:22:38
categories: [Learning, React]
tags: [react,merge-sort,bubble-sort,requestAnimationframe,Hooks]
---

## Introduction

We all have implemented some sorting algorithm in school or maybe at work. We all have also learned the differences between these algorithms in terms of their time complexity, but most of us have never visualized the working of these algorithms. In this post will try to visualize the working to two sorting algorithms using react.

## Lets get coding

First we will implement the 2 algorithms. We will use in place sorting.

1. Bubble sort O(n^2)
2. Merge sort O(log n)

```javascript

// Inplace buble sort
 function bublesort(arr) {
    setsortButton(true);
    for (let i = 1; i < arr.length; i++) {
      let oneSwap = false;
      for (let j = 0; j < arr.length - i; j++) {
        if (arr[j] > arr[j + 1]) {
          oneSwap = true;
          let tmp = arr[j];
          arr[j] = arr[j + 1];
          arr[j + 1] = tmp;
       
        }
      }
      if (oneSwap === false) break;
    }

    return;
  }
  
  
// Inplace Implementation merge sort
  function merge(arr, start, mid, end) {
    let start2 = parseInt(mid + 1);
    // If the direct merge is already sorted
    if (arr[mid] <= arr[start2]) {
      return;
    }

    // Two pointers to maintain start
    // of both arrays to merge
    while (start <= mid && start2 <= end) {
      // If element 1 is in right place
      if (arr[start] <= arr[start2]) {
        start++;
      } else {
        let value = arr[start2];
        let index = start2;

        // Shift all the elements between element 1
        // element 2, right by 1.
        while (index > start) {
          arr[index] = arr[index - 1];
          index--;
        }
        arr[start] = value;

        // Update all the pointers
        start++;
        mid++;
        start2++;
      }
    }
  }

  /* l is for left index and r is right index of the  
   sub-array of arr to be sorted */
  function mergeSort(arr, l, r) {
    if (l < r) {
      // Same as (l + r) / 2, but avoids overflow
      // for large l and r
      let m = parseInt(l + (r - l) / 2);

      // Sort first and second halves
      mergeSort(arr, l, m);
      mergeSort(arr, m + 1, r);

      merge(arr, l, m, r);
    }
  }

```


Next we will setup out react app with states and ref.

```javascript
export default function App() {

  // to store our array
  const [randArr, setRandArr] = useState(null);
  // which indices  we are comparing 
  const [compIdx, setCompIdx] = useState([]);
  // to disable sort button 
  const [sortButton, setsortButton] = useState(false);
  // to store Raf object
  const rafRef = useRef();
  
    return (
    <div className="App">
      <div className="num-container">
        {randArr &&
          randArr.map((n, i) => (
            <div
              className="num"
              style={{
                height: '${n * 10}px',
                backgroundColor: compIdx.includes(i) ? "red" : "blue"
              }}
              key={i}
            >
              {n}
            </div>
          ))}
      </div>
      <button
        disabled={sortButton}
        onClick={(e) => mergeSort(randArr, 0, randArr.length - 1)}
      >
        Sort
      </button>
    </div>
  );
}

```
When our component mounts we want to initialize it with random array. To accompolish this task we will introduce useEffect hook.
```javascript

useEffect(() => {
    setRandArr(
      Array.from({ length: getRandomInt(5, 50) }, (_) => getRandomInt(3, 100))
    );
  }, []);//Empty dependency makes sure that is runs only on mount

```
Next the animate the comparisons we will use requestAnimationFrame api. we will set our ***compIdx*** when we are comparing two indices and to make sure that our browser gets chance to paint the indices we will wait for some time after that. We will use settimeout to wait.

Below are the following code snippets

```javascript

//wait function
function wait(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
  
}

// setting cmpIdx
rafRef.current = window.requestAnimationFrame(() => {
            setCompIdx([j, j + 1]);
          });
          await wait(50);
        }

```


Using above two snippets we implement out merge sort and bubble sort. We need to implement async version because we do not want it to be blocking and want out viewer to see the changes.



```javascript
 async function bublesort(arr) {
    setsortButton(true);
    for (let i = 1; i < arr.length; i++) {
      let oneSwap = false;
      for (let j = 0; j < arr.length - i; j++) {
        if (arr[j] > arr[j + 1]) {
          oneSwap = true;
          let tmp = arr[j];
          arr[j] = arr[j + 1];
          arr[j + 1] = tmp;
          rafRef.current = window.requestAnimationFrame(() => {
            setCompIdx([j, j + 1]);
          });
          await wait(50);
        }
      }
      if (oneSwap === false) break;
    }

    return;
  }

  // Inplace Implementation
  async function merge(arr, start, mid, end) {
    let start2 = Math.floor(mid + 1);
    // If the direct merge is already sorted
    rafRef.current = window.requestAnimationFrame(() => {
      setCompIdx([mid, start2]);
    });
    await wait(50);
    if (arr[mid] <= arr[start2]) {
      return;
    }

    // Two pointers to maintain start
    // of both arrays to merge
    while (start <= mid && start2 <= end) {
      // If element 1 is in right place
      rafRef.current = window.requestAnimationFrame(() => {
        setCompIdx([start, start2]);
      });
      await wait(50);
      if (arr[start] <= arr[start2]) {
        start++;
      } else {
        let value = arr[start2];
        let index = start2;

        // Shift all the elements between element 1
        // element 2, right by 1.
        while (index > start) {
          rafRef.current = window.requestAnimationFrame(() => {
            setCompIdx([index, start]);
          });
          await wait(50);
          arr[index] = arr[index - 1];
          index--;
        }
        arr[start] = value;

        // Update all the pointers
        start++;
        mid++;
        start2++;
      }
    }
  }

  /* l is for left index and r is right index of the  
   sub-array of arr to be sorted */
  async function mergeSort(arr, l, r) {
    if (l < r) {
      // Same as (l + r) / 2, but avoids overflow
      // for large l and r
      let m = Math.floor(l + (r - l) / 2);

      // Sort first and second halves
      await mergeSort(arr, l, m);
      await mergeSort(arr, m + 1, r);

      await merge(arr, l, m, r);
    }
  }

```

Finally we need to clear the animation frame on component unmount. Again we will need useEffect hook

```javascript

useEffect(() => {
    return () => {
      if (rafRef.current) window.cancelAnimationFrame(rafRef.current);
    };
  }, []);

```

## Final Code
Here is our final code

```javascript
import React, { useEffect, useState, useRef } from "react";
import "./styles.css";
function getRandomInt(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
function wait(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

export default function App() {
  const [randArr, setRandArr] = useState(null);
  const [compIdx, setCompIdx] = useState([]);
  const [sortButton, setsortButton] = useState(false);
  const rafRef = useRef();

  useEffect(() => {
    console.log("running once");
    setRandArr(
      Array.from({ length: getRandomInt(5, 50) }, (_) => getRandomInt(3, 100))
    );
  }, []);

  useEffect(() => {
    return () => {
      if (rafRef.current) window.cancelAnimationFrame(rafRef.current);
    };
  }, []);

  async function bublesort(arr) {
    setsortButton(true);
    for (let i = 1; i < arr.length; i++) {
      let oneSwap = false;
      for (let j = 0; j < arr.length - i; j++) {
        if (arr[j] > arr[j + 1]) {
          oneSwap = true;
          let tmp = arr[j];
          arr[j] = arr[j + 1];
          arr[j + 1] = tmp;
          rafRef.current = window.requestAnimationFrame(() => {
            setCompIdx([j, j + 1]);
          });
          await wait(50);
        }
      }
      if (oneSwap === false) break;
    }

    return;
  }

  // Inplace Implementation
  async function merge(arr, start, mid, end) {
    let start2 = Math.floor(mid + 1);
    // If the direct merge is already sorted
    rafRef.current = window.requestAnimationFrame(() => {
      setCompIdx([mid, start2]);
    });
    await wait(50);
    if (arr[mid] <= arr[start2]) {
      return;
    }

    // Two pointers to maintain start
    // of both arrays to merge
    while (start <= mid && start2 <= end) {
      // If element 1 is in right place
      rafRef.current = window.requestAnimationFrame(() => {
        setCompIdx([start, start2]);
      });
      await wait(50);
      if (arr[start] <= arr[start2]) {
        start++;
      } else {
        let value = arr[start2];
        let index = start2;

        // Shift all the elements between element 1
        // element 2, right by 1.
        while (index > start) {
          rafRef.current = window.requestAnimationFrame(() => {
            setCompIdx([index, start]);
          });
          await wait(50);
          arr[index] = arr[index - 1];
          index--;
        }
        arr[start] = value;

        // Update all the pointers
        start++;
        mid++;
        start2++;
      }
    }
  }

  /* l is for left index and r is right index of the  
   sub-array of arr to be sorted */
  async function mergeSort(arr, l, r) {
    if (l < r) {
      // Same as (l + r) / 2, but avoids overflow
      // for large l and r
      let m = Math.floor(l + (r - l) / 2);

      // Sort first and second halves
      await mergeSort(arr, l, m);
      await mergeSort(arr, m + 1, r);

      await merge(arr, l, m, r);
    }
  }
  return (
    <div className="App">
      <div className="num-container">
        {randArr &&
          randArr.map((n, i) => (
            <div
              className="num"
              style={{
                height: '${n * 10}px',
                backgroundColor: compIdx.includes(i) ? "red" : "blue"
              }}
              key={i}
            >
              {n}
            </div>
          ))}
      </div>
      <button
        disabled={sortButton}
        onClick={(e) => mergeSort(randArr, 0, randArr.length - 1)}
      >
        Merge Sort
      </button>
      <button disabled={sortButton} onClick={(e) => bublesort(randArr)}>
        Bubble Sort
      </button>
    </div>
  );
}


```
link to sandbox: [Sandbox](https://codesandbox.io/s/youthful-ishizaka-75ze0?file=/src/App.js:0-3698)
Thank you!