# 一、工具简介

DTU配置平台:https://dtu.yinerda.com

DTU测试平台:http://test.yinerda.com

串口测试软件:"[YEDTestTools](https://yinerda.yuque.com/yt1fh6/4gdtu/rfvpd0gwbr6vhfb4)"软件,或者任意自己熟悉的串口调试软件。

USB转串口调试工具:"[YED-UUART-211](https://yinerda.yuque.com/yt1fh6/4gdtu/rfvpd0gwbr6vhfb4)"，集成电源，TTL，RS232，RS485专门为设备调试设计,或者任意自己熟悉的串口调试工具。

# 二、必要条件

**2.1、****如果您是首次使用DTU配置平台，请先参考**[**《WEB配置入门教程》**](https://yinerda.yuque.com/yt1fh6/4gdtu/textbcabgx9evwvd)**进行操作，包括设备的添加、分组的创建以及设备在分组中的分配。随后，依据本页指南完成云平台的参数设置及建立连接。**

**2.**2、设备接上天线，插上卡，正常10W电源供电，NET LED 500ms或者1000ms闪烁一次，表示网络正常。

# 三、配置参数视频教程

此处为语雀视频卡片，点击链接查看：[TCP透传测试教程.mp4](https://yinerda.yuque.com/yt1fh6/4gdtu/wr3hgtr9gav7muvs#PyLVt)

# 四、配置参数图文教程

**注意:文档的图片直接看比较模糊，点击图片，放大看。**

## 3.1、获取测试服务器地址和端口

打开DTU测试平台:http://test.yinerda.com，选择“TCP测试工具”，点击"打开"，可以获取到TCP测试IP地址/域名 和端口号。 IP地址或者域名就是TCP的服务器地址，任选其一。

注意:浏览器工具只是用来测试和验证设备使用。10分钟没有任何交互会自动关闭服务器，如果发现连接不上了，重新刷新浏览器，重新打开，获取新资源测试。

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713926527433-97c2f5a7-283d-4964-a3c5-d7d41ee3c34d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_42%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 3.2、配置参数

在“网络通道参数”界面配置TCP协议的参数。

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713926735742-683b72ac-e1e5-451f-818a-fd2695682511.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_29%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 3.3、更新参数

配置完参数后，点击保存参数，断电重启设备，等待20-30秒让设备更新参数。

如果你只有一台设备，可以在分组里面，观察未更新设备数量，如果是0表示更新。

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713927027739-67dffe35-a4dc-4dfc-a572-8da3fc8f3d0d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_49%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)如果有多台设备，可以在设备列表里面查看，当“分组参数版本” 等于“设备参数版本”，表示参数更新了。

## ![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713927120390-06cb2022-c62c-495a-b84a-4d8698dba3eb.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_53%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)3.4、观察服务器连接情况

在测试服务器上面，观察连接状态，如果连接成功，测试服务器会收到注册包，并且显示连接信息。

注意连接列表显示的IP和端口，并不是4G模块的地址，是运营商基站的地址。

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713927625103-12f05a23-a344-44ec-a43d-5c5d8c6f953b.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_42%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 3.5、服务器发送数据到串口

 打开银尔达调试工具，串口工具连接模块。在发送串口输入数据后，点击发送，数据就会透传到设备串口。如果测试的时候发现没有传送成功。检查一下网络通道参数里面绑定的串口是否正确，或者检查一下测试服务器是否过期，重新刷新浏览器，重新配置参数。

![img](https://cdn.nlark.com/yuque/0/2024/png/36017947/1720839809742-8eecfcc8-35c8-4a1b-a8e0-058e40eae12d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_37%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 3.6、串口透传数据到服务器

串口数据一般是字符串和HEX 2种，在测试服务器上面可以点击HEX模式看原始数据。![img](https://cdn.nlark.com/yuque/0/2024/png/36017947/1720839847288-f694bcb1-26c0-44f4-9585-01de3db77b08.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_37%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 3.7、服务器远程控制设备

方法1：

  服务器直接发送config命令控制查询设备。

  命令详情查看DTU命令手册:https://yinerda.yuque.com/yt1fh6/4gdtu/zyngfvlgylqny15n

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713928384464-db3ce7d0-a5da-4f01-8601-19fb4a6d1826.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_34%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713929309317-ca615d43-65df-4cff-80c9-37f6a2a96405.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_48%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)方法2:

  服务器发任意数据，DTU用任务的方式，解析服务器数据，控制设备查询设备。

  使用任务控制继电器参考:https://yinerda.yuque.com/yt1fh6/4gdtu/wzlywm8diivdv551

  复制任务到DTUWEB平台，更新参数，控制台发送{"cmd":"on1"}控制继电器，{"cmd":"off1"}关闭继电器1。



​     