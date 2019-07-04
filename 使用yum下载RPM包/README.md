# 使用yum下载RPM包

> Library reference
>
> [How to use yum to download a package without installing it](https://access.redhat.com/solutions/10154)

## Yumdownloader

```shell
$ yum install -y yum-utils
# 默认下载在当前目录，--destdir=DESTDIR	destination directory (defaults to current directory)
$ yumdownloader --downloadonly <package>
```

```bash
$ yum install --download
```



