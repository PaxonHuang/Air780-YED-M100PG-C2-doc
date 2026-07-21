想要通过服务器远程控制DTU的硬件资源有两种方式。

第一种方式是通过DTU标准的config命令去控制。

第二种方式是通过任务功能截取数据，然后解析数据来控制，这种方式比较自由，可以任意定义数据的格式和类型，比如JSON、字符串、Modbus等格式，不过需要编写代码，语言为Lua。如果根据demo无法自己写出的，可以提供详细需求，找工程师支持。

 一、工具简介

DTU配置平台:https://dtu.yinerda.com

DTU测试平台:http://test.yinerda.com

串口测试软件:"[YEDTestTools](https://yinerda.yuque.com/yt1fh6/4gdtu/rfvpd0gwbr6vhfb4)"软件,或者任意自己熟悉的串口调试软件。

USB转串口调试工具:"[YED-UUART-211](https://yinerda.yuque.com/yt1fh6/4gdtu/rfvpd0gwbr6vhfb4)"，集成电源，TTL，RS232，RS485专门为设备调试设计,或者任意自己熟悉的串口调试工具。

 二、必要条件

2.1、如果您是首次使用DTU配置平台，请先参考[《WEB配置入门教程》](https://yinerda.yuque.com/yt1fh6/4gdtu/textbcabgx9evwvd)进行操作，包括设备的添加、分组的创建以及设备在分组中的分配。随后，依据本页指南完成云平台的参数设置及建立连接。

2.2、设备接上天线，插上卡，正常10W电源供电，NET LED 500ms或者1000ms闪烁一次，表示网络正常。

 三、使用config命令远程控制

注意:文档的图片直接看比较模糊，点击图片，放大看。

config命令控制方式，Air724/Air780/Air780EP/ Y100P全系列通用。

 3.1、获取测试服务器地址和端口

打开DTU测试平台:http://test.yinerda.com，选择“MQTT测试工具”，点击"打开"，可以获取到MQTT测试IP地址/域名 和端口号，登录客户端ID，登录用户名，登录密码。 IP地址或者域名就是MQTT的服务器地址，任选其一。

注意:浏览器工具只是用来测试和验证设备使用。10分钟没有任何交互会自动关闭服务器，如果发现连接不上了，重新刷新浏览器，重新打开，获取新资源测试。

![图片.png](https://cdn.nlark.com/yuque/0/2024/png/804193/1713939841423-db317668-3f38-4bde-a0b5-bbe8a3e548e9.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_48%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_48%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

 3.2、配置参数

想要实现远程控制的前提是在基本参数打开远程控制命令选项。此选项默认为关闭，需要手动打开并保存。

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42958080/1729221666712-616dea04-ac45-4288-843b-4ae522fbd2bb.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_37%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

在“网络通道参数”界面配置MQTT协议的参数。填写参数的时候，注意，不要填写错了，不要有空格。

注意心跳包发送间隔时间，Air724系列可调，Air780/Y100P系列固定240秒。

注意协议版本，Air724系列可设置3.1和3.1.1。Air780/Y100P系列只能设置3.1.1，其他无效。

![图片.png](https://cdn.nlark.com/yuque/0/2024/png/804193/1713939953729-73c02751-9730-4206-a4e1-54b703c2b5e4.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_32%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_32%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

![图片.png](https://cdn.nlark.com/yuque/0/2024/png/804193/1713940149950-3f323ca5-a597-4784-a7cb-c2f9af2110be.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_31%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_31%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

 3.3、更新参数

配置完参数后，点击保存参数，断电重启设备，等待20-30秒让设备更新参数。

如果你只有一台设备，可以在分组里面，观察未更新设备数量，如果是0表示更新。

![图片.png](https://cdn.nlark.com/yuque/0/2024/png/804193/1713927027739-67dffe35-a4dc-4dfc-a572-8da3fc8f3d0d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_49%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_49%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

如果有多台设备，可以在设备列表里面查看，当“分组参数版本” 等于“设备参数版本”，表示参数更新了。

 ![图片.png](https://cdn.nlark.com/yuque/0/2024/png/804193/1713927120390-06cb2022-c62c-495a-b84a-4d8698dba3eb.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_53%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_53%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)3.4、观察服务器连接情况

在测试服务器上面，观察连接状态，如果连接成功，测试服务器会收到注册包，并且显示连接信息。

![图片.png](https://cdn.nlark.com/yuque/0/2024/png/804193/1713940323515-943cb9eb-a1ed-496d-82ba-28c670b500e8.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_42%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_42%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

 3.5、发送config命令控制设备

当成功连接到服务器后，下发控制继电器命令和读取设备IMEI命令。控制继电器等硬件命令只有使用的设备具有对应的硬件资源才能支持。注意如果有多个topic的，发送控制命令只能用第一个订阅的topic。应答用第一个发布的topic。

例：开继电器1 config,set,doout,1,1\r\n 

关继电器1 config,set,doout,1,0\r\n 

读取IMEI config,get,imei\r\n

重启设备config,set,reboot\r\n

详细命令解析和命令功能跳转指令手册：



DTU指令手册

注意:非必要，建议使用WEB配置，方便后续修改参数，记录SIM卡ICCID，使用任务和数据模板等高级功能。一、简介1）参数配置银尔达DTU透传固件支持串口命令配置设备参数。可以使用MCU发送配置命令配置，建议优先使用WEB配置，如果不满足需求，再用MCU配置。2) 获取状态和控制串口命令可以获...

4G DTU产品教程

当设备连接到MQTT测试服务器后，首先打开回车换行选项，这样下发的数据会自动后缀回车换行（\r\n），然后在发送topic区域填写设备的订阅topic，在发送窗口下发命令，填写命令的时候，后面不要带\r\n，下发格式和命令无误会收到设备的回复。

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42958080/1729231409180-380a55b5-7bbf-426b-80a1-9e56047ba251.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_54%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

 四、通过任务实现自定义命令控制继电器

由于其他步骤与第三小节的步骤相同，因此不再重复描述。对于部分不同的步骤，将在下文中单独进行说明。

使用自定义命令控制需要使用任务功能，截取服务器下发的数据，然后对数据进行解析，然后调用相应的API来实现各种逻辑。

不过需要注意启用任务并不会影响DTU的config命令，如果打开了远程控制，服务器下发正确的config命令，设备依旧会识别并正常返回数据。

Air724系列是lua 5.1版本，Air780/Air780EP/ Y100P系列是lua 5.3 版本，示例代码功能相同，系统api不一样，不兼容，注意不要混用。

解析json数据，判断cmd字符串数据"on1"打开继电器1；"on2"打开继电器2;"off1"关闭继电器1；"off2"关闭继电器2；"onoff1" 点动继电器1,打开后延迟2秒关闭。

服务器发送命令:

{"cmd":"on1"}

{"cmd":"off1"}

{"cmd":"onoff1"}

 4.1、Air724系列任务示例代码



 4.2、Air780/Air780EP/ Y100P系列任务示例代码





Air780/Air780EP/ Y100P系列示例代码



1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50

51

52

53

54

55

56

function

​	local taskname="userDataTask"

​	log.info(taskname,"start")

​	local nid,netr,nets=1,"",""

​	local uid,uartr,uarts=1,"",""

​	local oldtime=os.time()

​	PronetStopProRecCh(1)

​	PerSetDo(1,0)

​	PerSetDo(2,0)

​	while true do

​	    --解析网络数据

​		netr = PronetGetRecChAndDel(nid)

​		if netr then 

​			log.info(taskname,"netr data",netr)

​			local j =json.decode(netr)

​			if j and type(j)=="table" and j.cmd then 

​				if j.cmd =="on1" then 

​					PerSetDo(1,1)

​				elseif j.cmd =="on2" then 

​					PerSetDo(2,1)

​				elseif j.cmd =="off1" then 

​					PerSetDo(1,0)

​				elseif j.cmd =="off2" then 

​					PerSetDo(2,0)

​				elseif j.cmd =="onoff1" then 

​					PerSetDo(1,1)

​					sys.wait(2000)

​					PerSetDo(1,0)

​				end 

​				

​				local jb={}          --返回服务器下发成功数据 

​				jb.cmd =j.cmd.."cmdbck"

​				local bck = json.encode(jb)

​				if bck and 1==PronetGetNetSta(nid) then 

​					PronetSetSendCh(nid,bck)

​				end 

​			end 

​		end 

​		local nowtime=os.time()

​		if nowtime-oldtime >= 60 then

​			oldtime = nowtime

​			local d={}

​			d.imei=mobile.imei()

​			d.csq=mobile.csq()

​			d.out1sta=PerGetDoSta(1)

​			d.out2sta=PerGetDoSta(2)

​			d.in1sta=PerGetDiById(1)

​			d.in2sta=PerGetDiById(2)

​			local updata=json.encode(d)

​			if updata and 1==PronetGetNetSta(nid) then 

​				PronetSetSendCh(nid,updata)

​			end 

​		end

​		sys.wait(100)

​	end

end

 4.3、参数配置

TCP协议的参数同第三节一致。只是示例代码使用的通道id为1，所以配置网络通道必须要选择通道1配置服务器

任务配置：打开任务功能，将代码粘贴至当前位置，注意检查function前和最后一个end后不要有空格。

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42958080/1729239608131-df60d864-78f1-4e59-8da5-efd45b4c68c3.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_29%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

 4.4、示例演示

当成功连接到服务器，在发送topic窗口填写DTU的订阅topic，然后在发送窗口下发任务自定义的命令控制继电器的开关和点动。

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42958080/1729242346945-579051e8-7d77-41a7-bd85-710c06eaad24.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_46%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)