---
title: How to Quickly Setup Hexo from GitHub
date: 2025-11-15 23:0:06
tags: hexo
category: HOW TO
---

# How to Quickly Setup Hexo from GitHub

## 概述

- 这里主要是使用不同分支来区分静态文件和源文件：
    - `main` 是静态
    - `source` 是源文件

## 步骤

### Step 1

```shell
git clone git@github.com:yiguyigu/yiguyigu.github.io.git
```

### Step 2

- 同步子模块：

```shell
git submodule update --init --recursive
```

### Step 3

- 在 `./source/_posts` 目录下添加你想要显示到博客网站的内容

## 环境准备（optional）

```bash
sudo apt install -y nodejs

npm install

npm install hexo-deployer-git --save
```

## 更新 Blog 步骤

首先需要在 `source/_posts/` 目录下添加 `.md` 文档，必要时记得推送到 repo。

```bash
# You can use only the initials of each word to simplify commands.

npx hexo clean

npx hexo generate

npx hexo deploy
```
