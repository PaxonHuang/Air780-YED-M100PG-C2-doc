任务和数据模板代写:可以先参考demo自己修改一下，如果不会修改，可以提交需求文档，然后让银尔达工程师评估代写！根据工作量，简单的不收费，难的可能会收费。



# 零、任务教学视频

讲解了任务能做什么，如果提需求，如何看API，如何检查语法，调试打印日志

https://www.bilibili.com/video/BV1pN1aBuEr7

# 一、功能描述

YED-GNNS1-P,G2111Y 是专门的定位器硬件 支持8-90V供电,1路ACC输入检测(8~90V 高电平触发),1路输出,可以驱动继电器等。

输出是一般控制继电器,继电器控制油路电路等，一般情况是接长闭,要断的时候断开 。支持最高24V控制

ADC能检测到8-90V电压,一般用来检测电平电压

经度, 正数为东经, 负数为西经。纬度, 正数为北纬, 负数为南纬。

# 二、任务

## 2.1、代码

```lua
function 
	local taskname="userTask"
	log.info(taskname,"start")
	local nid=1
    libgnss.debug(true)
	
	while true do 
		local gpscount =0
		local lngt,latt,speed =nil,nil,nil
		--等待GPS定位成功，最多等60秒
		while  gpscount < 60 do 
			if libgnss.isFix() then 
				local tg =libgnss.getRmc(2)
				lngt=tg.lat
				latt=tg.lng
				speed=tg.speed
				break
			end 
			gpscount =gpscount+1
			sys.wait(1000)
		end 
		local d ={}
		d.imei=mobile.imei()
		d.csq=mobile.csq()
		d.vin = PerGetAdcGatherValByAdcId(0)
		d.vbatt=PerGetVbattV()
		d.in1 = PerGetDiById(1)
		d.isFix =0
		if lngt and latt and speed then 
			d.isFix =1
			d.lngt = lngt
			d.latt = latt
			d.speed = speed
		end 
		local updata = json.encode(d)
		log.info(taskname,"updata",updata)
		if updata and PronetGetNetSta(nid) ==1 then 
			PronetSetSendCh(nid,updata)
		end
		sys.wait(30000)--单位ms
	end 
end
```

## 2.2、任务讲解

libgnss.getRmc是获取经纬度，速度这些，还可以获取海拔，航向，方向角和基站WIFI定位信息信息，具体可以参考合宙API和下面第四点的扩展数据上报。

![img](https://cdn.nlark.com/yuque/0/2025/png/804193/1755828241210-75903c7c-b797-43e3-a6d3-21f3ada42ff7.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_22%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

![img](https://cdn.nlark.com/yuque/0/2025/png/42958080/1760085870984-8d9f05c4-e1cb-4551-913e-8755de5b5061.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_23%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

# 三、测试

## 3.1、设备上报的数据

vbatt电压，是说的模块VBATT电压，如果是宽电压供电的，固定的是3.7V左右。如果你直接接电池，就是电池电压M100PG-C 。

有一部设备是电池供电，但是内部用有稳压芯片，用vin参数读取电池电压,不是vbatt。比如G2200，G2200-P

有一部设备的供电电压是vin 参数，使用ADC采集的。G2111Y，G2100C，G2100W，G2100Y等。



![img](https://cdn.nlark.com/yuque/0/2025/png/804193/1755074263718-d4526378-f2b0-40d0-b057-1cab4b0d2a39.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_25%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 3.2、USB打印的日志

![img](https://cdn.nlark.com/yuque/0/2025/png/804193/1755074375467-0b0a3b0e-4f72-45b2-8fbf-ee9b13514e08.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_37%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

# 四、GPS定位扩展数据上报

通过libgnss库获取到更多GPS功能数据，比如速度，椭球高，速度，GPS时间等。

https://docs.openluat.com/osapi/core/libgnss/

```lua
function 
	sys.wait(15000)
	local taskname="userTask"
	log.info(taskname,"start")
	local nid=1
	local uid=1
	local netsta =0
	while true do 
		local d ={}
		d.datetime=os.date("%Y-%m-%d %H:%M:%S")
		d.csq=mobile.csq()
		d.imei=mobile.imei()
		d.iccid = PerGetIccid()
		d.vbatt=PerGetVbattV()
		d.gps={}
		d.gps.isFix=libgnss.isFix()
		if libgnss.isFix() then 
			local tg =libgnss.getRmc(2)
			local ga =libgnss.getGga(2)
			d.gps.lat=tg.lat
			d.gps.lng=tg.lng
			d.gps.speed=tg.speed   --速度
			d.gps.altitude=ga.altitude   --海拔
			local t = {year=tg.year,month=tg.month,day=tg.day,hour=tg.hour,min=tg.min,sec=tg.sec}    --GPS TIME
			d.gps.gpstime=os.date("%Y-%m-%d %H:%M:%S",os.time(t))
			d.gps.variation=tg.variation   --航向
		end 
		d.lbs={}
		d.lbs.lbslat,d.lbs.lbslng = GetLbs()
		local updata = json.encode(d)
		local netsta = PronetGetNetSta(nid)
		log.info(taskname,"updata",updata,"netsta",netsta)
		if updata and netsta ==1 then  
			PronetSetSendCh(nid,updata)
		end
		sys.wait(10000)
	end 
end
```