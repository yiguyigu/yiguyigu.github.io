---
title: MIIVII Product – MIIVII SDK
timer: 2025-07-15
tags: MIIVII
category: Intern
---

# MIIVII Product – MIIVII SDK

## introduce

miivii 交付的产品可以大致分为三类：硬件设备、软件SDK、售后服务

硬件设备

- 主要是基于nvidia的模组来进行垂直领域的硬件设计（比如针对无人驾驶领域，增加多路GMSL接口，增加多路网卡接口等）
- 设计这个过程需要软件参加评审，得到认可才能投入生产计划

软件bsp

- 负责公司新版本的镜像开发，并完成自动化集成部署
- 提供售后，解决一些需求

## software workflow

基于模组的软件生态，原料是：simpleRootfs + bootloader + toolchain