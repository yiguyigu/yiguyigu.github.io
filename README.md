---
title: How to Setup  
date: 2025-11-07  
tags: hexo  
category: HOW TO  
---

# How to Build from Scratch (Windows)

[Hello World!](./source/_posts/hello-world.md)

# How to Quickly Setup Hexo from Repo (Linux)

[How to Quickly Setup Hexo from GitHub](https://yiguyigu.github.io/2025/11/15/How-to-Quickly-Setup-Hexo-from-GitHub/) 或 [here](./source/_posts/How-to-Quickly-Setup-Hexo-from-GitHub.md)

# How to Quickly Setup Hexo from Repo (Windows)

## Prepare（可选）

## Node.js

Hexo 基于 Node.js，需要先确认是否已安装。

检查：

```bash
node -v
npm -v
```

若未安装，请访问 <https://nodejs.org/> 下载并安装最新稳定版（Windows x64）。  
npm（Node Package Manager）会随 Node.js 一起安装。

## Hexo

在工作目录中执行以下命令安装 Hexo 及其依赖。

安装 Hexo 命令行工具（可选，全局安装）：

```bash
npm install -g hexo-cli
```

初始化项目（仅全新项目需要）：

```bash
hexo init
```

安装项目依赖：

```bash
npm install
```

安装用于将站点发布到远程仓库的插件：

```bash
npm install hexo-deployer-git --save
```

## Setup

### Step 1

```bash
git clone git@github.com:yiguyigu/yiguyigu.github.io.git
```

### Step 2

同步子模块：

```bash
git submodule update --init --recursive
```

### Step 3

在 `./source/_posts` 目录下添加你希望发布到博客的文章。

## Usage

```bash
# You can use only the initials of each word to simplify commands.

npx hexo clean

npx hexo generate

npx hexo deploy
```


## Summary

使用 GitHub + Hexo 搭建个人博客，核心组件包括 Node.js、Hexo 以及相关依赖。  
主题、样式等配置文件已经存放在仓库中，因此在新环境中只需配置好运行环境并拉取仓库，即可开始创作和发布内容。