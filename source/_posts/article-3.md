---
title: Python环境管理--pipenv
date: 2020-04-28 14:18:59
tags: 
- Python
- 环境管理
- pipenv
categories:
- 技术
- 环境管理
---

Python环境管理--pipenv管理库

<!-- more -->

## 安装pipenv

```shell
pip3 install pipenv
```

初始化

安装到项目目录里

```shell
export PIPENV_VENV_IN_PROJECT=1
```

使用 Python 3.7.3

```shell
pipenv --python $(pyenv root)/versions/3.7.3/bin/python
```

设置源

```shell
PIPENV_PYPI_MIRROR=http://mirrors.aliyun.com/pypi/simple
或
pipenv install --pypi-mirror http://mirrors.aliyun.com/pypi/simple
```

## 删除

```shell
pip uninstall pipenv
```

## 说明

### pipenv 可以触发 pyenv 安装 Python 版本

.python-version

### pipenv 可以打开依赖的包

export EDITOR=subl # 设置 pipenv open 的 默认编辑器
pipenv open fabric

### 其他

删除在 /usr/local/bin/pipenv 目录的pipenv
建议用 pip3 安装 pipenv