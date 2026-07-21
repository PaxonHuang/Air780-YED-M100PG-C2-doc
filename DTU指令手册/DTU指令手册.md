注意:非必要，建议使用WEB配置，方便后续修改参数，记录SIM卡ICCID，使用任务和数据模板等高级功能。

# 一、简介

1）参数配置

银尔达DTU透传固件支持串口命令配置设备参数。可以使用MCU发送配置命令配置，建议优先使用WEB配置，如果不满足需求，再用MCU配置。

2) 获取状态和控制

串口命令可以获取IMEI，ICCID，CSQ信号强度，检查数字量输入，控制继电器输出等状态，方便设备外部指示状态。

3)服务器远程控制

同时还可以通过服务器发送与串口相同的命令，把执行结果返回给服务器，实现设备远程控制功能，前提是开启“远程控制命令”功能

4)命令格式

串口命令是银尔达定义的私有命令，格式如config,get,imei\r\n 返回\r\nconfig,imei,ok,123456789012345\r\n

详情参看串口命令手册。

# 二、命令测试工程资料下载连接

https://yinerda.yuque.com/yt1fh6/4gdtu/rfvpd0gwbr6vhfb4

# 三、DTU 专用PC配置软件

目前只支持Air780系列DTU，Air724系列不支持。

https://yinerda.yuque.com/yt1fh6/4gdtu/rfvpd0gwbr6vhfb4

# 四、DTU透传固件命令

| 序号 | 网络协议                    | 测试实例                                                     |
| ---- | --------------------------- | ------------------------------------------------------------ |
| 1    | 指令使用场景                | [【点击查看命令】](https://yinerda.yuque.com/yt1fh6/4gdtu/gqi5k6m4dcpybiw9) |
| 2    | DTU透传固件命令约定         | [【点击查看命令】](https://yinerda.yuque.com/yt1fh6/4gdtu/fgo2thu9tug82slq) |
| 3    | DTU透传固件变量说明         | [【点击查看命令】](https://yinerda.yuque.com/yt1fh6/4gdtu/tt7pg323in5ak6vd) |
| 4    | DTU透传固件基本命令         | [【点击查看命令】](https://yinerda.yuque.com/yt1fh6/4gdtu/uaiefqly3vbfkhks) |
| 5    | DTU透传固件网络维护命令     | [【点击查看命令】](https://yinerda.yuque.com/yt1fh6/4gdtu/fatuge3231duksbc) |
| 6    | DTU透传固件定位命令         | [【点击查看命令】](https://yinerda.yuque.com/yt1fh6/4gdtu/qmevnwraemriyoo1) |
| 7    | DTU透传固件硬件资源命令     | [【点击查看命令】](https://yinerda.yuque.com/yt1fh6/4gdtu/gf4hhsu9crloo64r) |
| 8    | DTU透传固件APN设置命令      | [【点击查看命令】](https://yinerda.yuque.com/yt1fh6/4gdtu/sbpun6v584zgfsdl) |
| 9    | DTU透传固件设置串口参数命令 | [【点击查看命令】](https://yinerda.yuque.com/yt1fh6/4gdtu/xn4bft5x05amuq8n) |
| 10   | DTU透传固件自动轮询命令     | [【点击查看命令】](https://yinerda.yuque.com/yt1fh6/4gdtu/mp0e6tbgh0m2btab) |
| 11   | DTU透传固件网络协议命令     | [【点击查看命令】](https://yinerda.yuque.com/yt1fh6/4gdtu/gdn5iybdnmmxm1n6) |
| 12   | DTU透传固件发送短信命令     | [【点击查看命令】](https://yinerda.yuque.com/yt1fh6/4gdtu/toi12f0ybed582hx) |