# 跨平台离线安装Python包

> Library reference
>
> [pypi](https://pypi.org/)
>
> 有时需要在内网离线环境安装 Python 包，并且开发和测试环境平台不一致，在此以 snmpsim 为例

## 获取 requirement.txt
```shell
# 首先在有网络的环境下安装 snmpsim，得到 requirement.txt
$ virtualenv venv
$ source venv/bin/activate
(venv)$ pip install snmpsim
(venv)$ pip freeze > requirement.txt
```

*requirement.txt*

```txt
ply==3.11
pyasn1==0.4.5
pycryptodomex==3.7.3
pysmi==0.3.3
pysnmp==4.4.9
snmpsim==0.4.7
```

## 下载所需 whl 格式安装包

根据 requirement.txt 文件，到 [pypi](https://pypi.org/) 去找对应的包

![1552305147357](D:\develop\project\how-to-use-it\离线安装Python包\download_page.png)

注意 python 版本以及 linux 平台带 u 与不带的区别，最好两个都下，一般用带 u 的，差异是 Unicode 码占 2 字节还是4 字节

## 安装

将安装包上传到服务器，解压后安装，注意安装顺序从上到下

```shell
$ pip install <python_package>.whl
```

