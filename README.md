---
title: How to Setup 
date: 2025-11-07 
tags: hexo
category: HOW TO
---

# How to build from scratch (Windows)


# How to Quickly Setup Hexo base on repo (Windows)

## Prepare (optional)

### Install needle app

**Node.js**

Hexo 是基于 Node.js 构建的，需要检查是否安装。

检查：
```bash
node -v
npm -v
```
安装：

访问 https://nodejs.org/ 下载并安装最新稳定版本。npm (Node Package Manager) 会随 Node.js 一起安装。

**HEXO**

安装：

在工作目录运行以下命令来安装 Hexo 及其依赖

```base
npm install -g hexo-cli
```
初始化：

```bash
hexo init
```

安装依赖：

```bash
npm install
```

## Setup
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
