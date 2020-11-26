---
title: Mordern C++ Tricks
author: Bhargav Lad
date: 2020-11-25 11:33:00 
categories: [Learning, Cpp]
tags: [cpp,c14,programming]

---

# Initalization with {}

- The initialization of variables was uniform in C++11. 
- {} initalization is always applicable
- {} init prevents narrowing conversion of  implicit conversion of arithmetic values
- In C++14, `auto` with `{}` always generates `initializer_list`.

```cpp
string str{"my String"}; // Direct init
string str = {"my String"}; // Copy init
char c{8};
int i{29};
float f{3.14};
vector<int> v={1,2,3,4};
set<int> mySet = {-10, 5, 1, 4, 5};
unordered_map<string,int> dict={{"scotty",1},{"bhargav":2}};
int intArray[] = {1,2,3,4,5};
```



# Automatic type inference with auto

```cpp
auto it = v.begin();
type(it); //iterator
```



# Initialize long long and floats

```cpp
long long int n = 0LL;
unsigned long long int n = 1ULL;

// Creating a double type variable
double a = 3.912348239293;

// Creating a float type variable
float b = 3.912348239293f;


```

# to_string and stoi

```cpp
string s = to_string(123)
string s = to_string(12.5)

int n=stoi("123")

```

