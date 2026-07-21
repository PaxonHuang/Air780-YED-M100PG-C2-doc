WIFI定位目前只有Air780支持,固件版本等于大于V1.1.1。

# 一、工具简介

DTU配置平台:https://dtu.yinerda.com

DTU测试平台:http://test.yinerda.com

串口测试软件:"[YEDTestTools](https://yinerda.yuque.com/yt1fh6/4gdtu/rfvpd0gwbr6vhfb4)"软件,或者任意自己熟悉的串口调试软件。

USB转串口调试工具:"[YED-UUART-211](https://yinerda.yuque.com/yt1fh6/4gdtu/rfvpd0gwbr6vhfb4)"，集成电源，TTL，RS232，RS485专门为设备调试设计,或者任意自己熟悉的串口调试工具。

# 二、必要条件

2.1、参考[《WEB配置入门教程》](https://yinerda.yuque.com/yt1fh6/4gdtu/textbcabgx9evwvd)，完成**添加设备，创建分组，分组里面分配设备。**

**2.**2、设备接上天线，插上卡，正常10W电源供电，NET LED 500ms或者1000ms闪烁一次，表示网络正常。

# 三、定位说明

## 3.1、限制说明

定位分基站定位，GPS定位和WIFI定位，各有优势和劣势。

基站定位，可以在室内，室外定位，中国大陆使用，但是误差大，500~几公里。

GPS定位，精确，全球能用，但是必须在室外空旷定位。

WIFI定位，精确，中国大陆使用，必须需要有WIFI信号的地方。

基站和WIFI定位，都需要定位服务器的支持，如果基站和WIFI信息不在服务器里面，就无法定位。

## 3.2、位置纠偏

不管是基站定位还是GPS定位，定位数据都是WGS-84坐标，不能直接用在地图上面定位，直接使用有偏差。需要纠偏使用。

纠偏测试地址和方法: http://old.openluat.com/GPS-Offset.html

## 3.3、定位数据小数点与精度的对应关系

dd.dddd格式数据要转换成113.XXXXXXX，XXX是6-7位小数。

| 小数点后位数 | 精度(约等于) |
| ------------ | ------------ |
| 第1位        | 10000米      |
| 第2位        | 1000米       |
| 第3位        | 100米        |
| 第4位        | 10米         |
| 第5位        | 1米          |
| 第6位        | 0.1米        |
| 第7位        | 0.01米       |
| 第8位        | 0.001米      |

# 四、基站定位

数据支持不上报、固定格式和自定义格式；不支持原始数据上报。

不上报，一般是由于串口需要获取或者在任务里面获取基站定位。如果不提前开启，调用获取基站定位地方可能最大延迟是60秒才会成功。会明显加长等待时间。

固定格式,固定为经度_纬度。

自定义，一般使用基站定位变量，自己组织上报格式。

注意:基站定位和WIFI定位都需要耗费流量，一次定位预计要1K左右，合理设置定位间隔，不免流量过多消耗。

##  4.1、固定格式上报

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1714110508255-2424b5db-ce1c-44b0-83b9-5211dd49c56d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_31%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)服务器收到的数据格式如下

## ![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1714110691531-4af384a8-6ff0-4e10-b222-c11f8328d92e.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_31%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)4.2、自定义格式上报

自定义格式上报，支持变量，可以注册成自己需要的字符串格式，比如json格式。

设置数据: {"GPS":"${GPS}","LBSLON":"${LBSLON}","LBSLAT":"${LBSLAT}"}

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1714110966301-ccd62b63-a9ba-4c5a-9259-a32fda102e17.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_33%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

服务器收到的数据格式如下

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1714110993690-7107b216-7a6d-4f92-9f81-52c1d3a8a216.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_38%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)纠偏工具上查询的地址，大概差距20米左右。

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1714113395041-e245fb59-594d-4867-aaf1-8b86c5698a53.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_32%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

# 五、GPS定位

数据支持不上报，固定格式和自定义格式，原始数据不支持。

注意GPS定位，天线必须放到室外定位，即使放到窗边，也可能无法定位。GPS天线方向朝天方向。GPS原始数据是dd.dddd格式。定位成功后，如果设备有GPS LED，会点亮或者闪烁。第一次定位时间，如果信号环境好，可能比较久，40-60秒，耐心等待。

## 5.1、固定格式上报

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1714111256259-fe7a271d-ae9b-40c0-8eff-36eca0c91e7e.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_30%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)服务器收到的数据如下:

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1714113135282-f13ca8c5-1367-433c-b255-e5fbddc853cd.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_41%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 5.2、自定义格式上报

自定义格式上报，支持变量，可以注册成自己需要的字符串格式，比如json格式。

设置数据GPS_${GPSLONT}_${GPSLON}_${GPSLATT}_${GPSLAT}_END

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1714112330473-170bc286-410a-475f-8f83-5728e5ffa364.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_32%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

务器收到的数据如下:

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1714112821968-a2372ec6-23f2-4893-bc69-52f549e91130.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_41%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

纠偏工具上查询的地址，多次定位后去看偏离情况，天线是放在玻璃窗附近。

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1714113615271-94df74b6-78ba-4a65-93c8-391b2f86b884.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_25%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

![img](https://cdn.nlark.com/yuque/0/2024/png/804193/1714113102195-42c362a6-78c6-4dec-8cfc-7faf7fa09460.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_32%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

# 六、WIFI定位

wifi定位只需要开启即可，与设备自动与基站定位配合使用。WIFI优先，如果WIFI无法定位使用基站信息定位，不用外部干预。

# 七、任务在定位功能的应用

4,5,6点讲解独立的定位功能，可以自动上报简单功能。如果想获取GPS的详细信息，比如速度，角度，海拔等，如果想实现GPS的超低功耗定位，周期休眠定位，或者把定位数据和业务有效数据拼接成一个包上传给服务器。需要利用任务来实现。详情可以参考任务实现，或者找工程师代写。



# 八、实际应用常见的问题

## 1、原始数据小数点只有4位，能不能更多

目前GPS定位数据是固定的度分dd.dddd格式数据,小数点后面4位，要转换成113.XXXXXXX，XXX保留是6-7位小数。

## 2、GPS纠偏是否有SDK或者在设备上纠偏

没有，GPS定位不准与地图有偏差：问题在于没有纠偏，基站定位和GPS定位的坐标，都是WGS-84标准坐标，需要纠偏，不能直接用在地图上面，具体原因可以百度。

纠偏的方法是针对地图来的，不是纠偏设备，比如百度、高德或，腾讯这些方法都不一样，所以不能设备纠偏，需要服务器纠偏，用什么定位供应商就用他们提供对应的API纠偏。具体可以去看对应地图供应商的API。可以参考示例:http://old.openluat.com/GPS-Offset.html

## 3、GPS天线安装的注意事项

1) 所有金属实体会完全遮闭GPS信号。注意含有金属涂层的汽车贴膜对GPS也有遮闭作用。 
2) 厚度为1厘米以上的水体（液态）会完全衰减GPS信号至不可用。 
3) 玻璃和塑料对GPS信号有轻度衰减，所以你可以把GPS放在衣服口袋里面，或汽车风档下。
4) 厚重的非金属物体会完全遮挡信号，比如几厘米的木头会完全遮闭GPS信号。 
5) 1575.42MHz的微波信号波长是19厘米，所以理论上可以透过孔径(直径)大于19厘米的金属网，或普通金属绗架结构，比如输电铁塔。 