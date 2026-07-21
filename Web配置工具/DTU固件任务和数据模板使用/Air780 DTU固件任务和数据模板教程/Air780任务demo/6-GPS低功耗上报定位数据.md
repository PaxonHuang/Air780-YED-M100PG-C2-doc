任务和数据模板代写:可以先参考demo自己修改一下，如果不会修改，可以提交需求文档，然后让银尔达工程师评估代写！根据工作量，简单的不收费，难的可能会收费。



# 零、任务教学视频

讲解了任务能做什么，如果提需求，如何看API，如何检查语法，调试打印日志

https://www.bilibili.com/video/BV1pN1aBuEr7

# 一、功能描述

电池供电等场景需要低功耗。设备周期唤醒上报定位数据给服务。程序关闭了4G和GPS模块电源。

还有一种情况是4G模块还要工作，只关闭GPS模块也能节省电源。

# 二、任务代码

## 2.1、任务

本任务实现等待网络最长60秒，等到GPS最长60秒，然后周期200秒定时休眠唤醒。可以自己修改唤醒时间。

```lua
function 
	mobile.flymode(0, false)
	local taskname="userTask"
	log.info(taskname,"start")
	local nid=1
  libgnss.debug(true)
	
	function TaskSysInSuperLowPower(t)
		--关闭串口
		uart.close(1)
		uart.close(2)
		uart.close(3)
		adc.close(0)
		adc.close(1)
		adc.close(2)
		adc.close(3)
		--关闭LED
		PerSetLedSta(0)
		--关闭内部RS485芯片电源
		PerSetResPower(0)
		--关闭GPS芯片
		GpsSetPower(0)
		--等待系统LED关闭
		while PerGetLedSta()==1 do 
			log.info("TaskSysInSuperLowPower wait led")
			sys.wait(500)
		end 
		gpio.close(30) --关闭不用的GPIO
		gpio.close(22)
		gpio.close(27)
			   
		if t < 100 then 
			t =100
		end 
		t =t*1000
		log.info("TaskSysInSuperLowPower",t)
		mobile.flymode(0, true)
		pm.power(pm.USB, false)
		pm.force(pm.HIB)
		pm.dtimerStart(3,t) --设置唤醒时间
		sys.wait(5000) 
		pm.reboot() --一般不会进入这个模式的,这个地方重启是解决进入低功耗异常
	end 

	--等待网络正常,最多等60秒
	local netcount =0
	while netcount < 60 do 
		if PronetGetNetSta(nid) ==1 then 
			break
		end 
		netcount =netcount +1
		sys.wait(1000)
	end 
	--等待GPS定位成功，最多等60秒
	local gpscount=0
	local lngt,latt,speed =nil,nil,nil
	while  gpscount < 60 do 
		if libgnss.isFix() then 
			local tg =libgnss.getRmc(2)
			lngt=tg.lng
			latt=tg.lat
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
	LbsCheckLbs()
	d.lbs={}
	d.lbs.lbslat,d.lbs.lbslng = GetLbs()
	local updata = json.encode(d)
	log.info(taskname,"updata",updata)
	if updata and PronetGetNetSta(nid) ==1 then 
		PronetSetSendCh(nid,updata)
	end
	sys.wait(2000)
	SysNeedInSuperPower()
	log.info(taskname,"superlowpower")
	TaskSysInSuperLowPower(200)--休眠唤醒时间,单位秒
end
```

## 2.2、任务讲解

libgnss.debug会把GPS的输出日志全部打印到luatool上面，如果不需要就设置成false

![img](https://cdn.nlark.com/yuque/0/2025/png/804193/1755827337585-0b10a418-a3f4-467c-b9e2-b0eb6ac9eddd.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_22%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

gps，网络这些运行时间越长，功耗越高，根据实际需求调整等待时间和唤醒频率。

libgnss.getRmc是获取经纬度，速度这些，还可以获取海拔，航向，方向角和基站WIFI定位信息信息，具体可以参考合宙API和下面第三点的扩展数据上报。

![img](https://cdn.nlark.com/yuque/0/2025/png/804193/1755827781975-690ce0d8-3c4e-40a4-9e74-42af000d886c.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_21%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

# 三、GPS定位扩展数据上报

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
	libgnss.on("raw", function(data)
		log.info("GNSS", data)
	end)
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
		LbsCheckLbs()
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