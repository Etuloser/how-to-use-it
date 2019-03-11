# snmpsim
> Library reference
>
> [Github Repo](https://github.com/etingof/snmpsim)
>
> [HomePage](http://snmplabs.com/snmpsim/)
>
> snmpsim 是一个用来模拟网络设备的 snmp agent 的工具，可以用来测试诸如 Zabbix 之类的监控工具

## Quick start

### Installation

```shell
$ virtualenv venv
$ source venv/bin/activate
$ pip install snmpsim
```

### Run SNMP simulation

```shell
# 要注意的是，这里的 DIR 是相对 snmpsimd.py 文件
$ snmpsimd.py --data-dir=./data --agent-udpv4-endpoint=127.0.0.1:1024
```

### Test the setup

```shell
# 使用 snmpwalk 简单测试
$ snmpwalk -v2c -c public 127.0.0.1:1024 system
```

## 命令行参数

```shell
# 以守护进程模式启动 
--daemonize

# 解决进程权限问题
--process-user=root --process-group=root

# 模拟器数据 .snmprec 所在目录
--data-dir=

# 输出目录
--output-file=

# 要加载的 mib 模块，常用 SNMPv2-MIB、IF-MIB
-mib-module=
```

## 构建模拟器数据

```shell
# 可以使用 snmpwalk 来生成一份真实设备的信息快照
$ snmpwalk -v2c -c public -ObentU localhost 1.3.6 > myagent.snmpwalk
# 使用 mib2dev.py 来构建一份基于 mib 库的模拟器数据
$ mib2dev.py --mib-module=SNMPv2-MIB --start-oid=1.3.6.1.2.1.1.1 --stop-oid=1.3.6.1.2.1.1.8 --output-file=public.snmprec
```

## 处理 .snmpwalk 数据

```shell
# 使用 datafile.py 来处理 .snmpwalk 数据，转换成 .snmprec 格式
# 需要注意的是 datafile.py 无法处理多行数据，会报错
# 文件名即共同体
$ datafile.py --input-file=linux.snmpwalk --source-record-type=snmpwalk --output-file=public.snmprec
```

