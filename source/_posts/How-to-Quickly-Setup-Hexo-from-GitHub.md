---
title: How to Quickly Setup Hexo from GitHub
date: 2025-05-25 23:0:06
tags: hexo
category: HOW TO
---

# How to Quickly Setup Hexo from GitHub

## 概述
- 这里主要是使用不同分支来区分静态文件和源文件
    - main是静态
    - source是源文件

## 步骤
### Step 1

```shell
git clone git@github.com:yiguyigu/yiguyigu.github.io.git
```
### Step 2

- 同步子模块
    ```shell
    git submodule update --init --recursive
    ```

### Step 3
- 在 `./source/_posts` 目录下添加你想要显示到博客网站的内容
