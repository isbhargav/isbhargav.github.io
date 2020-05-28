---
title: Text and Typography
author: Cotes Chung
date: 2020-05-28 11:33:00 +0800
categories: [Blogging, Tutorial]
tags: [C++, Vscode config]
---

VScode Configuration files for C++ Debugging with input file.

### **launch.json**
```terminal
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "clang++ - Build and debug active file With input",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "lldb",
            "preLaunchTask": "clang++ build active file",
            "setupCommands": [
                {
                    "text": "settings set target.input-path ${workspaceFolder}/in.txt"
                }
            ]
        }
    ]
}
```
###  **tasks.json**

```terminal
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "g++",
            "args": [
                "-g",
                "-std=c++17",
                "${file}"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        },
        {
            "type": "shell",
            "label": "clang++ build active file",
            "command": "/usr/bin/clang++",
            "args": [
                "-std=c++17",
                "-g",
                "${file}",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}"
            ],
            "options": {
                "cwd": "/usr/bin"
            }
        }
    ]
}
```