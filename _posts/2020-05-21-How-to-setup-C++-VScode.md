---
title: How set up VScode for C++ 
author: Bhargav Lad
date: 2020-05-21 20:57
category:[Blogging, Tutorial] 
tags: [c++,vscode c++,code runnner]
---

# How to setup VScode and Code Runner for C++ 17 (Mac/Windows)

## Step1. Download VScode and exteentions.

1. Download and install Vscode and from the microsoft website https://code.visualstudio.com/download.

2. Open VScode.
![]({{"/assets/img/posts/5/welcome.png"}})

3. Download and install C++ Extensions for VScode.
![]({{"/assets/img/posts/5/cpp_ext.png"}})

4. Download  and install Code Runner Extension.
![]({{"/assets/img/posts/5/coderunner.jpg"}})

## Step 2. Set up Code Runner with C++17 flag

1. Go to *Code / Prefrences / Settings* then Select **Extensions -> Run Code Configure -> Executor Map**
![]({{"/assets/img/posts/5/coderunner_setup.png"}})

2. Add the following command to run for cpp file in the Executor map.


        "code-runner.executorMap": {
            "cpp": "cd $dir && g++ -std=c++17 -g $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt"
        }


3. Now create a test file *test.cpp* and run the file using Code Runner Play button. You can see the excuted Command and Outuput in the terminal Window.
![]({{"/assets/img/posts/5/run.png"}})

Similarly you can set up which commands to execute for different languages. Here is a [link](https://blog.atwork.at/post/Run-Code-from-Visual-Studio-Code) to a blog to help you with it.

# Thank You 