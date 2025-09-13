---
layout: post
title: Creating a Flexible Python Environment with virtualenv
date: 2024/9/19
toc: true
---

内容选自 https://stackoverflow.com/questions/55600132/installing-local-packages-with-python-virtualenv-system-site-packages 。

- 目标：创建一个虚拟环境，可以在其中使用已经在全局范围安装的库，但在安装新的库时，会安装到新的虚拟环境中，保证独立性。
- 实现方式：
  - 直接使用 virtualenv venv 语句创建基本环境（不使用 --system-site-packages 参数）；
  - 修改 venv/pyvenv.cfg 文件中的参数：第五行改为 include-system-site-packages = true（使用全局包，但保持虚拟环境的独立）。

## 小技巧：查看包的安装路径

使用 pip show <package-name> 指令可以显示包的具体信息，其中包括安装路径。
