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

## 下载包

```bash
# 找一台与部署环境发行版本一致 Python 解释器版本一致的，能上外网的机器下载对应依赖包
$ pip3 download -r requirement.txt -d libs/
$ tar -cf libs.tar libs/ requirement.txt
```



## 安装

将安装包上传到服务器，解压后安装

```shell
$ pip install -r requirement.txt --no-index --find-links=/python_package
```
