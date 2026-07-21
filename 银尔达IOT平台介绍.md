<font style="color:#DF2A3F;">注意本文档是介绍银尔达IOT平台的，感兴趣的客户可以看一下。</font>

<font style="color:#DF2A3F;">如果只用银尔达DTU平台连接自己的平台，不需要关心IOT平台介绍。</font>



服务器连接：[https://iot.yinerda.com](https://iot.yinerda.com/)

使用入门教程:[https://yinerda.yuque.com/yt1fh6/iot/mmtu92gx798qmo2n](https://yinerda.yuque.com/yt1fh6/iot/mmtu92gx798qmo2n#)

银尔达物联网平台(IOT)平台，是自主研发平台。

IOT平台公版能够快速的让设备对接服务器，实现设备研发，功能评估，原型搭建，对自有品牌要求不高的场景。

IOT平台支持私有应用，可以部署自己品牌的小程序，推广自有品牌。

IOT平台支持私有化部署，在自己服务器部署IOT平台，独立的服务器，安全，可靠，方便推广自己的品牌。

IOT平台支持定制，包含功能定制，UI定制，小程序定制，APP定制等服务。



# 一、银尔达IOT平台与DTU配置平台，DTU任务的关系
从本质来讲，IOT平台和DTU配置没任何关系。IOT是独立的业务数据处理平台。他的数据来源可以是DTU和RTU或者其他网络设备，比如电脑。

在使用上面注意：

1、 IOT平台是业务平台，是收集和显示数据控制设备的，可以对接任何设备，比如银尔达的DTU，RTU  或其他联网设备，只要按照银尔达协议上报和解析数据就能对接。<font style="color:#DF2A3F;">IOT平台是收费的</font>。

2、 DTU配置平台是配置DTU/RTU要连接的服务器和参数的，配置DTU参数连接IOT平台或者其他平台 ，比如配置服务器的IP，topic，串口波特率等。<font style="color:#DF2A3F;">DTU配置平台是免费的。</font>

3、DTU的任务是DTU具有的一个边缘计算功能代码片段，通过DTU配置平台配置到设备本地运行的。 让银尔达的DTU/RTU 按IOT要求的协议格式组装数据上报，解析数据控制外设用的 。代码部分就是需要懂研发懂技术的工程师来做。客户可以参考demo自己修改。也可以让银尔达工程师根据客户需求代替实现，客户只需要提供详细逻辑需求，工程师评估就可以。收费根据需求工作量评估。

# 二、WEB数据控制界面
<img src="https://cdn.nlark.com/yuque/0/2025/png/804193/1755485664659-4be14c8d-649c-49d4-84d6-133dc46e49e4.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_53%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" width="1859" title="" crop="0,0,1,1" id="u6b418816" class="ne-image">

<img src="https://cdn.nlark.com/yuque/0/2025/png/804193/1755485638368-e4b119b3-115a-4bd7-a6f0-383159c03e83.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_51%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" width="1795" title="" crop="0,0,1,1" id="u7fc10190" class="ne-image">

# 三、小程序UI设计界面
UI编辑，可见可得

<img src="https://cdn.nlark.com/yuque/0/2025/png/804193/1755485810303-6764751a-a492-4fe2-97d1-2f4a0b367ea4.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_54%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" width="1881" title="" crop="0,0,1,1" id="u82505246" class="ne-image">

# 四、小程序显示UI
<img src="https://cdn.nlark.com/yuque/0/2025/jpeg/804193/1755512684629-90430d2d-9c41-4766-9376-783b5ad5c086.jpeg?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_31%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" width="238" title="" crop="0,0,1,1" id="ua591ac45" class="ne-image"> <img src="https://cdn.nlark.com/yuque/0/2025/jpeg/804193/1755512581490-006644cc-1fab-4068-b75b-dafd9b6f7ecf.jpeg?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_31%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" width="237" title="" crop="0,0,1,1" id="u8894e593" class="ne-image"><img src="https://cdn.nlark.com/yuque/0/2025/jpeg/804193/1755512689141-c88852fd-a7c8-45f7-b505-bd953f8f4637.jpeg?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_31%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" width="237" title="" crop="0,0,1,1" id="u715c5b91" class="ne-image">

# 五、IOT对接评估说明
IOT平台可以能很方便快速的实现一个特定需求功能的平台，涉及到使用的银尔达硬件产品，用户硬件的通讯控制协议，业务逻辑控制流程，UI控制和显示数据。对于有研发能力熟悉lua语言的用户可以自己根据dmeo修改。对于普通客户来讲没办法快速，稳定的实现设备对接数据处理，也不用担心，只要你能提出设备的工作逻辑需求，工程师会给你处理好。<font style="color:#DF2A3F;">对于有需求的客户，可以整理填写自己的需求，我们来给你处理设备对接的问题，这样可以大大减小项目研发周期。</font>

<font style="color:#DF2A3F;">复杂的项目需求一般与用户实际需求和项目有关系，比如使用RS485控制变频器，RS485采集电表电能；比如完成一套农业大棚控制系统；比如完成一套自动洗碗机系统等。这种复杂的需求一般包含数据采集，输入检查，输出控制，并且有流程有逻辑的有时序的需求，还会涉及到APP控制UI等。</font>

需求有很多，但是无非就是围绕串口采集和控制，继电器控制，输入检查，模拟量检查，屏幕显示，WEB管理，小程序控制，然后实现一定需求的业务逻辑。可以根据自己的需求和工程师沟通评估是否能实现，快速评估。

联系电话:杨洋 13560789567。

IOT平台可实现的常见项目和功能：

| 功能 | 说明 | 工作量 |
| --- | --- | --- |
| 简单资源控制 | 单继电器远程控制，远程断电，断油，工程收款控制，电灯，电机控制，设备启停等 | 很容易<br/>30分钟完成 |
| | 输入报警器，设备状态检测，震动检查，水位检查，门磁检查，转动停止检查等 | 很容易<br/>30分钟完成 |
| | 所有的4-20ma,1-10V ,1-5V 模拟量传感器的数据采集检测 | 很容易<br/>30分钟完成 |
| 串口采集和控制 | modbus协议的传感器数据采集，比如电表，水表，气压，水位传感器等数据采集 | 简单<br/>0.5天完成 |
| | modbus协议传感器的数据采集和远程控制 | 容易<br/>1~2天完成 |
| 项目类型 | 使用RS485对接温控器，实现温度参数的设置和参数的读取 | 容易<br/>1个星期完成 |
| | 使用RS485对接变频器控制正反转，检查电压，电流，故障 | 容易<br/>1~2个星期完成 |
| | 智慧机房监控，RS485采集电压，功率电流，实现继电器的远程控制启停，实现环境温度湿度的监控，风机电机的监控 | 复杂<br/>1~2个星期完成 |
| | 智慧农业大棚控制，RS485串口屏幕显示，继电器控制，输入检查，传感器的检查，定时灌溉 | 困难<br/>一个月完成 |
| | 智慧家电，比如咖啡机，串口屏控制显示进度，一键冲泡，后台形成记录，远程锁机 | 困难<br/>一个月完成 |


# 
