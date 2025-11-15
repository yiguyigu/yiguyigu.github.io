---
title: Debug in the VSCode
date: 2025-10-26
tags: 
    - skill
    - vscode
category: skill
---

# 为C/C++工作去配置Debug环境步骤

## 配置步骤

### Step 1
`cd` 到对应目录，使用 `code .` 通过vscode打开当目录。

### Step 2
点击左侧边栏 `Run and Debug` 选项，点击 create a launch,json file.

> 此时会在工作区的 `.vscode/` 目录下创建 `launch.json` 文件，可以跟着提示选择传教对应的configurations，也可以参照下面launch.json来改写。

### Step 3
书写launch.json文件

example:
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/a.out",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "preLaunchTask": "gcc build active file",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                },
                {
                    "description": "Set Disassembly Flavor to Intel",
                    "text": "-gdb-set disassembly-flavor intel",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}

```

> Tips: 可以先编译好debug版本的可执行文件，然后直接配置launch.json即可；也可以配置一个tasks.json，然后在"preLaunchTask":写入一tasks.json中label同样的参数，即可编译调试一起

### Step 4 (option)
书写tasks.json

example:
```json
{
	"version": "2.0.0",
	"tasks": [
		{
			"label": "build",
			"type": "shell",
			"command": "gcc",
			"args": [
				"-g",
				"-O0",
				"${workspaceFolder}/main.c",
				"-o",
				"${workspaceFolder}/a.out"
			],
			"group": {
				"kind": "build",
				"isDefault": true
			},
			"problemMatcher": ["$gcc"],
			"detail": "Compile main.c with debug symbols for use with the cppdbg launch configuration"
		}
	]
}

```

