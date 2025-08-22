---
title: How to get Jetpack6.2 source
date: 2025-07-11
tags:
  - MIIVII
  - nvidia
  - EN
category: Intern
---

# How to get Jetpack 6.2 source

## step1:

- Download the latest Jetson Linux release package and sample file system for your Jetson developer kit from [Jetson Linux Archive](https://developer.nvidia.com/linux-tegra).

- You can look the page about quickly start [nv_quickly_start_page](https://docs.nvidia.com/jetson/archives/r36.4.4/DeveloperGuide/IN/QuickStart.html ).

- if you just need kernel source, you can not download sample_rootfs.

  ```shell
  $ wget https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v4.4/release/Jetson_Linux_r36.4.4_aarch64.tbz2
  $ tar xf Jetson_Linux_r36.4.4_aarch64.tbz2
  ```

## step2:

- Get kernel source, but you have two way to do that.

### 1. Sync the sources using Git

- Use this way, you need know the <release-tag>, it can be know form [Release Note](https://docs.nvidia.com/jetson/archives/r36.4.4/ReleaseNotes/Jetson_Linux_Release_Notes_r36.4.4.pdf).

- Such as `jetson_36.4.4`

  ```shell
  $ cd /Linux_for_Tegra/source
  $ ./source_sync.sh -k -t <release-tag>
  ```

- If you want to learn about more, you can reference [Kernel Customization](https://docs.nvidia.com/jetson/archives/r36.4.4/DeveloperGuide/SD/Kernel/KernelCustomization.html).
- ==tips==:  the way sometime is very low.

### 2. download the kernel source files and manually extract them

- Download the Jetson Linux source files from [Jetson Linux](https://developer.nvidia.com/embedded/jetson-linux), choice [Driver Package (BSP) Sources](https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v4.4/sources/public_sources.tbz2)

- Extract the `.tbz2` file:

  ```shell
  $ tar xf public_sources.tbz2 -C <install-path>/Linux_for_Tegra/..
  ```

- Extract the kernel and the NVIDIA out-of-tree modules source files:

  ```shell
  $ cd <install-path>/Linux_for_Tegra/source
  $ tar xf kernel_src.tbz2
  $ tar xf kernel_oot_modules_src.tbz2
  $ tar xf nvidia_kernel_display_driver_source.tbz2
  ```

  This extracts the kernel source to the `kernel/` subdirectory, and the NVIDIA out-of-tree kernel modules sources to the current directory.

- ==tips==: the way will download many compressed file.

  You can use a shell comman to slove this:

  ```shell
  $ find . -type f ! -name "*.tbz2" ! -name "*.sha1sum" ! -name "*.txt" -exec cp {} ~/backup/ \;
  ```

  up way is bad, because it make all file stay one directour.





