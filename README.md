# EsDialerGD
广东天翼校园三方客户端（登陆器）

## 告示

由于本人**错误地选择了MIT协议**，导致部分有一定“商业头脑”的同学将本工程源码进行二次包装、再分发并**收费**。

然而这不是我所希望的，也与我开源EsDialerGD的出发点相违背。

因此决定停止更新**EsDialerGD**，以作为对自己错误行为的惩戒。

后会有期：）

## 简介

鉴权协议分析：https://4fk.me/post/gdes-auth-proto.html

## 功能更新

- 2018-09-26：增加对curl命令行调用的支持。如果不想使用libcurl，可在`Makefile`中的`LDFLAGS`加入`-DUSE_CURL_CMD`，并删除`-lcurl`。

- 2018-09-18：修复一个假性登陆成功导致的0间隔重试bug

- 2018-09-17：修改重试机制：若先前登陆成功，则掉线后忽略重试间隔立即重连。

- 2018-09-14：增加自动生成ClientID与MAC地址功能；增加对支持302跳转的支持；删除ESD_DEBUG选项，直接显示所有调试信息；删除连通性检查功能。

- 2018-05-30：增加自定义接口地址功能。

- 2018-05-24：EsDialerGD发布。

## 下载

仅macOS和Win32。
地址：https://github.com/claw6148/EsDialerGD/releases

## 使用方式

程序通过**环境变量**的方式进行参数传递

**必选变量**

- ``ESD_USERID`` 用户名

- ``ESD_PASSWD`` 密码

**可选变量**

- ``ESD_CDC_DOMAIN`` 学校拼音首字母小写

- ``ESD_CDC_AREA`` 区号

- ``ESD_CDC_SCHOOLID`` 学校ID

- ``ESD_RETRY_COUNT`` 重试次数；0-不重试直接退出；(-1)-无限重试；默认值：-1

- ``ESD_RETRY_DELAY`` 重试间隔（秒）；默认值：30

- ``ESD_TICKET_URL`` **Ticket接口URL**

- ``ESD_AUTH_URL`` **Auth接口URL**

- ``ESD_KEEP_URL`` **Keep接口URL**

- ``ESD_TERM_URL `` **Term接口URL**

- ``ESD_IPV4_ADDR`` **客户端IP地址**

备注：

1. ``ESD_CDC_DOMAIN``、``ESD_CDC_AREA``、``ESD_CDC_SCHOOLID``的获取需要抓官方客户端的包，在HTTP Headers里，不过测试发现三者留空也不影响。
2. 有的学校不能由程序动态获取上面**加粗项**的值，这里提供手动设置功能。手动指定的项将在动态获取阶段中直接跳过。

**例子**

将以下内容写到一个脚本文件内，保存到与EsDialer二进制文件同级目录下。

UNIX（对应``wrapper-unix.sh``）

```
#!/bin/sh

export ESD_USERID=foo
export ESD_PASSWD=bar

# export ESD_RETRY_COUNT=-1
# export ESD_RETRY_DELAY=20

# export ESD_TICKET_URL=http://XXX/ticket.cgi
# export ESD_AUTH_URL=http://XXX/auth.cgi
# export ESD_KEEP_URL=http://XXX/keep.cgi
# export ESD_TERM_URL=http://XXX/term.cgi
# export ESD_IPV4_ADDR=XXX.XXX.XXX.XXX

# 运行
./EsDialer
```

Windows（对应``wrapper-win.bat``）

```
@echo off

set ESD_USERID=foo
set ESD_PASSWD=bar

REM set ESD_RETRY_COUNT=-1
REM set ESD_RETRY_DELAY=20

REM set ESD_TICKET_URL=http://XXX/ticket.cgi
REM set ESD_AUTH_URL=http://XXX/auth.cgi
REM set ESD_KEEP_URL=http://XXX/keep.cgi
REM set ESD_TERM_URL=http://XXX/term.cgi
REM set ESD_IPV4_ADDR=XXX.XXX.XXX.XXX

REM 运行
.\EsDialer

```

## 源码

``main.c`` 程序入口

``auth.c`` 鉴权交互逻辑

``codec.c`` 通信数据编解码

``md5.c`` MD5函数

``str_extract.c`` 字符串提取函数

``http_req.c`` HTTP请求

## 编译方法

程序依赖libcurl，请先装好。

```
git clone https://github.com/claw6148/EsDialerGD.git
cd EsDialerGD
```

1. 本机编译：直接``make``。

2. 交叉编译：修改Makefile，``CROSS=``后面添上目标平台前缀，如``CROSS=arm-linux-androideabi-``，然后``make``。

## 问题反馈

邮箱：a@4fk.me
