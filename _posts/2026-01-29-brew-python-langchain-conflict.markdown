---
layout: post
title: The Biggest Pitfall When Using LangChain / Transformers on macOS
date: 2026-01-29 00:14
categories: [blog]
tags: [AI, LangChain]
description:
---

# Mac 上使用 LangChain / Transformers 最大的坑：Homebrew Python 冲突（终极解决方案）

# The Biggest Pitfall When Using LangChain / Transformers on macOS: Homebrew Python Conflict (Definitive Fix)

------------------------------------------------------------------------

## 问题现象 \| The Symptom

在 Mac 上安装 AI 相关库时：

``` bash
pip install -U langchain langchain-community transformers torch accelerate
```

出现报错：

    Cannot uninstall packaging 26.0
    The package was installed by brew

或者 Jupyter 里始终无法正确 import：

``` python
import langchain
```

------------------------------------------------------------------------

## 根本原因 \| The Root Cause

你正在使用的是：

    /opt/homebrew/bin/python3

这是 **Homebrew 管理的 Python**，它是给：

-   jupyterlab
-   系统工具
-   brew 依赖

使用的。

**它不是给 AI / ML / LangChain / Transformers 用的。**

------------------------------------------------------------------------

## 关键认知（非常重要）

> **永远不要在 /opt/homebrew 的 Python 环境里安装 AI 库**

这是 2025 年 Mac 用户做 LangChain 最常见的致命错误。

------------------------------------------------------------------------

## 正确做法（3 分钟永久解决）

### 第一步：使用系统自带 Python（不是 brew）

``` bash
/usr/bin/python3 --version
```

### 第二步：创建干净的虚拟环境

``` bash
/usr/bin/python3 -m venv ~/ai_env
```

### 第三步：激活环境

``` bash
source ~/ai_env/bin/activate
```

验证：

``` bash
which python
```

必须显示：

    /Users/你的用户名/ai_env/bin/python

### 第四步：升级 pip

``` bash
pip install --upgrade pip
```

### 第五步：安装 AI 库（现在会完全正常）

``` bash
pip install -U langchain langchain-community huggingface_hub transformers torch accelerate
```

------------------------------------------------------------------------

## 关键一步：让 Jupyter 使用这个环境

``` bash
pip install ipykernel jupyterlab
python -m ipykernel install --user --name ai_env --display-name "Python (ai_env)"
```

以后这样启动：

``` bash
jupyter lab
```

验证：

``` bash
which jupyter
```

------------------------------------------------------------------------

## 结果 \| The Result

修复后你可以：

-   正常安装 LangChain
-   正常运行 Transformers
-   正常在 Jupyter 中 import 所有库
-   再也不会看到 `packaging` 冲突

------------------------------------------------------------------------

## 一句话总结 \| One-line Summary

> 不要在 `/opt/homebrew` Python 里做 AI 开发\
> 永远使用 `venv` 独立环境
