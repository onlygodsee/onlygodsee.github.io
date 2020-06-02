---
title: Python环境管理--pyenv
date: 2020-04-28 14:05:35
tags:
- Python
- pyenv
- 环境管理
categories:
- 技术
- 环境管理
---

## 安装pyenv

### Linux

#### pyenv-installer 安装

```shell
curl https://pyenv.run | bash
```

或者

```shell
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
```

以上命令会弹出提示 把以下内容加入 ~/.bashrc 中

```shell
export PATH="~/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

### Mac

#### homebrew 安装

```shell
brew install pyenv
```

## 更新

### Linux

```shell
pyenv update
```

### Mac

```shell
brew upgrade pyenv
```

## 安装 Python 3.7.3

### Linux

安装依赖
sudo apt-get update
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

下载源码包到 pyenv 的 缓存目录里，加快安装速度

```shell
wget https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tar.xz -P $(pyenv root)/cache
```

安装

```shell
pyenv install 3.7.3
```

### Mac

下载源码包到 pyenv 的 缓存目录里，加快安装速度

```shell
wget https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tar.xz -P $(pyenv root)/cache
```

安装依赖及设置环境变量

如果缺少 zlib 报错，则需要安装 zlib
zipimport.ZipImportError: can’t decompress data; zlib not available

安装 zlib
brew install zlib

设置zlib环境变量

```shell
export LDFLAGS="-L/usr/local/opt/zlib/lib"
export CPPFLAGS="-I/usr/local/opt/zlib/include"
```

如果缺少 SQLite3 警告，则需要设置sqlite 环境变量

```shell
The Python sqlite3 extension was not compiled. Missing the SQLite3 lib?
Installed Python-3.7.3 to /Users/slipper/.pyenv/versions/3.7.3
```

设置 sqlite 环境变量

```shell
export LDFLAGS="-L/usr/local/opt/zlib/lib -L/usr/local/opt/sqlite/lib"
export CPPFLAGS="-I/usr/local/opt/zlib/include -I/usr/local/opt/sqlite/include"
```

安装 python
pyenv install 3.7.3

## 卸载 python

```shell
pyenv uninstall 3.7.3
```