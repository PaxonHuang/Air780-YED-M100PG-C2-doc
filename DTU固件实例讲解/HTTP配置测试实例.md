# 一、工具简介

DTU配置平台:https://dtu.yinerda.com

DTU测试平台:http://test.yinerda.com

串口测试软件:"[YEDTestTools](https://yinerda.yuque.com/yt1fh6/4gdtu/rfvpd0gwbr6vhfb4)"软件,或者任意自己熟悉的串口调试软件。

USB转串口调试工具:"[YED-UUART-211](https://yinerda.yuque.com/yt1fh6/4gdtu/rfvpd0gwbr6vhfb4)"，集成电源，TTL，RS232，RS485专门为设备调试设计,或者任意自己熟悉的串口调试工具。

# 二、必要条件

**2.1、****如果您是首次使用DTU配置平台，请先参考**[**《WEB配置入门教程》**](https://yinerda.yuque.com/yt1fh6/4gdtu/textbcabgx9evwvd)**进行操作，包括设备的添加、分组的创建以及设备在分组中的分配。随后，依据本页指南完成云平台的参数设置及建立连接。**

**2.**2、设备接上天线，插上卡，正常10W电源供电，NET LED 500ms或者1000ms闪烁一次，表示网络正常。

# 三、配置参数视频教程

此处为语雀视频卡片，点击链接查看：[HTTP透传测试教程(1).mp4](https://yinerda.yuque.com/yt1fh6/4gdtu/sqk72vapn8l56hy0#Wy6F2)

# 四、配置参数图文教程

**注意:文档的图片直接看比较模糊，点击图片，放大看。**

## 4.1、获取测试服务器地址和端口

打开DTU测试平台:http://test.yinerda.com，选择“HTTP测试工具”，点击"打开"，可以获取到HTTP测试IP地址/域名 和端口号和URL。 IP地址或域名就是HTTP的服务器地址，任选其一,端口号就是HTP服务器的端口，url就是测试的rul。注意测试工具只支持http不支持https。实际自己服务器使用的时候，只需要填写https开头，支持https协议。

注意:浏览器工具只是用来测试和验证设备使用。10分钟没有任何交互会自动关闭服务器，如果发现数据不通了，重新刷新浏览器，重新打开，获取新资源测试。

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713938437095-ba6f01cb-8456-42c0-ba40-2436b144fa0f.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_48%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 4.2、配置参数

在“网络通道参数”界面配置HTTP协议的参数。注册包可以在不用串口参与的情况下，用来判断网络参数是否正常。注意如果是Get协议，根据HTTP协议要求，在url后面需要加英文的?问号，然后后面拼接有效数据，因为Get的数据是拼接在url后面传送的。Post协议无这个要求，Post的数据是在body里面上传的。

HTTP还是HTTPS说明：就是服务器地址开头http://xxx 或者https://xxx。

端口注意说明:如果服务器没有指定端口，HTTPS协议是443，HTTP是80。

URL注意说明:GET协议 url后面需要加英文的问号?；并且串口参数需要符合url编码格式



![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713938497640-7189e4a5-1cec-4677-a245-58055ea6e755.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_32%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 4.3、更新参数

配置完参数后，点击保存参数，断电重启设备，等待20-30秒让设备更新参数。

如果你只有一台设备，可以在分组里面，观察未更新设备数量，如果是0表示更新。

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713927027739-67dffe35-a4dc-4dfc-a572-8da3fc8f3d0d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_49%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)如果有多台设备，可以在设备列表里面查看，当“分组参数版本” 等于“设备参数版本”，表示参数更新了。

## ![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713927120390-06cb2022-c62c-495a-b84a-4d8698dba3eb.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_53%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)4.4、观察服务器连接情况

在测试服务器上面，观察连接状态，如果连接成功，测试服务器会收到注册包，并且显示连接信息。

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713938068618-2db0c982-03c5-4e35-aad5-e3b9da1bde84.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_48%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)![img](https://cdn.nlark.com/yuque/0/2024/png/36017947/1720857286425-3832ac7e-18d7-47ed-a91c-91fe180149a2.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_46%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 4.5、服务器发送数据到串口

 服务器不能主动给设备发数据，只有在设备请求服务器的时候，服务器才能返回。

## 4.6、串口发送数据到服务器

串口收到的数据，会透传到提前配置的url路径下。然后服务器返回的数据通过串口传出去。在使用过的时候，可以过滤返回码和head，只返回body数据给串口。

## ![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713938873854-637cca83-2b1f-4faf-9009-4538bfef027a.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_24%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)4.7、自定义head头部

HTTP中已经自动添加了Host、Connection、Content-Length三个请求头，其他的需要自己添加。

可以自定义添加，每个变量以/0d/0a结尾,支持变量，一般IMEI变量用来做设备身份识别。

Content-Type=application/json/0d/0aIMEI=${IMEI}/0d/0a

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1713939186088-aafa9316-c573-4ac1-bb46-1d806dd24a68.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_43%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



有时候服务器返回了信息，但是串口延迟很多，这个要检查服务器返回的 Connection 字段是不是keep-alive，如果是改成close试试Content-Type=close/0d/0a

![img](https://cdn.nlark.com/yuque/0/2025/png/804193/1751598984627-2945982f-9e15-469a-8f68-a07a383f164b.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_20%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

注意：有一种服务器如果用postman测试加了端口就访问不了，不加端口就能访问的服务器，Air780系列需要配置Host header 指向服务器域名。因为这种服务器的url不能添加端口（服务器配置的安全策略导致的）。

Host=服务器域名/0d/0a 

![img](https://cdn.nlark.com/yuque/0/2026/png/804193/1770773621830-bb4100c0-44c4-482e-9ca7-a7d4fc212e6b.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_25%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 4.8、HTTP 协议 url匹配变量

本功能需要Air780EPM 固件版本大于等于V2.0.4支持

可以直接在url后面加一下设备标识，方便路径分层。

如图在url后面加入了${IMEI}和${CSQ}变量，就指定替代了模块的IMEI编号和信号强度功能

![img](https://cdn.nlark.com/yuque/0/2025/png/804193/1765263159996-c63d5364-622f-4cb8-8dcd-8329639adffd.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_25%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

![img](https://cdn.nlark.com/yuque/0/2025/png/804193/1765263185481-a9ea5b6d-6fa2-4b6b-b433-5dee6eeb987f.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_38%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



## 4.9、服务器远程控制设备

HTTP协议，不能控制DTU内部资源，不支持远程控制命令。







   、】、